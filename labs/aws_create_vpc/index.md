# AWS - Create a Basic VPC and Associated Components

## Introduction

AWS networking consists of multiple components, and understanding the relationship between the networking components is key to understanding the overall functionality and capabilities of AWS. In this hands-on lab, we will create a VPC with an internet gateway and subnets across multiple Availability Zones.

### Create a VPC

1. Navigate to **VPC**.

2. Click **Create VPC**.

3. For **Resources to create**, select **VPC only**.

4. Configure the VPC settings:

   - **Name tag**: In the text field, enter *VPC1*.
   - **IPv4 CIDR**: In the text field, enter *172.16.0.0/16*.

5. Leave the other settings as the defaults and click **Create VPC**.

   The VPC is created but does not have an internet gateway, subnets, or any other functionality.

### Create an Internet Gateway, and Attach It to the VPC

1. In the sidebar menu, navigate to **Virtual private cloud** and select **Internet gateways**.

2. On the right, click **Create internet gateway**.

3. In the **Name tag** field, enter *IGW*.

4. Click **Create internet gateway**.

5. After the internet gateway is created, use the **Actions** dropdown to select **Attach to VPC**.

6. In the **Available VPCs** field, select **VPC1**.

7. Click **Attach internet gateway**.

   Your internet gateway should now be in an **Attached** state.

### Create a Public and Private Subnet in Different Availability Zones

1. In the sidebar menu, select **Subnets**.

2. On the right, click **Create subnet**.

3. Configure the public subnet:

   - **VPC ID**: Use the dropdown to select **VPC1**.
   - **Subnet name**: In the text field, enter *Public1*.
   - **Availability Zone**: Use the dropdown to select **us-west-1a**.
   - **IPv4 CIDR block**: In the field, select **172.16.1.0/24**. Any EC2 instances assigned to this subnet will have an IP address prefix of `172.16.1`.

4. Click **Create subnet**.

   The public subnet is created.

5. On the right, click **Create subnet** again.

6. Configure the private subnet:

   - **VPC ID**: Use the dropdown to select **VPC1**.
   - **Subnet name**: In the text field, enter *Private1*.
   - **Availability Zone**: Use the dropdown to select **us-west-1b**.
   - **IPv4 CIDR block**: In the field, enter *172.16.2.0/24*.

7. Click **Create subnet**.

   The private subnet is created.

### Create Two Route Tables, and Associate Them with the Correct Subnet

#### Create the Route Tables

1. In the sidebar menu, select **Route tables**.

   You should see a default route table available. However, you will not use this route table.

2. On the right, click **Create route table**.

3. Configure the public route table:

   - **Name**: In the text field, enter *PublicRT*.
   - **VPC**: Use the dropdown to select **VPC1**.

4. Click **Create route table**.

5. In the sidebar menu, select **Route tables** again.

6. On the right, click **Create route table**.

7. Configure the private route table:

   - **Name**: In the text field, enter *PrivateRT*.
   - **VPC**: Use the dropdown to select **VPC1**.

8. Click **Create route table**.

#### Associate the Route Tables with the Correct Subnet

1. In the sidebar menu, select **Route tables** again.

2. Check the checkbox next to the **PublicRT** route table, ensuring that this is the only route table selected.

3. Add a route to the internet gateway:

   - Select the **Routes** tab and click **Edit routes**.

   - Click **Add route** to add a route to the internet gateway.

   - In the **Destination** field, select **0.0.0.0/0**.

   - In the **Target** field, select **Internet Gateway**, and then select the **igw** gateway.

   - Click **Save changes**.

     Now, any traffic that's not destined for the local network will be routed to the internet gateway.

4. In the sidebar menu, select **Route tables** again, and ensure the checkbox next to **PublicRT** is checked.

5. Associate the route table with the public subnet:

   - Select the **Subnet associations** tab.
   - In the **Subnets without explicit associations** section, click **Edit subnet associations**.
   - Check the checkbox next to the **Public1** subnet.
   - Click **Save associations**.

6. In the sidebar menu, select **Route tables** again.

7. Check the checkbox next to the **PrivateRT** route table, ensuring that this is the only route table selected.

8. Select the **Routes** tab and note that the local route already exists. For this route table, you don't need to create another route to the internet gateway.

9. Associate the route table with the private subnet:

   - Select the **Subnet associations** tab.
   - In the **Subnets without explicit associations** section, click **Edit subnet associations**.
   - Check the checkbox next to the **Private1** subnet.
   - Click **Save associations**.

### Create Two Network Access Control Lists (NACLs), and Associate Each With the Proper Subnet

#### Create and Configure a Public NACL

1. In the sidebar menu, navigate to **Security** and select **Network ACLs**.

   You should see a default NACL available. However, you won't use this NACL.

2. On the right, click **Create network ACL**.

3. Configure the public NACL:

   - **Name**: In the text field, enter *PublicNACL*.
   - **VPC**: Use the dropdown to select **VPC1**.

4. Click **Create network ACL**.

5. After the NACL is created, check the checkbox next to **PublicNACL**.

6. Select the **Inbound rules** tab, and then click **Edit inbound rules**.

7. Add two inbound rules:

   - Click **Add new rule**.
   - Fill in the rule details:
     - **Rule number**: In the text field, enter *100*.
     - **Type**: Use the dropdown to select **HTTP (80)**.
     - **Source**: Leave the source as **0.0.0.0/0**.
   - Click **Add new rule** again.
   - Fill in the rule details:
     - **Rule number**: In the text field, enter *110*.
     - **Type**: Use the dropdown to select **SSH (22)**.
     - **Source**: Leave the source as **0.0.0.0/0**.

8. Click **Save changes**.

9. From the **PublicNACL** details, select the **Outbound rules** tab and then click **Edit outbound rules**.

10. Add an outbound rule:

    - Click **Add new rule**.
    - Fill in the rule details:
      - **Rule number**: In the text field, enter *100*.
      - **Type**: Leave the type as **Custom TCP**.
      - **Port range**: In the text field, enter *1024-65535*.
      - **Destination**: Leave the destination as **0.0.0.0/0**.

11. Click **Save changes**.

12. From the **PublicNACL** details, select the **Subnet associations** tab, and then click **Edit subnet associations**.

13. Check the checkbox next to the **Public1** subnet.

14. Click **Save changes** to associate your public NACL with the public subnet.

#### Create and Configure a Private NACL

1. From the **Network ACLs** page, click **Create network ACL**.
2. Configure the private NACL:
   - **Name**: In the text field, enter *PrivateNACL*.
   - **VPC**: Use the dropdown to select **VPC1**.
3. Click **Create network ACL**.
4. Check the checkbox next to **PrivateNACL**, ensuring that it's the only NACL selected.
5. Select the **Inbound rules** tab and then click **Edit inbound rules**.
6. Add an inbound rule:
   - Click **Add new rule**.
   - Fill in the rule details:
     - **Rule number**: In the text field, enter *100*.
     - **Type**: Use the dropdown to select **SSH (22)**.
     - **Source**: In the text field, enter *172.16.1.0/24*.
7. Click **Save changes**.
8. From the **PrivateNACL** details, select the **Outbound rules** tab and then click **Edit outbound rules**.
9. Add an outbound rule:
   - Click **Add new rule**.
   - Fill in the rule details:
     - **Rule number**: In the text field, enter *100*.
     - **Type**: Leave the type as **Custom TCP**.
     - **Port range**: In the text field, enter *1024-65535*.
     - **Destination**: Leave the destination as *0.0.0.0/0*.
10. Click **Save changes**.
11. From the **PrivateNACL** details, select the **Subnet associations** tab, and then click **Edit subnet associations**.
12. Check the checkbox next to the **Private1** subnet.
13. Click **Save changes** to associate your private NACL with the private subnet.



## Congratulations 

In this lab, we built a foundational AWS network architecture by creating a **Virtual Private Cloud (VPC)** with a custom CIDR block. We attached an **Internet Gateway** to the VPC for Internet access and created **public** and **private subnets** across multiple availability zones for redundancy. To manage traffic, we configured two **route tables**: the public route table directs traffic to the internet, while the private route table isolates its subnet. We also established **Network Access Control Lists (NACLs)** to control traffic, allowing HTTP and SSH for the public subnet while restricting access to the private subnet. We created a secure, scalable network infrastructure that can support future cloud workloads.