

## Active Directory 用户和计算机

+   文件
+   操作
+   查看
+   帮助

+   Active Directory 用户和计算机 [WEISSMAN-SERVER.weissman.edu]
+     已保存的查询
+     weissman.edu
+       Builtin
+       Canandaigua
+         Administrative
+         Clients
+           Faculty
+             Desktops
+             Laptops
+           Staff
+             Desktops
+             Laptops
+           Students
+             Desktops
+             Laptops
+       Groups
+         Bursar
+         Business Services
+         Facilities and Grounds
+         Faculty
+         Finance
+         Human Resources
+         Information Technology
+         Marketing
+         Registrar
+         Research and Development
+         Staff
+         Students
+       Servers
+         Application Servers
+         Database Servers
+         DHCP Servers
+         DNS Servers
+         Domain Controllers
+         Exchange Servers
+         File Servers
+         FTP Servers
+         Mail Servers
+         Print Servers
+         RADIUS Servers
+         Terminal Servers
+         VPN Servers
+         Web Servers
+       Users
+         Faculty
+         Staff
+         Students
+       Computers
+       Domain Controllers
+       ForeignSecurityPrincipals
+       Geneva
+       Managed Service Accounts
+       Newark
+       Users
+       Victor

## 图 14-3 weissman.edu 域

+   a. 在 Windows Server 2019 虚拟机上，打开服务器管理器，点击工具，然后点击 Active Directory 用户和计算机。你会注意到多个默认容器。点击每个容器，右侧窗格会显示其内容。Builtin 包含默认组对象。Computers 包含代表刚刚加入域的 Windows 10 虚拟机的计算机对象。双击该计算机对象可查看有关它的更多信息。Domain controllers 包含你当前所在的 Windows Server 2019 虚拟机的计算机对象。双击该计算机对象可查看有关它的更多信息。ForeignSecurityPrincipals 和 Managed Service Accounts 将为空。Users 包含默认用户对象和组对象。

+   b. 右键单击域名称（我的为 weissman.edu），将鼠标悬停在新建上，然后选择组织单位。

+   c. 在名称：文本框中，输入 Canandaigua，取消选中标记为防止意外删除而保护容器的复选框，然后单击确定按钮。

如果选中此框，并且你想删除该 OU，则需要从顶部菜单点击查看，然后选择高级功能（这是一个切换选择，因此再次点击查看然后高级功能会将视图恢复到原来的样子）。选中高级功能后，在域名称下层次结构中会出现更多项目，每个对象的属性列表中也会出现更多选项卡。右键单击要删除的 OU，选择属性，并在对象选项卡中（与其他选项卡一样，如果未选中高级功能则不可见），取消选中旁边标记为防止意外删除而保护对象的复选框。现在，该 OU 将可以被删除。如何执行删除操作将在后面介绍。如果你确实打开了高级功能，请在此处将其关闭。

+   d. 以同样的方式，并且直接在域名称下，创建以下 OU：Geneva、Newark 和 Victor。创建 OU 的顺序无关紧要。它们将在下次打开 Active Directory 用户和计算机时自动按字母顺序排列。

+   e. 直接在 Canandaigua OU 内部（这次，右键单击 Canandaigua OU，将鼠标悬停在新建上，然后选择组织单位），创建以下二级 OU：Administrative、Clients、Servers、Users 和 Groups。

+   f. 直接在 Clients OU 内部，创建以下三级 OU：Faculty、Staff 和 Students。

+   g. 直接在 Faculty OU、Students OU 和 Staff OU 内部，创建以下四级 OU：Desktops 和 Laptops。

+   h. 直接在 Servers OU 内部，创建以下三级 OU：Application Servers、Database Servers、DHCP Servers、DNS Servers、Domain Controllers、Exchange Servers、File Servers、FTP Servers、Mail Servers、Print Servers、RADIUS Servers、Terminal Servers 和 VPN Servers。

+   i. 直接在 Users OU 内部，创建以下三级 OU：Faculty、Staff 和 Students。

+   j. 直接在 Groups OU 内部，创建以下三级 OU：Bursar、Business Services、Facilities and Grounds、Faculty、Finance、Human Resources、Information Technology、Marketing、Registrar、Research and Development、Staff 和 Students。此处创建的 OU 将作为各个类别中各种组的容器。例如，Staff Level 1 不会拥有 Staff Level 2 的所有权限。

+   k. 展开所有 OU 并将你的工作与图 14-3 进行比较。

### 注意

可以在 AD 中创建计算机对象，使其从一开始就位于正确的 OU 中，从而使其受应用于 OU 的 GPO 控制。你会以与之前相同的方式将计算机添加到域中，但此方法不会将计算机对象放置在默认的 Computers 容器中。

2f

## 第 2 步 部门、员工角色、资源等会随时间变化。

网络的管理已经具有挑战性，而随着动态变化，挑战更大。然而，Active Directory 的结构允许通过简单步骤进行结构更改。

你即将移动、删除和重命名 OU。

+   a. FLCC 已将 Bursar 和 Registrar 部门合并为 One Stop Center（他们确实这样做了）。右键单击 Bursar OU，选择删除，并在提示确认要删除 OU 时单击是按钮。此 OU 内的任何内容也将被删除。
+   b. 右键单击 Registrar OU，选择重命名，输入 One Stop Center，然后按 ENTER。
+   c. 右键单击默认 Computers 容器中客户端计算机的计算机对象，选择移动...，展开 Canandaigua，展开 Clients，展开 Students，选择 Desktops，然后单击确定按钮。
+   d. 导航到 Desktops OU 并在右侧窗格中单击计算机对象。将该对象拖放到左侧窗格中 Students 内的 Laptops OU 中。单击弹出警告中关于移动对象时可能发生的情况的“是”按钮。
+   e. 导航到 Laptops OU 并在右侧窗格中单击计算机对象。按 CTRL-X 或右键单击计算机对象并选择剪切。单击 Desktops OU 并按 CTRL-V 或右键单击并选择粘贴。单击弹出警告中关于移动对象时可能发生的情况的“是”按钮。粘贴甚至可以在右侧窗格中完成，只要选择了正确的 OU 即可。有很多方法可以移动对象。OU 可以以与刚才移动计算机对象相同的方式移动。
+   f. 展开所有 OU 并选择 Desktops OU 以显示计算机对象。

![](img/6e921d997751f0a0f87e5170ac9657a1_449_0.png)

## 第 3 步

委派 OU 的控制权可以使大型组织在多个系统管理员之间划分角色和职责。例如，系统管理员可以管理几个域中的对象，所有

Principal 列。再次点击它以按反字母顺序排序。再次点击一次以按字母顺序排列项目。请注意，前六个条目的主体为 Account Operators，如图 14-4 所示。即使在步骤 3c 中你没有选择创建/删除 InetOrgPerson 对象、创建/删除计算机对象、创建/删除组对象或创建/删除用户对象，在委派控制向导中，Account Operators 组成员默认拥有这些权限。然而，Account Operators 组成员在你委派该权限之前无法更改密码。

![](img/6e921d997751f0a0f87e5170ac9657a1_451_0.png)

## 图 14-4 用户的高级安全设置

除了能够修改帐户和组中的

