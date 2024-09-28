# Lambda - Launch EC2 VM

In this AWS hands-on lab, we will write a Lambda function that will create an EC2 instance. This Lambda function will be written in Python using the Boto3 library. We will also create a custom Lambda execution policy for our IAM role.



## Log into the AWS Console

Log into the AWS Console in a browser using the information from the spreadsheet.

Once you have logged in, select **Snowflake{account_number}** ->  **AWSAdministratorAccess**

In the console, on the top right side, select the `us-west-1` region.

## Create EC2 Key Pair

1. Navigate to EC2.

2. In the navigation pane, under **Network & Security**, choose **Key Pairs**.

3. Choose **Create Key Pair**.

4. Enter a name for the new key pair (e.g., "LambdaEC2keypair") in the **Key pair name** field of the **Create Key Pair** dialog box, and then choose **Create key pair**.

5. **Note**: Make sure you remember the name of your key.

   

## Create a Lambda Function

1. Navigate to Lambda.

2. Click **Create a function**.

3. Choose **Author from scratch** and use the following settings:

   - *Name*: **CreateEC2**
   - *Runtime*: **Latest version of Python**

4. Expand *Change default execution role*.

5. Set *Execution role* to **Create a new role with basic Lambda permissions**.

6. Copy the execution role name that appears (it will be something like *CreateEC2-role-*). Paste it into a text file for use later in the lab.

7. Click **Create function**.

8. In a new browser tab, navigate to IAM.

9. Click **Roles** in the left-hand menu.

10. In the search box, type in the name of the role we just created.

11. Select the role.

12. Click the policy that's currently attached to the role.

13. Click **{} JSON**.

14. Click **Edit policy** > **JSON**.

15. Paste in the following policy:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [{
          "Effect": "Allow",
          "Action": [
            "logs:CreateLogGroup",
            "logs:CreateLogStream",
            "logs:PutLogEvents"
          ],
          "Resource": "arn:aws:logs:*:*:*"
        },
        {
          "Action": [
            "ec2:RunInstances",
            "ec2:Start*",
            "ec2:Stop*",
            "ec2:Describe*"
          ],
          "Effect": "Allow",
          "Resource": "*"
        }
      ]
    }
    ```

16. Click **Next** and then **Save changes**.

17. Back in the Lambda console, refresh the page.

18. On the CreateEC2 function page, scroll down to the *lambda_function* section.

19. Paste in the following Python code

    ```python
    import os
    import boto3
    
    AMI = os.environ['AMI']
    INSTANCE_TYPE = os.environ['INSTANCE_TYPE']
    KEY_NAME = os.environ['KEY_NAME']
    SUBNET_ID = os.environ['SUBNET_ID']
    
    ec2 = boto3.resource('ec2')
    
    
    def lambda_handler(event, context):
    
        instance = ec2.create_instances(
            ImageId=AMI,
            InstanceType=INSTANCE_TYPE,
            KeyName=KEY_NAME,
            SubnetId=SUBNET_ID,
            MaxCount=1,
            MinCount=1
        )
    
        print("New instance created:", instance[0].id)
    ```

20. Select **Configuration** -> **Environment variables**.

21. Click **Edit** -> **Add environment variable**

22. Set the following environment variables:

    - `AMI`
      - *Key*: **AMI**
      - *Value*: Open **EC2** in a new browser tab, click **Launch Instance**, copy the `ami` value under **Software Image (AMI)** on the right side, and paste the `ami` value.
    - `INSTANCE_TYPE`
      - *Key*: **INSTANCE_TYPE**
      - *Value*: **t3.micro**
    - `KEY_NAME`
      - *Key*: **KEY_NAME**
      - *Value*: The name of the EC2 key pair you created earlier.
    - `SUBNET_ID`
      - *Key*: **SUBNET_ID**
      - *Value*: Navigate to **VPC** > **Subnets**, and copy and paste the ID of one of the public subnets in your VPC.

23. Click **Save**

24. Click **Code**

25. Click **Deploy** to Deploy the Lambda function.



### Test Lambda Function

1. Click the blue **Test** button.

2. Set **Event name** to `ec2_instance`

3. Define an empty test event. Its contents must simply be `{}`.

4. Click **Save**.

5. Click **Test**.

6. Navigate to **EC2** > **Instances**, and observe that an EC2 instance is initializing.

   

## Cleanup

* Delete the EC2 instance
* Delete the Lambda function



## Congrats!
