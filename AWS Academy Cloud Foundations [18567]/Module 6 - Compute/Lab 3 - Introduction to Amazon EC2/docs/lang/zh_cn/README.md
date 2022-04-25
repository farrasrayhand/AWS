# 实验 3：Amazon EC2 简介

<!-- Note to translators: This is based on SPL-200. Copy the translation from there. Do not re-translate the whole document. -->

**版本 1.1.7 (spl200)**

## 概览

<img src="images/lab-scenario.jpeg" alt="架构图" width="400">

&nbsp;

本实验概述了如何启动、管理和监控 Amazon EC2 实例以及调整其大小。

**Amazon Elastic Compute Cloud (Amazon EC2)** 是一种 Web 服务，可在提供大小可调的云端计算容量。该服务旨在让开发人员更轻松地进行 Web 规模的云计算。

Amazon EC2 的 Web 服务界面非常简单，您可以在此界面上轻松获取和配置容量。使用该服务，您可以完全控制您的计算资源，并可以在经过验证的 Amazon 计算环境中运行。Amazon EC2 将获取并启动新服务器实例所需要的时间缩短至几分钟，这样一来，在您的计算要求发生变化时，您便可以快速扩展或缩减计算容量。

Amazon EC2 按您实际使用的容量收费，改变了计算的成本结算方式。Amazon EC2 还为开发人员提供了相关工具来创建能够迅速从故障中恢复的应用程序并避免常见故障。

&nbsp;
### 涵盖的主题

本实验结束时，您将能够：

* 在启用终止保护的情况下，启动 Web 服务器
* 监控您的 EC2 实例
* 修改您的 Web 服务器使用的安全组以允许 HTTP 访问
* 调整 Amazon EC2 实例的大小以进行扩展或缩减
* 探索 EC2 限制
* 测试终止保护
* 终止 EC2 实例

&nbsp;

### 持续时间

完成本实验大约需要 **35 分钟**。

&nbsp;
&nbsp;
## 访问 AWS 管理控制台

1. 在这些说明的顶部，单击 <span id="ssb_voc_grey">Start Lab</span>（启动实验）启动您的实验。

   “Start Lab”（启动实验）面板随即会打开，其中显示了实验状态。

2. 请耐心等待，直到您看到“**Lab status: ready**”（实验状态：就绪）消息，然后单击 **X** 关闭 Start Lab（启动实验）面板。

3. 在这些说明的顶部，单击 <span id="ssb_voc_grey">AWS</span>

   在您执行此操作后，AWS 管理控制台将会在一个新的浏览器标签页中打开。您将自动登录系统。

   **提示：**如果未打开新的浏览器选项卡，则您的浏览器顶部通常会出现一个横幅或图标，表明您的浏览器阻止该网站打开弹出窗口。单击横幅或图标，然后选择 Allow pop ups（允许弹出窗口）。

4. 排列 AWS 管理控制台选项卡，使其与这些说明一起显示。理想情况下，您将能够同时看到这两个浏览器选项卡，以便更轻松地执行实验步骤。

&nbsp;
&nbsp;
## 任务 1：启动 Amazon EC2 实例

在此任务中，您将启动带有_终止保护_的 Amazon EC2 实例。终止保护可以防止您意外终止 EC2 实例。您将使用用户数据脚本部署实例，该脚本使您能够部署简单的 Web 服务器。

5. 在 **AWS 管理控制台**的 **Services**（服务）菜单上，单击 **EC2**。

6. 选择 <span id="ssb_orange">Launch Instance <i class="fas fa-caret-down"></i></span>（启动实例）。然后选择 <span id="ssb_white">Launch Instance</span>（启动实例）

### 第 1 步：选择 Amazon 系统映像 (AMI)

<i class="fas fa-info-circle"></i>**Amazon 系统映像 (AMI)** 提供启动实例（云中的虚拟服务器）所需的信息。AMI 包括：

* 用于实例根卷的模板（例如，操作系统或者包含应用程序的应用程序服务器）
* 启动许可，用于控制哪些 AWS 账户可以使用 AMI 来启动实例。
* 块储存设备映射，用于指定在实例启动时要向其挂载的卷

**Quick Start**（快速启动）列表包含最常用的 AMI。您也可以创建自己的 AMI 或从 AWS Marketplace 选择一个 AMI。AWS Marketplace 是一个在线商店，您可以在其中销售或购买在 AWS 上运行的软件。

7. 单击 **Amazon Linux 2 AMI**（列表顶部）旁边的 <span id="ssb_blue">Select</span>（选择）。

&nbsp;

### 第 2 步：选择实例类型

<i class="fas fa-info-circle"></i>Amazon EC2 提供了多种经过优化的_实例类型_，适合不同的使用案例。实例类型包括由 CPU、内存、存储和网络容量组成的不同组合，使您能够灵活地为应用程序选择适当的资源组合。每种实例类型都包含一个或多个_实例大小_，使您能够根据目标工作负载的需求扩展资源。

默认情况下，您要使用的 **t2.micro** 实例应处于选中状态<i class="fas fa-square" style="color:blue"></i>。此实例类型具有 1 个虚拟 CPU 和 1GiB 内存。**注意**：本实验可能会限制您使用其他实例类型。

8. 单击 <span id="ssb_grey">Next: Configure Instance Details</span>（下一步：配置实例详细信息）

&nbsp;
### 第 3 步：配置实例详细信息

您可以在此页面配置实例以满足您的需求。这包括联网和监控设置。

**Network**（网络）指明您希望在哪个 Virtual Private Cloud (VPC) 中启动实例。您可以拥有多个网络，例如分别用于开发、测试和生产的不同网络。

9. 对于 **Network**（网络），选择 **Lab VPC**。

   Lab VPC 是在实验的设置过程中使用 AWS CloudFormation 模板创建的。此 VPC 包含在两个不同的可用区中的两个公有子网。

10. 对于 **Enable termination protection**（启用终止保护），请选择 <i class="far fa-check-square"></i> **Protect against accidental termination**（防止意外终止）。

<i class="fas fa-info-circle"></i>当不再需要某个 Amazon EC2 实例时，可以将其_终止_，这意味着该实例已停止并释放了其资源。实例一旦终止就无法再次启动。如果要防止实例意外终止，可以为实例启用_终止保护_，以防止实例终止。

11. 向下滚动，然后展开 <i class="fas fa-caret-right"></i> **Advanced Details**（高级详细信息）。

此时将显示 **User data**（用户数据）字段。

<i class="fas fa-info-circle"></i>当您启动实例时，可以将_用户数据_传递到实例，用户数据可用于执行常见的自动配置任务甚至在实例启动后运行脚本。

您的实例运行的是 Amazon Linux，因此需要提供一个将在实例启动时运行的 _Shell 脚本_。

12. 复制以下命令并将其粘贴到 **User data**（用户数据）字段中：

    ```plain
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
    ```

    该脚本将执行以下操作：
    
    * 安装 Apache Web 服务器 (httpd)
    * 将 Web 服务器配置为在引导时自动启动
    * 激活 Web 服务器
    * 创建简单的网页


13. 单击 <span id="ssb_grey">Next: Add Storage</span>（下一步：添加存储）


&nbsp;
### 第 4 步：添加存储

<i class="fas fa-info-circle"></i>Amazon EC2 将数据存储在名为 *Elastic Block Store* 的网络挂载的虚拟磁盘上。

您将使用默认的 8GiB 磁盘卷启动 Amazon EC2 实例。该卷将是您的根卷（也称为“启动卷”）。

14. 单击 <span id="ssb_grey">Next: Add Tags</span>（下一步：添加标签）

&nbsp;
### 第 5 步：添加标签

<i class="fas fa-info-circle"></i>标签让您能够按各种标准（例如用途、拥有者或环境）对 AWS 资源进行分类。这在您具有许多同类资源时会很有用 &mdash; 您可以根据分配给资源的标签快速识别特定资源。每个标签都包含您定义的一个键和一个值。

15. 单击 <span id="ssb_grey">Add Tag</span>（添加标签），然后进行以下配置：

   * **Key**（键）：`Name`
   * **Value**（值）：`Web Server`

16. 单击 <span id="ssb_grey">Next: Configure Security Group</span>（下一步：配置安全组）

&nbsp;
### 第 6 步：配置安全组

<i class="fas fa-info-circle"></i>_安全组_将起到虚拟防火墙的作用，用于控制一个或多个实例的流量。在您启动某个实例时，会将一个或多个安全组与该实例相关联。您可以向每个安全组添加_规则_，以允许进出关联实例的流量。您可以随时修改安全组的规则；新规则将自动应用于与该安全组关联的所有实例。

17. 在**第 6 步：配置安全组**中，进行以下配置：

   * **Security group name**（安全组名称）：`Web Server security group`
   * **Description**（描述）：`Security group for my web server`

   在本实验中，您无法使用 SSH 登录您的实例。删除 SSH 访问权限将提高实例的安全性。

18. 删除<i class="fas fa-times-circle"></i>现有的 SSH 规则。

19. 单击 <span id="ssb_blue">Review and Launch</span>（审核和启动）

&nbsp;
### 第 7 步：核查实例启动

“Review”（核查）页面将显示您要启动的实例的配置。

20. 单击 <span id="ssb_blue">Launch</span>（启动）

此时将显示 **Select an existing key pair or create a new key pair**（选择现有密钥对或创建新密钥对）窗口。

<i class="fas fa-info-circle"></i>Amazon EC2 使用公有密钥加密法来加密和解密登录信息。要登录您的实例，您必须创建密钥对，并在启动该实例时指定密钥对名称，然后在连接到该实例时提供私有密钥。

在本实验中，您无需登录实例，因此不需要密钥对。

21. 单击 **Choose an existing key pair** <i class="fas fa-angle-down"></i>（选择现有密钥对）下拉菜单，然后选择 *Proceed without a key pair*（在没有密钥对的情况下继续）。

22. 选择 <i class="far fa-check-square"></i> **I acknowledge that ...**（我确认...）。

23. 单击 <span id="ssb_blue">Launch Instance</span>（启动实例）

   此时，您的实例将会启动。

24. 单击 <span id="ssb_blue">View Instances</span>（查看实例）

   该实例的状态将显示为 _pending_（等待中），这表示它正在启动。随后，它的状态将变为 _running_（正在运行），这表示该实例已开始启动。一小段时间之后，您就可以访问该实例了。

   该实例将获得一个_公有 DNS 名称_，您可使用此名称通过互联网与实例通信。

   您的 <i class="fas fa-square" style="color:blue"></i> **Web Server**（Web 服务器）必须处于选中状态。**Description**（描述）选项卡显示有关您的实例的详细信息。

   <i class="fas fa-comment"></i>要在 Description（描述）选项卡中查看更多信息，请向上拖动窗口分隔线。

   查看 **Description**（描述）选项卡中显示的信息。其中包括有关实例类型、安全设置和网络设置的信息。

25. 等待您的实例显示以下内容：

* **Instance State**（实例状态）：<span style="color:green"><i class="fas fa-circle"></i></span>running（正在运行）
* **Status Checks:**（状态检查）：<span style="color:green"><i class="fas fa-check-circle"></i></span>2/2 checks passed（2/2 已通过检查）

<span style="color:blue"><i class="far fa-thumbs-up"></i></span> **恭喜！**您已成功启动自己的第一个 Amazon EC2 实例。

&nbsp;
&nbsp;
## 任务 2：监控实例

监控是保持 Amazon Elastic Compute Cloud (Amazon EC2) 实例和 AWS 解决方案可靠性、可用性和性能的重要一环。

26. 单击 **Status Checks**（状态检查）选项卡。

   <i class="fas fa-info-circle"></i>使用实例状态监控，您可以快速确定 Amazon EC2 是否已经检测到可能阻止您的实例运行应用程序的任何问题。Amazon EC2 将自动对所有正在运行的 EC2 实例执行检查，以识别硬件和软件问题。

   请注意，本实验中的实例已通过 **System reachability**（系统可访问性）和 **Instance reachability**（实例可访问性）检查。

27. 单击 **Monitoring**（监控）选项卡。

   此选项卡显示您的实例的 Amazon CloudWatch 指标。目前，此选项卡显示的指标并不多，因为实例是最近才启动的。

   您可以单击图表来查看放大图。

   <i class="fas fa-info-circle"></i>Amazon EC2 将指标发送到用于您的 EC2 实例的 Amazon CloudWatch。默认情况下，基本监控（5 分钟）处于启用状态。您可以启用详细监控（1 分钟）。

28. 在 <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span>（操作）菜单中，选择**Monitor and troubleshoot**（监控并排查问题）<i class="fas fa-caret-right"></i> **Get System Log**（获取系统日志）。

   系统日志将显示实例的控制台输出，这是一种很有价值的工具，可用于问题诊断。它尤其适合用于排查内核问题和服务配置问题，它们可能会导致实例在 SSH 后台程序启动前终止或变得不可访问。如果您没有看到系统日志，请等待几分钟，然后重试。

29. 滚动浏览输出，请注意，HTTP 包是在您创建实例时利用您添加的**用户数据**安装的。

<img src="images/Console-output.png" alt="控制台输出" width="600">

30. 选择 **Cancel**（取消）。

31. 在 <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span>（操作）菜单中，选择**Monitor and troubleshoot**（监控并排查问题）<i class="fas fa-caret-right"></i> **Get Instance Screenshot**（获取实例屏幕截图）。

   系统此时会向您展示当屏幕连接到 Amazon EC2 实例控制台时，该控制台的外观。

<img src="images/Screen-shot.png" alt="屏幕截图" width="600">

   <i class="fas fa-info-circle"></i>如果您无法通过 SSH 或 RDP 访问您的实例，可以截取实例的屏幕截图，以图像形式查看。这可以让您了解实例的状态，更快地进行故障排查。

32. 选择 **Cancel**（取消）。

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **恭喜！**您已探索了多种监控实例的方法。

&nbsp;
&nbsp;
## 任务 3：更新安全组并访问 Web 服务器

启动 EC2 实例时，您提供了一个脚本，该脚本安装了 Web 服务器并创建了一个简单的网页。在此任务中，您将从该 Web 服务器访问内容。


33. 单击 **Details**（详细信息）选项卡。

34. 将您实例的 **IPv4 Public IP**（IPv4 公有 IP）复制到剪贴板。

35. 在 Web 浏览器中打开一个新选项卡，粘贴您刚刚复制的 IP 地址，然后按 **Enter** 键。

   **问题**：您能否访问您的 Web 服务器？为什么无法访问？

   您目前**无法**访问您的网络服务器，因为_安全组_不允许端口 80 上的入站流量，该端口用于 HTTP Web 请求。这演示了如何将安全组用作防火墙来限制允许进出实例的网络流量。

   要解决此问题，您现在应更新安全组以允许端口 80 上的 Web 流量。

36. 让浏览器选项卡保持打开状态，同时返回 **EC2 管理控制台**选项卡。

37. 在左侧导航窗格中，单击 **Security Groups**（安全组）。

38. 选择 <i class="fas fa-square" style="color:blue"></i> **Web Server security group**（Web 服务器安全组）。

39. 单击 **Inbound**（入站）选项卡。

   此安全组当前没有规则。

40. 单击 <span id="ssb_grey_square">Edit inboud rules</span>（编辑入站规则），然后完成以下配置：

   * **Type**（类型）：_HTTP_
   * **Source**（源）：_Anywhere_（任何位置）
   * 单击 <span id="ssb_orange">Save rules</span>（保存规则）

41. 返回您之前打开的 Web 服务器选项卡，然后刷新<i class="fas fa-sync"></i>页面。

   您应该会看到以下消息：_Hello From Your Web Server!_（Web 服务器向您问好！）

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **恭喜！**您已成功修改安全组以允许 HTTP 流量进入您的 Amazon EC2 实例。

&nbsp;
&nbsp;
## 任务 4：调整实例大小：实例类型和 EBS 卷

随着您的需求变化，您可能会发现您的实例被过度使用（过小）或未得到充分使用（过大）。如果是这样，您可以更改_实例类型_。例如，如果 _t2.micro_ 实例对于其工作负载来说过小，您可将其更改为 _m5.medium_ 实例。同样，您可以更改磁盘的大小。


### 停止实例

在调整实例大小之前，您必须将其_停止_。

<i class="fas fa-info-circle"></i>当您停止实例时，它会关闭。已停止的 EC2 实例不会产生任何费用，但 Amazon EBS 卷仍会产生存储费用。

42. 在 **EC2 管理控制台**中，单击左侧导航窗格中的 **Instances**（实例）。

   <i class="fas fa-square" style="color:blue"></i> **Web Server** 应已经选中。

43. 在 <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span>（实例状态）菜单中，选择 **Stop instance**（停止实例）。

44. 选择 <span id="ssb_orange">Stop</span>（停止）

   您的实例将执行正常的关机操作，然后将停止运行。

45. 等待 **Instance State**（实例状态）显示：<span style="color:red"><i class="fas fa-circle"></i></span>stopped（已停止）

### 更改实例类型

46. 在 <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span>（操作）菜单中，选择 **Instance Settings**（实例设置）<i class="fas fa-caret-right"></i> **Change Instance Type**（更改实例类型），然后进行以下配置：

   * **Instance Type**（实例类型）：*t2.small*
   * 选择 <span id="ssb_orange">Apply</span>（应用）

   当您再次启动该实例时，它将是 _t2.small_，其内存是 _t2.micro_ 实例的两倍。**注意**：本实验可能会限制您使用其他实例类型。

### 调整 EBS 卷的大小

47. 在左侧导航菜单中，单击 **Volumes**（卷）。

48. 在 <span id="ssb_grey">Actions <i class="fas fa-caret-down"></i></span>（操作）菜单中，选择 **Modify Volume**（修改卷）。

   当前磁盘卷的大小为 8GiB。现在，您将增加该磁盘的大小。

49. 将大小更改为：`10` **注意**：本实验可能会限制您创建较大的 Amazon EBS 卷。

50. 选择 <span id="ssb_grey">Modify</span>（修改）

51. 选择 <span id="ssb_blue">Yes</span>（是）以确认并增加卷的大小。

52. 选择 <span id="ssb_blue">Close</span>（关闭）

### 启动调整大小后的实例

现在，您将再次启动该实例，该实例现在将具有更多的内存和更多的磁盘空间。

53. 在左侧导航窗格中，单击 **Instances**（实例）。

54. 在 <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span>（实例状态）菜单中，选择 **Start instance**（启动实例）。

55. 选择 <span id="ssb_orange">Start</span>（启动）。

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **恭喜！**您已成功调整 Amazon EC2 实例的大小。在此任务中，您已将实例类型从 _t2.micro _ 更改为 _t2.small_。您还将根磁盘卷的大小从 8GiB 修改为了 10GiB。

&nbsp;
&nbsp;
## 任务 5：了解 EC2 限制

Amazon EC2 提供了不同的资源供您使用。这些资源包括映像、实例、卷和快照。在您创建 AWS 账户时，应遵循这些资源在每个区域的默认限制。

56. 在左侧导航窗格中，单击 **Limits**（限制）。

57. 从下拉列表中，选择 **Running instances**（正在运行的实例）。

   请注意，您可在此区域中启动的实例数存在限制。在您启动实例时，该请求不得导致您的用量超出您在该区域的当前实例限制。

   您可以请求提高其中许多限制。

&nbsp;
&nbsp;
## 任务 6：测试终止保护

当您不再需要实例时，可将其删除。这项操作称为_终止_实例。实例终止后，您将无法连接或重启该实例。

在此任务中，您将了解如何使用_终止保护_功能。

58. 在左侧导航窗格中，单击 **Instances**（实例）。

59. 在 <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span>（实例状态）菜单中，选择 **Terminate instance**（终止实例）。

60. 然后选择 <span id="ssb_orange">Terminate</span>（终止）

   请注意，会出现这样一则消息：*Failed to terminate the instance i-1234567xxx.The instance 'i-1234567xxx' may not be terminated.Modify its 'disableApiTermination' instance attribute and try again.*（无法终止实例 i-1234567xxx。实例 i-1234567xxx 可能不会终止。请修改其 disableApiTermination 实例属性，然后重试。）

   这是一种保护措施，用于防止意外终止实例。如果确实要终止该实例，则您需要禁用终止保护。

61. 在 <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span>（操作）菜单中，选择 **Instance Settings**（实例设置）<i class="fas fa-caret-right"></i> **Change Termination Protection**（更改终止保护）。

62. 移除对 <i class="far fa-square"></i> **Enable**（启用）旁复选框的选择。

63. 选择 <span id="ssb_orange">Save</span>（保存）

   现在，您可以终止该实例。

64. 在 <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span>（实例状态）菜单中，选择 **Terminate**（终止）。

65. 选择 <span id="ssb_orange">Terminate</span>（终止）

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **恭喜！**您已成功测试终止保护并终止了您的实例。

&nbsp;
&nbsp;

## 实验完成

<i class="icon-flag-checkered"></i>恭喜！您已完成本实验。

66. 单击此页面顶部的 <span id="ssb_voc_grey">End Lab</span>（结束实验），然后单击 <span id="ssb_blue">Yes</span>（是）确认您要结束实验。  

   此时将显示一个面板，指示“DELETE has been initiated...You may close this message box now.”（删除操作已启动...您现在可以关闭此消息框。）

67. 单击右上角的 **X** 关闭面板。

如有反馈、建议或更正，请发送电子邮件至：*aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
## 其他资源

* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html" target="_blank">启动实例</a>
* <a href="https://aws.amazon.com/ec2/instance-types" target="_blank">Amazon EC2 实例类型</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html" target="_blank">Amazon 系统映像 (AMI)</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html" target="_blank">Amazon EC2 – 用户数据和 Shell 脚本</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html" target="_blank">Amazon EC2 根设备卷</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html" target="_blank">标记 Amazon EC2 资源</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html" target="_blank">安全组</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html" target="_blank">Amazon EC2 密钥对</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html?icmpid=docs_ec2_console" target="_blank">实例状态检查</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html" target="_blank">获取控制台输出和重启实例</a>
* <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html" target="_blank">Amazon EC2 指标和维度</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html" target="_blank">调整实例大小</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html" target="_blank">停止和启动实例</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html" target="_blank">Amazon EC2 服务限制</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">终止实例</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">实例终止保护</a>
