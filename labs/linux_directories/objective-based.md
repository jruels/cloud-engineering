### **Problem-Solving Lab: Managing Files and Analyzing Data on an EC2 Instance**

---

### **Lab Overview**

In this lab, you'll tackle file management and analysis tasks on an EC2 instance within a Virtual Private Cloud (VPC). The goal is to test your ability to navigate the Linux file system, download and manipulate files, and use Linux commands to solve common file-related challenges. You'll have practical experience managing files and working with core Linux utilities by the end.

---

### **Learning Objectives**

- Establish a remote connection to an EC2 instance using SSH.
- Download and manage compressed files using `wget` and `unzip`.
- Analyze file properties and solve tasks using commands like `ls`.
- Use absolute and relative paths effectively in the Linux terminal.
- Understand and manipulate file attributes such as size, modification time, and access time.

---

### **Problem 1: Access the EC2 Instance and Download Files**

#### Scenario:

You need to access an EC2 instance and download a set of files from a remote server. Use `wget` to download the file from the provided URL and ensure `unzip` is installed to extract its contents.

**Tasks:**
- SSH into the EC2 instance using the provided key pair.
- Download the file from the following URL:  
  `https://www.dropbox.com/scl/fi/fdjtplmjx1yegv1ou8zw8/files.zip?rlkey=wmtlg2gaqvpc5zdv3tvrbryri&dl=1`
- Ensure `unzip` is installed on the instance.
- Extract the `files.zip` file and prepare the files for further analysis.

---

### **Problem 2: Identify the Oldest File**

#### Scenario:

You are tasked with finding the oldest file in the `/var/log` directory of the extracted files. Use the `ls` command to sort files by modification time and determine which file was modified the earliest.

**Tasks:**
- Navigate to the `Practice/Test/var/log/` directory.
- Use the `ls` command to list all files in the directory, sorted by modification date.
- Identify the oldest file in the directory.

---

### **Problem 3: Determine the Largest File**

#### Scenario:

In the same directory, you need to determine which file is the largest. Modify your approach with the `ls` command to list files by size.

**Tasks:**
- Use the `ls` command to list the files by size in descending order.
- Identify which file is the largest based on the output.

---

### **Problem 4: Check When a File Was Last Modified**

#### Scenario:

You are investigating a file located in the `sos_commands/networking` directory. Your goal is to determine the last time this file was modified.

**Tasks:**
- Locate the file `netstat_-W_-neopa` in the `sos_commands/networking/` directory.
- Use the appropriate `ls` option to display the file's last modification time.

---

### **Problem 5: Check When a File Was Last Accessed**

#### Scenario:

After reviewing the modification time, your next task is to find out when the file `netstat_-W_-neopa` was last accessed. This information will help you verify recent activity on the file.

**Tasks:**
- Use the `ls` command to display the file's last access time.
- Verify the current system date using the `date` command to compare against the access time.

---

### **Lab Summary**

In this lab, you were presented with various challenges requiring you to manage and analyze files on an EC2 instance. You successfully connected to the instance using SSH, downloaded and extracted files, and used the `ls` command to solve problems such as identifying the oldest and largest files, and determining when files were last modified and accessed. By completing these tasks, you gained practical experience with file management and Linux utilities, preparing you for further system administration challenges.