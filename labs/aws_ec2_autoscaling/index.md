### **AWS - Auto Scale EC2 instances**

In this lab, you will create an Auto Scaling Group for a set of EC2 instances to dynamically scale in and out based on traffic and load. You will configure and launch three **t2.micro** instances named **web-1**, **web-2**, and **web-3** using the default Amazon Linux image, create a default VPC, and set up an Auto Scaling Group to efficiently manage your infrastructure. You will also test scaling behavior by generating load and observing the automatic scaling of the instances.

---

### **Learning Objectives**

- Create a default VPC and set up necessary networking components.
- Launch three **t2.micro** instances named **web-1**, **web-2**, and **web-3** using the default Amazon Linux image.
- Configure and create a Launch Template for Auto Scaling.
- Set up an Auto Scaling Group to automatically adjust instance count based on load.
- Test the scaling behavior by generating load on the instances and observing how Auto Scaling responds.

---

### **Step-by-Step Solution: Auto Scaling with EC2 Instances**

---

### **Step 1: Create a Default VPC**

1. **Log into AWS**:
   - Log into the AWS Management Console using the credentials provided.

2. **Navigate to the VPC Dashboard**:
   - In the AWS Management Console, type **VPC** in the search bar and select **VPC** from the dropdown.
   - Click **Actions** -> **Create default VPC**.

   - This creates a default VPC with all the necessary components (subnets, internet gateway, route table).
   - After creating the VPC, navigate back to the **EC2 Dashboard**.

---

### **Step 2: Launch Three t2.micro Instances (web-1, web-2, web-3)**

1. **Navigate to the EC2 Dashboard**:
   - In the AWS Management Console, type **EC2** in the search bar and select **EC2** to navigate to the EC2 Dashboard.
2. **Launch Instances**:
   - Click **Launch Instance**.
3. Name it `web-1`
4. **Select Amazon Linux AMI**:
   - Select the **Amazon Linux AMI**
5. **Choose Instance Type**:
   - Choose **t2.micro**.
6. **Configure Instance Details**:
   - Select the **default VPC** that was created earlier.
   - Enable **Auto-assign Public IP**.
   - Leave other options as default.
7. **Add Storage**:
   - Keep the default storage options.
8. **Configure Security Group**:
   - Choose to **Create a new security group**.
   - Allow **HTTP (port 80)** and **SSH (port 22)** access.
   - Click **Review and Launch**.
9. **Launch Instance**:
   * Choose **Launch Instance**

1. **Repeat for web-2 and web-3**:
   - Repeat the steps above to launch two more instances, naming them **web-2** and **web-3**.

---

### **Step 3: Create a Launch Template for Auto Scaling**

1. **Navigate to Launch Templates**:
   - In the EC2 Dashboard, click **Launch Templates** under the **Instances** section.
2. **Create Launch Template**:
   - Click **Create Launch Template** and set the following values:
     - **Launch Template Name**: WebTemplate
     - **Template Version Description**: Version 1
     - **AMI**: Amazon Linux AMI (use the same one you used for the three instances)
     - **Instance Type**: t2.micro
     - **Key Pair**: Select the key you created earlier.
     - **Subnet**: Don't include in launch template. We will configure this in the auto scaling group.
     - **Security Group**: Select the security group that allows **HTTP** and **SSH** access.
     - **EBS Volumes**: Use the default storage (8 GiB) and click **Create Launch Template**.

---

### **Step 4: Create an Auto Scaling Group**

1. **Navigate to Auto Scaling Groups**:
   - In the EC2 Dashboard, click **Auto Scaling Groups** on the left-hand side.

2. **Create Auto Scaling Group**:
   - Click **Create Auto Scaling Group**.

3. **Configure the Group**:
   - Set the following values:
     - **Auto Scaling Group Name**: WebInstances
     - **Launch Template**: Select **WebTemplate** (the one you created earlier).
     - **VPC**: Select the default VPC.
     - **Availability Zones**: Choose both available subnets for the default VPC.

4. **Configure Scaling Policies**:
   - **Desired Capacity**: 1
   - **Minimum Capacity**: 1
   - **Maximum Capacity**: 3
   - Select **Target Tracking Scaling Policy**.
   - Set **Target Value** to **85% CPU Utilization**.
   - Set **Instances need X seconds warm-up** to **300 seconds**.
   - Click **Next** and follow through the steps to create the Auto Scaling Group.

---

### **Step 5: Generate Load to Test Auto Scaling**

1. **Install Apache on web-1, web-2, and web-3**:
   - Connect to each instance via SSH using the key pair you created earlier:

   ```bash
   ssh -i webkeypair.pem ec2-user@<PUBLIC_IP_OF_INSTANCE>
   ```

   - Install Apache web server on each instance:

   ```bash
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

2. **Generate Load**:
   - On one of the instances, generate CPU load to simulate high traffic:

   ```bash
   sudo yum install stress -y
   stress --cpu 8 --timeout 300
   ```

   - This will generate CPU load for 5 minutes, causing the Auto Scaling Group to launch new instances as the CPU utilization exceeds the target threshold.

3. **Monitor Scaling**:
   - Navigate to the **Auto Scaling Group** in the EC2 Dashboard and monitor the number of instances.
   - You should see the Auto Scaling Group launch additional instances (up to 3) as load increases.

---

### **Step 6: Clean Up Resources**

1. **Terminate Instances**:
   - Once testing is complete, navigate to the **EC2 Instances** page.
   - Select all instances (web-1, web-2, and web-3) and click **Terminate**.

2. **Delete Auto Scaling Group**:
   - Navigate to the **Auto Scaling Groups** page.
   - Select the **WebInstances** group and click **Delete**.

3. **Delete Launch Template**:
   - Navigate to **Launch Templates** and delete the **WebTemplate** you created earlier.

---

### **Conclusion**

In this lab, you successfully launched three EC2 instances, created a default VPC, configured a launch template, and set up an Auto Scaling Group to manage traffic loads. You tested the scaling behavior by generating load, and observed the Auto Scaling Group adding or removing instances based on traffic demand.