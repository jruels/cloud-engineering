# Explore the Linux Filesystem

## Introduction 

This section will cover how to launch an EC2 instance into the Virtual Private Cloud (VPC) you created earlier. Following these steps, you will configure an EC2 instance using an Amazon Machine Image (AMI) with the appropriate settings, including instance type, key pair, network security group, and storage. Additionally, we will explore how to connect to the instance remotely using SSH and demonstrate basic file management tasks using absolute and relative paths in the terminal. By the end of this section, you’ll have a running EC2 instance and hands-on experience with navigating the file system in Linux.

### Launch an EC2 Instance 

1. **Access the EC2 Dashboard**:
   - In the search bar at the top of the AWS portal, type **EC2** and select it. This will take you to the EC2 management console, where you can launch and manage instances.
   
2. **Launch a New EC2 Instance**:
   - Under the **Launch instance** dropdown, click **Launch instance** to create a new instance.
   - Under **Name**, enter *Lab Example* to label your EC2 instance for easy identification.
   
3. **Choose an Amazon Machine Image (AMI)**:
   - Under **Application and OS Images (Amazon Machine Image)**, ensure that the **Amazon Linux** tile is selected. This AMI provides a lightweight, Linux-based operating system commonly used in cloud labs and development environments.
   
4. **Instance Type Selection**:
   - Under **Instance type**, ensure that **t2.micro** is selected. This is part of AWS's free-tier offering and provides a small, general-purpose instance suitable for basic workloads.
   
5. **Key Pair Configuration**:
   - Under **Key pair (login)**, click **Create a new key pair**. In the pop-up menu, enter *Lab Example* as the key pair name. 
   - Once the instance is launched, a key pair will allow you to securely SSH into it. Download the key pair file (.pem) to your local machine.
   
6. **Network Settings**:
   - Under **Network settings > Firewall (security groups)**, ensure **Create security group** is selected.
   - Ensure **Allow SSH traffic from** is selected, then select **Anywhere** from the dropdown menu to allow SSH access from any IP address. (Note: This is suitable for lab environments, but in production, limit access to specific IP ranges.)
   
7. **Storage Configuration**:
   - Under **Configure storage**, ensure that **8 GiB** of storage is allocated to the instance. This is enough for basic operations and is part of the AWS free tier.
   
8. **Launch the Instance**:
   - Leave the remaining settings at their default values and click **Launch Instance**. Once the instance is launched, a confirmation message will appear.
   - Click on the instance ID at the top of the page to view the instance details.
   
9. **Verify the Instance is Running**:
   - Wait for the instance to reach the **Running** state. You may need to click the refresh icon at the top of the page to update the instance status.
   
   - Review the **VPC ID** and **subnet ID** associated with the instance to ensure that they match the VPC and subnet you created earlier.
   
     

### Using Absolute and Relative Paths



#### Connecting to the Lab

1. Open your terminal application, and run the following command (remember to replace `<PUBLIC_IP>` with the public IP of your EC2 instance):

```bash
ssh -i lab.pem ec2-user@<PUBLIC_IP>
```

* Enter `yes` at the prompt.

#### Use Absolute Paths to Move the File

2. Determine the current working directory.

```bash
pwd
```

3. Create a new file:

```bash
echo 'Linux rules!' > $HOME/important.txt
```

4. Display the contents of the file

```bash
cat $HOME/important.txt
```

5. Redirect the contents of the file to a new file

```bash
cat $HOME/important.txt > $HOME/value.txt
```

6. List the contents of the current working directory.

```bash
ls
```

7. Display the contents of the `value.txt` file we just created.

```bash
cat value.txt
```

#### Use Relative Paths to Move the File

1. Change to the `/usr/share` directory.

```
cd /usr/share/
```

1. Run the following command:

```bash
cat ../../home/ec2-user/value.txt >> ../../home/ec2-user/value.txt
```

1. List the details for the `value.txt` file.

```bash
ls -l ~/value.txt
```

1. Change to the home directory.

```bash
cd
```

1. List the contents of the `value.txt` file.

```bash
cat value.txt
```



---

### Summary

In this section, you successfully launched an **EC2 instance** in the newly created VPC and learned to connect to it using SSH remotely. Additionally, you practiced essential Linux file management using absolute and relative paths, gaining a deeper understanding of directory navigation and file manipulation in a cloud environment. After completing these steps, you have acquired essential skills in cloud networking and Linux file operations.