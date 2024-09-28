# Lambda - Stop EC2 Instances

In this tutorial, we will create a Python script to stop EC2 instances, then apply that script to a Lambda function. 



## Create Dev Instances

To test our Python script, we want to create a few instances and tag them as `Environment: Dev`. To do this, follow these steps.

1. Navigate to EC2.

2. Click **Launch instances**

3. Skip naming the instance

4. Click **Add additional tags**

5. Set the following

   1. `Key: Environment`

   2. `Value: Dev`

6. Select the key pair name you created in the last lab.

7. Set **Number of instances** to **3**

8. Leave all other settings default

9. Click **Launch instance**

   


## Create a custom AWS IAM Lambda policy

1. Navigate to **IAM**.

2. Click **Policies** on the left side.

3. Click the **Create policy** button

4. Select the **JSON** tab and paste the following policy

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents"
               ],
               "Resource": "arn:aws:logs:*:*:*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:Start*",
                   "ec2:Stop*",
                   "ec2:Describe*",
                   "ec2:RunInstances"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

   

5. Click **Next**

7. Name the policy **Lambda_Policy_EC2_Instances**

7. Click **Create policy**.

## Create a Custom AWS IAM Execution Role for Lambda

1. Within the IAM console, navigate to the left side menu and click **Roles** then **Create role**.
2. Under **Trusted entity type**, select **AWS service** and under **Use case** select **Lambda** then **Next**.
3. On the **Add permissions** screen, we want to select the IAM policy that we just created, then click **Next**.
4. Name the role **Lambda_Role_EC2_Instances**
5. Click **Create role**



## Create a Lambda Function to Stop EC2 Instances with a Specific Tag

1. Navigate to Lambda.

2. Click **Create a function**.

3. Choose **Author from scratch** and use the following settings:

   - *Name*: **StopEC2**
   - *Runtime*: **Latest version of Python**

4. Expand *Change default execution role*.

5. Select **Use an existing role**.

6. Under **Existing role**, select the IAM role you created earlier **Lambda_Role_EC2_Instances**

7. Click **Create function**.

8. On the **StopEC2** function page, scroll down to the *lambda_function* section.

9. Paste in the following Python code

   ```python
   import json
   import boto3
   
   ec2 = boto3.resource('ec2')
   def lambda_handler(event, context):
      instances = ec2.instances.filter(Filters=[{'Name': 'instance-state-name', 'Values': ['running']},{'Name': 'tag:Environment','Values':['Dev']}])
      for instance in instances:
          id=instance.id
          ec2.instances.filter(InstanceIds=[id]).stop()
          print("Instance ID is stopped:- "+instance.id)
      return "success"
   ```

10. Click **Deploy**

11. Select **Configuration** -> **General configuration**

12. Change the **Timeout** to 10 seconds, then click **Save**

13. Deploy the Lambda function.

    

### Test Lambda Function

We are now ready to test our function. When we run it, the function should stop our EC2 instances that have an`Environment: Dev` tag. First, confirm your EC2 instances are running in the EC2 console. 

1. Click the blue **Test** button.

2. Set **Event name** to `stop_ec2_instance`

3. Define an empty test event. Its contents must simply be `{}`.

4. Click **Save**.

5. Click **Test**.

6. Navigate to **EC2** > **Instances**, and observe that your instances with the `Environment: Dev` tag are stopping.



## Cleanup

* Delete the EC2 instances
* Delete the Lambda function
