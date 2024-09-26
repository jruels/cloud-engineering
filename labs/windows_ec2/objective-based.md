### Create a Windows EC2 instance

You will create and configure a Windows EC2 instance on AWS in this lab. You will learn to select the appropriate Amazon Machine Image (AMI), configure networking settings, assign a public IP, and manage security groups. Additionally, you will decrypt the Admin password using the previously created key pair, launch the instance, and establish a Remote Desktop Protocol (RDP) connection.

---

### **Learning Objectives**

- Launch a Windows EC2 instance on AWS using a t2.micro instance type.
- Configure the instance’s network settings, including assigning a public IP and selecting the appropriate security group.
- Use a key pair to decrypt the admin password.
- Launch the instance and wait for it to enter the running state.
- Using the decrypted password, connect to the instance via Remote Desktop Protocol (RDP).

---

### **Problem 1: Launch a Windows EC2 Instance**

#### **Scenario:**

Launching a new EC2 instance using the AWS Windows Server AMI is your first task. You will configure the network, assign a public IP, and set up the instance with the required storage and tags.

#### **Tasks:**

- Navigate to the EC2 dashboard in the AWS Management Console.
- Launch a new Windows EC2 instance using the free-tier Windows Server AMI.
- Ensure the instance type is set to **t2.micro**.
- Configure the network settings and enable **Auto-assign Public IP**.
- Add the tag `Name: WinRDP` to the instance.

---

### **Problem 2: Configure Security Group and Select a Key Pair**

#### **Scenario:**

Once the instance is configured, you must select a security group to control access and select a key pair to securely access the instance using RDP.

#### **Tasks:**

- Create a new security group to enable RDP access.
- Select a key pair and ensure you have the `.pem` file on your local machine.

---

### **Problem 3: Launch the EC2 Instance**

#### **Scenario:**

After configuring the security group and the key pair, you can launch the instance. Once launched, it must be in a running state before you can connect to it.

#### **Tasks:**

- Launch the instance with the selected configurations.
- Navigate to the **View Instances** page and wait for the instance to enter the **running** state.

---

### **Problem 4: Decrypt the Instance Password**

#### **Scenario:**

To connect to the instance via RDP, you must decrypt the administrator password using the key pair from earlier. Once decrypted, you will use this password to establish the RDP connection.

#### **Tasks:**

- Download the **Remote Desktop File** for the instance.
- Use the **Get Password** option to decrypt the password by uploading your key pair.
- Copy the decrypted password for future use.

---

### **Problem 5: Connect Using Remote Desktop Protocol (RDP)**

#### **Scenario:**

Now that you have the decrypted password and the Remote Desktop file, you can establish an RDP connection to the instance. This will allow you to log in to the Windows server and manage it remotely.

#### **Tasks:**

- Open the **Remote Desktop File** (.rdp) you downloaded earlier.
- Use the decrypted password to log in and establish the RDP connection.
- Verify that the connection works and the Windows server instance is accessible.

---

### **Lab Summary**

In this lab, you launched and configured a Windows EC2 instance on AWS. You configured key instance settings, including network configuration, security groups, and key pairs. After launching the instance, you decrypted the password and successfully connected to the instance using RDP. By completing these tasks, you have gained hands-on experience deploying and managing Windows instances on AWS and securely accessing them.

