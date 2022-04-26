# Activity: AWS Elastic Beanstalk

<!-- Note to translators: This activity is unique to this course. -->



## Overview

This activity provides you with an Amazon Web Services (AWS) account where an AWS Elastic Beanstalk environment has been pre-created for you. You will deploy code to it and observe the AWS resources that make up the Elastic Beanstalk environment.

This lab is based on the AWS Command Line Interface (AWS CLI).

Elastic Beanstalk resources can be managed by the [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/index.html) or the [Elastic Beanstalk Command Line Interface (EB CLI)](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html). In this lab, you will use the AWS CLI to configure Elastic Beanstalk.

### Duration

This activity takes approximately **30 minutes** to complete.



## Launching your lab environment

1. At the top of these instructions, select <span id="ssb_voc_grey">Start Lab</span> to launch this lab.
  
    A **Start Lab** panel opens and displays the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by selecting the **X**.
  
    This lab launches an Amazon Elastic Compute Cloud (Amazon EC2) instance that's named *Bastion Host*. You will use this server to create lab resources by running the AWS CLI commands.

3. Above these instructions, select the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>
  
    Copy all the lab details—such as *BastionHost*, *Region*, and others—to a text file and save it as `Lab Details.txt`. 

    
    **Note**: Make sure that you use a *text editor* (a program for editing text files) to save the file. Examples of text editors include [Atom](https://atom.io/), [Sublime Text](https://www.sublimetext.com/), or [Microsoft Visual Studio Code](https://code.visualstudio.com/).
    
    In the lab, the information that you saved will be referred to as <u>*Lab Details*</u>.



## Using SSH to connect to the bastion host

### Microsoft Windows users

<i class="fas fa-comment"></i> These instructions are specifically for *Microsoft Windows* users. If you use macOS or Linux, <a href="#ssh-MACLinux">skip to the next section</a>.

4. Above these instructions (that you are currently reading), choose the <span id="ssb_voc_grey">Details</span> dropdown menu, and then choose <span id="ssb_voc_grey">Show</span> 
  
     A **Credentials** window opens.

5. Choose the **Download PPK** button and save the **labsuser.ppk** file. 

  **Note**: Typically, your browser will save this file in the **Downloads** directory.

6. Exit the **Credentials** window by choosing the **X**.

7. From the Lab Details file, copy the **Bastion Host** address. 

8. If needed, download **PuTTY** so that you can use Secure Shell (SSH) to connect to the EC2 instance. If PuTTY is not installed on your computer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download it here</a>.

9. Open **putty.exe**.

10. To keep the PuTTY session open for a longer period of time, configure the PuTTY timeout.
    * Choose **Connection**
    * Set **Seconds between keepalives** to `30`

11. Configure your PuTTY session to connect through SSH.
    * Choose **Session**  
    * In the **Host Name (or IP address)** box, paste the **Bastion Host** address that you copied in a previous step
    * Back in PuTTY, in the **Connection** list, expand <i class="far fa-plus-square"></i> **SSH**
    * Choose **Auth** *(do not expand it)*
    * Choose **Browse**
    * Browse to and select the **labsuser.ppk** file that you downloaded
    * Choose **Open** to select it
    * Choose **Open** again

12. To trust and connect to the host, choose **Yes**.

13. When prompted with **login as**, enter: `ec2-user`
  
    This command will connect you to the EC2 instance.

14. Microsoft Windows Users: <a href="#ssh-after">Click here to skip ahead to the next task.</a>

<a id='ssh-MACLinux'></a>

### macOS and Linux users

These instructions are specifically for macOS or Linux users. If you use Microsoft Windows, <a href="#ssh-after">skip ahead to the next task.</a>
​

15. Above these instructions (that you are currently reading), choose the <span id="ssb_voc_grey">Details</span> dropdown menu, and then choose <span id="ssb_voc_grey">Show</span> 
  
    A **Credentials** window opens.

16. Choose the **Download PEM** button and save the **labsuser.pem** file.

17. Exit the **Credentials** window by choosing the **X**.

18. From the Lab Details file, copy the **Bastion Host** address. 

19. Open a terminal window, and change directory (by using `cd`) to the directory where you downloaded the *labsuser.pem* file.
  
    For example, if the *labuser.pem* file was saved to your **Downloads** directory, run this command:

    ```bash
    cd ~/Downloads
    ```
20. Change the permissions on the key to be read-only, by running the following command.

    ```bash
    chmod 400 labsuser.pem
    ```
21. Run the following command (replace &lt;*public-ip-address*&gt; with the **Bastion Host** address that you copied in a previous step). 

    ```bash
    ssh -i labsuser.pem ec2-user@<public-ip-address>
    ```
22. When prompted to allow the first connection to this remote SSH server, enter `yes`.
  
    Because you are using a key pair for authentication, you won't be prompted for a password.
  ​
  <a id='ssh-after'></a>



### Configuring the AWS CLI

You will use the information from the Lab Details text file, which you created in the **Launching your lab environment** task.

The Bastion Host instance has an instance profile called *EC2InstanceProfile* that's associated to it. The Bastion Host is granted the required permissions by this instance profile. 

23. In a terminal window, configure the AWS CLI.

    ```bash
    aws configure
    ```
    
24. At the prompts, enter the following information:
      **Note**: Ensure that the correct user values are copied and pasted for the following steps. 

     -  **AWS Access Key ID**: Press ENTER
     -  **AWS Secret Access Key**: Press ENTER
     -  **Default region name**: Enter the name of the AWS Region (for example, `us-east-1` or `eu-west-2`)
     -  **Default output format**: `json`



## Task 1: Accessing the Elastic Beanstalk environment

In this task, you will explore the Elastic Beanstalk environment that was created in the lab environment.

25. In the terminal, enter the following command. 

    If needed, you can scroll through the output by using the down arrow key, and you can quit by pressing Q.

    ```bash
    aws elasticbeanstalk describe-environments
    ```

    Example output: 

    > {
    >
    > ​    "Environments": [
    >
    > ​        {
    >
    > ​            "**EnvironmentName**": "eb-samp-XXXXXXXXX",
    >
    > ​            "EnvironmentId": "e-Jjjxxxtta5",
    >
    > ​            "**ApplicationName**": "eb-sampleApplication-J9ITXXXXX8H7",
    >
    > ​            "SolutionStackName": "64bit Amazon Linux 2018.03 v3.3.7 running Tomcat 8.5 Java 8",
    >
    > ​            "PlatformArn": "arn:aws:elasticbeanstalk:ap-southeast-2::platform/Tomcat 8.5 with Java 8 running on 64bit Amazon Linux/3.3.7",
    >
    > ​            "EndpointURL": "awseb-e-j-AWSEBLoa-XXXXXXXXX.ap-southeast-2.elb.amazonaws.com",
    >
    > ​            "**CNAME**": "eb-samp-XXXXXXXXX.eba-xxxxxxxxxx.ap-southeast-2.elasticbeanstalk.com",
    >
    > ​            "Status": "Ready",
    >
    > ​            "AbortableOperationInProgress": false,
    >
    > ​            **"Health": "Green"**,
    >
    > ​            "Tier": {
    >
    > ​                "Name": "WebServer",
    >
    > ​                "Type": "Standard",
    >
    > ​                "Version": "1.0"
    >
    > ​            },
    >
    > ​            "EnvironmentArn": "arn:aws:elasticbeanstalk:ap-southeast-2:012345678910:environment/eb-sampleApplication-J9ITXXXXX8H7/eb-samp-XXXXXXXXX"
    >
    > ​        }
    >
    > ​    ]
    >
    > }

    <i class="fas fa-info-circle"></i> **Note**: If the **Health** status is not *Green*, the environment has not finished starting yet. Wait for a few minutes, and try the same command again. It should change to *Green*. This status indicates that the Elastic Beanstalk environment is ready to host an application. However, it doesn't have running code yet.

26. In the Lab Details text file, note the *CNAME*, *ApplicationName*, and *EnvironmentName* values. You will use these values to validate the Elastic Beanstalk application.

27. You can test the Elastic Beanstalk CNAME either by using a web browser or the AWS CLI.

    <u>Option 1:</u>
    
    If you can browse the internet, you can use a web browser to access the CNAME value that you noted in the previous step. However, you should see that the browser displays an *HTTP Status 404 - Not Found* message. *This behavior is expected* because this application server doesn't have an application running on it yet. 

    The following table describes the status of common HTTP codes. Source: [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
        
    
    | HTTP Code | Status                                                       |
    | --------- | ------------------------------------------------------------ |
    | 100       | The request was received, continuing process.                |
    | 200       | The request was successfully received and accepted.          |
    | 300       | Further action needs to be taken in order to complete the request. |
    | 400       | The request contains bad syntax or cannot be fulfilled.      |
    | 404       | The requested resource could not be found but may be available in the future. |
    | 500       | The server failed to fulfil an apparently valid request.     |

    
    <u>Option 2:</u>
    
    Alternatively, you can check the HTTP status code by running the following command in the terminal window. Replace <*cname*> with the CNAME value that you noted in the previous step.    
    
    ```bash
    curl -I <cname>
    ```
    
    Example output:
    
    > **HTTP/1.1 404**
    >
    > Content-Language: en
    > Content-Type: text/html;charset=utf-8
    > Date: Tue, 09 Jun 2020 12:00:46 GMT
    > Server: Apache/2.4.43 (Amazon) OpenSSL/1.0.2k-fips
    >
    > Connection: keep-alive

    You receive an *HTTP 404* status because this application server doesn't have an application running on it yet. 

    In the next step, you will deploy an application in your Elastic Beanstalk environment.



## Task 2: Deploying a sample application to Elastic Beanstalk

First, you will create an Amazon Simple Storage Service (Amazon S3) bucket to store code for a sample application. You will use the application code later to update the Elastic Beanstalk environment. 

28. In the terminal, run the following command. Make sure to replace <*bucket-name*> with your *name* and *a date*. 
  
    **Note**: 
    - Bucket names must be unique and they only support lowercase letters. Example: *aws s3 mb s3://shirleyrodriguez-20200609*
    - In the Lab Details text file, note the bucket name. You will use this value later in the lab.
    
    ```bash
    aws s3 mb s3://<bucket-name>
    ```

29. Download a sample application to the S3 bucket that you created in the previous step. In the terminal, run the following command. Replace <*bucket-name*> with the name of your bucket. 

    ```bash
    curl "https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip" | aws s3 cp - s3://<bucket-name>/tomcat.zip
    ```

    <!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html-->

Next, you will create an application version that's based on the sample application, which is stored in Amazon S3.

30. Replace <*application-name*> and <*bucket-name*> with the values that you noted from the Lab Details text file. Replace <*your-name-date*> with a unique value, such as your name followed by a date. Example: *shirleyrodriguez-2020609*
    - Note the *version-label* value in the Lab Details text file. You will use this value in the next step.

    ```bash
    aws elasticbeanstalk create-application-version --application-name <application-name> --version-label <your-name-date> --description "Updating sample application" --source-bundle S3Bucket="<bucket-name>",S3Key="tomcat.zip"
    ```

    Example output:

      > {
      >     "ApplicationVersion": {
      >         "ApplicationVersionArn": "arn:aws:elasticbeanstalk:ap-southeast-2:012345678910:applicationversion/test-sampleApplication-J9XXXXXX8H7/user20200609",
      >         "ApplicationName": "test-sampleApplication-J9XXXXXX8H7",
      >         "Description": "Updating sample application",
      >         "VersionLabel": "user20200609",
      >         "SourceBundle": {
      >             "S3Bucket": "userbucket2020",
      >             "S3Key": "java-tomcat-v3.zip"
      >         },
      >         "DateCreated": "2020-06-09T13:37:32.973000+00:00",
      >         "DateUpdated": "2020-06-09T13:37:32.973000+00:00",
      >         "Status": "UNPROCESSED"
      >     }
      > } 

31. Update the Elastic Beanstalk application by using the application version that you created in the previous step. Replace <*environment-name*> and <*version-label*> with the values that you noted from the Lab Details text file.

    ```bash
    aws elasticbeanstalk update-environment --environment-name <environment-name> --version-label <version-label>
    ```

    Example output:
    
      > {
      >     "EnvironmentName": "test-samp-6Gxxxxxxx76Z",
      >     "EnvironmentId": "e-jjcknxtta5",
      >     "ApplicationName": "test-sampleApplication-J9IXXXXXX8H7",
      >     "VersionLabel": "user20200609",
      >     "SolutionStackName": "64bit Amazon Linux 2018.03 v3.3.7 running Tomcat 8.5 Java 8",
      >     "PlatformArn": "arn:aws:elasticbeanstalk:ap-southeast-2::platform/Tomcat 8.5 with Java 8 running on 64bit Amazon Linux/3.3.7",
      >     "Description": "AWS ElasticBeanstalk Sample Environment",
      >     "EndpointURL": "awseb-e-j-XXXXXXXXXXXXXXXXXXX.ap-southeast-2.elb.amazonaws.com",
      >     "CNAME": "test-samp-6Gxxxxxxx76Z.eba-m9pqq32b.ap-southeast-2.elasticbeanstalk.com",
      >     "DateCreated": "2020-06-09T10:46:11.759000+00:00",
      >     "DateUpdated": "2020-06-09T13:48:47.687000+00:00",
      >     "Status": "Updating",
      >     "AbortableOperationInProgress": true,
      >     "Health": "Grey",
      >     "Tier": {
      >         "Name": "WebServer",
      >         "Type": "Standard",
      >         "Version": "1.0"
      >     },
      >     "EnvironmentArn": "arn:aws:elasticbeanstalk:ap-southeast-2:012345678910:environment/test-sampleApplication-J9IXXXXXX8H7/test-samp-6Gxxxxxxx76Z"
      > }
    
    It will take a minute or two for Elastic Beanstalk to update your environment and deploy the application.
    
32. Check if the update was successful. Proceed after the status is *Green*.

    ```bash
    aws elasticbeanstalk describe-environments
    ```

33. After the application deployment is complete, you can test if it was successful by either using a web browser or the AWS CLI.

    <u>Option 1:</u>
    If you can browse the internet, then you can use a web browser to access the *CNAME* value that you noted from the Lab Details text file.
    
    The web application that you deployed displays.

    <img src="../../images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    <u>Option 2:</u>
    Alternatively, you can check the HTTP status code in the terminal window by running the following command. Make sure that you replace <*cname*> with the CNAME value you noted from the Lab Details text file.

    ```bash
    curl -I <cname>
    ```

    Example output:

      > **HTTP/1.1 200**
      > Content-Type: text/html;charset=UTF-8
      > Date: Tue, 09 Jun 2020 14:11:01 GMT
      > Server: Apache/2.4.43 (Amazon) OpenSSL/1.0.2k-fips
      > Set-Cookie: JSESSIONID=B36EXXXXXXXXXXXXXXXXXXXXD6D6B; Path=/; HttpOnly
      > X-Amzn-Trace-Id: Root=1-5edf9874-d6a40sdjf8d6fg87gfd98831b97;
      > Connection: keep-alive

    An *HTTP 200* status indicates that the request was successfully received and accepted.
    
    Congratulations, you have successfully deployed an application on Elastic Beanstalk!




## Task 3: Exploring the AWS resources that support your application

34. In your terminal, you can list all the instances and their *Name* tags by using the following command.

    ```bash
    aws ec2 describe-instances --query 'Reservations[].Instances[].{InstanceId:InstanceId,Tags:Tags[?Key==`Name`]|[0].Value}|[]' --output table
    ```
    
    Example output: 

    > +-------------------------------------------------------+
    > |                             DescribeInstances                        |
    > +---------------------------+---------------------------+
    > |            InstanceId            |                Tags                  |
    > +---------------------------+---------------------------+
    > |  i-04abcdefghijklmnopq |  Bastion Host                 |
    > |  i-01rstuvwxyzabcdefgh |  test-samp-6GOxx76Z  |
    > |  i-0bijklmnopqrstuvwxy |  test-samp-6GOxx76Z  |
    > +---------------------------+---------------------------+
    
    Note that *three instances* are running. The two instances with *samp* in their names are part of the Elastic Beanstalk application that hosts your web application. The *Bastion Host* instance is used to run AWS CLI commands.

35. You can view the Elastic Beanstalk configuration information by running the following command. Replace <*environment-name*> and <*application-name*> with the values that you noted from the Lab Details text file.

    ```bash
    aws elasticbeanstalk describe-configuration-settings --application-name <application-name> --environment-name <environment-name>
    ```

    Example output: The following example is part of the output.
    
      > {
      >  "ConfigurationSettings": [
      >      {
      >          "SolutionStackName": "64bit Amazon Linux 2018.03 v3.3.7 running Tomcat 8.5 Java 8",
      >          "PlatformArn": "arn:aws:elasticbeanstalk:ap-southeast-2::platform/Tomcat 8.5 with Java 8 running on 64bit Amazon Linux/3.3.7",
      >          "ApplicationName": "test-sampleApplication-J9IXXXXX8H7",
      >          "Description": "AWS ElasticBeanstalk Sample Environment",
      >          "EnvironmentName": "test-samp-J9IXXXXX8H7",
      >          "DeploymentStatus": "deployed",
      >          "DateCreated": "2020-06-09T10:46:10+00:00",
      >          "DateUpdated": "2020-06-09T14:00:42+00:00",
      >      }
      >      {
      >      "ResourceName": "AWSEBAutoScalingLaunchConfiguration",
      >      "Namespace": "aws:autoscaling:launchconfiguration",
      >      "OptionName": "ImageId",
      >      "Value": "ami-098df1xxxxxx85450"
      >      },
      >      {
      >      "Namespace": "aws:autoscaling:launchconfiguration",
      >      "OptionName": "InstanceType",
      >      **"Value": "t2.micro"**
      >      },
      >      ...
      > }

36. Scroll through the output and observe the configuration details of the environment. You will find the following configuration for your Elastic Beanstalk application:

    - An *Auto Scaling group* that runs from 2–6 instances, depending on the network load
    - The EC2 *instance type*
    - A *security group* with port 80 open
    - A *load balancer* that's used by both instances
    
    Though Elastic Beanstalk created these resources for you, you can still access them.



## Lab complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

37. To confirm that you want to end the lab, at the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then choose <span id="ssb_blue">Yes</span>

   A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*
   ​
38. To close the panel, choose the **X** in the top-right corner.


### Contact us

[Report an error or correction.](mailto:aws-course-feedback@amazon.com)

For other questions, contact us through the [Contact AWS Training and Certification page.](https://aws.amazon.com/contact-us/aws-training)

*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*