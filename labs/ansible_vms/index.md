# Create Ansible instances

In this lab, you will apply what you learned about Terraform to create some EC2 instances using variables and modules. 

After the instances are online, you will install and configure Ansible to manage them. 



## Create a Cloud9 environment

This lab requires a Cloud9 environment, as CloudShell lacks the required disk space.

To create a Cloud9 environment: 

1. Confirm you are in the `US-WEST-1` region.
2. Search for and open Cloud9 at the top of the AWS Console. 
3. Create a new environment with the following settings: 
4. Name: `tf-ansible`
5. Instance type: `t3.small`
6. Click "Create"

Open the Cloud9 IDE



## Setup lab files 

Create a working directory for the lab files:

```sh
mkdir tf-ansible1 
cd $_
```



## Install Terraform 

Install Terraform in Cloud9 using the steps from the previous lab. 



In the new working directory, create a `main.tf` that:

* Creates an ec2 instance

  * region = `us-west-1`

  * Queries EC2 for the latest Ubuntu 20.04 AMI

    ```json
    ##Find latest Ubuntu AMI
    data "aws_ami" "ubuntu" {
      most_recent = true
      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
      }
      filter {
        name   = "virtualization-type"
        values = ["hvm"]
      }
      owners = ["099720109477"] # Canonical
    }
    ```

  * Applies the tag

    * `Name = tf-ansible`

  * Set `ami` value to the queried result from above

    * `data.aws_ami.ubuntu.id`

Initialize this configuration.

```sh
terraform init
```

Now apply the configuration.

```sh
terraform apply
```



## Create multiple instances 

Use a `number` type to define how many instances this configuration supports. 

Add a variable block to `variables.tf` with the following: 

- variable name: `instance_count`
- description: `Number of instances to provision.`
- type: number 
- default: `2`

Update the EC2 instances resource to use the `instance_count` variable in `main.tf`

Terraform will convert the values into the correct type. The `instance_count` variable would also work using a string ( "2" ) instead of number ( 2 ). 

Apply your changes

```bash
terraform apply
```



## Create an SSH key

To manage our instances, we must be able to authenticate. This is done using an SSH key, which we will create using Terraform.



The private key will be saved in Cloud9, while the public key will be stored on AWS.

Before we can create the key pair, we’ll need to create an SSH key. This can be done by executing `ssh-keygen`. When you’ve executed this command, you’ll be asked to choose a directory where it can store the eys and to set a passphrase. Choose the default options by hitting return multiple times. 

After we’ve created an SSH key, we can update our Terraform code to create a key pair. In this code, we’ll define what we want the key pair to be called and what our public SSH key is. Retrieving the public SSH key is easy; you’ll configure variables for the private and public keys.

Add two variable blocks to `variables.tf`

- variable name: `public_key_path`
- description: `Path to the public SSH key.`
- type: string 
- default: `/home/<username>/.ssh/id_rsa.pub`



- variable name: `private_key_path`
- description: `Path to the private SSH key.`
- type: string 
- default: `/home/<username./.ssh/id_rsa`



Add an `aws_key_pair` resource with the name of `keypair` to the `main.tf` file.  It must have two resource arguments: 

- `key_name` 
- `public_key` - paste the contents of `$HOME/.ssh/id_rsa.pub`

```json
resource "aws_key_pair" "keypair" {
    key_name    = "TerraformAnsible-Keypair"
    public_key  = "public-key-here"
}
```



## Update Terraform configuration

Add the following to the `main.tf` file

* Right below the `aws_key_pair` resource add a security group

  ```json
  resource "aws_security_group" "sg-ec2" {
      name        = "tfansible"
  
      ingress {
          description = "HTTP from everywhere"
          from_port   = 80
          to_port     = 80
          protocol    = "tcp"
          cidr_blocks = ["0.0.0.0/0"]
      }
  
      ingress {
          description = "SSH from everywhere"
          from_port   = 22
          to_port     = 22
          protocol    = "tcp"
          cidr_blocks = ["0.0.0.0/0"]
      }
  
      egress {
          from_port   = 0
          to_port     = 0
          protocol    = "-1"
          cidr_blocks = ["0.0.0.0/0"]
      }
  
      tags = {
          Name = "tfansible"
      }
  }
  ```

* Update the `aws_instance` resource

  * `depends_on` = `[aws_security_group.sg-ec2]`
  * `vpc_security_group_ids` = `[aws_security_group.sg-ec2.id]`
  * `key_name` = `aws_key_pair.keypair.key_name`

  

  ## Add outputs

  Create `outputs.tf` with the following 

  ```json
  output "instance_ids" {
    description = "IDs of EC2 instances"
    value       = aws_instance.tfansible.*.id
  }
  output "instance_public_ips" {
    description = "Public IPs of EC2 instances"
    value       = aws_instance.tfansible.*.public_ip
  }
  ```

  

  Apply your changes

  ```
  terraform apply
  ```

  

  ## Verify connectivity

  Very you can SSH to the newly created machines

  ```
  ssh ubuntu@<node ip>
  ```

  exit the VM

## Install Ansible

To manage the instances using Ansible, we must first install it in Cloud9. 

Run the following command to install Ansible using Pip.

```bash
pip install ansible
```

After installation is complete confirm it was installed successfully.

```bash
ansible --version 
```

The output should contain a version similar to: 

```
ansible [core 2.11.12] 
```



## The Scenario

Our company has been increasing the deployment of small brochure-style websites for clients. The head of IT has decided that each client should have their own web server for better client isolation and has tasked us with creating concept automation to quickly deploy web nodes with simple static website content.

We must create an Ansible inventory containing a host group named `web`. The web group should contain `node1` and `node2`.

Then we’ve got to design an Ansible playbook that will execute the following tasks on your configured inventory:

- Install `apache2`
- Start and enable the `apache` service
- Install a simple website provided on a repository server.



### Prerequisites

Create and enter a working directory

```
 mkdir lab-playbook-fun && cd lab-playbook-fun
```

## Create an inventory

Create an inventory that contains a Host Group named `web` containing node1 and node2

```
[web]
node1 ansible_host=<IP of node1>
node2 ansible_host=<IP of node2>
```

## Create a Playbook

Using Vim, create`lab-playbook-fun/web.yml` file with these contents:

```
---
- hosts: web
  become: yes
  tasks:
    - name: install apache
      apt: 
        name: apache2 
        state: latest
    - name: start and enable apache
      service: 
        name: apache2 
        state: started 
        enabled: yes
    - name: retrieve website from repo
      get_url: 
        url: https://github.com/jruels/ansible-best-practices/raw/main/labs/playbook-fun/files/website.zip 
        dest: /tmp/website.zip
    - name: install website
      unarchive: 
        remote_src: yes 
        src: /tmp/website.zip 
        dest: /var/www/html/
```

## Execute the playbook

```
ansible-playbook --private-key ~/.ssh/id_rsa -u ubuntu -i inventory web.yml 
```

This playbook will fail with the following error:

```
Make sure the required command to extract the file is installed. Unable to find required 'unzip' or 'zipinfo' binary in the path.
```

## Update the playbook

The playbook must be updated to install the `unzip` package.

Change the `apt` section to install `apache2` and `unzip`

```
tasks:
  - name: install apache and unzip
    apt: 
      name:
        - unzip
        - apache2
      state: latest
```

Rerun the playbook and confirm it is succesful.

## Conclusion

This new setup is everything we need. The Ansible playbook installs `httpd` and `unzip`, starts and enables it, and creates a simple website, on its own web node. That’s what we needed. Congratulations!

