### **AWS - Auto Scale EC2 instances**

In this lab, you will create an Auto Scaling Group for a set of EC2 instances to dynamically scale in and out based on traffic and load. You will create a default VPC and set up an Auto Scaling Group to manage your infrastructure efficiently. You will also test scaling behavior by generating load and observing the automatic scaling of the instances.

---

### **Learning Objectives**

- Create a default VPC and set up necessary networking components.
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

### **Step 2: Create a Launch Template for Auto Scaling**

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

### **Step 3: Create an Auto Scaling Group**

1. **Navigate to Auto Scaling Groups**:
   - In the EC2 Dashboard, click **Auto Scaling Groups** on the left-hand side.

2. **Create Auto Scaling Group**:
   - Click **Create Auto Scaling Group**.

3. **Configure the Group**:
   - Set the following values:
     - **Auto Scaling Group Name**: WebInstances
     - **Launch Template**: Select **WebTemplate** (the one you created earlier).
     - Click **Next**.
     - **VPC**: Select the default VPC.
     - **Availability Zones**: Choose both available subnets for the default VPC.
     - Click **Next**.

4. **Default routing**: Choose to create a new target group

   * **New target group name**: `web`

5. **Configure Scaling Policies**:

   - **Desired Capacity**: 1

   - **Minimum Capacity**: 1

   - **Maximum Capacity**: 3

6. Select **Target Tracking Scaling Policy**.

   - **Scaling poilcy name**: `CPU Average`

   - Set **Target Value** to **60% CPU Utilization**.

   - Set **Instances need X seconds warm-up** to **300 seconds**.

   - Click **Next** and follow through the steps to create the Auto Scaling Group.

7. After a few minutes, the Auto Scaling Group creates an EC2 instance.

---

### **Step 4: Generate Load to Test Auto Scaling**

1. **Connect to the server created by the auto-scaling group**:
   
   - Connect to the instance via SSH using the key pair you created earlier.
   
2. **Generate Load**:
   - On the instance, generate CPU load to simulate high traffic:

   ```bash
   sudo yum install stress -y
   stress --cpu 15 --timeout 300
   ```

   - This will generate CPU load for 5 minutes, causing the Auto Scaling Group to launch new instances as the CPU utilization exceeds the target threshold.

3. **Monitor Scaling**:
   - Navigate to the **Auto Scaling Group** in the EC2 Dashboard and monitor the number of instances.
   - You should see the Auto Scaling Group launch additional instances (up to 3) as load increases.

---

### **Step 5: Clean Up Resources**

1. **Terminate Instances**:
   - Once testing is complete, navigate to the **EC2 Instances** page.
   - Select all instances and click **Terminate**.

2. **Delete Auto Scaling Group**:
   - Navigate to the **Auto Scaling Groups** page.
   - Select the **WebInstances** group and click **Delete**.

3. **Delete Launch Template**:
   - Navigate to **Launch Templates** and delete the **WebTemplate** you created earlier.

---

## Congratulations! 

