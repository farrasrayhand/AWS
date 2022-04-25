# Lab 3: Introducing Amazon EC2

<!-- Note to translators: This is based on SPL-200 Version 1.1.7. Copy the translation from there. This version is however based on CLI and therefore other languages will have to be retranslated. -->

## Overview

<img src="images/lab-scenario.jpeg" alt="architectural diagram" width="400">

&nbsp;

This lab provides you with a basic overview of launching, resizing, managing, and monitoring an Amazon Elastic Compute Cloud (Amazon EC2) instance. This lab is based on the AWS Command Line Interface (AWS CLI). Feel free to explore the [AWS CLI documentation for Amazon EC2](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html) as you work through the lab.

*Amazon Elastic Compute Cloud (Amazon EC2)* is a web service that provides resizable compute capacity in the cloud. It's designed to make web-scale cloud computing easier for developers.

Amazon EC2 has a simple web service interface that enables you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and enables you to run on the Amazon Web Services (AWS) Cloud. Amazon EC2 reduces the time that's needed to obtain and launch new server instances to minutes. It enables you to quickly scale capacity, both up and down, as your computing requirements change.

&nbsp;
### Topics covered

After completing this lab, you should be able to:

  * Launch a web server that has termination protection
  * Monitor your EC2 instance
  * Modify the security group of your web server so that it allows HTTP access
  * Resize your EC2 instance to scale
  * Explore the maximum number of EC2 instances
  * Test termination protection
  * Terminate your EC2 instance


&nbsp;

### Duration

This lab takes approximately **35 minutes** to complete.

&nbsp;

**Other AWS services**

During this lab, you might receive error messages when you perform actions beyond the steps in this lab guide. These messages will not impact your ability to complete the lab.



## Launching your lab environment

1. At the top of these instructions, launch this lab by choosing <span id="ssb_voc_grey">Start Lab</span> 

   A **Start Lab** panel opens, and it displays the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by selecting the **X**.

   This lab launches an EC2 instance that's named *Bastion Host*. You will use this server to create lab resources by running the AWS CLI commands.

3. Above these instructions, select the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>
  
    Copy all the lab details—such as *BastionHost*, *UserAccessSecretKey*, *Region*, and others—to a text file and save it as `Lab Details.txt`. 



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

21. Run the following command *(replace **&lt;public-ip-address&gt;** with the **Bastion Host** address that you copied previously)*. 

    ```bash
    ssh -i labsuser.pem ec2-user@<public-ip-address>
    ```
22. When you are prompted to allow the first connection to this remote SSH server, enter: `yes` 
  

Because you are using a key pair for authentication, you will not be prompted for a password.
​
<a id='ssh-after'></a>



### Configuring the AWS CLI

You will use the information from the Lab Details text file that you created in the *Launch Your Lab Environment* task.

The *Bastion Host* instance has an instance profile that's called *EC2InstanceProfile* associated with it. This instance profile gives it the required permissions. 


23. In the terminal window, configure the AWS CLI.
    ​

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

## Task 1: Launching your EC2 instance

In this task, you will launch an Amazon EC2 instance with _termination protection_. Termination protection prevents you from accidentally terminating an EC2 instance. You will deploy your instance with a user data script that enables you to deploy a simple web server.

You will run an AWS CLI command to deploy your EC2 instance. However, you must first gather some information and create resources that will be referenced in this command. 


### Subtask 1: Identifying the most recent AMI

<i class="fas fa-info-circle"></i> An *Amazon Machine Image (AMI)* provides the information that's needed to launch an EC2 instance. An AMI includes:

  * A template for the root volume for the instance (for example, an operating system or an application server with applications)
  * Launch permissions that control which AWS accounts can use the AMI to launch instances
  * A block device mapping that specifies the volumes to attach to the instance when it launches

  AWS provides predefined AMIs that you can use. You can also create your own AMI or select an AMI from the AWS Marketplace, which is an online store where you can sell or buy software that runs on AWS.

25. Retrieve a list of the available AMIs and their associated details by using the `describe-images` command. In this example, you will limit the number of AMI IDs to the *100 most-current* images.

   ```bash
   aws ec2 describe-images --owners amazon --query 'reverse(sort_by(Images, &CreationDate))[:101]' --output text
   ```

   To scroll through the results, press the SPACE BAR. 
   
   Your query results contain a large amount of information. You don't need to scroll to the end of the list. 
   
   You can quit the search by pressing Q.
   
   Example output:

    > x86_64 2020-05-17T21:08:53.000Z        True  xen   **ami-05288dce5f2684b34**  amazon/aws-parallelcluster-2.7.0-centos7-hvm-202005172030    amazon machine aws-parallelcluster-2.7.0-centos7-hvm-202005172030   247102896272      Linux/UNIX   True
    >
    > /dev/sda1    ebs   simple available    RunInstances  hvm
    >
    >   BLOCKDEVICEMAPPINGS   /dev/sda1
    >
    > EBS   True  False  snap-0aa7e45a79e75176f 25   gp2
    >
    > x86_64 2020-05-17T21:05:29.000Z        True  xen   **ami-073f5b928a31ec540**  amazon/aws-parallelcluster-2.7.0-ubuntu-1804-lts-hvm-202005172030    amazon machine aws-parallelcluster-2.7.0-ubuntu-1804-lts-hvm-202005172030   247102896272      Linux/UNIX   True  /dev/sda1    ebs   simple available    RunInstances  hvm
    >
    > BLOCKDEVICEMAPPINGS   /dev/sda1
    >
    > EBS   True  False  snap-0e68176ba4e6c6826 25   gp2
    >
    > x86_64 2020-05-17T20:59:29.000Z        True  xen   **ami-0d16e36ead9387903**  amazon/aws-parallelcluster-2.7.0-amzn2-hvm-202005172030 amazon machine aws-parallelcluster-2.7.0-amzn2-hvm-202005172030    247102896272      Linux/UNIX   True  /dev/xvda    ebs   simple available    RunInstances  hvm
    >
    > BLOCKDEVICEMAPPINGS   /dev/xvda
    >
    > EBS   True  False  snap-03d64af6539570ff3 25   gp2
    >
    > x86_64 2020-05-17T20:57:10.000Z        True  xen   **ami-03e9ff251993d462f**  amazon/aws-parallelcluster-2.7.0-amzn-hvm-202005172030 amazon machine aws-parallelcluster-2.7.0-amzn-hvm-202005172030 247102896272      Linux/UNIX   True  /dev/xvda
    >
    > ​    ebs   simple available    RunInstances  hvm
    >
    > BLOCKDEVICEMAPPINGS   /dev/xvda
    >
    > EBS   True  False  snap-0fa141a70d87e247b 25   gp2
    >
    > x86_64 2020-05-17T20:56:28.000Z        True  xen   **ami-07a9d3a5f966b4382**  amazon/aws-parallelcluster-2.7.0-ubuntu-1604-lts-hvm-202005172030    amazon machine aws-parallelcluster-2.7.0-ubuntu-1604-lts-hvm-202005172030   247102896272      Linux/UNIX   True  /dev/sda1    ebs   simple available    RunInstances  hvm
    >
    > BLOCKDEVICEMAPPINGS   /dev/sda1
    >
    > EBS   True  False  snap-025c16a08ae624351 25   gp2
    >
    > x86_64 2020-05-17T20:55:04.000Z        True  xen   **ami-08ffdd73e861bfefc**  amazon/aws-parallelcluster-2.7.0-centos6-hvm-202005172030    amazon machine aws-parallelcluster-2.7.0-centos6-hvm-202005172030   247102896272      Linux/UNIX   True
    >
    > /dev/sda1    ebs   simple available    RunInstances  hvm
    >
    >   BLOCKDEVICEMAPPINGS   /dev/sda1
    >



26. For this lab, you will use the most recent AMI that runs the Amazon Linux 2 operating system. Find the AMI ID for this image by running the following command:

   ```bash
   aws ec2 describe-images --owners amazon --filters 'Name=name,Values=amzn2-ami-hvm-2.0.????????.?-x86_64-gp2' 'Name=state,Values=available' --query 'reverse(sort_by(Images, &CreationDate))[:1].ImageId' --output text
   ```
   
   Example output: You might see a different AMI ID than the following example.

    > ami-0323c3dd2da7fb37d
    >



27. Save this AMI *ImageId* value to the Lab Details text file.

&nbsp;

### Subtask 2: Choosing an instance type

  <i class="fas fa-info-circle"></i> Amazon EC2 provides a wide selection of _instance types_ that are optimized to fit different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity. They give you the flexibility to choose the appropriate mix of resources for your applications. Each instance type includes one or more _instance sizes_, which enables you to scale your resources to meet the requirements of your target workload.

  For this task, you will use a *t3.micro* instance. This instance type has 2 virtual CPUs and 1 GiB of memory. 

  **Note**: In this lab, you might be restricted from using other instance types.

28. In the Lab Details text file, note the *t3.micro* instance type.

&nbsp;
### Subtask 3: Creating a security group

In this subtask, you will create a VPC security group. 

<i class="fas fa-info-circle"></i> Recall that a _security group_ acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you associate one or more security groups with the instance. You add _rules_ to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time. The new rules are automatically applied to all instances that are associated with the security group.

Before you can create a new *security group*, you must know the VPC ID for the VPC where the group will reside. In this lab, you will use the *Lab VPC*, which was created during the lab setup process by using an AWS CloudFormation template. 

29. Using the *Bastion Host*, find the value of the *VpcId* field by running this command:

   ```bash
   aws ec2 describe-vpcs --filters Name=tag:Name,Values="Lab VPC"
   ```

   Example output:

      >{
      >
      >"Vpcs": [
      >
      >​    {
      >
      >​      **"VpcId": "vpc-09ad15ed25xxxxc"**, 
      >
      >​      "InstanceTenancy": "default", 
      >
      >​      "Tags": [
      >
      >​        {
      >
      >​          "Value": "Lab VPC", 
      >
      >​          "Key": "Name"
      >
      >​        }
      >
      >​      ], 
      >
      >​      "CidrBlockAssociationSet": [
      >
      >​        {
      >
      >​          "AssociationId": "vpc-cidr-assoc-0abc37b35e6b7e4d3", 
      >
      >​          "CidrBlock": "10.0.0.0/16", 
      >
      >​          "CidrBlockState": {
      >
      >​            "State": "associated"
      >
      >​          }
      >
      >​        }
      >
      >​      ], 
      >
      >​      "State": "available", 
      >
      >​      "DhcpOptionsId": "dopt-04541c0cef2906622", 
      >
      >​      "OwnerId": "523831018285", 
      >
      >​      "CidrBlock": "10.0.0.0/16", 
      >
      >​      "IsDefault": false
      >
      >​    }
      >
      >]
      >
      >}
      >



30. In the Lab Details text file, save the *VpcId* value. It will look similar to *vpc-09ad15ed25xxxxc*.

31. Create a new *security group*.

   Run the following command with the *VpcId* value:

   ```bash
   aws ec2 create-security-group --description "Security group for my web server" --group-name "Web Server security group" --vpc-id <VpcId>
   ```

   Example output:

    >{
    >
    >"GroupId": "sg-0a0879f902xxxxxx7"
    >
    >}
    >



32. In the Lab Details text file, save the *GroupId* value. It will be similar to *sg-08e8e4f61xxxxxxf*.



### Subtask 4: Identifying the public subnet ID

In this subtask, you will build an *AWS CLI* command that configures the EC2 instance to suit your requirements. This command must include networking settings.

The value that you assign to the `--subnet-id` parameter indicates the virtual private cloud (VPC) subnet that you will launch the instance into. You can have multiple networks, such as different ones for development, testing, and production.

For the `--subnet-id` parameter, you will use the *SubnetId* value of a public subnet in the *Lab VPC*. The AWS CloudFormation template that was used to create the *Lab VPC* includes two public subnets in two different Availability Zones.

33. Look up the *SubnetId* value for one of the public subnets. The following command searches for *Public Subnet 1*. Use the *VpcId* value that you saved previously. 

  ```bash
  aws ec2 describe-subnets --filters Name=vpc-id,Values="<VpcId>" Name=tag:Name,Values="Public Subnet 1"
  ```
  
  Example output:

    >{
    >
    >  "Subnets": [
    >
    >​    {
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "AvailabilityZoneId": "use1-az2",
    >
    >​      "AvailableIpAddressCount": 251,
    >
    >​      "CidrBlock": "10.0.1.0/24",
    >
    >​      "DefaultForAz": false,
    >
    >​      "MapPublicIpOnLaunch": true,
    >
    >​      "MapCustomerOwnedIpOnLaunch": false,
    >
    >​      "State": "available",
    >
    >​      **"SubnetId": "subnet-0218b6ec86be7560d"**,
    >
    >​      "VpcId": "vpc-034517b46f5d0a094",
    >
    >​      "OwnerId": "255879253054",
    >
    >​      "AssignIpv6AddressOnCreation": false,
    >
    >​      "Ipv6CidrBlockAssociationSet": [],
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "Name",
    >
    >​          "Value": "Public Subnet 1"
    >
    >​        },
    >
    >   ...



34. Save the *SubnetId* value of *Public Subnet 1* to your Lab Details text file.

  ​     


### Subtask 5: Preparing the user data file 

<i class="fas fa-info-circle"></i>  When you launch an instance, you can pass _user data_ to the instance. The user data can be used to perform common automated configuration tasks. It can also run scripts after the instance starts.

Your instance runs Amazon Linux, so you will provide a _shell script_ that runs when the instance launches.

35. Retrieve the user_data.txt file by running the following command.

   ```bash
   wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/lab3-cli-ec2/user_data.txt
   ```

   This file contains commands that will run on the Amazon EC2 host after it launches.

36. Review the contents of the user_data.txt file, which include these lines:

   ```bash
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello from Your Web Server!</h1></html>' > /var/www/html/index.html
   ```

   The script will:
   
   * Install an Apache web server (*httpd*)
   * Configure the web server to automatically start on launch
   * Activate the web server
   * Create a simple webpage


&nbsp;
### Subtask 6: Building the AWS CLI run-instances command

In this subtask, you will update the `run-instances` command with the following parameter settings.

- **image-id**: AMI *ImageId* value 

- **count**: `1`

  * This parameter specifies the number of instances to deploy. 

- **instance-type**: `t3.micro`

- **key-name**: `vockey`

  * <i class="fas fa-info-circle"></i> Amazon EC2 uses public–key cryptography to encrypt and decrypt login information. To log in to your instance, you must create a key pair, specify the name of the key pair when you launch the instance, and provide the private key when you connect to the instance.

- **security-group-ids**: Security group *GroupId* value 

- **subnet-id**: *SubnetId* value

- **associate-public-ip-address**

  * When this parameter is included, a public IP address will be assigned to the new instance.

- **user-data**: `file://user_data.txt`

- **Tag-specifications**: `'ResourceType=instance,Tags=[{Key=Name,Value="Web Server 1"}]'`

  * <i class="fas fa-info-circle"></i> Recall that tags enable you to categorize your AWS resources in different ways (such as by purpose, owner, or environment). Tags are useful when you have many resources of the same type&mdash;you can quickly identify a specific resource based on the tags that you assigned to it. Each tag consists of a *Key* and a *Value*, both of which you define.
  
- **disable-api-termination**

  * To protect against accidental termination, include the `--disable-api-termination` parameter.

  * <i class="fas fa-info-circle"></i> When you no longer need an EC2 instance, you can _terminate_ it. Termination means that the instance is stopped and its resources are released. A terminated instance can't be started again. If you want to prevent the instance from being accidentally terminated, you can enable _termination protection_ for the instance, which prevents it from being terminated.

37. Using the information in the Lab Details text file, update the following command with the values from your environment.

  ```bash
  aws ec2 run-instances --image-id <ami-ImageId> --count 1 \
  --instance-type t3.micro --key-name vockey \
  --security-group-ids <security-group-GroupId> \
  --subnet-id <public-subnet-1-SubnetId> --associate-public-ip-address \
  --disable-api-termination --user-data file://user_data.txt \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value="Web Server 1"}]'
  ```



### Subtask 7: Deploying the instance and reviewing the instance launch

Next, you will create a new EC2 instance by running the *AWS CLI* command that you built.

38. In the terminal of the *Bastion Host*, run the updated command.

    Example of an updated command:

    ```bash
    aws ec2 run-instances --image-id ami-0323c3dd2daxxxd --count 1 \
    --instance-type t2.micro --key-name vockey  \  
    --security-group-ids sg-0a84ea456439xxxx7 \  
    --subnet-id subnet-043fba742234xxxx8 --associate-public-ip-address  \  
    --disable-api-termination --user-data file://user_data.txt \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value="Web Server 1"}]'
    ```

39. Review the output of the command.   

    Example output: This excerpt is at the beginning of the output.

    >{
    >
    >"Groups": [],
    >
    >"Instances": [
    >
    >​    {
    >
    >​      "AmiLaunchIndex": 0,
    >
    >​      "ImageId": "ami-0323c3dd2daxxxxd",
    >
    >​      **"InstanceId": "i-03b35aedba9xxxx9b"**,
    >
    >​      "InstanceType": "t2.micro",
    >
    >​      "KeyName": "vockey",
    >
    >​      "LaunchTime": "2020-05-20T19:35:57+00:00",
    >
    >​      "Monitoring": {
    >
    >​        "State": "disabled"
    >
    >​      },
    >
    >​      "Placement": {
    >
    >​        "AvailabilityZone": "us-east-1a",
    >
    >​        "GroupName": "",
    >
    >​        "Tenancy": "default"
    >
    >​      },
    >
    >​      "PrivateDnsName": "ip-10-0-1-169.ec2.internal",
    >
    >​      "PrivateIpAddress": "10.0.1.169",
    >
    >​      "ProductCodes": [],
    >
    >​      "PublicDnsName": "",
    >
    >​      **"State"**: {
    >
    >​        "Code": 0,
    >
    >​        "Name": **"pending"**
    >
    >​      },
    >
    >​      "StateTransitionReason": "",
    >
    >​      "SubnetId": "subnet-0809fc801acxxxx6",
    >
    >​      "VpcId": "vpc-0a10eead43ddxxxxd",
    >
    >​      "Architecture": "x86_64",
    >
    >...

    
    
    
    Notice that the **State** of the instance is *pending*.
    
    The instance will first appear in a _pending_ state, which means it is being launched. It will then change to _running_, which indicates that the instance launched. After a short time, you can access the instance.
    
    The instance receives a _public DNS name_ that you can use to contact the instance from the internet. During the time that the instance state is *pending*, this value will be empty.
    
    The output of the `run-instances` command includes detailed information about your instance. Review the output. It includes information about the instance type, security settings, and network settings.
    
40. Save the *InstanceId* value of this EC2 instance.

41. If you are not automatically returned to the command prompt after scrolling through the output, quit the command by pressing Q.

42. Wait a couple of minutes, and then query your instance to see if the **State** value includes *running*.

    ```bash
    aws ec2 describe-instances --filter Name=instance-id,Values=<ec2-instance-InstanceId>
    ```
    
    Example output:

    >{
    >
    >  "Reservations": [
    >
    >​    {
    >
    >​      "Groups": [],
    >
    >​      "Instances": [
    >
    >​        {
    >
    >​          "AmiLaunchIndex": 0,
    >
    >​          "ImageId": "ami-0323c3dd2daxxxxxd",
    >
    >​          "InstanceId": "i-010030526c06xxxxx2",
    >
    >​          "InstanceType": "t2.micro",
    >
    >​          "KeyName": "vockey",
    >
    >​          "LaunchTime": "2020-05-21T18:37:50+00:00",
    >
    >​          "Monitoring": {
    >
    >​            "State": "disabled"
    >
    >​          },
    >
    >​          "Placement": {
    >
    >​            "AvailabilityZone": "us-east-1a",
    >
    >​            "GroupName": "",
    >
    >​            "Tenancy": "default"
    >
    >​          },
    >
    >​          "PrivateDnsName": "ip-10-0-1-40.ec2.internal",
    >
    >​          "PrivateIpAddress": "10.0.1.40",
    >
    >​          "ProductCodes": [],
    >
    >​          "PublicDnsName": "ec2-52-91-216-59.compute-1.amazonaws.com",
    >
    >​          "PublicIpAddress": "52.91.216.59",
    >
    >​          **"State"**: {
    >
    >​            "Code": 16,
    >
    >​            "Name": **"running"**
    >
    >​          },
    >
    >​          "StateTransitionReason": "",
    >
    >​          "SubnetId": "subnet-06677690a61xxxxx8",
    >
    >​          "VpcId": "vpc-0cdf8d5981dxxxxxd",
    >
    >​          "Architecture": "x86_64",
    >
    >​          "BlockDeviceMappings": [

&nbsp;


​		You have successfully launched your first EC2 instance.

&nbsp;

## Task 2: Monitoring your instance

Monitoring is an important part of maintaining the reliability, availability, and performance of your EC2 instances and your AWS solutions. 

43. View the *instance state* and *status checks* by querying the status of your instance. Use the following command, updated with the *InstanceID* value of the new EC2 instance:

    ```bash
    aws ec2 describe-instance-status --instance-ids <ec2-instance-InstanceId>    
    ```
    
    Example output:

    >{
    >
    >"InstanceStatuses": [
    >
    >​    {
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "InstanceId": "i-03b35aedba966c19b",
    >
    >​      **"InstanceState"**: {
    >
    >​        "Code": 16,
    >
    >​        "Name": **"running"**
    >
    >​      },
    >
    >​      **"InstanceStatus"**: {
    >
    >​        "Details": [
    >
    >​          {
    >
    >​            "Name": "reachability",
    >
    >​            "Status": **"passed"**
    >
    >​          }
    >
    >​        ],
    >
    >​        "Status": "ok"
    >
    >​      },
    >
    >​      **"SystemStatus"**: {
    >
    >​        "Details": [
    >
    >​          {
    >
    >​            "Name": "reachability",
    >
    >​            "Status": **"passed"**
    >
    >​          }
    >
    >​        ],
    >
    >​        "Status": "ok"
    >
    >​      }
    >
    >​    }
    >
    >]
    >
    >}



Before you continue, confirm that the instance is running and healthy by checking the following values.

* **InstanceState:** *running*

* **InstanceStatus:** *passed*

* **SystemStatus:** *passed*

<i class="fas fa-info-circle"></i> By monitoring the instance status, you can determine whether Amazon EC2 detected any problems that could prevent your instances from running applications. Amazon EC2 performs automated checks on every running EC2 instance to identify hardware and software issues.

44. To view the types of metrics that are collected for an EC2 instance, run the following command.

    ```bash
    aws cloudwatch list-metrics --namespace "AWS/EC2"  --dimensions Name=InstanceId,Value=<ec2-instance-InstanceId> 
    ```
    
    Example output: This example is an excerpt of the output.

    >{
    >
    >"Metrics": [
    >
    >​    {
    >
    >​      "Namespace": "AWS/EC2",
    >
    >​      "MetricName": **"StatusCheckFailed_System"**,
    >
    >​      "Dimensions": [
    >
    >​        {
    >
    >​          "Name": "InstanceId",
    >
    >​          "Value": "i-03b35aedba9xxxxxb"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "Namespace": "AWS/EC2",
    >
    >​      "MetricName": **"StatusCheckFailed_Instance"**,
    >
    >​      "Dimensions": [
    >
    >​        {
    >
    >​          "Name": "InstanceId",
    >
    >​          "Value": "i-03b35aedba9xxxxxb"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "Namespace": "AWS/EC2",
    >
    >​      "MetricName": **"StatusCheckFailed"**,
    >
    >​      "Dimensions": [
    >
    >​        {
    >
    >​          "Name": "InstanceId",
    >
    >​          "Value": "i-03b35aedba9xxxxxb"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "Namespace": "AWS/EC2",
    >
    >​      "MetricName": **"NetworkPacketsIn"**,
    >
    >​      "Dimensions": [
    >
    >​        {
    >
    >​          "Name": "InstanceId",
    >
    >​          "Value": "i-03b35aedba9xxxxxb"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "Namespace": "AWS/EC2",
    >
    >​      "MetricName": **"NetworkPacketsOut"**,
    >
    >​      "Dimensions": [
    >
    >​        {
    >
    >​          "Name": "InstanceId",
    >
    >​          "Value": "i-03b35aedba9xxxxxb"
    >
    >​        }
    >
    >...
    
    
    
    This command displays the Amazon CloudWatch metrics that are available for your instance. 
    
    You can query a single metric to find details that are specific to your instance's health or performance.
    
      
    
45. To see the CPU utilization of your instance, run the following command.

    ```bash
    aws cloudwatch get-metric-statistics --metric-name CPUUtilization --start-time 2014-04-08T23:18:00Z --end-time 2014-04-09T23:18:00Z --period 3600 --namespace AWS/EC2 --statistics Maximum --dimensions Name=InstanceId,Value=<ec2-instance-InstanceId>    
    ```
    
    Example output:

    >{
    >
    >  "Label": "DiskReadOps",
    >
    >  "Datapoints": []
    >
    >}
    
    
    
    The metric might not have any data to report yet. This situation is normal because the instance has not been running very long.
    
    <i class="fas fa-info-circle"></i> Amazon EC2 sends metrics to Amazon CloudWatch for your EC2 instances. Basic (5-minute) monitoring is enabled by default. You can enable detailed (1-minute) monitoring.
    
    Sometimes, you need to know what happened in the console of an EC2 instance. To find this information, you must access the *System Log*.
    
    The System Log displays the console output of the instance, which is a valuable tool for problem diagnosis. It is especially useful for troubleshooting kernel problems and service configuration issues that could cause an instance to terminate or become unreachable before its Secure Shell (SSH) daemon can start. 
    
    
    
46. To view the System Log for your instance, use the following command.

    ```bash
    aws ec2 get-console-output --instance-id <ec2-instance-InstanceId> --latest --output text
    ```
    
    Example output: The output in the System Log varies. You can page through the output by pressing the SPACE BAR.

    >[ OK ] Started amazon-ssm-agent.^M
    >
    >​     Starting amazon-ssm-agent...^M
    >
    >​     Starting Notify NFS peers of a restart...^M
    >
    >​     Starting Permit User Sessions...^M
    >
    >[ OK ] Started Notify NFS peers of a restart.^M
    >
    >[ OK ] Started Permit User Sessions.^M
    >
    >​     Starting Wait for Plymouth Boot Screen to Quit...^M
    >
    >[ OK ] Started Command Scheduler.^M
    >
    >​     Starting Command Scheduler...^M
    >
    >​     Starting Terminate Plymouth Boot Screen...^M
    >
    >[ OK ] Started Job spooling tools.^M
    >
    >​     Starting Job spooling tools...^M
    >
    >[ OK ] Started System Logging Service.^M
    >
    >[  8.199563] cloud-init[2424]: Cloud-init v. 19.3-2.amzn2 running 'modules:config' at Fri, 29 May 2020 15:12:53 +0000. Up 8.11 seconds.
    >
    >[  9.003319] cloud-init[2424]: Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
    >
    >[  9.019518] cloud-init[2424]: Existing lock /var/run/yum.pid: another copy is running as pid 2565.
    >
    >[  9.023274] cloud-init[2424]: Another app is currently holding the yum lock; waiting for it to exit...
    >
    >[  9.027483] cloud-init[2424]: The other application is: yum
    >
    >[  9.031730] cloud-init[2424]: Memory : 31 M RSS (321 MB VSZ)
    >
    >[  9.034065] cloud-init[2424]: Started: Fri May 29 15:12:53 2020 - 00:01 ago
    >
    >[  9.036734] cloud-init[2424]: State : Running, pid: 2565
    >
    >[  11.025199] cloud-init[2424]: Another app is currently holding the yum lock; waiting for it to exit...
    >
    >[  11.029007] cloud-init[2424]: The other application is: yum



​		You have now explored several ways to monitor your instance.
&nbsp;


## Task 3: Updating your security group and accessing the web server

When you launched the EC2 instance, you provided a user data script that installed a web server and created a simple webpage. In this task, you will access content from the web server.

47. To retrieve details about the web server instance, enter the following command in the terminal. 

    ```bash
    aws ec2 describe-instances --filter Name=instance-id,Values=<ec2-instance-InstanceId>
    ```
    
    Example output:

    >{
    >
    >  "Reservations": [
    >
    >​    {
    >
    >​      "Groups": [],
    >
    >​      "Instances": [
    >
    >​        {
    >
    >​          "AmiLaunchIndex": 0,
    >
    >​          "ImageId": "ami-0323c3dd2daxxxxxd",
    >
    >​          "InstanceId": "i-010030526c0xxxxx2",
    >
    >​          "InstanceType": "t2.micro",
    >
    >​          "KeyName": "vockey",
    >
    >​          "LaunchTime": "2020-05-21T18:37:50+00:00",
    >
    >​          "Monitoring": {
    >
    >​            "State": "disabled"
    >
    >​          },
    >
    >​          "Placement": {
    >
    >​            "AvailabilityZone": "us-east-1a",
    >
    >​            "GroupName": "",
    >
    >​            "Tenancy": "default"
    >
    >​          },
    >
    >​          "PrivateDnsName": "ip-10-0-1-40.ec2.internal",
    >
    >​          "PrivateIpAddress": "10.0.1.40",
    >
    >​          "ProductCodes": [],
    >
    >​          "PublicDnsName": "ec2-52-91-216-59.compute-1.amazonaws.com",
    >
    >​          **"PublicIpAddress": "52.91.216.59"**,
    >
    >​          "State": {
    >
    >​            "Code": 16,
    >
    >​            "Name": "running"
    >
    >​          },
    
    
    
    This output should look familiar. This time, you must find the public IP address of your instance.
    
    
    
48. Find the *PublicIpAddress* value and save it to the Lab Details text file.


49. Open a new tab in your web browser, copy the *PublicIpAddress* value, paste it into the browser, and press ENTER.

    **Question:** Can you access your web server? Why not?

    **Answer:** You *can't access* your web server. The _security group_ doesn't permit inbound traffic on port 80, which is used for HTTP web requests. This behavior demonstrates the use of a security group as a firewall to restrict network traffic that's allowed in and out of an instance.

    To correct this situation, you will now update the security group to permit web traffic on port 80.

50. Add an _ingress rule_ to the **Web Server security group** security group.

    Recall that _ingress rules_ control traffic that enters a VPC. They allow requests to access designated ports by using the specified protocols. The requests are also filtered by the source IP address through the `--cidr` option.

    Update the following command setting by using these options.

    - **group-id:** Security group *GroupId* that you saved previously
    - **protocol:** `tcp`
    - **port:** `80`
    - **cidr:** `0.0.0.0/0`

    ```bash
    aws ec2 authorize-security-group-ingress --group-id <security-group-GroupId> --protocol tcp --port 80 --cidr 0.0.0.0/0
    ```


51. Return to the web server tab that you previously opened, and refresh <i class="fas fa-sync"></i> the page.

    You should find the messageL _Hello from Your Web Server!_
    
    You have now successfully modified your security group to permit HTTP traffic into your EC2 instance.


&nbsp;

## Task 4: Resizing your instance – Instance type and EBS volume

As your needs change, your instance might be overutilized (too small) or underutilized (too large)—or you might need a different amount of Amazon Elastic Block Store (Amazon EBS) storage. If so, you can change the _instance type_ or change the size of a disk. For example, if a _t2.micro_ instance is too small for its workload, you can change it to an _m5.medium_ instance. 



### Subtask 1: Stopping your instance

Before you can resize an instance, you must _stop_ it.

<i class="fas fa-info-circle"></i> When you stop an instance, it shuts down. A stopped EC2 instance incurs no charges. However, any attached EBS volumes storage will continue to incur charges.

52. In your terminal window, enter the following command.

    ```bash
    aws ec2 stop-instances --instance-id <ec2-instance-InstanceId>
    ```
    
    Example output:

    >{
    >
    >"StoppingInstances": [
    >
    >​    {
    >
    >​      **"CurrentState"**: {
    >
    >​        "Code": 64,
    >
    >​        "Name": **"stopping"**
    >
    >​      },
    >
    >​      "InstanceId": "i-010030526c064ff62",
    >
    >​      "PreviousState": {
    >
    >​        "Code": 16,
    >
    >​        "Name": "running"
    >
    >​      }
    >
    >​    }
    >
    >]
    >
    >}
    >



53. Wait a couple of minutes and describe the instance again.

    ```bash
    aws ec2 describe-instances --filter Name=instance-id,Values=<ec2-instance-InstanceId>
    ```
    
    Example output:

    >{
    >
    >"Reservations": [
    >
    >​    {
    >
    >​      "Groups": [],
    >
    >​      "Instances": [
    >
    >​        {
    >
    >​          "AmiLaunchIndex": 0,
    >
    >​          "ImageId": "ami-0323c3dd2da7fb37d",
    >
    >​          "InstanceId": "i-010030526c064ff62",
    >
    >​          "InstanceType": "t2.micro",
    >
    >​          "KeyName": "vockey",
    >
    >​          "LaunchTime": "2020-05-21T18:37:50+00:00",
    >
    >​          "Monitoring": {
    >
    >​            "State": "disabled"
    >
    >​          },
    >
    >​          "Placement": {
    >
    >​            "AvailabilityZone": "us-east-1a",
    >
    >​            "GroupName": "",
    >
    >​            "Tenancy": "default"
    >
    >​          },
    >
    >​          "PrivateDnsName": "ip-10-0-1-40.ec2.internal",
    >
    >​          "PrivateIpAddress": "10.0.1.40",
    >
    >​          "ProductCodes": [],
    >
    >​          "PublicDnsName": "",
    >
    >​          **"State"**: {
    >
    >​            "Code": 80,
    >
    >​            "Name": **"stopped"**
    >
    >​          },
    >
    >​          "StateTransitionReason": "User initiated (2020-05-21 19:48:53 GMT)",
    >



54. Wait for the instance **State** field to include the value: *stopped*.



### Subtask 2: Changing the instance type

55. In the terminal window, change the instance type by running the following command.

    ```bash
    aws ec2 modify-instance-attribute \
        --instance-id <ec2-instance-InstanceId> \
        --instance-type "{\"Value\": \"t2.small\"}"
    ```
    
    When the instance is started again, it will be of the _t2.small_ type, which has twice as much memory as a _t2.micro_ instance. 
    
    **Note**: In this lab, you might be restricted from using other instance types.




### Subtask 3: Resizing the EBS volume

To resize the EBS volume through the AWS CLI, you must know the volume ID of the volume that's associated with your instance.

56. Describe your instance again. However, this time, locate the *VolumeId* value. It will be in the **BlockDeviceMappings** section of the output.

    Example output:

    >{
    >
    >"Reservations": [
    >
    >​    {
    >
    >​      "Groups": [],
    >
    >​      "Instances": [
    >
    >​        {
    >
    >​          "AmiLaunchIndex": 0,
    >
    >​          "ImageId": "ami-0323c3dd2da7fb37d",
    >
    >​          "InstanceId": "i-0983c6ecd2fc0bf05",
    >
    >​          "InstanceType": "t2.small",
    >
    >​          "KeyName": "vockey",
    >
    >​          "LaunchTime": "2020-05-21T22:02:09+00:00",
    >
    >​          "Monitoring": {
    >
    >​            "State": "disabled"
    >
    >​          },
    >
    >​          "Placement": {
    >
    >​            "AvailabilityZone": "us-east-1a",
    >
    >​            "GroupName": "",
    >
    >​            "Tenancy": "default"
    >
    >​          },
    >
    >​          "PrivateDnsName": "ip-10-0-1-162.ec2.internal",
    >
    >​          "PrivateIpAddress": "10.0.1.162",
    >
    >​          "ProductCodes": [],
    >
    >​          "PublicDnsName": "",
    >
    >​          "State": {
    >
    >​            "Code": 80,
    >
    >​            "Name": "stopped"
    >
    >​          },
    >
    >​          "StateTransitionReason": "User initiated (2020-05-21 22:19:44 GMT)",
    >
    >​          "SubnetId": "subnet-0be589967f2b72d9d",
    >
    >​          "VpcId": "vpc-07dc9f6f1068a6f23",
    >
    >​          "Architecture": "x86_64",
    >
    >​          "BlockDeviceMappings": [
    >
    >​            {
    >
    >​              "DeviceName": "/dev/xvda",
    >
    >​              "Ebs": {
    >
    >​                "AttachTime": "2020-05-21T22:02:10+00:00",
    >
    >​                "DeleteOnTermination": true,
    >
    >​                "Status": "attached",
    >
    >​                **"VolumeId": "vol-0756a637688xxxxx3"**
    >
    >​              }
    >
    >​            }
    >
    >​          ],
    >



57. Save the *VolumeId* value. It will be similar to *vol-0756a637688xxxxx3*.

    The disk volume currently has a size of *8 GiB*. Next, you will increase the size of this disk.

58. Resize the volume to **10 GiB** by running the following command. 

    **Note**: In this lab, you might be restricted from creating large EBS volumes.

    ```bash
    aws ec2 modify-volume --size 10 --volume-id  <ebs-volume-VolumeId>
    ```
    
    Example output:

    >{
    >
    >  "VolumeModification": {
    >
    >​    "VolumeId": "vol-0756a637688480383",
    >
    >​    "ModificationState": "modifying",
    >
    >​    "TargetSize": 10,
    >
    >​    "TargetIops": 100,
    >
    >​    "TargetVolumeType": "gp2",
    >
    >​    "OriginalSize": 8,
    >
    >​    "OriginalIops": 100,
    >
    >​    "OriginalVolumeType": "gp2",
    >
    >​    "Progress": 0,
    >
    >​    "StartTime": "2020-05-21T22:50:52+00:00"
    >
    >  }
    >
    >}



### Subtask 4: Starting the resized instance

You will now start the web server instance again.

59. In your terminal, run the following command:

    ```bash
    aws ec2 start-instances --instance-id <ec2-instance-InstanceId>
    ```
    
    If you like, you can describe your ec2 instance to verify that it is using the new instnace size. You can also describe the volume to make sure that it is now 10GiB.

&nbsp;


## Task 5: Exploring the maximum number of EC2 instances

Amazon EC2 provides different resources that you can use. These resources include images, instances, volumes, and snapshots. When you create an AWS account, resources have default maximum numbers that are defined on a per-Region basis.

60. In the terminal, run the following command.

    ```bash
    aws ec2 describe-account-attributes
    ```
    
    Example output:

    >{
    >
    >  "AccountAttributes": [
    >
    >​    {
    >
    >​      "AttributeName": "supported-platforms",
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": "VPC"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "AttributeName": "vpc-max-security-groups-per-interface",
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": "5"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "AttributeName": "max-elastic-ips",
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": "5"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "AttributeName": **"max-instances"**,
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": **"20"**
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "AttributeName": "vpc-max-elastic-ips",
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": "5"
    >
    >​        }
    >
    >​      ]
    >
    >​    },
    >
    >​    {
    >
    >​      "AttributeName": "default-vpc",
    >
    >​      "AttributeValues": [
    >
    >​        {
    >
    >​          "AttributeValue": "vpc-03a4b979"
    >
    >​        }
    >
    >​      ]
    >
    >​    }
    
    
    
    Note that the attributes list a maximum number of instances that you can launch in this Region (**max-instances**). When you launch an instance, the request shouldn't cause your usage to reach the current maximum number of instances that are defined in the Region.
    
    You can request an increase beyond the maximum number for many AWS resources.
    &nbsp;

## Task 6: Testing termination protection

You can delete your instance when you no longer need it. This action is referred to as _terminating_ your instance. You can't connect to or restart an instance after you terminate it.

In this task, you will learn how to use _termination protection_.

61. To test termination protection for your instance, run the following command.

    ```bash
    aws ec2 terminate-instances --instance-id <ec2-instance-InstanceId>
    ```
    
    Example output:

    >An error occurred (OperationNotPermitted) when calling the TerminateInstances operation: The instance 'i-09a480015b2743b17' may not be terminated. Modify its 'disableApiTermination' instance attribute and try again.
    
    Note that an error message indicates that the `terminate-instances operation` is not permitted.
    
    This feature is a safeguard that helps prevent the accidental termination of an instance. If you truly want to terminate the instance, you must disable termination protection.
    
62. To disable termination protection, run the following command.

    ```bash
    aws ec2 modify-instance-attribute --no-disable-api-termination --instance-id <ec2-instance-InstanceId>
    ```

63. Now, try the `terminate-instances` command again. 

    ```bash
    aws ec2 terminate-instances --instance-id <ec2-instance-InstanceId>
    ```
    
    Example output: This time, you should receive the following output.

    >{
    >
    >  "TerminatingInstances": [
    >
    >​    {
    >
    >​      "CurrentState": {
    >
    >​        "Code": 32,
    >
    >​        "Name": "shutting-down"
    >
    >​      },
    >
    >​      "InstanceId": "i-09a480015b2743b17",
    >
    >​      "PreviousState": {
    >
    >​        "Code": 16,
    >
    >​        "Name": "running"
    >
    >​      }
    >
    >​    }
    >
    >  ]
    >
    >}
    
    
    
    The output should show that the instance is terminating and the value of the **CurrentState** field includes *shutting-down*.
    
    You have now successfully tested termination protection and terminated your instance.

&nbsp;

## Additional resources

* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html" target="_blank">Launch Your Instance</a>
* <a href="https://aws.amazon.com/ec2/instance-types" target="_blank">Amazon EC2 Instance Types</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html" target="_blank">Amazon Machine Images (AMI)</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html" target="_blank">Amazon EC2 - User Data and Shell Scripts</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html" target="_blank">Amazon EC2 Root Device Volume</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html" target="_blank">Tagging Your Amazon EC2 Resources</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html" target="_blank">Security Groups</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html" target="_blank">Amazon EC2 Key Pairs</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html?icmpid=docs_ec2_console" target="_blank">Status Checks for Your Instances</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html" target="_blank">Getting Console Output and Rebooting Instances</a>
* <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html" target="_blank">Amazon EC2 Metrics and Dimensions</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html" target="_blank">Resizing Your Instance</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html" target="_blank">Stop and Start Your Instance</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html" target="_blank">Amazon EC2 Service Limits</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Terminate Your Instance</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Termination Protection for an Instance</a>

&nbsp;

## Lab complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

64. To confirm that you want to end the lab, at the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then choose <span id="ssb_blue">Yes</span>

   A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*
   ​
65. To close the panel, choose the **X** in the top-right corner.


### Contact us

[Report an error or correction.](mailto:aws-course-feedback@amazon.com)

For other questions, contact us through the [Contact AWS Training and Certification page.](https://aws.amazon.com/contact-us/aws-training)

*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*