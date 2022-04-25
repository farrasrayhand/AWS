# 模块 3 – 指导实验：托管静态网站
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## 实验概览和目标
静态网站中的内容固定，并且没有后端处理。它们可以包含 HTML 页面、图像、样式表和呈现网站所需的所有文件。然而，静态网站不使用服务器端脚本或数据库。如果您希望自己的静态网页提供交互性并运行编程逻辑，可以使用在用户的 Web 浏览器中运行的 JavaScript。

通过上传内容并允许公开访问所上传的内容，您可以轻松在 Amazon Simple Storage Service (Amazon S3) 上托管静态网站。不需要服务器，您可以使用 Amazon S3 在任何时间从 Web 上的任何地方存储和检索任何数量的数据。

完成本实验后，您应能够：

- 在 Amazon S3 中创建存储桶
- 将内容上传到存储桶
- 允许访问存储桶对象
- 更新网站

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
## 任务 1：在 Amazon S3 中创建存储桶

在此任务中，您将创建一个 S3 存储桶并对其进行配置，以便托管静态网站。

5. 在 **AWS 管理控制台**中的 <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span>（服务）菜单上，单击 **S3**。

6. 选择 <span id="ssb_orange">Create bucket</span>（创建存储桶）

   S3 存储桶的名称是全局唯一的，而且该命名空间由所有 AWS 账户共享。在您创建一个存储桶之后，该存储桶的名称不能被任何 AWS 区域中的其他 AWS 账户使用，除非您删除了该存储桶。

   因此，在本实验中，您将使用包含随机编号的存储桶名称，例如：_website-123_

7. 对于**存储桶名称**，输入`“website-<123>”`（用随机编号替代 _123_

   默认情况下，禁止对存储桶的公共访问。由于需要通过互联网访问静态网站中的文件，因此您必须允许公共访问。

8. 清除选中 **Block all public access**（阻止所有公开访问），然后选中 **I acknowledge that the current settings may result in this bucket and the objects within becoming public**（我了解，当前设置可能会导致此存储桶及其中的对象被公开）复选框。

9. 选择 <span id="ssb_orange">Create bucket</span>（创建存储桶）。

   您可以使用标签向存储桶添加其他信息，例如项目代码、成本中心或所有者。

10. 点击新存储桶的名称。

11. 选择 **Properties**（属性）选项卡。

12. 滚动到 **Tags**（标签）面板。

13. 选择 <span id="ssb_white">Edit</span>（编辑），然后选择 <span id="ssb_white">Add tag</span>（添加标签）并输入：

    * **Key**（键）：`Department`
    * **Value**（值）：`Marketing`

14. 选择 <span id="ssb_orange">Save Changes</span>（保存更改）以保存标签。

    现在，您将配置存储桶，以便用于静态网站托管。

15. 停留在 **Properties**（属性）控制台。

16. 滚动到 **Static website hosting**（静态网站托管）面板。

17. 选择 <span id="ssb_white">Edit</span>（编辑）

18. 配置以下设置：

    - **Static web hosting**（静态网站托管）：启用
    - **Hosting type**（托管类型）：托管静态网站
    - **Index document**（索引文档）：`index.html`
       - **注意**：即使已显示此值，也必须输入该值。
    - **Error document**（错误文档）：`error.html`

19. 选择 <span id="ssb_orange">Save Changes</span>（保存更改）

20. 选择 **Bucket website endpoint**（存储桶网站终端节点）下的链接。

    因为尚未配置存储桶权限，您将收到 *403 Forbidden* 消息。在 Web 浏览器中将此选项卡保持打开状态，以便稍后可以返回该选项卡。

    现在，您的存储桶已配置为托管静态网站。

<br/>
## 任务 2：将内容上传到存储桶

在此任务中，您会将静态文件上传到存储桶。

21. 右键单击各个链接并将文件下载到您的计算机上：

    <i class="fas fa-exclamation-triangle"></i> 请确保每个文件的文件名保持不变，包括扩展名在内。

    - [index.html](../../../scripts/index.html)
    - [script.js](../../../scripts/script.js)
    - [style.css](../../../scripts/style.css)

22. 返回 Amazon S3 控制台，然后单击 **Objects（对象）**选项卡。

23. 选择 <span id="ssb_orange">Upload</span>（上传）

24. 选择 <span id="ssb_white">Add files</span>（添加文件）

25. 选择您已下载的 3 个文件。

26. 选择 <i class="far fa-check-square"> I acknowledge that existing objects with the same name will be overwritten.</i>（我确认具有相同名称的现有对象将被覆盖。）

27. 选择 <span id="ssb_orange">Upload</span>（上传）

    您的文件将上传到存储桶。

<br/>
任务 3：允许访问对象

默认情况下，存储在 Amazon S3 中的对象是私有的。这可以确保贵组织的数据保持安全。

在此任务中，您要将上传的对象设置为可公开访问。

首先，您需要确认这些对象目前是私有的。

28. 返回到显示 *403 Forbidden* 消息的浏览器标签页。

29. 刷新 <i class="fas fa-sync"></i> 网页。

    <i class="fas fa-comment"></i> 如果您无意中关闭了此标签页，请转到 **Properties**（属性）选项卡，然后再次在 **Static website hosting**（静态网站托管）面板中选择 **Endpoint**（终端节点）链接。

    您应该仍然会看到消息 *403 Forbidden* 的消息。预计会收到答复！此消息表明您的静态网站由 Amazon S3 托管，但内容是私有的。

    您可以通过两种不同的方式将 Amazon S3 对象公开：

    – 要将整个存储桶公开或存储桶中的特定目录公开，请使用*存储桶策略*。
    – 要将存储桶中的单个对象公开，请使用*访问控制列表 (ACL)*。

    公开_单个对象_通常会更安全，因为这样可以避免意外将其他对象公开。但是，如果您知道整个存储桶不包含敏感信息，则可以使用_存储桶策略_。

    现在，您可以将单个对象配置为可公开访问。

30. 使用 Amazon S3 控制台返回 Web 浏览器标签页（但不要关闭该网站标签页）。

31. 选择 <i class="far fa-check-square"></i> 所有三个对象。

32. 在 <span id="ssb_s3_blue">Actions <i class="fas fa-angle-down"></i></span>（操作）菜单中，选择 **Make public**（设为公开）。

此时将显示三个对象的列表。

33. 选择 <span id="ssb_s3_blue">Make public</span>（设为公开）

您的静态网站现在就可以公开访问。

34. 返回到含有 *403 Forbidden* 消息的 Web 浏览器标签页。

35. 刷新 <i class="fas fa-sync"></i> 网页。

您现在应该可以看到由 Amazon S3 托管的静态网站。

<br/>
## 任务 4：更新网站

您可以通过编辑 HTML 文件并将其再次上传到 S3 存储桶来更改网站。

Amazon S3 是一项_对象存储服务_，因此您必须上传整个文件。此操作将替换存储桶中的现有对象。您不能编辑对象的内容，而是必须替换整个对象。

36. 在您的计算机上，将 **index.html** 文件加载到文本编辑器（例如 Notepad 或 TextEdit）中。

37. 查找文本 **Served from Amazon S3**，然后将其替换为 `Created by <您的名字>`，将 <*您的名字*> 替换为您的名字（例如，_Created by Jane_)。

38. 保存文件。

39. 返回 Amazon S3 控制台并上传您刚才编辑的 **index.html** 文件。

40. 选择 <i class="far fa-check-square"></i> **index.html**，然后使用 **Actions**（操作）菜单再次选择 **Make public**（设为公开）选项。

41. 通过静态网站返回到 Web 浏览器标签页，然后刷新 <i class="fas fa-sync"></i> 页面。

    您的名字现在应该在页面上了。



现在可以在互联网上访问您的静态网站。由于该网站托管在 Amazon S3 上，因此该网站具有高可用性，可以在不使用任何服务器的情况下提供大量流量。

您还可以使用自己的域名将用户定向到托管在 Amazon S3 上的静态网站。为此，您可以将 Amazon Route 53 域名系统 (DNS) 服务与 Amazon S3 结合使用。

<br/>
## 提交作业

42. 在这些说明的顶部，选择 <span id="ssb_blue">Submit</span>（提交）记录您的进度，在出现提示时，选择 **Yes**（是）。

43. 如果在几分钟后仍未显示结果，请返回到这些说明的顶部，并选择 <span id="ssb_voc_grey">Grades</span>（成绩）

    **提示**：您可以多次提交作业。更改作业后，再次选择 **Submit**（提交）即可。您最后一次提交的作业将记录为本实验的成绩。

44. 要查找有关您作业的详细反馈，请选择 <span id="ssb_voc_grey">Details</span>（详细信息），然后选择 <i class="fas fa-caret-right"></i> **View Submission Report**（查看提交报告）。

<br/>

## 实验完成 <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> 恭喜！您已完成实验。

45. 选择此页面顶部的 <span id="ssb_voc_grey">End Lab</span>（结束实验），然后选择 <span id="ssb_blue">Yes</span>（是）确认您要结束实验。

    此时将显示一个面板，表明 *DELETE has been initiated... You may close this message box now.*（删除操作已启动...您现在可以关闭此消息框。）

46. 选择右上角的 **X** 以关闭面板。



*©2020 Amazon Web Services, Inc. 及其附属公司。保留所有权利。未经 Amazon Web Services, Inc. 事先书面许可，不得复制或转载本文的部分或全部内容。禁止商业性复制、出租或出售。*