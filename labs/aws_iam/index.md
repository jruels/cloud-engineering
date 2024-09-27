# AWS Identity and Access Management

### Introduction

AWS Identity and Access Management (IAM) allows you to securely manage user access, permissions, and roles. This lab will walk through the steps to create IAM users, groups, and policies to control access to AWS resources. We will focus on user management, group management, and custom IAM policies using the **`us-west-1`** region.

### Prerequisites

- An active AWS account
- Access to the AWS Management Console

### Solution Overview

In this lab, we will:

1. Create IAM users
2. Create user groups
3. Attach policies to groups
4. Assign users to groups
5. Test permissions by signing in as different users

### Instructions

#### Step 1: Log in to the AWS Management Console

1. Log in to the AWS Management Console using your credentials.
2. Ensure that the **`us-west-1`** region (N. California) is selected in the top-right corner of the console.

---

### Step 2: Create IAM Users

We will create three users: **user-1**, **user-2**, and **user-3**.

1. Navigate to **IAM** by typing "IAM" in the search bar and selecting **IAM** from the results.
2. In the left sidebar, click **Users**, then click **Create user**.
3. In the **User name** field, enter **user-1**.
4. Select the option **Provider user access to the AWS Console**, and choose **Custom password**. Provide a secure password for **user-1**.
5. Uncheck the option **Users must create a new password at next sign-in**.
6. Click **Next**.
7. Skip attaching any permissions at this stage by selecting **Next**.
8. (Optional) Add any tags you'd like to assign to the user, then click **Create user**.
9. Remember the password you set.
10. Repeat steps 2-9 for **user-2** and **user-3**.

---

### Step 3: Create IAM User Groups

Next, we'll create three groups: **EC2-Admin**, **EC2-Support**, and **S3-Support**.

1. In the **IAM** console, click **User groups** in the left sidebar, then click **Create group**.
2. In the **User group name** field, enter **EC2-Admin**.
3. On the **Attach permissions policies** page, skip attaching a policy for now, and click **Create user group**.
4. Repeat steps 1-3 to create two more groups named **EC2-Support** and **S3-Support**.

---

### Step 4: Create IAM Policies for Groups

We will now create custom policies for each group and attach them to the relevant groups.

#### 1. Create a Policy for EC2-Admin (Full EC2 Access)

1. In the left sidebar of the IAM console, select **Policies**, then click **Create policy**.

2. Select the **JSON** tab and enter the following policy, which grants full access to EC2:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "ec2:*",
         "Resource": "*"
       }
     ]
   }
   ```

3. Click **Next**.

4. In the **Name** field, enter **EC2FullAccessPolicy**, then click **Create policy**.

#### 2. Create a Policy for EC2-Support (Read-Only EC2 Access)

1. Repeat the policy creation process, but this time use the following policy for read-only EC2 access:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "ec2:Describe*",
           "ec2:Get*",
           "ec2:List*"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

2. Name this policy **EC2ReadOnlyPolicy**, then click **Create policy**.

#### 3. Create a Policy for S3-Support (Read-Only S3 Access)

1. Repeat the policy creation process for S3 read-only access with the following JSON:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:Get*",
           "s3:List*"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

2. Name this policy **S3ReadOnlyPolicy**, then click **Create policy**.

---

### Step 5: Attach Policies to User Groups

1. Navigate to **User groups** in the IAM console.
2. Select the **EC2-Admin** group, then click the **Permissions** tab.
3. Click **Add permissions** and select **Attach policies**.
4. In the list of policies, search for and select **EC2FullAccessPolicy**.
5. Click **Attach policy**.

Repeat this process for:

- **EC2-Support**, attaching the **EC2ReadOnlyPolicy**.
- **S3-Support**, attaching the **S3ReadOnlyPolicy**.

---

### Step 6: Assign Users to Groups

1. Navigate to **User groups** in the IAM console.
2. Select the **S3-Support** group, go to the **Users** tab, and click **Add users**.
3. Select **user-1**, then click **Add users**.
4. Repeat the process to add:
   - **user-2** to the **EC2-Support** group.
   - **user-3** to the **EC2-Admin** group.

---

### Step 7: Testing User Permissions

#### 1. Sign In as user-1 (S3-Support)

1. In the IAM sidebar, click **Dashboard**, then copy the **IAM sign-in URL**.
2. Open a new incognito browser window, paste the IAM sign-in URL, and log in as **user-1** using the password you set.
3. After logging in, navigate to **S3** and attempt to create a new bucket. You should receive an **Access Denied** error, as this user only has read-only access.
4. Verify that **user-1** cannot access **EC2** by attempting to navigate to **EC2**.

#### 2. Sign In as user-2 (EC2-Support)

1. Repeat the sign-in process for **user-2**.
2. Navigate to **EC2**, view running instances, and attempt to stop one. You should receive an error indicating that **user-2** only has read-only permissions.

#### 3. Sign In as user-3 (EC2-Admin)

1. Repeat the sign-in process for **user-3**.
2. Navigate to **EC2** and stop a running instance to verify that **user-3** has full access.

---

### Conclusion

In this lab, you created and configured IAM users, groups, and policies from scratch, providing different levels of access to AWS resources. You demonstrated user and group management, tested various permissions, and ensured that only the right users had access to specific AWS services.