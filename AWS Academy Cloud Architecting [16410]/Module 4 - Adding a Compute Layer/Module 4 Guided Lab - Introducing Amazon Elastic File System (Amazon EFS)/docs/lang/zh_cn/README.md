# 模块 4 – 指导实验：介绍 Amazon Elastic File System (Amazon EFS)
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: SPL-151"

## 实验概览和目标
本实验旨在向您介绍如何通过 AWS 管理控制台使用 Amazon Elastic File System (Amazon EFS)。

完成本实验后，您应能够：

- 登录到 AWS 管理控制台

- 创建 Amazon EFS 文件系统

- 登录到运行 Amazon Linux 的 Amazon Elastic Compute Cloud (Amazon EC2) 实例

- 将文件系统挂载到 EC2 实例

- 检查和监视文件系统的性能

<br/>
## 持续时间

完成本实验大约需要 **20 分钟**。

<br/>
## AWS 服务限制

在本实验环境中，对 AWS 服务和服务操作的访问可能仅限于完成实验说明所需的服务和服务操作。如果您尝试访问其他服务或执行本实验中所述之外的操作，可能会遇到错误。

<br/>

## 访问 AWS 管理控制台

1. 在这些说明的顶部，选择 <span id="ssb_voc_grey">Start Lab</span>（启动实验）即可启动您的实验。

   **Start Lab**（启动实验）面板随即会打开，其中显示了实验状态。

   <i class="fas fa-info-circle"></i> **提示**：如果您需要更多时间来完成实验，请再次选择 <span id="ssb_voc_grey">Start Lab</span>（启动实验）按钮，重新启动环境计时器。

2. 请耐心等待，直到 **Start Lab**（启动实验）面板显示消息 *Lab status: ready*（实验状态：就绪），然后选择 **X** 关闭此面板。

3. 在这些说明的顶部，选择 <span id="ssb_voc_grey">AWS</span>。

   此操作将在一个新的浏览器选项卡中打开 AWS 管理控制台。您将自动登录系统。

   <i class="fas fa-exclamation-triangle"></i> **提示**：如果未打开新的浏览器选项卡，您的浏览器顶部通常会出现一个横幅或图标，并显示一条消息，指明您的浏览器阻止该了网站打开弹出窗口。请选择横幅或图标，然后选择 **Allow pop-ups**（允许弹出窗口）。

4. 排列 **AWS 管理控制台**选项卡，使其与这些说明一起显示。理想情况下，您将同时打开两个浏览器选项卡，以便能够更轻松地执行实验步骤。

   <i class="fas fa-exclamation-triangle"></i> **除非特别要求，否则请勿更改区域**。

<br/>

## 任务 1：创建安全组来访问您的 EFS 文件系统

与挂载目标关联的安全组*必须允许网络文件系统 (NFS) 端口 2049 上的 TCP 进行入站访问*。这是您现在将创建、配置并附加到 EFS 挂载目标的安全组。


5. 在 **AWS 管理控制台**的 <span id="ssb_services">Services</span>（服务）菜单中，选择 **EC2**。

6. 在左侧导航窗格中，选择 **Security Groups**（安全组）。

7. 将 *EFSClient* 安全组的**安全组 ID** 复制到文本编辑器中。

   组 ID 应类似于 *sg-03727965651b6659b*。

8. 选择 <span id="ssb_orange">Create security group</span>（创建安全组），然后配置：

   <a id='securitygroup'></a>

   * **Security group name**（安全组名称）：`EFS Mount Target`
   * **Description**（描述）：`Inbound NFS access from EFS clients`
   * **VPC**：*Lab VPC*

9. 在 **Inbound rules**（入站规则）部分中，选择 <span id="ssb_white">Add Rule</span>（添加规则），然后进行以下配置：

   * **Type**（类型）：*NFS*
   * **Source**（来源）：
      * *自定义*
      * 在 *Custom*（自定义）框中，将复制到文本编辑器中的安全组的**安全组 ID** 粘贴到该安全组中
   * 选择 <span id="ssb_orange">Create security group</span>（创建安全组）。

<br/>

## 任务 2：创建 EFS 文件系统

EFS 文件系统可以挂载到在同一区域的不同可用区中运行的多个 EC2 实例。这些实例使用在每个*可用区*中创建的*挂载目标*，通过使用标准 NFSv4.1语义挂载文件系统。一次只能在一个 Virtual Private Cloud (VPC) 的实例上挂载文件系统。文件系统和 VPC 必须在同一区域。


10. 在 <span id="ssb_services">Services</span>（服务）菜单中，选择 **EFS**。

11. 选择 <span id="ssb_orange">Create file system</span>（创建文件系统）

12. 在 **Create file system**（创建文件系统）窗口中，选择 <span id="ssb_white">Customize</span>（自定义）

13. 在**第 1 步**中：

    - 取消选中 <i class="far fa-square"></i> Enable automatic backups（启用自动备份）。
    - **Lifecycle management**（生命周期管理）：选择*None*（无）
    - 在 **Tags**（标签）部分，配置：
       - **Key**（键）：`Name`
       - **Value**（值）：`My First EFS File System`

14. 选择 <span id="ssb_orange">Next</span>（下一步）

15. 对于 **VPC**，选择 *Lab VPC*。

16. 通过选中每个默认安全组上的 <i class="fas fa-times"></i> 复选框，将默认安全组从每个*可用区*装载目标中分离出来。

17. 通过以下方式将 **EFS 挂载目标**安全组挂载到每个*可用区*挂载目标：

    * 选中每个 **Security groups**（安全组）复选框。
    * 选择 **EFS Mount Target**。

    系统将为每个子网创建一个装载目标。

    您的挂载目标应该类似于以下示例。该图显示了 **Lab VPC** 中使用 **EFS 挂载目标**安全组的两个装载目标。在本实验中，您应该使用 Lab VPC。

    <img src="images/mount-targets-security-groups.png" alt="目标安全组" width="600" >

18. 选择 <span id="ssb_orange">Next</span>（下一步）

19. 在**第 3 步**中，选择 <span id="ssb_orange">Next</span>（下一步）

20. 在**第 4 步**中：

    * 查看配置。
    * 选择 <span id="ssb_orange">Create</span>（创建）

    <i class="far fa-thumbs-up"></i> 恭喜！您已在 Lab VPC 中创建了新的 EFS 文件系统，并在每个 Lab VPC 子网中挂载目标。几秒钟后，文件系统的**文件系统状态**将更改为*可用*，然后是 2 到 3 分钟后的挂载目标。

    在每个挂载目标的**挂载目标状态**变为*可用*之后，继续下一步。在 2 到 3 分钟后选择屏幕刷新按钮以检查其进度。

<br/>

## 任务 3：通过 SSH 连接到您的 EC2 实例

在此任务中，您可以使用安全外壳 (SSH) 连接至 EC2 实例。

### <i class="fab fa-windows"></i> Microsoft Windows 用户

<i class="fas fa-comment"></i> 这些说明仅适用于 Microsoft Windows 用户。如果您使用的是 macOS 或 Linux，<a href="#ssh-MACLinux">请跳至下一部分</a>。


21. 在您当前读取的这些说明上方，选择 <span id="ssb_voc_grey">Details</span>（详细信息）下拉菜单，然后选择 <span id="ssb_voc_grey">Show</span>（显示）

    系统将打开一个 **Credentials**（凭证）窗口。

22. 选择 **Download PPK**（下载 PPK）按钮并保存 **labsuser.ppk** 文件。

    **注意：**通常情况下，您的浏览器会将文件保存到 **Downloads**（下载）目录。

23. 请注意 **EC2PublicIP** 地址（如果显示）。

24. 通过选择 **X** 退出 **Details**（详细信息）面板。

25. 要使用 SSH 访问 EC2 实例，您必须使用***\*PuTTY\****。如果计算机上未安装 PuTTY，<a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">下载 PuTTY</a>。

26. 打开 **putty.exe**。

27. 要使 PuTTY 会话在更长时间内保持打开状态，请配置 PuTTY 超时：

    * 选择 **Connection**（连接）
    * **Seconds between keepalives**（keepalive 之间的秒数）：`30`

28. 使用以下设置配置您的 PuTTY 会话。

    * 选择 **Session**（会话）
    * **Host Name** (or IP address)（主机名（或 IP 地址））：粘贴您之前记下的实例的 **EC2PublicIP**
       * 或者，返回 Amazon EC2 控制台，并选择 **Instances**（实例）
       * 选择要连接到的实例
       * 在 *Description*（描述）选项卡中，复制 **IPv4 Public IP**（IPv4 公有 IP）的值
    * 返回 PuTTY，在 **Connection**（连接）列表中，展开 <i class="far fa-plus-square"></i> **SSH**
    * 选择 **Auth**（身份验证）（但不要展开）
    * 选择 **Browse**（浏览）
    * 浏览到已下载的 *labsuser.ppk* 文件，选中并选择 **Open**（打开）
    * 再次选择 **Open**（打开）


29. 要信任并连接到主机，请选择 **Yes**（是）。

30. 在系统提示 **login as**（登录身份）时，输入：`ec2-user`

    此操作会将您连接到 EC2 实例。

31. Microsoft Windows 用户：<a href="#ssh-after">选择此链接向前跳至下一个任务。</a>



<a id='ssh-MACLinux'></a>

### macOS <span style="font-size: 30px; color: #808080;"><i class="fab fa-apple"></i></span> 和 Linux <span style="font-size: 30px; "><i class="fab fa-linux"></i></span> 用户

这些说明仅适用于 macOS 或 Linux 用户。如果您是 Windows 用户，<a href="#ssh-after">请向前跳至下一个任务。</a>

32. 在您当前读取的这些说明上方，选择 <span id="ssb_voc_grey">Details</span>（详细信息）下拉菜单，然后选择 <span id="ssb_voc_grey">Show</span>（显示）

   系统将打开一个 **Credentials**（凭证）窗口。

33. 选择 **Download PEM**（下载 PEM）按钮并保存 **labsuser.pem** 文件。

34. 请注意 **EC2PublicIP** 地址（如果显示）。

35. 通过选择 **X** 退出 **Details**（详细信息）面板。

36. 打开一个终端窗口，并将目录更改为使用 `cd` 命令下载的 *labsuser.pem* 文件所在的目录。

    例如，如果将 *labsuser.pem* 文件保存到了您的 **Downloads**（下载）目录，请运行以下命令：

    ```bash
    cd ~/Downloads
    ```

37. 通过运行以下命令，将密钥的权限更改为只读：

    ```bash
    chmod 400 labsuser.pem
    ```

38. 运行以下命令（将 **<public-ip\>** 替换为您之前复制的 **EC2PublicIP** 地址）。

    * 或者，要查找本地实例的 IP 地址，请返回到 Amazon EC2 控制台并选择 **Instances**（实例）
    * 选择要连接到的实例
    * 在 **Description**（描述）选项卡中，复制 **IPv4 Public IP**（IPv4 公有 IP）的值

     ```bash
     ssh -i labsuser.pem ec2-user@<public-ip>
     ```

39. 在系统提示允许首次连接此远程 SSH 服务器时，输入：`yes`（是）。

    由于您使用密钥对进行身份验证，因此系统不会提示您输入密码。

<a id='ssh-after'></a>

<br/>
## 任务 4：创建新目录并挂载 EFS 文件系统

<i class="fas fa-info-circle" aria-hidden="true"></i> Amazon EFS 在 EC2 实例上挂载文件系统时支持 NFSv4.1 和 NFSv4.0 协议。尽管支持 NFSV4.0，但我们建议您使用 NFSV4.1。在 EC2 实例上挂载 EFS 文件系统时，还必须使用支持所选 NFSv4 协议的 NFS 客户端。作为本实验的一部分启动的 EC2 实例包括已安装在其上的 NFSv4.1 客户端。


40. 回到 **AWS 管理控制台**，在 <span id="ssb_services">Services</span>（服务）菜单上，单击 **EFS**。

41. 单击 **My First EFS File System**。

42. 在 **Amazon EFS 控制台**的页面右上角，选择 <span id="ssb_orange">Attach</span>（连接）以打开 Amazon EC2 挂载说明。

43. 在 SSH 会话中，按照 **To set up your EC2 instance**（设置 EC2 实例）部分中的说明进行操作。复制并粘贴（或手动键入）命令以安装所需的实用程序。

    该命令应与此示例类似：

    ```
    sudo yum install -y amazon-efs-utils
    ```

    *提示：*要在浏览器中粘贴到终端中，请将光标放在命令提示符的右侧，然后右键单击以查看粘贴选项。

44. 向下滚动到 **Mounting your file system**（挂载文件系统）部分。

45. 在 SSH 会话中，通过输入 `sudo mkdir efs` 来创建一个新目录

46. 复制 **Using the NFS client**（使用 NFS 客户端）部分中的整个命令。

    该挂载命令应与此示例类似：

    `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/ efs`

    <i class="fas fa-comment" aria-hidden="true"></i> 提供的 `sudo mount...` 命令使用默认的 Linux 挂载选项。

47. 在 Linux SSH 会话中，通过以下方式挂载 Amazon EFS 文件系统：

    * 粘贴命令
    * 按 ENTER 键


48. 输入以下内容，获取可用和已用磁盘空间使用情况的完整摘要：

    `sudo df -hT`

    以下屏幕截图是以下*磁盘文件系统*命令输出的示例：

    `df -hT`

    请注意已挂载的 EFS 文件系统的*类型*和*大小*。

    <img src="images/disk-space.png" alt="磁盘空间" width="600" >


<br/>
## 任务 5：检查新 EFS 文件系统的性能表现


### 使用灵活 IO 检查性能

<i class="fas fa-info-circle"></i> 灵活 IO (fio) 是一个适用于 Linux 的合成 I/O 基准测试实用程序。它用于对 Linux I/O 子系统进行基准测试和测试。在启动过程中，*fio* 会自动安装在您的 EC2 实例上。

49. 通过输入以下内容来检查文件系统的写入性能特征：

    ```
    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
    ```

    <i class="fas fa-comment"></i>`fio` 命令将需要 5 到 10 分钟才能完成。输出应与以下屏幕截图中的示例类似。确保检查 `fio` 命令的输出，特别是此 WRITE 测试的摘要状态信息。

    <img src="images/fio.png" alt="fio" width="600" >

<br/>
### 使用 Amazon CloudWatch 监控性能

50. 在 **AWS 管理控制台**的 <span id="ssb_services">Services</span>（服务）菜单中，选择 **CloudWatch**。

51. 在左侧的导航窗格中，选择 **Metrics**（指标）。

52. 在 **All metrics**（全部指标）选项卡中，选择 **EFS**。

53. 选择 **File System Metrics**（文件系统指标）。

54. 选择含有 **PermittedThroughput** 指标名称的行。

    <i class="fas fa-comment"></i> 您可能需要等待 2 到 3 分钟，并在计算和填充所有可用指标（包括**PermittedThroughput**）之前多次刷新屏幕。

55. 在图表上，选择并拖动数据线。如果没有看到折线图，请调整图表的时间范围以显示运行 `fio` 命令的时间段。

    <img src="images/graph.png" alt="选择拖动" width="600" >


56. 将指针暂停在图表中的数据行上。该值应为 *105M*。

    <img src="images/throughput.png" alt="吞吐量" width="600" >

    Amazon EFS 的吞吐量随着文件系统的增长而扩展。基于文件的工作负载通常会猛增，在短时间内以高吞吐量驱动，然后在其余时间内以较低的吞吐量驱动。基于此，Amazon EFS 专为在一段时间内突增到高吞吐量而设计。所有文件系统，无论大小如何，都可以突增至 100 MiB/s 的吞吐量。有关 EFS 文件系统性能特征的更多信息，请参阅官方 <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">Amazon Elastic File System 文档</a>。

57. 在 **All metrics**（全部指标）选项卡中，*取消选中* **PermittedThroughput** 复选框。

58. 选中 **DataWriteIOBytes** 复选框。

    <i class="fas fa-comment"> </i> 如果您在指标列表中没有看到 *DataWriteIOBytes*，请使用 **File System Metrics**（文件系统指标）搜索来查找它。

59. 选择 **Graphed metrics**（图形化指标）选项卡。

60. 在 **Statistics**（统计信息）列中，选择 **Sum**（总和）。

61. 在 **Period**（周期）列中，选择 **1 Minute**（1 分钟）。

62. 将指针停留在折线图的顶点上。将这个数字（以字节为单位）除以持续时间（以秒为单位）（60 秒）。结果给出了测试期间文件系统的写入吞吐量 (B/s)。

    <img src="images/Sum-1-minute.png" alt="总计 1 分钟" width="600" >

    文件系统可用的吞吐量随着文件系统的增长而扩展。所有文件系统均提供每 TiB 存储 50 MiB/s 的一致基准性能。此外，所有文件系统（无论大小如何）都可以突增至 100 MiB/s。大于 1TB 的文件系统可以突增至每 TiB 存储 100 MiB/s。向文件系统添加数据时，文件系统可用的最大吞吐量会随着存储空间自动地线性扩展。

    文件系统吞吐量在连接到文件系统的所有 EC2 实例之间共享。有关 EFS 文件系统性能特征的更多信息，请参阅官方 <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">Amazon Elastic File System 文档</a>。

    <i class="far fa-thumbs-up" style="color:blue"></i> 恭喜！您创建了 EFS 文件系统，将其挂载到 EC2 实例，并运行了 I/O 基准测试来检查其性能特征。


<br/>
## 提交作业

63. 在这些说明的顶部，选择 <span id="ssb_blue">Submit</span>（提交）记录您的进度，在出现提示时，选择 **Yes**（是）。

64. 如果在几分钟后仍未显示结果，请返回到这些说明的顶部，并选择 <span id="ssb_voc_grey">Grades</span>（成绩）

    **提示**：您可以多次提交作业。更改作业后，再次选择 **Submit**（提交）即可。您最后一次提交的作业将记录为本实验的成绩。

65. 要查找有关您作业的详细反馈，请选择 <span id="ssb_voc_grey">Details</span>（详细信息），然后选择 <i class="fas fa-caret-right"></i> **View Submission Report**（查看提交报告）。


<br/>
## 实验完成 <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> 恭喜！您已完成实验。


66. 选择此页面顶部的 <span id="ssb_voc_grey">End Lab</span>（结束实验），然后选择 <span id="ssb_blue">Yes</span>（是）确认您要结束实验。

    此时应显示一个面板，并显示消息：*DELETE has been initiated... You may close this message box now.*（删除操作已启动...您现在可以关闭此消息框。）

67. 选择右上角的 **X** 以关闭面板。


*©2020 Amazon Web Services, Inc. 及其附属公司。保留所有权利。未经 Amazon Web Services, Inc. 事先书面许可，不得复制或转载本文的部分或全部内容。禁止商业性复制、出租或出售。*