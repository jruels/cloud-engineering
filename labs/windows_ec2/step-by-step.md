### Creating a Windows EC2 Instance on AWS**

---

### **Step 1: Launch a Windows EC2 Instance**

1. **Navigate to the EC2 Dashboard**:
   - Open the AWS Management Console.
   - In the search bar at the top, type **EC2** and select it to access the EC2 dashboard.

2. **Click "Launch Instance"**:
   - On the EC2 dashboard, click **Launch Instance** to begin the process of creating a new EC2 instance.

3. **Select an Amazon Machine Image (AMI)**:
   - On the **Choose an Amazon Machine Image (AMI)** page, scroll down to find and select the **Windows Server AMI**.

4. **Choose an Instance Type**:
   - On the next page, ensure that **t2.micro** is selected (this is the free-tier eligible instance).
   - Click **Next: Configure Instance Details**.

---

### **Step 2: Configure Instance Details**

1. **Configure Network Settings**:
   - On the **Configure Instance Details** page, select a **Network** and **Subnet** with public access.

2. **Enable Auto-assign Public IP**:
   - Set **Auto-assign Public IP** to **Enable** to ensure that the instance will be accessible via a public IP.

3. **Proceed to the Next Step**:
   - Leave Storage default settings.
   - **Add Tags**

---

### **Step 3: Add Tags to Your Instance**

1. **Add a Name Tag**:
   - On the **Add Tags** page, click **Add Tag**.
   - Set the **Key** field to **Name** and the **Value** field to **WinRDP**.

2. **Proceed to the Next Step**:
   - Click **Next: Configure Security Group**.

---

### **Step 4: Configure Security Group**

1. **Create a new Security Group**:
   - On the **Configure Security Group** page, use the default new group with RDP access.

---

### **Step 5: Select a Key Pair**

1. **Select a Key Pair**:
   - In the **Key Pair** section, select your existing key pair.
2. **Download the Key Pair**:
   - Confirm you have the `.pem` file on your local machine.
3. **Launch the Instance**:
   - Click **Launch Instances**.

---

### **Step 6: Monitor Instance Launch**

1. **View Instances**:
   - After launching the instance, click **View Instances** to navigate back to the EC2 dashboard.
   - Wait a few minutes for the instance to enter the **running** state.

---

### **Step 7: Connect to the Instance**

1. **Download Remote Desktop File**:
   - Once the instance is running, select it in the dashboard and click **Connect** at the top.
   - Click **Download Remote Desktop File** and save the `.rdp` file to your local machine.

2. **Decrypt the Password**:
   - On the **Connect** page, click **Get Password**.
   - Click **Browse…** and upload the `.pem` key file you downloaded earlier.
   - Click **Decrypt Password** to get the password, then copy it for later use.

---

### **Step 8: Connect Using Remote Desktop Protocol (RDP)**

1. **Open the Remote Desktop File**:
   - Navigate to your **Downloads** folder and open the `.rdp` file you saved earlier.

2. **Handle Security Warnings**:
   - If prompted with a message about the connection not being secure, click **Continue**.

3. **Log In to the Instance**:
   - When prompted for credentials, paste the password you decrypted earlier.
   - Click **OK** and **Continue** to establish the connection.

4. **Access the Windows Instance**:
   - The RDP connection should now open, giving you access to your Windows EC2 instance.

---

### **Conclusion**

In this lab, you successfully launched a Windows EC2 instance on AWS, configured it with appropriate settings (including network, security groups, and key pair), and connected to it using Remote Desktop Protocol (RDP). By following these steps, you gained practical experience with launching and managing Windows instances in the cloud.


