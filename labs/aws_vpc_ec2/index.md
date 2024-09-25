# AWS - Create a VPC and EC2 instance

## Log into the AWS Console

Log into the AWS Console in a browser using the information from the spreadsheet.

Once you have logged in, select **AWSAdministratorAccess**

In the console, on the top right side, select the `us-west-1` region.



---

## Introduction 

In this lab, we will walk through creating a **Virtual Private Cloud (VPC)** and launching an **EC2 instance** into the newly created VPC on AWS. This lab focuses on building a basic, isolated network infrastructure that can host cloud workloads while introducing the fundamentals of AWS VPC configuration, subnets, and EC2 instance management. By the end of this lab, you will have a functional VPC with an EC2 instance that is isolated and ready to be further developed with networking components like subnets, route tables, and internet gateways.

### Create a New Virtual Private Cloud (VPC)

1. **Access the VPC Console**:
   - In the search bar at the top of the AWS portal, type "VPC" and select **VPC** from the dropdown list.
   - This will take you to the VPC dashboard, where you can manage existing VPCs and create new ones.

2. **Create a VPC**:
   - In the upper left corner of the dashboard, click **Create VPC** to begin configuring your new VPC.
   - Under **Resources to create**, ensure that **VPC and more** is selected. This allows AWS to generate additional networking components like subnets and route tables automatically.

3. **Set VPC Name**:
   - Under **Name tag auto-generation**, select **Auto-generate** and enter *Example* as the VPC name tag.
   - AWS uses this name to identify your VPC and its associated resources.

4. **Preview VPC Components**:
   - In the **Preview section**, review the structure of the VPC that AWS will create. This section shows a high-level view of the subnets, route tables, and network connections that will be built for you.
   
5. **Adjust the IPv4 CIDR Block**:
   - Under **IPv4 CIDR block**, change the number of bits in the address to **/24** (e.g., 10.0.0.0/24). This creates a smaller network, limiting the number of IP addresses available to the VPC to 256.
   
6. **Availability Zones (AZs)**:
   - Scroll down to **Number of Availability Zones (AZs)** and select **1**. This ensures that your VPC resources, such as subnets, will only be deployed in a single availability zone, simplifying the lab.
   
7. **Subnet Configuration**:
   - Under **Number of public subnets**, select **0**. This configuration will create a private network, where instances won't be directly accessible from the internet.
   - Notice how the preview automatically updates based on your selections.

8. **Create the VPC**:
   - Leave the remaining settings at their default values, and click **Create VPC**. Once the VPC is created, you will see a confirmation message.
   - Click **View VPC** to see the VPC details and its associated resources, like route tables and subnets.

---

### Launch an EC2 Instance into the Newly Created VPC

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

---

### Summary

In this lab, we successfully created a custom **Virtual Private Cloud (VPC)** with private network configuration and launched an **EC2 instance** into this isolated network. We configured the VPC with a **/24 CIDR block**, restricting the network size, and selected a single availability zone to simplify the deployment. Additionally, we launched a **t2.micro EC2 instance** using an Amazon Linux AMI and configured SSH access with a key pair. By completing these steps, you now have an operational VPC and EC2 instance within a controlled environment, forming the basis for further cloud networking exploration and development.