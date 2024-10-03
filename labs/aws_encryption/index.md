---

# **AWS Data Protection and Encryption Lab**

### **Introduction**
This lab walks you through the steps to implement encryption in AWS using AWS Key Management Service (KMS), Amazon S3, and Amazon Elastic Block Store (EBS). You will learn how to protect data both at rest and in transit by creating and managing encryption keys, encrypting storage resources, and ensuring data security during transmission.

---

## **Lab Objectives**
By the end of this lab, you will:
1. Create and manage encryption keys using AWS KMS.
2. Encrypt S3 buckets using KMS-managed keys.
3. Encrypt Amazon EBS volumes.
4. Ensure encryption for data in transit using SSL/TLS.

---

## **Step 1: Log in to the AWS Management Console**
1. Log in to the AWS Management Console using your credentials.
2. In the top-right corner of the page, ensure that you are in the **`us-west-1` (N. California)** region.

---

## **Step 2: Create a Customer Managed Key (CMK) in AWS KMS**

### Create a KMS Key:
1. In the search bar at the top, type **KMS** and select **Key Management Service** from the results.
2. In the left sidebar, click **Customer managed keys**, then click **Create key**.
3. For **Key type**, select **Symmetric** (the default). Symmetric keys are typically used for data encryption because they use the same key for encryption and decryption.
4. For **Key usage**, leave it as **Encrypt and decrypt** and click **Next**.
5. Provide a name for your key, such as `my-data-encryption-key`, and optionally provide a description. Click **Next**.
6. On the **Define key administrative permissions** page, select the IAM roles or users who should be able to manage this key. For now, select your admin user or role and click **Next**.
7. On the **Define key usage permissions** page, select the IAM roles or users who should be able to use this key to encrypt and decrypt data. Select your admin user or role and click **Next**.
8. Review the configuration, then click **Finish** to create the key.

---

## **Step 3: Encrypt an S3 Bucket using KMS**

### Create an S3 Bucket:
1. In the AWS Management Console, type **S3** in the search bar and select **S3** from the results.
2. Click **Create bucket**.
3. Enter a unique name for your bucket, such as `my-encrypted-bucket-[your-initials]`.
4. In the **Region** dropdown, ensure that **us-west-1 (N. California)** is selected.
5. Scroll down to the **Bucket settings for Block Public Access** section and uncheck it to enable public access.
   * Check the box acknowledging the bucket objects will be public
6. Scroll to the **Default encryption** section.
7. Select **Enable** and choose **AWS Key Management Service (SSE-KMS)**.
8. Under **AWS KMS key**, select **my-data-encryption-key** (the key you created in Step 2).
9. Click **Create bucket**.

### Upload and Encrypt an Object:
1. Select your newly created bucket from the list.
2. Click **Upload**, then click **Add files** and choose a test file from your computer to upload.
3. After selecting the file, click **Upload** to complete the process.
4. AWS will automatically encrypt the object using your KMS key. You can verify this by selecting the uploaded object, then checking the **Encryption** section in the **Properties** tab, where you should see **AWS-KMS** listed as the encryption method.

### Confirm the Object is Encrypted:

1. After uploading the object, click **Close** in the top right corner to return the S3 bucket page. 
2. Select your uploaded object 
3. In the top right corner click **Open** to view the file in a new browser tab. 
   * Notice the URL contains the decryption key, allowing you to access the object. 
4. Now, copy the "Object URL" from the S3 bucket page. 
5. Try to load it in a browser and notice how access is denied without the decryption key.

---

## **Step 4: Encrypt an Amazon EBS Volume**

### Create an EC2 Instance:
1. In the AWS Management Console, type **EC2** in the search bar and select **EC2** from the results.
2. Click **Launch Instance**.
3. Provide a name for your instance, such as `my-encrypted-instance`.
4. Under **Amazon Machine Image (AMI)**, select **Amazon Linux 2023** (or any preferred AMI).
5. Under **Instance type**, choose **t2.micro** (free tier eligible).
6. Scroll down to **Key pair (login)**, select an existing key pair or create a new one to allow SSH access to your instance.
7. Scroll down to **Network settings** and configure a security group to allow **SSH** access on port **22**.
8. Leave the **Storage** settings as default for now and click **Launch instance**.

### Create an Encrypted EBS Volume:
1. In the EC2 dashboard, click **Volumes** in the left sidebar.
2. Click **Create Volume**.
3. For **Volume type**, select **General Purpose SSD (gp2)**.
4. Specify the size of the volume, such as **8 GiB**.
5. In the **Availability Zone**, select the same availability zone as your EC2 instance (you can find the instance’s availability zone in the **Instances** section).
6. Check the **Encrypt this volume** box.
7. Under **Master key**, select **my-data-encryption-key** (the KMS key you created in Step 2).
8. Click **Create volume**.

### Attach the Encrypted Volume to the EC2 Instance:
1. In the EC2 dashboard, select **Volumes** in the left sidebar, and select the volume you just created.
2. Click **Actions** and select **Attach volume**.
3. In the **Instance** dropdown, select your EC2 instance.
4. Click **Attach volume**.

### Format and Mount the Encrypted Volume:
1. SSH into your EC2 instance using the key pair you selected during instance creation.
2. Once logged in, run the following commands to format and mount the volume:
    ```bash
    sudo mkfs -t ext4 /dev/xvdf  # Replace xvdf with your actual device name
    sudo mkdir /mnt/mydata
    sudo mount /dev/xvdf /mnt/mydata
    ```
3. Your volume is now encrypted and mounted. Any data written to it is automatically encrypted using the KMS key.

---

## **Step 5: Ensure Encryption for Data in Transit using SSL/TLS**

### Enable HTTPS for S3 Bucket Access:
1. In the S3 Management Console, select your bucket (`my-encrypted-bucket-[your-initials]`).

2. Click **Permissions**, then scroll down to **Bucket policy**.

3. Add the following bucket policy to enable public access to the files.

    * Replace the `[Bucket-Name]` placeholder with your actual bucket name.

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": [
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::[Bucket-Name]/*"
                ]
            }
        ]
    }
    ```

4. Upload a file and use `wget` to download it over **HTTP**

5. Update the bucket policy to require HTTPS access:
    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::testhttps-jrs/*"
        },
        {
          "Sid": "ForceSSLOnlyAccess",
          "Effect": "Deny",
          "Principal": "*",
          "Action": "s3:*",
          "Resource": [
            "arn:aws:s3:::my-encrypted-bucket-[your-initials]/*",
            "arn:aws:s3:::my-encrypted-bucket-[your-initials]"
          ],
          "Condition": {
            "Bool": {
              "aws:SecureTransport": "false"
            }
          }
        }
      ]
    }
    ```

6. Replace `my-encrypted-bucket-[your-initials]` with your actual bucket name, then click **Save changes**.

7. Now, if anyone tries to access your S3 bucket via an insecure HTTP connection, they will be denied.

### Verify HTTPS Access:
1. Use `get` to access the objects in your S3 bucket using an HTTP link (you should receive an error).
2. Access the objects via HTTPS (you should be able to download them successfully).

---

### Monitor Key Usage with AWS CloudTrail:
1. In the AWS Management Console, type **CloudTrail** in the search bar and select **CloudTrail**.
2. Click **Event history** in the left sidebar.
3. Use the filters to search for **KMS** operations (such as `Decrypt` or `Encrypt`) to see how your encryption keys have been used.

---

## **Conclusion**
In this lab, you successfully:
- Created a KMS key and used it to encrypt data at rest in S3 and EBS.
- Secured data in transit using SSL/TLS with S3.
- Applied encryption best practices, including monitoring key usage with CloudTrail.

By completing this lab, you now understand how to implement data protection and encryption in AWS using KMS, S3, and EBS.