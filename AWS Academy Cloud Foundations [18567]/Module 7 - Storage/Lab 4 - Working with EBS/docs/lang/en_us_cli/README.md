# Lab 4: Working with Amazon EBS

<!-- Note to translators: This activity is unique to this course. -->

## Lab overview

<img src="../../images/lab-scenario.jpeg" alt="architectural diagram" width="400">

This lab focuses on Amazon Elastic Block Store (Amazon EBS), a key underlying storage mechanism for Amazon Elastic Compute Cloud (Amazon EC2) instances. In this lab, you will learn how to create an EBS volume, attach it to an instance, apply a file system to the volume, and then take a snapshot backup.

This lab is based on the AWS Command Line Interface (AWS CLI). Feel free to explore the [AWS CLI documentation for Amazon EC2](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html) as you work through the lab.


### Topics covered

At the end of this lab, you should be able to:

- Create an EBS volume
- Attach and mount your volume to an EC2 instance
- Create a snapshot of your volume
- Create a new volume from your snapshot
- Attach and mount the new volume to your EC2 instance


### Lab prerequisites

To successfully complete this lab, you should be familiar with basic Amazon EC2 usage and basic Linux server administration. You should also feel comfortable with using Linux command-line tools.


### Other AWS services

 AWS Identity and Access Management (IAM) policies apply to the Amazon Web Services (AWS) products and services in this lab environment. The IAM policies might restrict your access to AWS services that are not used in this lab. In addition, you might be able to use only the service features that are required. In some cases, access to service features are further limited as an intentional aspect of the lab design. You should expect errors if you try to access other services or perform actions beyond the ones that are in this lab guide.



### Amazon EBS

*Amazon Elastic Block Store (Amazon EBS)* offers persistent storage for EC2 instances. EBS volumes are network-attached and persist independently from the life of an instance. EBS volumes are highly available, highly reliable volumes. They can be used as a boot partition for EC2 instances. They can also be attached to a running EC2 instance as a standard block device.

When EBS volumes are used as a boot partition, EC2 instances can be stopped and subsequently restarted. This feature enables you to pay only for the storage resources that are used, while also maintaining your instance's state. EBS volumes offer improved durability over local EC2 instance stores. EBS volumes are automatically replicated on the backend (in a single Availability Zone).

If you want more durability, you can use Amazon EBS to create point-in-time consistent snapshots of your volumes. These snapshots are then stored in Amazon Simple Storage Service (Amazon S3) and are automatically replicated across multiple Availability Zones. These snapshots can be used as the starting point for new EBS volumes, and they can protect your data for long-term durability. You can also share these snapshots with coworkers and other AWS developers.

This lab explains the basic concepts of Amazon EBS in a step-by-step fashion. However, it can only give a brief overview of Amazon EBS concepts. For further information, see the <a href="http://aws.amazon.com/ebs/" target="_blank">Amazon EBS documentation</a>.



### EBS volume features

EBS volumes deliver the following features:

- **Persistent storage:** Volume lifetime is independent of any particular EC2 instance
- **General purpose:** EBS volumes are raw, unformatted block devices that can be used from any operating system
- **High performance:** EBS volumes are equal to or better than local Amazon EC2 drives
- **High reliability:** EBS volumes have built-in redundancy within an Availability Zone
- **Designed for resiliency:** Amazon EBS volume types are designed to provide from 98.8 percent durability up to for five 9s (99.999 percent) of durability.
- **Variable size:** Volume sizes range from 1 GiB to 16 TiB
- **Easy to use:** EBS volumes can be created, attached, backed up, restored, and deleted


**Duration**
This lab takes approximately **30 minutes** to complete.



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
    ssh -i labsuser.pem ec2-user@<bastion-ip-address>
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
    ```
    aws configure
    ```


24. At the prompts, enter the following information. For the access key, secret access key, and Region settings, use the values from the Lab Details text file.

  **Note**: Ensure that you copy and paste the correct user values from the Lab Details text file to these fields. 

  -  **AWS Access Key ID**: *AccessKey* value
  -  **AWS Secret Access Key**: *SecretKey* value
  -  **Default region name**:  Name of the Region (for example, `us-east-1` or `eu-west-2`)
  -  **Default output format**: `json`


&nbsp;

## Task 1: Creating a new EBS volume

In this task, you will create and attach an EBS volume to a new EC2 instance.

25. In the terminal window, describe the EC2 instances in the lab environment by running the following command:

    ```bash
    aws ec2 describe-instances
    ```
    
    Example output:
    
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
    >​          "ImageId": "ami-08f3d892de2xxxxxx",
    >
    >​          **"InstanceId": "i-0ef6c8de97cxxxxxx"**,
    >
    >​          "InstanceType": "t2.micro",
    >
    >​          "KeyName": "xxxxxxx",
    >
    >​          "LaunchTime": "2020-07-16T16:05:38+00:00",
    >
    >​          "Monitoring": {
    >
    >​            "State": "disabled"
    >
    >​          },
    >
    >​          "Placement": {
    >
    >​            **"AvailabilityZone": "us-east-1a"**,
    >
    >​            "GroupName": "",
    >
    >​            "Tenancy": "default"
    >
    >​          },
    >
    >​          ...
    >
    >​          "BlockDeviceMappings": [
    >
    >​            {
    >
    >​              "DeviceName": "/dev/xvda",
    >
    >​              "Ebs": {
    >
    >​                "AttachTime": "2020-07-16T16:05:39+00:00",
    >
    >​                "DeleteOnTermination": true,
    >
    >​                "Status": "attached",
    >
    >​                **"VolumeId": "vol-0f4450ad739xxxxxx"**
    >
    >​              }
    >
    >​            }
    >
    >​          ],
    >
    >​         ...
    >
    >​          "Tags": [
    >
    >​            {
    >
    >​              "Key": "aws:cloudformation:logical-id",
    >
    >​              "Value": "Ec2Instance"
    >
    >​            },
    >
    >​            {
    >
    >​              "Key": "aws:cloudformation:stack-name",
    >
    >​              "Value": "ACF-lab4-cli-ebs"
    >
    >​            },
    >
    >​            {
    >
    >​              **"Key": "Name",**
    >
    >​              **"Value": "Lab"**
    >
    >​            },   

​		The **Tags** section of the output lists an Amazon EC2 instance named *Lab*, which was launched for this lab.


26. Note the following details of the *Lab* instance:

    * **InstanceId:** This value will look similar to *i-0ef6c8de97cxxxxxx*.

    * **AvailabilityZone:** This value will look similar to *us-west-2a*. You can also find the Availability Zone in the information that you saved in the Lab Details text file.

    * **VolumeId:** This value will look similar to *vol-0f4450ad739xxxxxx*.


27. From the output, copy the values of *InstanceId* and *VolumeId* for the *Lab* instance, and save them to the Lab Details text file. Make sure that you note that the *VolumeId* value is for the existing EBS volume. 


28. Query the details of the EBS volume that's associated with the *Lab* EC2 instance. Update the following command with the *VolumeId* value that you saved in the previous step. 

    ```bash
    aws ec2 describe-volumes --volume-ids <replace-with-VolumeId>
    ```

    Example output:
   
    >​    {
    >
    >"Volumes": [
    >
    >​    {
    >
    >​      "Attachments": [
    >
    >​        {
    >
    >​          "AttachTime": "2020-07-16T16:05:39+00:00",
    >
    >​          "Device": "/dev/xvda",
    >
    >​          "InstanceId": "i-0ef6c8de97cxxxxxx",
    >
    >​          "State": "attached",
    >
    >​          "VolumeId": "vol-0f4450ad739xxxxxx",
    >
    >​          "DeleteOnTermination": true
    >
    >​        }
    >
    >​      ],
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "CreateTime": "2020-07-16T16:05:39.339000+00:00",
    >
    >​      "Encrypted": false,
    >
    >​      **"Size": 8**,
    >
    >​      "SnapshotId": "snap-028531a8313xxxxxx",
    >
    >​      "State": "in-use",
    >
    >​      "VolumeId": "vol-0f4450ad739xxxxxx",
    >
    >​      "Iops": 100,
    >
    >​      "VolumeType": "gp2",
    >
    >​      "MultiAttachEnabled": false
    >
    >​    }
    >
    >]
    >
    >}
    >
    >
    
    This volume has a size of *8 GiB*. The volume size makes it easy to distinguish from the volume that you will create next, which will have a size of *1 GiB*.
    
28. To add a new volume, update the `create-volume` command with these settings:

    * **Volume Type:** `gp2`
    * **Size (GiB):** `1`
        * **Note**: You might be restricted from creating large volumes.
    * **Availability Zone:** *AvailabilityZone* value (the same Availability Zone as your EC2 instance)
    * Tag Specifications:
      * **Key:** `Name`
      * **Value:** `My Volume`

    

    ```bash
    aws ec2 create-volume --volume-type gp2 \
    --size 1 --availability-zone <AvailabilityZone> \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=My Volume}]'
    ```

    Example output:

    >​    {
    >
    >"AvailabilityZone": "us-east-1a",
    >
    >"CreateTime": "2020-07-16T19:15:26+00:00",
    >
    >"Encrypted": false,
    >
    >"Size": 1,
    >
    >"SnapshotId": "",
    >
    >"State": "creating",
    >
    >**"VolumeId": "vol-011e5902bb7xxxxxx"**,
    >
    >"Iops": 100,
    >
    >"Tags": [
    >
    >​    {
    >
    >​      "Key": "Name",
    >
    >​      "Value": "MyVolume"
    >
    >​    }
    >
    >],
    >
    >"VolumeType": "gp2",
    >
    >"MultiAttachEnabled": false
    >
    >}

    

    Note the *VolumeID* value of the new volume (*My Volume*). 

29. Before you proceed, verify that the state of *My Volume* is *available*. When you run the following command, be sure to use the *VolumeID* of the new volume.

    ```bash
    aws ec2 describe-volumes --volume-ids <my-volume-VolumeId>
    ```

    Example output:

    >​    {
    >
    >"Volumes": [
    >
    >​    {
    >
    >​      "Attachments": [],
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "CreateTime": "2020-07-16T19:15:26.832000+00:00",
    >
    >​      "Encrypted": false,
    >
    >​      "Size": 1,
    >
    >​      "SnapshotId": "",
    >
    >​      **"State": "available"**,
    >
    >​      "VolumeId": "vol-011e5902bb7xxxxxx",
    >
    >​      "Iops": 100,
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "Name",
    >
    >​          "Value": "MyVolume"
    >
    >​        }
    >
    >​      ],
    >
    >​      "VolumeType": "gp2",
    >
    >​      "MultiAttachEnabled": false
    >
    >​    }
    >
    >]
    >
    >}
    >
    >
    
    Notice that the state of the volume is *available*.

&nbsp;
&nbsp;

## Task 2: Attaching the new volume to an instance

In this task, you will attach *My Volume* to the *Lab* EC2 instance.

30. Run the `attach-volume` command with the following parameters.

  * **VolumeId:** *VolumeId* value for *My Volume* (the new, 1 GiB volume)

  * **Device:** `/dev/sdf`

  * **InstanceId:** *InstanceId* value for the *Lab* instance

    ```bash
    aws ec2 attach-volume --device /dev/sdf \
    --instance-id <lab-instance-InstanceId> \
    --volume-id <my-volume-VolumeId>
    ```

  Example output:
  
  >​    {
  >
  >"AttachTime": "2020-07-16T19:38:41.114000+00:00",
  >
  >"Device": "/dev/sdf",
  >
  >"InstanceId": "i-0ef6c8de97cxxxxxx",
  >
  >"State": "attaching",
  >
  >"VolumeId": "vol-011e5902bb7xxxxxx"
  >
  >}



31. Describe *My Volume* again:

    ```bash
    aws ec2 describe-volumes --volume-ids <my-volume-VolumeId>
    ```

    Example output:

    > {
    >
    > "Volumes": [
    >
    > ​    {
    >
    > ​      "Attachments": [
    >
    > ​        {
    >
    > ​          "AttachTime": "2020-07-16T19:38:41+00:00",
    >
    > ​          "Device": "/dev/sdf",
    >
    > ​          "InstanceId": "i-0ef6c8de97cxxxxxx",
    >
    > ​          "State": "attached",
    >
    > ​          "VolumeId": "vol-011e5902bb7xxxxxx",
    >
    > ​          "DeleteOnTermination": false
    >
    > ​        }
    >
    > ​      ],
    >
    > ​      "AvailabilityZone": "us-east-1a",
    >
    > ​      "CreateTime": "2020-07-16T19:15:26.832000+00:00",
    >
    > ​      "Encrypted": false,
    >
    > ​      "Size": 1,
    >
    > ​      "SnapshotId": "",
    >
    > ​      **"State": "in-use",**
    >
    > ​      "VolumeId": "vol-011e5902bb7xxxxxx",
    >
    > ​      "Iops": 100,
    >
    > ​      "Tags": [
    >
    > ​        {
    >
    > ​          "Key": "Name",
    >
    > ​          "Value": "MyVolume"
    >
    > ​        }
    >
    > ​      ],
    >
    > ​      "VolumeType": "gp2",
    >
    > ​      "MultiAttachEnabled": false
    >
    > ​    }
    >
    > ]
    >
    > }
    
    
    
    The volume state is now *in-use*.

&nbsp;

## Task 3: Connecting to the Lab instance

In this task, make sure that you keep your *Bastion Host* session open. You will create a new, separate session for the *Lab* instance. Connecting to the *Lab* instance is similar to connecting to the *Bastion Host*, but you will use a different IP address.

32. In the Lab Details text file, locate the IP address of the **LabInstance**.

33. Start a new SSH session by using the IP address of the *Lab* instance to configure your connection, and log in to it.

    **Note:** For Windows, use PuTTY. For macOS or Linux, use the `ssh` command. You do not need to download the PPK or PEM files again. To review how to create an SSH connection, refer to the section titled *Using SSH to Connect to the Bastion Host*.

    &nbsp;

## Task 4: Creating and configuring your file system

In this task, you will add the new volume to a Linux instance as an *ext3* file system under the */mnt/data-store* mount point.

<i class="fas fa-info-circle"></i> If you are using PuTTY, you can paste text by right-clicking in the PuTTY window.

34. Make sure that you are in the *Lab* instance SSH session.

35. View the storage that is available on your instance:

    ```plain
    df -h
    ```

    You should see output similar to the following example:
    
    >Filesystem   Size Used Avail Use% Mounted on
    >
    >devtmpfs    474M   0 474M  0% /dev
    >
    >tmpfs      492M   0 492M  0% /dev/shm
    >
    >tmpfs      492M 404K 492M  1% /run
    >
    >tmpfs      492M   0 492M  0% /sys/fs/cgroup
    >
    >/dev/xvda1   8.0G 1.3G 6.8G 16% /
    >
    >tmpfs      99M   0  99M  0% /run/user/1000
    
    This example output shows the original 8 GB disk volume. Your new volume is not yet shown.

36. Create an ext3 file system on the new volume:

    ```plain
    sudo mkfs -t ext3 /dev/sdf
    ```

37. Create a directory for mounting the new storage volume:

    ```plain
    sudo mkdir /mnt/data-store
    ```

38. Mount the new volume:

    ```plain
    sudo mount /dev/sdf /mnt/data-store
    ```

    To configure the Linux instance to mount this volume when the instance is started, you must add a line to */etc/fstab*.

    ```plain
    echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
    ```

39. To find the setting on the last line, review the configuration file:

    ```plain
    cat /etc/fstab
    ```

    Example output:
    
    >\#
    >
    >UUID=91ab5f73-23b0-4204-bbe0-813fc53eb4b5   /      xfs  defaults,noatime 1  1
    >
    >/dev/sdf  /mnt/data-store ext3 defaults,noatime 1 2

40. Review the available storage again:

    ```plain
    df -h
    ```

    The output will now contain an additional line - */dev/xvdf*:
    
    >Filesystem   Size Used Avail Use% Mounted on
    >
    >devtmpfs    474M   0 474M  0% /dev
    >
    >tmpfs      492M   0 492M  0% /dev/shm
    >
    >tmpfs      492M 404K 492M  1% /run
    >
    >tmpfs      492M   0 492M  0% /sys/fs/cgroup
    >
    >/dev/xvda1   8.0G 1.3G 6.8G 16% /
    >
    >tmpfs      99M   0  99M  0% /run/user/1000
    >
    >/dev/xvdf    976M 1.3M 924M  1% /mnt/data-store


41. On your mounted volume, create a file and add some text to it.

    ```plain
    sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
    ```

42. Verify that the text was written to your volume.

    ```plain
    cat /mnt/data-store/file.txt
    ```

    You should find the following output:
    
    >some text has been written
    >

&nbsp;


## Task 5: Creating an EBS snapshot

In this task, you will create a snapshot of your EBS volume.

You can create any number of point-in-time, consistent snapshots from EBS volumes at any time. EBS snapshots are stored in Amazon S3 with high durability. New EBS volumes can be created out of snapshots for cloning or for restoring backups. EBS snapshots can also be easily shared among AWS users or copied over AWS Regions.

43. Return to your *Bastion Host* session. If your session timed out, reconnect to.

44. Create a snapshot of the new volume (*My Volume*). Be sure to update the *InstanceId* value.

    ```bash
    aws ec2 create-snapshots --instance-specification \
    InstanceId=<lab-instance-InstanceId>,ExcludeBootVolume=true \
    --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=My Snapshot}]'
    ```

    Example output:
    
    >{
    >
    >"Snapshots": [
    >
    >​    {
    >
    >​      "Description": "",
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "Name",
    >
    >​          "Value": "MySnapshot"
    >
    >​        }
    >
    >​      ],
    >
    >​      "Encrypted": false,
    >
    >​      "VolumeId": "vol-011e5902bb7xxxxxx",
    >
    >​      "State": "pending",
    >
    >​      "VolumeSize": 1,
    >
    >​      "StartTime": "2020-07-16T20:49:29.742000+00:00",
    >
    >​      "Progress": "",
    >
    >​      "OwnerId": "135572168615",
    >
    >​      **"SnapshotId": "snap-042a3cdbcb4xxxxxx"**
    >
    >​    }
    >
    >]
    >
    >}
    
    Notice that the snapshot is tagged with the name *My Snapshot*.

45. Make a note of the *SnapshotId* value and save it to the Lab Details file. It will be similar to *snap-042a3cdbcbxxxxxx*.

46. Query the snapshot to ensure that its current state is *completed*:

    ```bash
    aws ec2 describe-snapshots --snapshot-ids <SnapshotId>
    ```

    Example output:
    
    >{
    >
    >"Snapshots": [
    >
    >​    {
    >
    >​      "Description": "",
    >
    >​      "Encrypted": false,
    >
    >​      "OwnerId": "135572168615",
    >
    >​      "Progress": "100%",
    >
    >​      "SnapshotId": "snap-042a3cdbcb4xxxxxx",
    >
    >​      "StartTime": "2020-07-16T20:49:29.742000+00:00",
    >
    >​      **"State": "completed"**,
    >
    >​      "VolumeId": "vol-011e5902bb7xxxxxx",
    >
    >​      "VolumeSize": 1,
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "Name",
    >
    >​          "Value": "MySnapshot"
    >
    >​        }
    >
    >​      ]
    >
    >​    }
    >
    >]
    >
    >}

47. The output includes information about the snapshot. It will start with a state of *pending*, which means that the snapshot is being created. It will then change to a state of *completed*. Only used storage blocks are copied to snapshots, so empty blocks do not take any snapshot storage space.

48. In your *Lab* instance SSH session, delete the file that you created on your volume. If needed, reconnect to the *Lab* instance.

    ```plain
    sudo rm /mnt/data-store/file.txt
    ```

49. Verify that the file was deleted.

    ```plain
    ls /mnt/data-store/
    ```

    Your file has been deleted.

&nbsp;
&nbsp;
## Task 6: Restoring the EBS snapshot

If you want to retrieve the data that is stored in a snapshot, you can **Restore** the snapshot to a new EBS volume.

### Creating a volume by using your snapshot

50. Return to your *Bastion Host* session.

51. Create a volume by using your new snapshot. 

    Instead of the `--volume-type` and `--size` parameters, you use `--snapshot-id`. When you create a volume, you must specify either the *snapshot-id* or the size. When you restore a snapshot to a new volume, you can also modify the configuration, such as changing the volume type, size, or Availability Zone.

    Be sure to update the command parameters with the values that you saved in the Lab Details text file:

    ```bash
    aws ec2 create-volume \
    --snapshot-id <SnapshotId> \
    --availability-zone <AvailabilityZone> \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=Restored Volume}]'
    ```

    Example output:

    >{
    >
    >  "AvailabilityZone": "us-east-1a",
    >
    >  "CreateTime": "2020-07-17T13:52:26+00:00",
    >
    >  "Encrypted": false,
    >
    >  "Size": 1,
    >
    >  "SnapshotId": "snap-042a3cdbcb4xxxxxx",
    >
    >  "State": "creating",
    >
    >  **"VolumeId": "vol-0044f4c123xxxxxx"**,
    >
    >  "Iops": 100,
    >
    >  "Tags": [
    >
    >​    {
    >
    >​      "Key": "Name",
    >
    >​      "Value": "Restored Volume"
    >
    >​    }
    >
    >  ],
    >
    >  "VolumeType": "gp2",
    >
    >  "MultiAttachEnabled": false
    >
    >}

52. Note the *VolumeId* value for the new, restored volume and save it to the Lab Details text file. It will be similar to *vol-0044f4c123xxxxx*.

    

### Attaching the restored volume to your Lab EC2 instance

53. Stay in the *Bastion Host* SSH session.

54. Update the `attach-volume` command with the following parameters and run it. (This time, the **Device** parameter will be set to */dev/sdg*.):

    **VolumeId:** `<restored-volume-VolumeId>`

    **Device:** `/dev/sdg`

    **InstanceId:** `<lab-instance-InstanceId>` 
    
    ```bash
    aws ec2 attach-volume --device /dev/sdg \
    --instance-id <lab-instance-InstanceId> \
    --volume-id <restored-volume-VolumeId>
    ```
    
    Example output:
    
    >{
    >
    >"AttachTime": "2020-07-17T14:07:05.937000+00:00",
    >
    >"Device": "/dev/sdg",
    >
    >"InstanceId": "i-0ef6c8de97cxxxxxx",
    >
    >"State": "attaching",
    >
    >"VolumeId": "vol-0044f4c1230xxxxxx"
    >
    >}

55. Describe the restored volume. Update the following command with the *VolumeID* of the restored volume:

    ```bash
    aws ec2 describe-volumes --volume-ids <restored-volume-VolumeId>
    ```

    Example output:
    
    >{
    >
    >"Volumes": [
    >
    >​    {
    >
    >​      "Attachments": [
    >
    >​        {
    >
    >​          "AttachTime": "2020-07-17T14:07:05+00:00",
    >
    >​          "Device": "/dev/sdg",
    >
    >​          "InstanceId": "i-0ef6c8de97cxxxxxx",
    >
    >​          "State": "attached",
    >
    >​          "VolumeId": "vol-0044f4c1230xxxxxx",
    >
    >​          "DeleteOnTermination": false
    >
    >​        }
    >
    >​      ],
    >
    >​      "AvailabilityZone": "us-east-1a",
    >
    >​      "CreateTime": "2020-07-17T13:52:26.582000+00:00",
    >
    >​      "Encrypted": false,
    >
    >​      "Size": 1,
    >
    >​      "SnapshotId": "snap-042a3cdbcb4xxxxxx",
    >
    >​      **"State": "in-use"**,
    >
    >​      "VolumeId": "vol-0044f4c1230xxxxxx",
    >
    >​      "Iops": 100,
    >
    >​      "Tags": [
    >
    >​        {
    >
    >​          "Key": "Name",
    >
    >​          "Value": "Restored Volume"
    >
    >​        }
    >
    >​      ],
    >
    >​      "VolumeType": "gp2",
    >
    >​      "MultiAttachEnabled": false
    >
    >​    }
    >
    >]
    >
    >}



### Mounting the restored volume

56. Return to the *Lab* instance SSH session. You might need to reconnect to it.

57. Create a directory for mounting the new storage volume:

    ```plain
    sudo mkdir /mnt/data-store2
    ```

58. Mount the new volume:

    ```plain
    sudo mount /dev/sdg /mnt/data-store2
    ```

59. Verify that the volume you mounted has the file that you created previously.

    ```plain
    ls /mnt/data-store2/
    ```

    You should find file.txt in the output.

    >file.txt   lost+found

&nbsp;
&nbsp;

## Lab summary

<i class="far fa-thumbs-up" style="color:blue"></i> You now have successfully:

- Created an EBS volume
- Attached the volume to an EC2 instance
- Created a file system on the volume
- Added a file to volume
- Created a snapshot of your volume
- Created a new volume from the snapshot
- Attached and mounted the new volume to your EC2 instance
- Verified that the file you created earlier was on the newly created volume



## Additional resources

<a href="http://aws.amazon.com/ebs/" target="_blank">Amazon Elastic Block Store features, functions, and pricing</a>

<a href="http://aws.amazon.com/training/" target="_blank">AWS Training and Certification</a>


*©2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*