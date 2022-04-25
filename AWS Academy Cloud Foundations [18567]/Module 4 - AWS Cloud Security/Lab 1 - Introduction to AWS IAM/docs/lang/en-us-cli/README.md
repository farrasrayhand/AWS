
# Lab 1: Introducing IAM

<!-- Note to translators: This is based on SPL-66 Version 3.1.2. Copy the translation from there. This version is however based on CLI and therefore other languages will have to be retranslated. -->

*AWS Identity and Access Management (IAM)* is a web service that enables Amazon Web Services (AWS) customers to manage users and user permissions in AWS. With IAM, you can centrally manage *users*, *security credentials* such as access keys, and *permissions* that control which AWS resources users can access. This lab is based on the AWS Command Line Interface (AWS CLI). Feel free to explore the [AWS CLI documentation for IAM](https://docs.aws.amazon.com/cli/latest/reference/iam/index.html) as you work through the lab.

## Topics covered

After you complete this lab, you should be able to:

<img src="./images/lab-scenario.jpeg" alt="LabOverview" width="600" />

 * Explore pre-created *IAM users and groups*
 * Inspect *IAM policies* that are applied to the pre-created groups
 * Follow a *real-world scenario* by adding users to groups that have specific enabled capabilities 
 * Test each user's *permissions*
 * *Experiment* with the effects of policies on service access




**Other AWS services**

During this lab, you might receive error messages when you perform actions beyond the steps in this lab guide. These messages will not impact your ability to complete the lab.


**AWS Identity and Access Management**

  AWS Identity and Access Management (IAM) can be used to:

  * *Manage IAM users and their access:* You can create users and assign individual security credentials to them (such as access keys, passwords, and multi-factor authentication devices). You can also manage permissions to control which operations a user can perform.

  * *Manage IAM roles and their permissions:* An IAM role is similar to a user. It's an AWS identity with permissions policies that determine what the identity can and can't do in AWS. However, instead of being uniquely associated with one person, a role is intended to be *assumable* by anyone who needs it.

  * *Manage federated users and their permissions:* You can enable *identity federation* to allow existing users in your enterprise to access the AWS Management Console or the AWS CLI. Federated users can also call AWS API operations and access resources, without the need to create an IAM user for each identity.

    


#### Duration

This lab takes approximately **40 minutes** to complete.



## Launching your lab environment

1. At the top of these instructions, launch this lab by choosing <span id="ssb_voc_grey">Start Lab</span> 

   A **Start Lab** panel opens, displaying the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by selecting the **X**.
  
    This lab launches an Amazon Elastic Compute Cloud (Amazon EC2) instance that's named *Bastion Host*. You will use this server to create lab resources by running the AWS CLI commands.

3. Above these instructions, select the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>
  
    Copy all the lab details—such as *BastionHost*, *Region*, and others—to a text file and save it as `Lab Details.txt`. 
    
    **Note**: Make sure that you use a *text editor* (a program for editing text files) to save the file. Examples of text editors include [Atom](https://atom.io/), [Sublime Text](https://www.sublimetext.com/), or [Microsoft Visual Studio Code](https://code.visualstudio.com/).
    
    In the lab, the information that you saved will be referred to as <u>*Lab Details*</u>.



## Using SSH to connect to the bastion host

### Windows users

<i class="fas fa-comment"></i> These instructions are specifically for Windows users. If you are using macOS or Linux, <a href="#ssh-MACLinux">skip to the next section</a>.
​

4. Choose the <span id="ssb_voc_grey">Details</span> dropdown menu above these instructions you are currently reading, and then click <span id="ssb_voc_grey">Show</span>. 
  
   A **Credentials** window will open.
   
5. Choose the **Download PPK** button and save the **labsuser.ppk** file.

    Typically, your browser will save the file to the **Downloads** directory.

6. Close the **Credentials** window by choosing the **X**.

7. From the Lab Details text file, note the **Bastion Host** address. 

8. Download  **PuTTY** so that you can use SSH to connect to the EC2 instance. If you don't have PuTTY installed on your computer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download it here</a>.

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

14. Windows Users: <a href="#ssh-after"> Skip ahead to the next task.</a>

<a id='ssh-MACLinux'></a>


### macOS and Linux users

These instructions are specifically for macOS and Linux users. If you are a Windows user, <a href="#ssh-after">skip ahead to the next task.</a>
​

15. Above these instructions (that you are currently reading), choose the <span id="ssb_voc_grey">Details</span> dropdown menu and then choose <span id="ssb_voc_grey">Show</span> 

A **Credentials** window will open.

16. Choose the **Download PEM** button and save the **labsuser.pem** file.

17. Make a note of the **Bastion Host** address. 

18. Exit the **Credentials** window by choosing the **X**.

19. Open a terminal window, and change directory (by using `cd`) to the directory where the *labsuser.pem* file was downloaded.
  
    For example, if the *labuser.pem* file was saved to your **Downloads** directory, run this command:

    ```bash
    cd ~/Downloads
    ```

20. Change the permissions on the key to be read-only by running this command:

    ```bash
    chmod 400 labsuser.pem
    ```

21. Run the following command *(replace **&lt;public-ip-address&gt;** with the **Bastion Host** address that you copied previously)*. 

    ```bash
    ssh -i labsuser.pem ec2-user@<public-ip>
    ```
22. When you are prompted to allow the first connection to this remote SSH server, enter: `yes` 
  

Because you are using a key pair for authentication, you will not be prompted for a password.
​
<a id='ssh-after'></a>



### Configuring the AWS CLI

You will use the information from the Lab Details text file that you created in the *Launching your lab environment* task.

The Bastion Host instance has an instance profile called *EC2InstanceProfile* that's associated to it. This instance profile gives the instance the permissions that it needs. 

23. In the terminal window, configure the AWS CLI.

    ```bash
    aws configure
    ```
    
24. At the prompts, enter the following information:
      
      **Note**: Ensure that you copy and paste the correct user values for the following steps. 

     -  **AWS Access Key ID**: Press ENTER
     -  **AWS Secret Access Key**: Press ENTER
     -  **Default region name**: Enter the name of the Region (for example: `us-east-1` or `eu-west-2`)
     -  **Default output format**: `json`



## Task 1: Exploring the users and groups

In this task, you will explore the IAM users and groups that were already created for you.

First, you will identify the name of the Amazon Simple Storage Service (Amazon S3) bucket, which was created for this lab. 

25. In the terminal, enter the following command:

    ```bash
    aws s3 ls
    ```

    Example output: 
    
    > 2020-05-03 14:56:11 samplebucket--<xxxxxxxx>
    
    The name of the S3 bucket consists of the string *samplebucket--* and a series of numbers, which is represented by <*xxxxxxxx*>.
    
    
    
26. In the Lab Details text file, record the name of the S3 bucket.

27. Create a sample text file and upload it to the S3 bucket that you identified previously. 

    Replace *samplebucket--*<*xxxxxxxx*> with the name of the S3 bucket.

    ```bash
    echo "Hello World" >> sample.txt
    aws s3 cp sample.txt s3://samplebucket--<xxxxxxxx>
    ```

    Example output:

    > upload: ./sample.txt to s3://samplebucket--<xxxxxxxx>/sample.txt


28. List the users that were created for the lab. In the terminal, enter the following command. 

    ```bash
    aws iam list-users
    ```

    **Note**: To exit the AWS CLI output, press Q, which should return the cursor to a prompt.

    Example output:  

      > {
      >     "Users": [
      >         {
      >             "Path": "/",
      >             **"UserName": "awsstudent",**
      >             "UserId": "AXXXXXXXXXXXXXXXXXX",
      >             "Arn": "arn:aws:iam::012345678910:user/awsstudent",
      >             "CreateDate": "2020-05-03T13:51:16+00:00"
      >         },
      >         {
      >             "Path": "/spl66/",
      >             **"UserName": "user-1",**
      >             "UserId": "AXXXXXXXXXXXXXXXXXXXX",
      >             "Arn": "arn:aws:iam::012345678910:user/spl66/user-1",
      >             "CreateDate": "2020-05-03T13:50:48+00:00"
      >         },
      >         {
      >             "Path": "/spl66/",
      >             **"UserName": "user-2",**
      >             "UserId": "AXXXXXXXXXXXXXXXXXXXX",
      >             "Arn": "arn:aws:iam::012345678910:user/spl66/user-2",
      >             "CreateDate": "2020-05-03T13:50:48+00:00"
      >         },
      >         {
      >             "Path": "/spl66/",
      >             **"UserName": "user-3",**
      >             "UserId": "AXXXXXXXXXXXXXXXXXXXX",
      >             "Arn": "arn:aws:iam::012345678910:user/spl66/user-3",
      >             "CreateDate": "2020-05-03T13:50:48+00:00"
      >         }
      >     ]
      > }
    
    The default lab user is *awsstudent*. You will explore the *user-1*, *user-2*, and *user-3* users in upcoming steps.

29. To query only the user names, run the following command.

    ```bash
    aws iam list-users --query 'Users[*].UserName'
    ```

    Example output:

    > [
    >     "awsstudent",
    >     "user-1",
    >     "user-2",
    >     "user-3"
    > ]

30. Query the information for *user-1*. In the terminal, run the following command.

    ```bash
    aws iam get-user --user-name user-1
    ```

    Example output:

    > {
    >     "User": {
    >         "Path": "/spl66/",
    >         "UserName": "user-1",
    >         "UserId": "AXXXXXXXXXXXXXXXXXXXX",
    >         "Arn": "arn:aws:iam::012345678910:user/spl66/user-1",
    >         "CreateDate": "2020-04-29T03:03:14+00:00"
    >     }
    > }

    This output is the summary page for *user-1*. 

31. Check if any policies are attached to *user-1* by running the following command.

    ```bash
    aws iam list-attached-user-policies --user-name user-1
    ```

    Notice that *user-1* doesn't have any policies attached to it.

32. Check if *user-1* is a member of any groups by running the following command.

    ```bash
    aws iam list-groups-for-user --user-name user-1
    ```

    The output should show that *user-1* isn't a member of any groups.

    You will now explore the AWS CLI *group* options for IAM.

33. List the different *IAM groups* that were created for you by running the following command.

    ```bash
    aws iam list-groups
    ```

    Example output:

    > {
    >     "Groups": [
    >         {
    >             "Path": "/spl66/",
    >             **"GroupName": "EC2-Admin",**
    >             "GroupId": "AXXXXXXXXXXXXXXXXXXXX",
    >             "Arn": "arn:aws:iam::012345678910:group/spl66/EC2-Admin",
    >             "CreateDate": "2020-04-29T03:03:15+00:00"
    >         },
    >         {
    >             "Path": "/spl66/",
    >             **"GroupName": "EC2-Support",**
    >             "GroupId": "AXXXXXXXXXXXXXXXXXXXX",
    >             "Arn": "arn:aws:iam::012345678910:group/spl66/EC2-Support",
    >             "CreateDate": "2020-04-29T03:03:14+00:00"
    >         },
    >         {
    >             "Path": "/spl66/",
    >             **"GroupName": "S3-Support",**
    >             "GroupId": "AXXXXXXXXXXXXXXXXXXXX",
    >             "Arn": "arn:aws:iam::012345678910:group/spl66/S3-Support",
    >             "CreateDate": "2020-04-29T03:03:13+00:00"
    >         }
    >     ]
    > }

    **(Optional) Challenge**: Try to query only the *GroupName* for each group.

34. Explore the *EC2-Support* group. In the terminal, enter the following command.

     ```bash
     aws iam get-group --group-name EC2-Support
     ```

     Example output:

      > {
      >     **"Users": [],**
      >     "Group": {
      >         "Path": "/spl66/",
      >         **"GroupName": "EC2-Support",**
      >         "GroupId": "AXXXXXXXXXXXXXXXXXXXX",
      >         "Arn": "arn:aws:iam::012345678910:group/spl66/EC2-Support",
      >         "CreateDate": "2020-04-29T03:03:14+00:00"
      >     }
      > }

      From the output, notice that the *EC2-Support group* currently has no users. 

35. Determine if any IAM policies are attached to the *EC2-Support* group by running the following command.

    ```bash
    aws iam list-attached-group-policies --group-name EC2-Support
    ```

    Example output:

    > {
    >     "AttachedPolicies": [
    >         {
    >             **"PolicyName": "AmazonEC2ReadOnlyAccess",**
    >             **"PolicyArn": "arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess"**
    >         }
    >     ]
    > }

    This group has a managed policy associated with it, which is called *AmazonEC2ReadOnlyAccess*. 

    *Managed policies* are pre-built policies that can be attached to IAM users and groups. They are built either by AWS or by your administrators. When the managed policy is updated, the changes to the policy immediately apply to all users and groups that are attached to it.

36. In the terminal, explore the *AmazonEC2ReadOnlyAccess* policy by entering the following command.

    ```bash
    aws iam get-policy-version --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess --version-id v1
    ```

    Example output:
    
    > {
    >  "PolicyVersion": {
    >      "Document": {
    >          "Version": "2012-10-17",
    >          "Statement": [
    >              {
    >                  "Effect": "Allow",
    >                  "Action": "ec2:Describe*",
    >                  "Resource": "\*"
    >              },
    >              {
    >                  "Effect": "Allow",
    >                  "Action": "elasticloadbalancing:Describe*",
    >                  "Resource": "\*"
    >              },
    >              {
    >                  "Effect": "Allow",
    >                  "Action": [
    >                      "cloudwatch:ListMetrics",
    >                      "cloudwatch:GetMetricStatistics",
    >                      "cloudwatch:Describe*"
    >                  ],
    >                  "Resource": "\*"
    >              },
    >              {
    >                  "Effect": "Allow",
    >                  "Action": "autoscaling:Describe*",
    >                  "Resource": "\*"
    >              }
    >          ]
    >      },
    >      "VersionId": "v1",
    >      "IsDefaultVersion": true,
    >      "CreateDate": "2015-02-06T18:40:17+00:00"
    >  }
    > }

     A *policy* defines what actions are allowed or denied for specific AWS resources. This policy grants permissions to *list* and *describe* information about Amazon EC2, Elastic Load Balancing, Amazon CloudWatch, and Amazon EC2 Auto Scaling. This ability to view resources, but not modify them, is ideal for assigning to a Support role.
    
    In an IAM policy, the basic structure of the statements is:

     *  **Effect** - Says whether to *Allow* or *Deny* the permissions
     *  **Action** - Specifies the API calls that can be made against an AWS service 
         *  For example: *cloudwatch:ListMetrics*
     *  **Resource** - Defines the scope of entities that are covered by the policy rule 
         *  For example: A specific S3 bucket or EC2 instance
         *  You can also use an asterisk (\*), which means *any resource*

37. Similarly, the *S3-Support* group has the *AmazonS3ReadOnlyAccess* policy attached to it.

    **(Optional) Challenge**: Use the AWS CLI to verify that the *AmazonS3ReadOnlyAccess* policy is attached to the *S3-Support* group. 
     If you need help, try the following command.

    ```
    aws iam list-attached-group-policies --group-name S3-Support
    ```

38. Explore the *EC2-Admin* group's permissions.

    This group is slightly different from the other two. Instead of a managed policy, it uses an *inline policy*, which is a policy that's assigned to only one user or group. 

    Inline policies are typically used to apply permissions for singular situations. Thus, if you try the `list-attached-group-policies` command for the *EC2-Admin* group, no managed policy would be listed.

    To list inline policies, use the `list-group-policies` command. In the terminal, enter the following command. 

    ```bash
    aws iam list-group-policies --group-name EC2-Admin
    ```

     Example output:

    > {
    >     "PolicyNames": [
    >         "EC2-Admin-Policy"
    >     ]
    > }

39. In the terminal, review the *EC2-Admin-Policy* inline policy by entering the following command.

    ```bash
        aws iam get-group-policy --group-name EC2-Admin --policy-name EC2-Admin-Policy
    ```

     Example output:

     > {
     >     "GroupName": "EC2-Admin",
     >     "PolicyName": "EC2-Admin-Policy",
     >     "PolicyDocument": {
     >         "Version": "2012-10-17",
     >         "Statement": [
     >             {
     >                 "Action": [
     >                     "ec2:Describe*",
     >                     "ec2:StartInstances",
     >                     "ec2:StopInstances"
     >                 ],
     >                 "Resource": [
     >                     "\*"
     >                 ],
     >                 "Effect": "Allow"
     >             }
     >         ]
     >     }
     > }

    This policy grants permission to view (*Describe*) information about Amazon EC2. It also grants the ability to start and stop instances (*StartInstances* and *StopInstances*).




## Business scenario

For the remainder of this lab, you will work with these users and groups to enable permissions that support the following business scenario.

You work for a company that's growing its use of AWS products and services. They use many EC2 instances and a large amount of Amazon S3 storage. You want to give access to new staff, depending on their job function.


|User|In Group|Permissions|
|----|--------|-----------|
|user-1|S3-Support|Read-only access to Amazon S3|
|user-2|EC2-Support|Read-only access to Amazon EC2|
|user-3|EC2-Admin|View, start and stop EC2 instances|

<!-- Use the image if the previous table doesn't render correctly.
<img src="./images/lab-6-table.png" alt="Permissions Table" width="400">
-->




## Task 2: Adding users to groups

You recently hired *user-1* into a role where they will provide support for Amazon S3. You will add them to the *S3-Support* group so that they inherit the necessary permissions through the attached *AmazonS3ReadOnlyAccess* policy.

<i class="fa fa-comment"></i> You can ignore any *not authorized* errors that appear during this task. These errors result from the limited permissions of your lab account, and will not impact your ability to complete the lab.



### Adding user-1 to the S3-Support group

40. In the terminal, add *user-1* to the *S3-Support* group by entering the following command.

    ```bash
    aws iam add-user-to-group --user-name user-1 --group-name S3-Support
    ```

41. Verify that *user-1* was added to the *S3-Support* group. 
    In the terminal, run the following command.

    ```bash
    aws iam get-group --group-name S3-Support
    ```

    Example output: The following output shows that *user-1* is in the *S3-Support* group.

     > {
     >     "Users": [
     >         {
     >             "Path": "/spl66/",
     >             "UserName": "user-1",
     >             "UserId": "AXXXXXXXXXXXXXXXXXXXX",
     >             "Arn": "arn:aws:iam::012345678910:user/spl66/user-1",
     >             "CreateDate": "2020-04-29T03:03:14+00:00"
     >         }
     >     ],
     >     "Group": {
     >         "Path": "/spl66/",
     >         "GroupName": "S3-Support",
     >         "GroupId": "AXXXXXXXXXXXXXXXXXXXX",
     >         "Arn": "arn:aws:iam::012345678910:group/spl66/S3-Support",
     >         "CreateDate": "2020-04-29T03:03:13+00:00"
     >     }
     > }


### Adding user-2 to the EC2-Support group

You hired *user-2* into a role where they will provide support for Amazon EC2.

42. Using actions that are similar to the previous steps, add *user-2* to the *EC2-Support* group.

    If you're not sure about the correct command, use the following example.

    ```
    aws iam add-user-to-group --user-name user-2 --group-name EC2-Support
    ```

    Now, *user-2* should be in the *EC2-Support* group.



### Adding user-3 to the EC2-Admin group

You hired *user-3* to be your Amazon EC2 administrator. They will manage your EC2 instances.

43. Using actions that are similar to the previous steps, add *user-3* to the *EC2-Admin* group.

    If you're not sure about the correct command, use the following example.

    ```
    aws iam add-user-to-group --user-name user-3 --group-name EC2-Admin
    ```
    
    Now, *user-3* should be in the *EC2-Admin* group.

    **(Optional) Challenge**: Verify that membership in each group consists of one user.



## Task 3: Testing the permissions of each user

In this task, you will test the permissions of each IAM user. To do this, you must first create [named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html) for each user so that they can log in to the AWS CLI with their credentials.

A *named profile* is a collection of settings and credentials that you can apply to an AWS CLI command. When you specify a profile to run a command, the settings and credentials in the profile are used to run that command. 

You can specify one profile that is the *default*. The default profile is used when no profile is explicitly referenced. Other profiles have names that you can specify for individual commands. For these cases, the profile names are passed as a parameter in the command line.

Previously in the lab, you used the `aws configure` command to configure the AWS CLI for the default lab user. In this task, you will similarly configure the AWS CLI for *user-1*, *user-2*, and *user-3*.

**Note:**

* You will use information from the Lab Details text file that you created in the *Launching your lab environment* task.
* Ensure that you use the correct user values for the following steps.

44. Configure a named profile for *user-1*. The profile should allow *user-1* to log in with their credentials.  

    Enter the following command:

    ```bash
    aws configure --profile user-1
    ```

45. At the prompts, enter the following information:
  
    **Note**: In the following steps, make sure that you copy and paste the correct user values.

    -  **AWS Access Key ID**: Enter the *AccessKey* value for the *User1AccessSecretKey* resource. You can copy it from the Lab Details text file and paste it into the terminal window.
    -  **AWS Secret Access Key**: Enter the *SecretKey* value for the *User1AccessSecretKey* resource. You can copy it from the Lab Details text file and paste it into the terminal window.
    -  **Default region name**: Enter the name of the Region from the Lab Details text file. You can also copy it from the Lab Details text file and paste it into the terminal window. For example, `us-east-1` or `eu-west-2`. 
    -  **Default output format**: `json`

    Example output:

    > AWS Access Key ID [****************CVVP]: AXXXXXXXXXXXXXXXXXXXX
    >  AWS Secret Access Key [****************CODE]: 012345678910abcdefghijklmnopqrstuvxyz
    >    Default region name []: us-east-1
    >  Default output format [json]: json

    This link can be used to log in to the AWS account you are currently using.
         
    
46. Verify that the profile for user-1 has been created by checking the AWS CLI configuration page.

    ```bash
    cat ~/.aws/config
    ```

    Example output: The below output lists the default lab user profile and *user-1*'s profile.

    > [default]
    > region = ap-southeast-2
    > output = json
    > **[profile user-1]**
    > region = ap-southeast-2
    > output = json

47. (Optional) You can also access the saved credentials for *user-1*.

    ```bash
    cat ~/.aws/credentials
    ```

    Example output:

    > [user-1]
    > aws_secret_access_key = ^dH90FOGIhkl909u34jbKHKjb98dfbjd824bja*y3re
    > aws_access_key_id = AXXXXXXXXXXXXXXXXX

48. In a similar way, configure named profiles for *user-2* and *user-3* by using the previous steps. The required details are in the Lab Details text file.

    The following table describes the options that you can enter.

    |          Option                    | user-2: *User2AccessSecretKey* from the Lab Details file | user-3: *User3AccessSecretKey* from the Lab Details file |
    | :------------------------ | :----: | :----: |
    | **AWS Access Key ID**     | Use the *AccessKey* value | Use the *AccessKey* value |
    | **AWS Secret Access Key** | Use the *SecretKey* value | Use the *SecretKey* value |
    | **Default region name** | Region | Region |
    | **Default output format** | json | json |



You will now run AWS CLI commands by using *user-1*'s profile. Recall that you hired *user-1* as your Amazon S3 storage support staff. 


49. List the S3 bucket that was created in the account.

    ```bash
    aws s3 ls --profile user-1
    ```

    Example output:

    > 2020-05-03 14:56:11 samplebucket--<xxxxxxxx>

50. Next, list the contents of the bucket from the previous step. 
    **Note**: The previous output is an example. When you run the following command, replace <*sample-bucket*> with the bucket name that was listed in your terminal window. 

    ```bash
    aws s3 ls s3://<sample-bucket> --profile user-1
    ```

    Example output:

    >    2020-05-04 00:33:52         12        sample.txt

    Because your *user-1* is part of the *S3-Support* group in IAM, they have permission to view a list of S3 buckets and their contents.

    Now, test whether *user-1* has access to Amazon EC2.

51. In the terminal, check if user-1 can list any instances in the Region by entering the following command.

    ```bash
    aws ec2 describe-instances --profile user-1
    ```

    Example output: *user-1* doesn't have access to any instances because they haven't been assigned permissions to use Amazon EC2.

    >    An error occurred (UnauthorizedOperation) when calling the DescribeInstances operation: You are not authorized to perform this operation.

    You will now run AWS CLI commands by using *user-2*'s profile. Recall that you hired *user-2* to be your Amazon EC2 support person.

52. In the terminal, enter the following command.

    ```bash
    aws ec2 describe-instances --profile user-2
    ```

    Example output: *Some of the values have been truncated in the below output.* Your output should be much longer than this.
    
    You are now able to see an Amazon EC2 instance because you have *read-only* permissions. However, you will not be able to make any changes to Amazon EC2 resources.

    >    {
    >
    >    ​    "Reservations": [
    >
    >    ​        {
    >
    >    ​            "Groups": [],
    >
    >    ​            "Instances": [
    >
    >    ​                {
    >
    >    ​                    "AmiLaunchIndex": 0,
    >
    >    ​                    "ImageId": "ami-012345678910",
    >
    >    ​                    "InstanceId": "i-012345678910",
    >
    >    ​                    "InstanceType": "t2.micro",
    >
    >    ​                    "KeyName": "labkey",
    >
    >    ​                    "LaunchTime": "2020-04-30T12:36:32+00:00",
    >
    >    ​                    "Placement": {
    >
    >    ​                        "AvailabilityZone": "ap-southeast-2a"
    >
    >    ​                    },
    >
    >    ​                    "Tags": [
    >
    >    ​                        {
    >
    >    ​                            "Key": "Name",
    >
    >    ​                            "Value": "Bastion Host"
    >
    >    ​                        }
    >
    >    ​                    ]
    >
    >    ​                }
    >
    >    ​            ]
    >
    >    ​        },
    >
    >    ​        {
    >
    >    ​            "Groups": [],
    >
    >    ​            "Instances": [
    >
    >    ​                {
    >
    >    ​                    "AmiLaunchIndex": 0,
    >
    >    ​                    "ImageId": "ami-012345678910",
    >
    >    ​                    "InstanceId": "i-0123655678910",
    >
    >    ​                    "InstanceType": "t2.micro",
    >
    >    ​                    "KeyName": "labkey",
    >
    >    ​                    "LaunchTime": "2020-04-30T12:36:32+00:00",
    >
    >    ​                    "Placement": {
    >
    >    ​                        "AvailabilityZone": "ap-southeast-2a"
    >
    >    ​                    },
    >
    >    ​                    "Tags": [
    >
    >    ​                        {
    >
    >    ​                            "Key": "Name",
    >
    >    ​                            "Value": "LabHost"
    >
    >    ​                        }
    >
    >    ​                    ]
    >
    >    ​                }
    >
    >    ​            ]
    >
    >    ​        }
    >
    >    ​    ]
    >
    >    }

53. (Optional) Use the `--query ` switch to limit the output to the instance ID, instance type, Availability Zone, and tags. 

    ```bash
    aws ec2 describe-instances --profile user-2 --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,Placement.AvailabilityZone,Tags[*]]'
    ```

    Example output: *Some of the tags have been truncated in the following output.* Your output should be longer than this example.

    >    [
    >
    >    ​    [
    >
    >    ​        [
    >
    >    ​            "i-012345678910",
    >
    >    ​            "t2.micro",
    >
    >    ​            "ap-southeast-2a",
    >
    >    ​            [
    >
    >    ​                {
    >
    >    ​                    "Key": "Name",
    >
    >    ​                    "Value": "Bastion Host"
    >
    >    ​                }
    >
    >    ​            ]
    >
    >    ​        ]
    >
    >    ​    ],
    >
    >    ​    [
    >
    >    ​        [
    >
    >    ​            "i-0123655678910",
    >
    >    ​            "t2.micro",
    >
    >    ​            "ap-southeast-2a",
    >
    >    ​            [
    >
    >    ​                {
    >
    >    ​                    "Key": "Name",
    >
    >    ​                    "Value": "LabHost"
    >
    >    ​                }
    >
    >    ​            ]
    >
    >    ​        ]
    >
    >    ​    ]
    >
    >    ]

54. Try to stop the *LabHost* instance. 

    In the following command, replace <*InstanceID*> with the value of the LabHostId (which is represented by *i-*<*xxxxxxxxxxxxxxxxx*>). This value should be in the Lab Details text file that you created in the *Launch Your Lab Environment* task. 

    ```bash
    aws ec2 stop-instances --instance-ids <InstanceID> --profile user-2
    ```

    Example output: *You receive an error.* This error demonstrates that the policy only permits read-only information, and that it does not allow changes.

    >    An error occurred (UnauthorizedOperation) when calling the StopInstances operation: You are not authorized to perform this operation. Encoded authorization failure message: H5RjoyMaJCnHek3DGGKIVlAGRoHDSsWOONdPxX26vBdC98jhRzFtOyKKxQnR_IefwBj8xgOvIIvkdA7AIF8nquyavU3L0kOoPaZtArmKtm19k6K9wAL_lbU7lLNay

55. Check if *user-2* can access Amazon S3.

    ```bash
    aws s3 ls --profile user-2
    ```

    Example output: *You receive an* Error Access Denied *message*. You get this message because *user-2* does not have the permissions to use Amazon S3.

    > An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied


56. Verify if *user-3* (who you hired as your Amazon EC2 administrator) can stop the *LabHost* instance. In the following command, replace <*InstanceID*> with the value of LabHostId ( i-<*xxxxxxxxxxxxxxxxx*> ). This value should be in the Lab Details text file that you created in the *Launch Your Lab Environment* task. 

    ```bash
    aws ec2 stop-instances --instance-ids <InstanceID> --profile user-3
    ```

    Example output: 

    >    {
    >        "StoppingInstances": [
    >            {
    >                "CurrentState": {
    >                    "Code": 64,
    >                    "Name": "stopping"
    >                },
    >                "InstanceId": "i-0123655678910",
    >                "PreviousState": {
    >                    "Code": 16,
    >                    "Name": "running"
    >                }
    >            }
    >        ]
    >    }

    The instance will enter the *stopping* state and will eventually shut down.

57. You can now exit the terminal.



## Lab summary

Now that you completed this lab, you should be able to:

 * Explore pre-created *IAM users and groups*
 * Inspect *IAM policies* that are applied to the pre-created groups
 * Follow a *real-world scenario* by adding users to groups that have specific enabled capabilities 
 * Test each user's *permissions*
 * *Experiment* with the effects of policies on service access



## Lab complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

58. To confirm that you want to end the lab, at the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then choose <span id="ssb_blue">Yes</span>

   A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*
   ​
59. To close the panel, choose the **X** in the top-right corner.


### Contact us

[Report an error or correction.](mailto:aws-course-feedback@amazon.com)

For other questions, contact us through the [Contact AWS Training and Certification page.](https://aws.amazon.com/contact-us/aws-training)

*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*

