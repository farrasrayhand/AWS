# Lab 5: Building Your DB Server and Interacting with Your DB by Using an Application

<!-- Note to translators: This activity is unique to this course. -->

&nbsp;&nbsp;

This lab is designed to reinforce the concept of using an AWS managed database (DB) instance for solving relational database needs.

With *Amazon Relational Database Service (Amazon RDS)*, you can quickly set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which enables you to focus on your applications and business. Amazon RDS provides you with six familiar database engines to choose from: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL, and MariaDB.

This lab is based on the AWS Command Line Interface (AWS). Feel free to explore the [AWS CLI documentation](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html) for Amazon Elastic Compute Cloud (Amazon EC2) and Amazon RDS as you work through the lab.

&nbsp;

**Objectives**

After completing this lab, you should be able to:

- Launch an Amazon RDS DB instance with high availability.
- Configure the DB instance to permit connections from your web server.
- Open a web application and interact with your database.

&nbsp;

**Duration**

This lab takes approximately **30 minutes**.

&nbsp;

**Scenario**

You start with the following infrastructure:

<img src="images/lab-5-starting-lab-architecture.png" width="800">

At the end of the lab, you will have built the following infrastructure:

<img src="images/lab-5-final-lab-architecture.png" width="800">

## Launching your lab environment

1. At the top of these instructions, launch this lab by choosing <span id="ssb_voc_grey">Start Lab</span> 

   A **Start Lab** panel opens, and it displays the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by selecting the **X**.

   This lab launches an EC2 instance that's named *Bastion Host*. You will use this server to run AWS CLI commands to create lab resources.

3. Above these instructions, select the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>
  
    Copy all the lab details—such as *BastionHost*, *UserAccessSecretKey*, *Region*, and others—to a text file and save it as `Lab Details.txt`. 

    

**Note**: Make sure that you use a *text editor* (a program for editing text files) to save the file. Examples of text editors include [Atom](https://atom.io/), [Sublime Text](https://www.sublimetext.com/), or [Microsoft Visual Studio Code](https://code.visualstudio.com/).
    
In the lab, the information that you saved will be referred to as <u>*Lab Details*</u>.

&nbsp;

## Using SSH to connect to the Bastion Host

### Microsoft Windows users

<i class="fas fa-comment"></i> These instructions are specifically for Microsoft Windows users. If you are using macOS or Linux, <a href="#ssh-MACLinux">skip to the next section</a>.
​

4. Above these instructions that you are currently reading, choose the <span id="ssb_voc_grey">Details</span> dropdown menu and then choose <span id="ssb_voc_grey">Show</span>
  
   A **Credentials** window opens.
   
5. Choose the **Download PPK** button and save the **labsuser.ppk** file.

    Typically, your browser will save the file to the **Downloads** directory.

6. Close the **Credentials** window by choosing the **X**.

7. From the Lab Details text file, note the IP address for the **Bastion Host**. 

8. Download **PuTTY** so that you can use SSH to connect to the EC2 instance. If you don't have PuTTY installed on your computer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download it here</a>.

9.  Open **putty.exe**

10. Configure PuTTY timeout to keep the PuTTY session open for a longer period of time.
    * Choose **Connection**
    * Set **Seconds between keepalives** to `30`

11. Configure your PuTTY session:
    * Choose **Session**
    * **Host Name (or IP address):** Paste the *Public DNS or IPv4 address* of the *Bastion Host* instance that you noted earlier. 
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

18. From the Lab Details text file, note the IP address for the **Bastion Host**. 

19. Open a terminal window, and change directory (by using `cd`) to the directory where the *labsuser.pem* file was downloaded.
  
    For example, if the *labsuser.pem* file was saved to your **Downloads** directory, run this command:

    ```bash
    cd ~/Downloads
    ```

20. Change the permissions on the key to be read-only by running this command:

    ```bash
    chmod 400 labsuser.pem
    ```

21. Run the following command (replace &lt;*bastion-ip-address*&gt; with the *BastionHost* address that you copied earlier). 

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
  
  


## Task 1: Creating a security group for the RDS DB instance

In this task, you will create a security group that allows your web server to access your RDS DB instance. The security group will be used when you launch the database instance.

25. You must look up the ID for the *Lab virtual private cloud (VPC)*. In your SSH session, enter the following command:

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
   >​      "CidrBlock": "10.0.0.0/16",
   >
   >​      "DhcpOptionsId": "dopt-3c4ab246",
   >
   >​      "State": "available",
   >
   >​      **"VpcId": "vpc-0723ed5a5aexxxxxx"**,
   >
   >​      "OwnerId": "135572168615",
   >
   >​      "InstanceTenancy": "default",
   >
   >​      "CidrBlockAssociationSet": [
   >
   >​        {
   >
   >​          "AssociationId": "vpc-cidr-assoc-0351a7dd6702971a8",
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

26. Note the **VpcId** value and save it your Lab Details.txt file. It will be similar to *vpc-0723ed5a5aexxxxxx*.

27. In the *Lab VPC*, create a security group. Update the following command with the **VpcId** value:

   Notice that you give this security group the following **Name** and **Description**:

   - **Security group name:** `DB Security Group`
   - **Description:** `Permit access from Web Security Group`

   ```bash
   aws ec2 create-security-group \
   --description 'Permit access from Web Security Group' \
   --group-name 'DB Security Group' --vpc-id <lab-vpc-VpcId>
   ```

   Example output:
   
   >{
   >
   >"GroupId": "sg-07bd350e508xxxxxx"
   >
   >}

28. Note the **GroupID** value for the *DB Security Group* and save it to your Lab Details text file. It will be similar to *sg-07bd350e508xxxxxx*.

    You will now add a rule to the security group to permit inbound database requests. The security group currently has no rules. You will add a rule to permit access from the _Web Security Group_.

29. When you add the rule, you will need to use the *GroupID* value of the *Web Security Group*. To look up the *GroupID* value, enter the following command:

    ```bash
    aws ec2 describe-security-groups --filters Name=tag:Name,Values="Web Security Group"
    ```

    Example output:
    
    >{
    >
    >"SecurityGroups": [
    >
    >​    {
    >
    >​      "Description": "Enable HTTP access",
    >
    >​      "GroupName": "Web Security Group",
    >
    >​      "IpPermissions": [
    >
    >​        {
    >
    >​          "FromPort": 80,
    >
    >​          "IpProtocol": "tcp",
    >
    >​          "IpRanges": [
    >
    >​            {
    >
    >​              "CidrIp": "0.0.0.0/0"
    >
    >​            }
    >
    >​          ],
    >
    >​          "Ipv6Ranges": [],
    >
    >​          "PrefixListIds": [],
    >
    >​          "ToPort": 80,
    >
    >​          "UserIdGroupPairs": []
    >
    >​        },
    >
    >​        {
    >
    >​          "FromPort": 22,
    >
    >​          "IpProtocol": "tcp",
    >
    >​          "IpRanges": [
    >
    >​            {
    >
    >​              "CidrIp": "0.0.0.0/0"
    >
    >​            }
    >
    >​          ],
    >
    >​          "Ipv6Ranges": [],
    >
    >​          "PrefixListIds": [],
    >
    >​          "ToPort": 22,
    >
    >​          "UserIdGroupPairs": []
    >
    >​        }
    >
    >​      ],
    >
    >​      "OwnerId": "135572168615",
    >
    >​      **"GroupId": "sg-0c4e127d3daxxxxx"**,
    >
    >​      "IpPermissionsEgress": [
    >
    >​        {
    >
    >​          "IpProtocol": "-1",
    >
    >​          "IpRanges": [
    >
    >​            {
    >
    >​              "CidrIp": "0.0.0.0/0"
    >
    >​            }
    >
    >​          ],
    >
    >​          "Ipv6Ranges": [],
    >
    >​          "PrefixListIds": [],
    >
    >​          "UserIdGroupPairs": []
    >
    >​        }
    >
    >​      ],

30. Note the **GroupID** value for the *Web Security Group* and save it to your Lab Details text file.

    You will now create an inbound traffic (ingress) rule to allow access to the database port.

31. Update the following command by setting the options :to

    - **group-id:** *GroupId* value for the *DB Security Group*
    - **protocol:** `tcp`
    - **port:** `3306`
    - **source-group:** *GroupId* value for the *Web Security Group*

    ```
    aws ec2 authorize-security-group-ingress --group-id <db-security-group-GroupId> \
    --protocol tcp --port 3306 --source-group <web-security-group-GroupID>
    ```

    This command configures the *DB Security Group* to permit inbound traffic on port 3306 (the MySQL port), from any EC2 instance that's associated with the _Web Security Group_.
    
    You will use the *DB Security Group* when you create the database in the next task.

&nbsp;
## Task 2: Create a DB Subnet Group

In this task, you will create a _DB subnet group_ that is used to tell Amazon RDS which subnets can be used for the database. Each DB subnet group requires subnets in at least two Availability Zones.

To create the DB subnet group through the command line, you must enter the subnet ID values for the private subnets. These private subnets were created as part of the *Lab VPC* when you launched the lab environment.

32. To look up the *SubnetId* value for *Private Subnet 1*, use the following command (replace the placeholder with the *VpcId* value for the *Lab VPC*): 

    ```bash
    aws ec2 describe-subnets --filters Name=cidr-block,Values=10.0.1.0/24 Name=vpc-id,Values=<lab-vpc-VpcId>
    ```
    
    Example output:

    >{
    >
    >"Subnets": [
    >
    >​    {
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "AvailabilityZoneId": "use1-az6",
    >
    >​      "AvailableIpAddressCount": 251,
    >
    >​      "CidrBlock": "10.0.1.0/24",
    >
    >​      "DefaultForAz": false,
    >
    >​      "MapPublicIpOnLaunch": false,
    >
    >​      "MapCustomerOwnedIpOnLaunch": false,
    >
    >​      "State": "available",
    >
    >​      **"SubnetId": "subnet-0a694c7ed69xxxxx"**,
    >
    >​      "VpcId": "vpc-0723ed5a5ae05ca0a",
    >
    >​      "OwnerId": "135572168615",
    >
    >​      "AssignIpv6AddressOnCreation": false,
    >
    >​      "Ipv6CidrBlockAssociationSet": [],
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "aws:cloudformation:stack-name",
    >
    >​          "Value": "acf-cli-lab5-rds"
    >
    >​        },...


33. Note the **SubnetId** value for *Private Subnet 1*. It will be similar to *subnet-0a694c7ed69xxxxxx*.

34. To find the *SubnetId* value for *Private Subnet 2*, replace the **cidr-block** value in the *describe-subnet* command and run the command:

    ```bash
    aws ec2 describe-subnets --filters Name=cidr-block,Values=10.0.3.0/24 Name=vpc-id,Values=<lab-vpc-VpcId>
    ```

	Example output:
	
	>{
	>
	>"Subnets": [
	>
	>​    {
	>
	>​      "AvailabilityZone": "us-east-1b",
	>
	>​      "AvailabilityZoneId": "use1-az1",
	>
	>​      "AvailableIpAddressCount": 251,
	>
	>​      "CidrBlock": "10.0.3.0/24",
	>
	>​      "DefaultForAz": false,
	>
	>​      "MapPublicIpOnLaunch": false,
	>
	>​      "MapCustomerOwnedIpOnLaunch": false,
	>
	>​      "State": "available",
	>
	>​      **"SubnetId": "subnet-054bdcb60c6xxxxx",**
	>
	>​      "VpcId": "vpc-0723ed5a5ae05ca0a",
	>
	>​      "OwnerId": "135572168615",
	>
	>​      "AssignIpv6AddressOnCreation": false,
	>
	>​      "Ipv6CidrBlockAssociationSet": [],
	>
	>​      "Tags": [
	>
	>​        {
	>
	>​          "Key": "aws:cloudformation:stack-name",
	>
	>​          "Value": "acf-cli-lab5-rds"
	>
	>​        },...

35. Note the **SubnetId** value for *Private Subnet 2*. It will be similar to *subnet-0a694c7ed69xxxxxx*.

36. Create the *DB-Subnet-Group* by updating the parameters in the command as follows:

    - **db-subnet-group-name:** `DB-Subnet-Group`
    - **db-subnet-group-description:** `DB Subnet Group`
    - **subnet-ids:** Update the list with the **SubnetId** values for *Private Subnet 1* and *Private Subnet 2*
    
    ```bash
    aws rds create-db-subnet-group \
    --db-subnet-group-name 'DB-Subnet-Group' \
    --db-subnet-group-description 'DB Subnet Group' \
    --subnet-ids '["<private-subnet-1-SubnetId>","<private-subnet-2-SubnetId>"]'
    ```

	Example output:
	
	>"{
	>"DBSubnetGroup": {
	>
	>​    **"DBSubnetGroupName": "db-subnet-group"**,
	>
	>​    "DBSubnetGroupDescription": "DB Subnet Group",
	>
	>​    "VpcId": "vpc-0723ed5a5ae05ca0a",
	>
	>​    "SubnetGroupStatus": "Complete",
	>
	>​    "Subnets": [
	>
	>​      {
	>
	>​        "SubnetIdentifier": "subnet-054bdcb60c6abb336",
	>
	>​        "SubnetAvailabilityZone": {
	>
	>​          "Name": "us-east-1b"
	>
	>​        },
	>
	>​        "SubnetOutpost": {},
	>
	>​        "SubnetStatus": "Active"
	>
	>​      },
	>
	>​      {
	>
	>​        "SubnetIdentifier": "subnet-0a694c7ed691d2fee",
	>
	>​        "SubnetAvailabilityZone": {
	>
	>​          "Name": "us-east-1a"
	>
	>​        },
	>
	>​        "SubnetOutpost": {},
	>
	>​        "SubnetStatus": "Active"
	>
	>​      }
	>
	>​    ],
	>
	>​    "DBSubnetGroupArn": "arn:aws:rds:us-east-1:135572168615:subgrp:db-subnet-group"
	>
	>  }
	>
	>}

37. Note the value of the **DBSubnetGroupName**, which is *db-subnet-group*, and save it to the Lab Details text file.

    You will use this DB subnet group when you create the database in the next task.

&nbsp;
___
## Task 3: Creating an RDS DB instance

In this task, you will configure and launch an RDS for MySQL DB instance in two different Availability Zones.

***Multi-AZ deployments*** in Amazon RDS provide enhanced availability and durability for DB instances. Multi-AZ DB instances work well for production database workloads. When you provision a Multi-AZ DB instance, Amazon RDS automatically creates a primary DB instance. Amazon RDS also replicates the data synchronously to a standby instance in a different Availability Zone.

38. Configure your database by using the following command. Be sure to update *vpc-security-group-ids* with the ID of the *DB Security Group* you created in Task 1.:

    **db-instance-identifier:** This identifier will be used for the database. This parameter is stored as a lowercase string.

    **db-name:** The meaning of this variable can change based on the engine type that's used. For MySQL, this parameter defines the name of the database to create when the DB instance is created. If this parameter isn’t specified, a database is not created in the DB instance.

    **db-instance-class:** This parameter indicates the compute and memory capacity of the DB instance (for example, *db.m4.large*). For the full list of DB instance classes, and availability for your engine, see [DB Instance Class](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html) in the *Amazon RDS User Guide.*
    
    **engine:** This parameter indicates the name of the database engine to use for this instance. For this lab, you will use *mysql*.
    
    **master-username:** This parameter indicates the name for the main user.
    
    **master-user-password:** This parameter indicates the password for the main user. The password can include any printable ASCII character except the slash (/), quotation marks (" "), or the at sign (@).

    **allocated-storage:** This parameter indicates the amount of storage (in gibibytes, or GiB) to allocate for the DB instance.
    
    **storage-type:** This parameter specifies the storage type to associate with the DB instance. Valid values include *standard*, *gp2*, and *io1*.

    **multi-az/no-multi-az:** Using one of these parameters indicates whether the DB instance is in a Multi-AZ deployment.  
    
    **vpc-security-group-ids:** This parameter lists the VPC security groups for Amazon EC2 to associate with this DB instance.

    **db-subnet-group-name:** This parameter indicates the DB subnet group to associate with this DB instance.

    **backup-retention-period:** This parameter indicates the number of days that automated backups are retained for. Setting this parameter to a positive number enables backups. Setting this parameter to 0 disables automated backups.

	```bash
    aws rds create-db-instance \
    --db-instance-identifier lab-db --db-name lab \
    --db-instance-class db.t3.micro --engine mysql \
    --master-username main --master-user-password lab-password \
    --allocated-storage 20 --storage-type gp2 --multi-az \
    --vpc-security-group-ids <db-security-group-GroupId> \
    --db-subnet-group-name db-subnet-group --backup-retention-period 0
	```
	
	Example output:
	
	>{
	>
	>"DBInstance": {
	>
	>​    "DBInstanceIdentifier": "lab-db",
	>
	>​    "DBInstanceClass": "db.t3.micro",
	>
	>​    "Engine": "mysql",
	>
	>​    "DBInstanceStatus": "creating",
	>
	>​    "MasterUsername": "main",
	>
	>​    "DBName": "lab",
	>
	>​    "AllocatedStorage": 20,
	>
	>​    "PreferredBackupWindow": "04:13-04:43",
	>
	>​    "BackupRetentionPeriod": 1,
	>
	>​    "DBSecurityGroups": [],
	>
	>​    "VpcSecurityGroups": [
	>
	>​      {
	>
	>​        "VpcSecurityGroupId": "sg-07bd350e5084d5a16",
	>
	>​        "Status": "active"
	>
	>​      }
	>
	>​    ],
	>
	>​    "DBParameterGroups": [
	>
	>​      {
	>
	>​        "DBParameterGroupName": "default.mysql5.7",
	>
	>​        "ParameterApplyStatus": "in-sync"
	>
	>​      }
	>
	>​    ],
	>
	>​    "DBSubnetGroup": {
	>
	>​      "DBSubnetGroupName": "db-subnet-group",
	>
	>​      "DBSubnetGroupDescription": "DB Subnet Group",
	>
	>​      "VpcId": "vpc-0723ed5a5ae05ca0a",
	>
	>​      "SubnetGroupStatus": "Complete",
	>
	>​      "Subnets": [
	>
	>​        {
	>
	>​          "SubnetIdentifier": "subnet-054bdcb60c6abb336",
	>
	>​          "SubnetAvailabilityZone": {
	>
	>​            "Name": "us-east-1b"
	>
	>​          },
	>
	>​          "SubnetOutpost": {},
	>
	>​          "SubnetStatus": "Active"
	>
	>​        },
	>
	>​        {
	>
	>​          "SubnetIdentifier": "subnet-0a694c7ed691d2fee",
	>
	>​          "SubnetAvailabilityZone": {
	>
	>​            "Name": "us-east-1a"
	>
	>​          },
	>
	>​          "SubnetOutpost": {},
	>
	>​          "SubnetStatus": "Active"
	>
	>​        }
	>
	>​      ]
	>
	>​    },...

Your database is now launched.

You must now wait *approximately 4 minutes* for the database to be available. The deployment process is deploying a database in two different Availability Zones.

<i class="fas fa-info-circle"></i> While you are waiting, you might want to review the [Amazon RDS FAQs](https://aws.amazon.com/rds/faqs/).

39. Query the details of your RDS instance. Wait until **DBInstanceStatus** changes to *Modifying* or *Available* before you proceed.

    ```bash
    aws rds describe-db-instances --db-instance-identifier lab-db
    ```

    Example output:
    
    >{
    >
    >"DBInstances": [
    >
    >​    {
    >
    >​      "DBInstanceIdentifier": "lab-db",
    >
    >​      "DBInstanceClass": "db.t3.micro",
    >
    >​      "Engine": "mysql",
    >
    >​      **"DBInstanceStatus": "modifying",**
    >
    >​      "MasterUsername": "main",
    >
    >​      "DBName": "lab",
    >
    >​      "Endpoint": {
    >
    >​        "Address": "lab-db.cfexhrgxxxxx.us-east-1.rds.amazonaws.com",
    >
    >​        "Port": 3306,
    >
    >​        "HostedZoneId": "Z2R2ITUGPM61AM"
    >
    >​      },...

40. In the RDS instance output, locate the **Endpoint** information. Note the **Address** value, and save it to the Lab Details text file.

    It will be similar to _lab-db.cggq8lhxxxxx.us-east-2.rds.amazonaws.com_.

&nbsp;
___
## Task 4: Interacting with your database

In this task, you will open a web application that is running on your web server and configure it to use the database.

41. To copy the **WebServer** IP address, above these instructions, choose the <span id="ssb_voc_grey">Details</span> dropdown menu, and then choose <span id="ssb_voc_grey">Show</span>

42. Open a new web browser tab, paste the _WebServer_ IP address and press ENTER.

    The web application will open, and show information about the EC2 instance.

43. At the top of the page, choose the **RDS** link.

    You will now configure the application to connect to your database.

44. Configure the following settings:

    - **Endpoint:** Paste the *Endpoint* value that you copied to the Lab Details text file
    - **Database:** `lab`
    - **Username:** `main`
    - **Password:** `lab-password`
    - Choose **Submit**

A message explains that the application is running a command to copy information to the database. After a few seconds, the application will display an **Address Book**.

The Address Book application uses the RDS database to store information.

45. Test the web application by adding, editing, and removing contacts.

The data is being persisted to the database, and it is replicating automatically to the second Availability Zone.

&nbsp;

## Lab complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

46. To confirm that you want to end the lab, at the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then choose <span id="ssb_blue">Yes</span>

   A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*
   ​
47. To close the panel, choose the **X** in the top-right corner.



&nbsp;
___
### Attributions

**Bootstrap v3.3.5 - [http://getbootstrap.com](http://getbootstrap.com "http://getbootstrap.com")/**

The MIT License (MIT)

Copyright (c) 2011-2016 Twitter, Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*