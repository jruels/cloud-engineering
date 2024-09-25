# Linux file attributes

## Introduction 

This section will cover how to launch an EC2 instance into the Virtual Private Cloud (VPC) you created earlier. Following these steps, you will configure an EC2 instance using an Amazon Machine Image (AMI) with the appropriate settings, including instance type, key pair, network security group, and storage. Additionally, we will explore how to connect to the instance remotely using SSH and demonstrate basic file management tasks using absolute and relative paths in the terminal. By the end of this section, you’ll have a running EC2 instance and hands-on experience with navigating the file system in Linux.

## Access EC2 instance

#### Connecting to the Lab

1. Open your terminal application, and run the following command (remember to replace `<PUBLIC_IP>` with the public IP of your EC2 instance):

```bash
ssh -i lab.pem ec2-user@<PUBLIC_IP>
```

### Download the lab files

In the terminal run the following commands: 

```bash
cd $HOME
wget -O "files.zip" https://www.dropbox.com/scl/fi/fdjtplmjx1yegv1ou8zw8/files.zip?rlkey=wmtlg2gaqvpc5zdv3tvrbryri&dl=1
```



### Ensure unzip is installed

```bash
dnf install -y unzip
```



### Extract the file

```bash
unzip files.zip
```



### Determine Which File Is the Oldest

2. Determine the current working directory.

```bash
pwd
```

3. List the contents of the current working directory.

```bash
ls
```

4. Change to the `Practice` folder.

```bash
cd Practice/
```

5. List the contents of the `Practice` folder.

```bash
ls
```

6. Change to the `Test` folder.

```bash
cd Test/
```

7. Open the man page for the `ls` command.

```
man ls
```

* Locate the entry for the `-l` option, and read the description.

* Locate the entry for the `-A` option, and read the description.

* Locate the entry for the `-t` option, and read the description.

8. Run the following command to determine the oldest file:

```
ls -lAt ~/Practice/Test/var/log/
```

### Determine Which File Is the Largest

1. Open the man page for the `ls` command.

```
man ls
```

2. Locate the entry for the `-S` option, and read the description.

3. Run the following command to determine the largest file:

```
ls -lS ~/Practice/Test/var/log/
```

### Determine when a file was last modified

1. Run the following command:

```
ls -l ~/Practice/Test/sos_commands/networking/netstat_-W_-neopa
```

## Determine when a file was last accessed

1. Open the man page for the `ls` command.

```
man ls
```

* Locate the entry for the `-u` option, and read the description.

* Run the following command to determine when the file was last accessed:

```
ls -lu ~/Practice/Test/sos_commands/networking/netstat_-W_-neopa
```

2. Check the current date to confirm.

```
date
```