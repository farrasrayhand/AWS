# Activity: AWS Lambda

<!-- Note to translators: This lab is unique to this course. -->

&nbsp;
&nbsp;
## Overview

<img src="images/lambda-activity.png" alt="architectural diagram" width="700">

&nbsp;
&nbsp;
&nbsp;
In this hands-on activity, you will create an AWS Lambda function. You will also create an Amazon CloudWatch Events event to trigger the function every minute. The function uses an AWS Identity and Access Management (IAM) role. This IAM role allows the function to stop an Amazon Elastic Compute Cloud (Amazon EC2) instance that is running in the Amazon Web Services (AWS) account.

This lab is based on the AWS Command Line Interface (AWS CLI). Feel free to explore the [AWS CLI documentation](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html) for Lambda, CloudWatch Events, IAM, and Amazon EC2 as you work through the lab.

&nbsp;
&nbsp;

### Duration

This activity takes approximately **30 minutes** to complete.

&nbsp;

## Launching your lab environment

1. At the top of these instructions, launch this lab by choosing <span id="ssb_voc_grey">Start Lab</span> 

   A **Start Lab** panel opens, and it displays the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by selecting the **X**.

   This lab launches an EC2 instance that's named *Bastion Host*. You will use this server to create lab resources by running the AWS CLI commands.

3. Above these instructions, select the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>
  
    Copy all the lab details—such as *BastionHost*, *UserAccessSecretKey*, *Region*, and others—to a text file and save it as `Lab Details.txt`. 

    

**Note**: Make sure that you use a *text editor* (a program for editing text files) to save the file. Examples of text editors include [Atom](https://atom.io/), [Sublime Text](https://www.sublimetext.com/), or [Microsoft Visual Studio Code](https://code.visualstudio.com/).
    
In the lab, the information that you saved will be referred to as <u>*Lab Details*</u>.



## Using SSH to connect to the Bastion Host

### Microsoft Windows users

<i class="fas fa-comment"></i> These instructions are specifically for Microsoft Windows users. If you are using macOS or Linux, <a href="#ssh-MACLinux">skip to the next section</a>.
​
4. Above these instructions that you are currently reading, choose the <span id="ssb_voc_grey">Details</span> dropdown menu and then choose <span id="ssb_voc_grey">Show</span>
  
   A **Credentials** window opens.
   
5. Choose the **Download PPK** button and save the **labsuser.ppk** file.

    Typically, your browser will save the file to the **Downloads** directory.

6. Close the **Credentials** window by choosing the **X**.

7. From the Lab Details text file, note the **Bastion Host** address. 

8. Download **PuTTY** so that you can use SSH to connect to the EC2 instance. If you don't have PuTTY installed on your computer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download it here</a>.

9.  Open **putty.exe**

10. Configure PuTTY timeout to keep the PuTTY session open for a longer period of time.
    * Choose **Connection**
    * Set **Seconds between keepalives** to `30`

11. Configure your PuTTY session:
    * Choose **Session**
    * **Host Name (or IP address):** Paste the *Public DNS or IPv4 address* of the Bastion Host instance that you noted earlier. 
    * Back in PuTTY, in the **Connection** list, expand <i class="far fa-plus-square"></i> **SSH**
    * Choose **Auth** *(don't expand it)*
    * Choose **Browse**
    * Browse to and select the **labsuser.ppk** file that you downloaded
    * Choose **Open** to select it
    * Choose **Open** again

12. To trust and connect to the host, choose **Yes**.

13. When prompted to **login as**, enter: `ec2-user`
  
    This action will connect you to the EC2 instance.

14. Windows users: <a href="#ssh-after"> Skip ahead to the next task.</a>

<a id='ssh-MACLinux'></a>


### macOS and Linux users

These instructions are specifically for macOS or Linux users. If you are a Windows user, <a href="#ssh-after">skip ahead to the next task.</a>

15. Above these instructions (that you are currently reading), choose the <span id="ssb_voc_grey">Details</span> dropdown menu and then choose <span id="ssb_voc_grey">Show</span> 

A **Credentials** window opens.

16. Choose the **Download PEM** button and save the **labsuser.pem** file.

17. Exit the **Credentials** window by choosing the **X**.

18. From the Lab Details text file, note the **Bastion Host** address. 

19. Open a terminal window, and change directory (by using `cd`) to the directory where the *labsuser.pem* file was downloaded.
  
    For example, if the *labsuser.pem* file was saved to your **Downloads** directory, run this command:

    ```bash
    cd ~/Downloads
    ```

20. Change the permissions on the key to be read-only by running this command:

    ```bash
    chmod 400 labsuser.pem
    ```

21. Run the following command (replace &lt;*bastion-host-ip-address*&gt; with the *BastionHost* address that you copied earlier). 

    ```bash
    ssh -i labsuser.pem ec2-user@<bastion-host-ip-address>
    ```

22. When you are prompted to allow the first connection to this remote SSH server, enter: `yes` 
  

Because you are using a key pair for authentication, you will not be prompted for a password.
​
<a id='ssh-after'></a>

&nbsp;


### Configuring the AWS CLI

You will use the information from the Lab Details text file that you created in the *Launch Your Lab Environment* task.

The *Bastion Host* instance has an instance profile that's called *EC2InstanceProfile* associated with it. This instance profile gives it the required permissions. 


23. In the terminal window, configure the AWS CLI.

   ```bash
   aws configure
   ```


24. At the prompts, enter the following information. For the access key, secret access key, and Region settings, use the values from the Lab Details text file.

  **Note**: Ensure that you copy and paste the correct user values from the Lab Details text file to these fields. 

  -  **AWS Access Key ID**: *AccessKey* value
  -  **AWS Secret Access Key**: *SecretKey* value
  -  **Default region name**:  Name of the Region (for example, `us-east-1` or `eu-west-2`)
  -  **Default output format**: `json`
&nbsp;
&nbsp;

## Task 1: Creating a Lambda function

25. To retrieve the code that you will use for this lab, run the following command in your terminal.

    ```bash
    wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/activity1-cli-lambda/activity1-cli-lambda.zip
    ```
    
26. Extract the activity1-cli-lambda.zip file.
  
    ```bash
    unzip activity1-cli-lambda.zip
    ```

27. Rename the file lambda_function1 to lambda_function.py.

    ```bash
    mv lambda_function1 lambda_function.py
    ```

28. Review the contents of lambda_function.

    ```bash
    more lambda_function.py
    ```
    > def lambda_handler(event, context):
    >
    >   \# TODO implement
    >
    >   return {
    >
    > ​    'statusCode': 200,
    >
    > ​    'body': json.dumps('Hello from Lambda!')
    >
    >   }

    This is a basic Python script that will output a *Hello from Lambda!* message when it's invoked.


29. Next, compress the file into .zip format to create a *deployment package*.

     ```bash
     zip lambda_function1.zip lambda_function.py
     ```

30. A role named *myStopinatorRole* was created when you started the lab environment. Look up the **Amazon Resource Name (ARN)** for this role.

     ```bash
     aws iam get-role --role-name myStopinatorRole
     ```
    
    Example output:

     > {
     >
     >   "Role": {
     >
     > ​    "Path": "/",
     >
     > ​    "RoleName": "myStopinatorRole",
     >
     > ​    "RoleId": "AROARLX5QE7HFFVPLQQJV",
     >
     > ​    "Arn": "arn:aws:iam::093947xxxxxx:role/myStopinatorRole",
     >
     > ​    "CreateDate": "2020-06-08T13:14:16+00:00",
     >
     > ​    "AssumeRolePolicyDocument": {
     >
     > ​      "Version": "2012-10-17",
     >
     > ​      "Statement": [
     >
     > ​        {
     >
     > ​          "Effect": "Allow",
     >
     > ​          "Principal": {
     >
     > ​            "Service": "lambda.amazonaws.com"
     >
     > ​          },
     >
     > ​          "Action": "sts:AssumeRole"
     >
     > ​        }
     >
     > ​      ]
     >
     > ​    },
     >
     > ​    "Description": "",
     >
     > ​    "MaxSessionDuration": 3600,
     >
     > ​    "Tags": [
     >
     > ​      {
     >
     > ​        "Key": "cloudlab",
     >
     > ​        "Value": "c14328a198258u616305t1w093947176910"
     >
     > ​      }
     >
     > ​    ],
     >
     > ​    "RoleLastUsed": {}
     >
     >     }
     >
     > }


31. Note the **Arn** value and save it in your Lab Details text file. It should be similar to *arn:aws:iam::093947xxxxxx:role/myStopinatorRole*.

32. Create a new Lambda function by running the following code. (Be sure to replace the placeholder `<Arn>` with the **Arn** value for *myStopinatorRole*.)

    - **Function name**: `myStopinator`
    - **Runtime**: `python3.8`
    - **Role**: **Arn** value for *myStopinatorRole*
    - **Handler**: `lambda_function.lambda_handler`
    - **Zip**: `fileb://lambda_function1.zip`

    ```bash
    aws lambda create-function --function-name myStopinator --runtime python3.8 \
    --role <iam-role-Arn> --handler lambda_function.lambda_handler \
    --zip fileb://lambda_function1.zip
    ```

    Example output:

    > {
    >
    >   "FunctionName": "myStopinator",
    >
    >   "FunctionArn": "arn:aws:lambda:us-west-2:09394xxxxxxx:function:myStopinator",
    >
    >   "Runtime": "python3.8",
    >
    >   "Role": "arn:aws:iam::093947176910:role/myStopinatorRole",
    >
    >   "Handler": "lambda_function.lambda_handler",
    >
    >   "CodeSize": 312,
    >
    >   "Description": "",
    >
    >   "Timeout": 3,
    >
    >   "MemorySize": 128,
    >
    >   "LastModified": "2020-06-08T14:17:11.018+0000",
    >
    >   "CodeSha256": "2w4qs6xIyyXlY7UpLtc2a5zgwzQUoY1ka3WpRgx8sGA=",
    >
    >   "Version": "$LATEST",
    >
    >   "TracingConfig": {
    >
    > ​    "Mode": "PassThrough"
    >
    >   },
    >
    >   "RevisionId": "dbe2b39f-ca02-4893-a239-536da2e613d4",
    >
    >   "State": "Active",
    >
    >   "LastUpdateStatus": "Successful"
    >
    > }

33. Note the **FunctionArn** value for the Lambda function and save it in your Lab Details text file. It should be similar to *arn:aws:lambda:us-west-2:09394xxxxxxx:function:myStopinator*.

&nbsp;
&nbsp;

## Task 2: Configuring the trigger

In this task, you will configure a scheduled event to trigger the Lambda function by setting a CloudWatch Events event as the event source (or trigger). The Lambda function can be configured to operate in a similar way to a cron job on a Linux server, or a scheduled task on a Microsoft Windows server. However, you do not need to have a server running to host it.

34. Create a *CloudWatch Events rule*:
     - **Rule name**: `everyMinute`
     - **Schedule expression**: `rate(1 minute)`

    ```bash
    aws events put-rule \
    --name everyMinute \
    --schedule-expression 'rate(1 minute)'
    ```
    Example output:

    > {
    >
    >     "RuleArn": "arn:aws:events:us-west-2:09394xxxxxxx:rule/everyMinute"
    >
    >   }

    **Note**: A more realistic, schedule-based stopinator Lambda function would probably be triggered by using a cron expression instead of a rate expression. However, for the purposes of this activity, using a rate expression ensures that the Lambda function will be triggered soon enough that you can see the results.

35. Save the **RuleArn** value. It will be similar to *arn: arn:aws:events:us-west-2:09394xxxxxxx:rule/everyMinute*.

36. Grant CloudWatch Events the permissions to call the Lambda function:

    - **Function name**: `myStopinator`
    - **Statement ID**: `myStopinatorEvent`
    - **Action**: `'lambda:InvokeFunction'`
    - **Principal**: `events.amazonaws.com`
    - **Source ARN**: **RuleArn** value for the *everyMinute* rule

    ```bash
    aws lambda add-permission \
    --function-name myStopinator \
    --statement-id myStopinatorEvent \
    --action 'lambda:InvokeFunction' \
    --principal events.amazonaws.com \
    --source-arn <every-minute-RuleArn>
    ```

    Example output:

      > {
      >
      >    "Statement": "{\"Sid\":\"myStopinatorEvent\",\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"events.amazonaws.com\"},\"Action\":\"lambda:InvokeFunction\",\"Resource\":\"arn:aws:lambda:us-west-2:093947176910:function:myStopinator\",\"Condition\":{\"ArnLike\":{\"AWS:SourceArn\":\"arn:aws:events:us-west-2:093947176910:rule/everyMinute\"}}}"
      >
      > }

    Next, you will associate the *myStopinator* function with the rule.

37. Edit the **targets.json** file by using a Linux-based text editor, such as GNU Nano or vi. Replace the `<replace-with-lambda-arn>` placeholder with the **FunctionArn** value for the *myStopinator* Lambda function.

    ```bash
    [
      {
          "Id": "1", 
          "Arn": "<replace-with-lambda-arn>"
      }
    ]
    ```

38. Save the changes to the file.

39. Associate the Lambda function with the CloudWatch Events rule by running the following AWS CLI command:

    ```bash
    aws events put-targets --rule everyMinute --targets file://targets.json
    ```
    
    Example output:
    
    > {
    >
    >   "FailedEntryCount": 0,
    >
    >   "FailedEntries": []
    >
    > }


&nbsp;
&nbsp;

## Task 3: Updating the Lambda function

In this task, you will update two values in Python code. You do not need to write code to complete this task.

40. Rename the lambda_function2 file to *lambda_function.py*.

    ```bash
    mv lambda_function2 lambda_function.py
    ```

41. **Challenge section**: Verify that an EC2 instance named *instance1* is running in your account, and copy the **InstanceID** value for *instance1*.

<details>
    <summary>You are encouraged to figure out how to do this task without specific step-by-step guidance. However, <b>if you need detailed guidance, click here</b>.</summary>
      <ul>
        <li>In the terminal session that is connected to the <i>Bastion Host</i>, run the following command:</li>
        <code>aws ec2 describe-instances --filters "Name=tag:Name,Values=instance1"</code>
        <li>Locate the section of the output that is called <b>State</b>, and review the <b>Name</b> value.</li>
      <pre><code>{
        "Reservations": [
            {
                "Groups": [],
                "Instances": [
                    {
                        "AmiLaunchIndex": 0,
                        "ImageId": "ami-0e34e7b9ca0ace12d",
                        "InstanceId": "i-0ed8c7cca649xxxxx",
                        "InstanceType": "t2.micro",
                        "KeyName": "vockey",
                        "LaunchTime": "2020-06-08T13:14:17+00:00",
                        "Monitoring": {
                            "State": "disabled"
                        },
                        "Placement": {
                            "AvailabilityZone": "us-west-2b",
                            "GroupName": "",
                            "Tenancy": "default"
                        },
                        "PrivateDnsName": "ip-172-31-21-39.us-west-2.compute.internal",
                        "PrivateIpAddress": "172.31.21.39",
                        "ProductCodes": [],
                        "PublicDnsName": "ec2-34-219-171-72.us-west-2.compute.amazonaws.com",
                        "PublicIpAddress": "34.219.171.72",
                        "State": {
                            "Code": 16,
                            "Name": "running"
                        },
                        "StateTransitionReason": "",
                        "SubnetId": "subnet-04bfc3340593xxxx9"
        </code></pre>
        Note that it is in a <i>running</i> state.    
      <li> From the output, copy the <b>InstanceId</b> value (it will start with *i-*). You might find it helpful to save the <b>InstanceId</b> value in the Lab Details text file.</li>
</details>



42. Open **lambda_function.py** by using a Linux-based text editor, such as Nano or vi. 

    ```
    import boto3
    region = '<replace-with-RegionName>'
    instances = ['<replace-with-InstanceId>']
    ec2 = boto3.client('ec2', region_name=region)
    
    def lambda_handler(event, context):
        ec2.stop_instances(InstanceIds=instances)
        print('stopped your instances: ' + str(instances))
    ```

42. Replace the `<replace-with-RegionName>` placeholder with the name of the actual Region that you are using. Use the same Region name that you used when you configured the AWS CLI.

    **Important**: Keep the single quotation marks (' ') around the Region in your code. For example, for the N. Virginia Region, it would be `'us-east-1'`.

44. Replace `<replace-with-InstanceId>` with the actual **InstanceID** value that you just saved.

    **Important**: Keep the single quotation marks (' ') around the **InstanceID** value in your code.

    Your code should now look similar to the following example. However, you might have a different value for the Region, and you will have a different value for the instance ID:

    Example output:

    >import boto3
    >
    >region = 'us-east-1'
    >
    >instances = ['i-0ed8c7cca649xxxxx']
    >
    >ec2 = boto3.client('ec2', region_name=region)
    >
    >
    >
    >def lambda_handler(event, context):
    >
    >​		ec2.stop_instances(InstanceIds=instances)
    >
    >​		print('stopped your instances: ' + str(instances))

45. Save the changes that you made to **lambda_function.py**.

46. As you did earlier, create a .zip file that contains the lambda_function.py file, which will be used as a deployment package. This time, name the .zip file *lambda_function2.zip*:

    ```bash
    zip lambda_function2.zip lambda_function.py
    ```

47. Replace the current function code with the new Python code:

    ```bash
    aws lambda update-function-code --function-name myStopinator \
    --zip fileb://lambda_function2.zip
    ```

    Example output:

    > {
    >
    >   "FunctionName": "myStopinator",
    >
    >   "FunctionArn": "arn:aws:lambda:us-west-2:093947176910:function:myStopinator",
    >
    >   "Runtime": "python3.8",
    >
    >   "Role": "arn:aws:iam::093947176910:role/myStopinatorRole",
    >
    >   "Handler": "lambda_function.lambda_handler",
    >
    >   "CodeSize": 362,
    >
    >   "Description": "",
    >
    >   "Timeout": 3,
    >
    >   "MemorySize": 128,
    >
    >   "LastModified": "2020-06-08T15:51:24.979+0000",
    >
    >   "CodeSha256": "iWnxeGLe1ycufSYhC7pgRh0dXnfDklFGd9Cmf68MRzo=",
    >
    >   "Version": "$LATEST",
    >
    >   "TracingConfig": {
    >
    > ​    "Mode": "PassThrough"
    >
    >   },
    >
    >   "RevisionId": "7e24b766-7f07-4305-9ef2-24b87cfad351",
    >
    >   "State": "Active",
    >
    >   "LastUpdateStatus": "Successful"
    >
    > }

The **LastUpdateStatus** value should be *Successful*.

Your Lambda function is now fully configured. It should attempt to stop your instance every minute.



## Task 4: Monitoring the Lambda function

Amazon CloudWatch collects metric data to enable the monitoring of Lambda functions. This information can be queried to view details about the function, such as the number of *invocations* and the *error count*. (The *Invocations* metric indicates the number of times that your function code runs, and the *Errors* metric indicates the number of invocations that result in a function error.)

48. First, you must determine your start and end times for the CloudWatch query. To view the current time on the EC2 instance, use the `date` command:

    ```bash
    date
    ```
    
    Example output:

    > Tue Jun 9 19:30:41 UTC 2020

    The query requires the timestamp to be in the format *YYYY-MM-DDTHH:MM:SSZ*. You can estimate the start time for your lab by subtracting 30 minutes from the current time. For the time parameters in this example, the values would be:
    
    - **Start time**: ` 2020-06-09T19:00:00Z`
    - **End time**: ` 2020-06-09T19:30:41Z`

49. To query the number of invocations:

    ```bash
    aws cloudwatch get-metric-data --cli-input-json file://invocations_query.json \
    --start-time <start-time> --end-time <end-time>
    ```

    Example output:

    > {
    >
    >   "MetricDataResults": [
    >
    > ​    {
    >
    > ​      "Id": "m1",
    >
    > ​      "Label": "Invocations",
    >
    > ​      "Timestamps": [
    >
    > ​        "2020-06-08T17:45:00+00:00"
    >
    > ​      ],
    >
    > ​      "Values": [
    >
    > ​        56.0
    >
    > ​      ],
    >
    > ​      "StatusCode": "Complete"
    >
    > ​    }
    >
    >   ],
    >
    >   "Messages": []
    >
    > }
    
    The number of invocations is represented by the numeric value in the **Values** array.

50. To query the number of function-invocation errors:

    ```bash
    aws cloudwatch get-metric-data --cli-input-json file://errors_query.json \
    --start-time <start-time> --end-time <end-time>
    ```

    Example output:

    > {
    >
    > "MetricDataResults": [
    >
    > ​    {
    >
    > ​      "Id": "m1",
    >
    > ​      "Label": "Errors",
    >
    > ​      "Timestamps": [
    >
    > ​        "2020-06-08T17:45:00+00:00"
    >
    > ​      ],
    >
    > ​      "Values": [
    >
    > ​        12.0
    >
    > ​      ],
    >
    > ​      "StatusCode": "Complete"
    >
    > ​    }
    >
    > ],
    >
    > "Messages": []
    >
    > }

    Like the *Invocations* metric, the error count is represented by the numeric value in the **Values** array.
    
    

## Task 5: Verifying that the Lambda function worked

51. Describe the *instance1* EC2 instance again:

    ```bash
    aws ec2 describe-instances --filters "Name=tag:Name,Values=instance1"
    ```

    Example output:

    > {
    >
    >  "Reservations": [
    >
    > ​     {
    >
    > ​         "Groups": [],
    >
    > ​         "Instances": [
    >
    > ​             {
    >
    > ​                 "AmiLaunchIndex": 0,
    >
    > ​                 "ImageId": "ami-0e34e7b9ca0ace12d",
    >
    > ​                 "InstanceId": "i-0ed8c7cca649xxxxx",
    >
    > ​                 "InstanceType": "t2.micro",
    >
    > ​                 "KeyName": "vockey",
    >
    > ​                 "LaunchTime": "2020-06-08T13:14:17+00:00",
    >
    > ​                 "Monitoring": {
    >
    > ​                     "State": "disabled"
    >
    > ​                 },
    >
    > ​                 "Placement": {
    >
    > ​                     "AvailabilityZone": "us-west-2b",
    >
    > ​                     "GroupName": "",
    >
    > ​                     "Tenancy": "default"
    >
    > ​                 },
    >
    > ​                 "PrivateDnsName": "ip-172-31-21-39.us-west-2.compute.internal",
    >
    > ​                 "PrivateIpAddress": "172.31.21.39",
    >
    > ​                 "ProductCodes": [],
    >
    > ​                 "PublicDnsName": "ec2-34-219-171-72.us-west-2.compute.amazonaws.com",
    >
    > ​                 "PublicIpAddress": "34.219.171.72",
    >
    > ​                 "State": {
    >
    > ​                     "Code": 80,
    >
    > ​                     "Name": "stopped"
    >
    > ​                 },
    >
    > ​                 "StateTransitionReason": "",
    >
    > ​                 "SubnetId": "subnet-04bfc3340593xxxx9"

    In the **State** section of the output, you should find that the instance has a value of *stopped*. 

52. Try starting the instance again. What do you think will happen?

    ```bash
    aws ec2 start-instances --instance-ids <instance1-InstanceId>
    ```

    Example output:

    > {
    >
    >   "StartingInstances": [
    >
    > ​    {
    >
    > ​      "CurrentState": {
    >
    > ​        "Code": 0,
    >
    > ​        "Name": "pending"
    >
    > ​      },
    >
    > ​      "InstanceId": "i-0ed8c7cca649xxxxx",
    >
    > ​      "PreviousState": {
    >
    > ​        "Code": 80,
    >
    > ​        "Name": "stopped"
    >
    > ​      }
    >
    > ​    }
    >
    >   ]
    >
    > }

    <details>
          <summary>Click <b>here</b> to reveal the answer.</summary>
          The instance will be stopped again within 1 minute.
    </details>

&nbsp;
&nbsp;
## Activity complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the activity.

53. To confirm that you want to end the lab, at the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then choose <span id="ssb_blue">Yes</span>

   A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*
   ​
54. To close the panel, choose the **X** in the top-right corner.


*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*
