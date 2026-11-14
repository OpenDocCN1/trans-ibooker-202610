
退出您的 AWS 账户，并以根用户身份重新登录。输入虚拟设备生成的代码（或插入硬件令牌）并完成登录过程。

### 清理

您应始终为根用户启用 MFA。如果您想禁用与根用户关联的 MFA 设备，请按照[停用设备](https://oreil.ly/pvpVw)的步骤操作。

## 讨论

[根用户](https://oreil.ly/qNtiE)是 AWS 账户中权限最大、特权最高的身份。用户名是您在首次建立 AWS 账户时配置的电子邮件地址，密码重置请求可以从 AWS 控制台发起，或通过联系 AWS 支持发起。如果您的电子邮件账户被盗，恶意行为者可能会请求重置您的根账户密码并获得对您账户的访问权限。根用户应始终受到第二因素认证的保护，以防止未经授权的账户访问。由于仅在[极少数场合](https://oreil.ly/Em4TW)需要根用户，因此您绝不应将其用于日常任务。

#### 提示

您需要启用 IAM 用户和角色对账单控制台的访问。您可以按照 AWS [步骤](https://oreil.ly/DVPbZ)执行该操作，并进一步减少对根用户执行任务的依赖。

配置 IAM 用户（或使用 ADFS 与 IAM、SAML 与 IAM 或 AWS Single Sign-On 设置联合访问）至关重要，切勿使用根用户登录 AWS 账户进行日常使用。[AWS 建议在开设 AWS 账户后立即创建管理员 IAM 用户](https://oreil.ly/MuSdE)，这应是在您为根用户启用 MFA 后首先要做的事情之一。对于可扩展的方法，您可以遵循[配方 9.6](#ch09.html#setting_up_aws_organizations_and_aws_si)启用并使用 AWS Single Sign-On 以满足您自己的（和委托的）访问需求。

### 挑战

为您账户中的其他 IAM 角色配置 MFA 保护（使用信任策略）（这是一个[提示](https://oreil.ly/e8YMQ)）。

# 9.6 设置 AWS Organizations 和 AWS Single Sign-On

## 问题

您需要一种可扩展的方式来集中管理用户名和密码，以访问您的 AWS 账户。

## 解决方案

启用 AWS Organizations，配置 AWS Single Sign-On，创建组，创建权限集，并在 AWS SSO 目录中为您创建一个用户，如[图 9-19](#ch09.html#aws_sso_view_of_permissions_and_users)所示。

![](assets/awsc_0919.png)

### 步骤

1.  在 AWS 控制台中导航到[AWS Organizations](https://console.aws.amazon.com/organizations)，然后单击“创建组织”。

    #### 提示

    AWS Organizations 提供了许多功能，使您能够大规模地使用多个 AWS 账户。有关更多信息，请参阅[AWS Organizations 文档](https://oreil.ly/Wdaa4)。

2.  组织创建将触发一封电子邮件发送到与您账户的根用户关联的电子邮件地址。通过单击“验证您的电子邮件地址”按钮来确认组织的创建。

3.  在您的区域中导航到[AWS SSO 控制台](https://console.aws.amazon.com/singlesignon)，然后单击“启用 SSO”按钮。

    #### 注意

    AWS SSO 的初始配置为您的用户使用本地目录。您可以联合到现有的用户目录，如 Active Directory 或 SAML 提供商。有关配置联合的信息，[AWS SSO 用户指南](https://oreil.ly/PeusI)提供了详细信息。

4.  转到 SSO 控制台的“组”部分，然后单击“创建组”。输入所需的组名并单击“创建”，如[图 9-20](#ch09.html#creating_a_group_in_aws_sso)所示。

    ![](assets/awsc_0920.png)

5.  转到 SSO 控制台的“用户”部分，然后选择“添加用户”。填写字段中所需的信息，将用户添加到您创建的组中，然后选择“创建”。您可以让 AWS SSO 控制台为您生成初始密码，或将电子邮件发送到必填的“电子邮件地址”字段中提供的地址。在“添加用户”向导的第二页上，将用户分配到您在第 4 步中创建的组（请参阅[图 9-21](#ch09.html#creating_a_user_in_aws_sso)）。

    ![](assets/awsc_0921.png)

6.  转到 SSO 控制台的“AWS 账户”部分（在控制台左侧导航菜单中），选择“权限集”选项卡，然后选择“创建权限集”。选择“使用现有工作职能策略”选项并选择`PowerUserAccess`（请参阅[图 9-22](#ch09.html#creating_a_permission_set_in_aws_sso)）。

    ![](assets/awsc_0922.png)

7.  要将权限集分配给您为 AWS 账户创建的组，请单击左侧导航菜单中的“AWS 账户”链接，选择“AWS 组织”选项卡，从账户列表中选择您的 AWS 账户，然后单击“分配用户”。单击“组”选项卡并选择您在第 4 步中添加用户的组，如[图 9-23](#ch09.html#adding_a_group_to_an_assignment)所示。单击“下一步：权限集”。

    ![](assets/awsc_0923.png)

8.  选择您在第 6 步中创建的权限集，然后单击“完成”（请参阅[图 9-24](#ch09.html#assigning_a_permission_set_in_aws_sso)）。

    ![](assets/awsc_0924.png)

    #### 注意

    单击“完成”后，该组的成员可以使用`PowerUserAccess` IAM 策略的权限访问您指定的 AWS 账户。AWS SSO 会在您的 AWS 账户中预置一个可供此目的承担的角色。请勿通过 AWS 账户中的 IAM 修改此角色，否则权限集将无法通过 AWS SSO 使用。

#### 验证检查

转到 AWS SSO 控制台仪表板页面上提供的 URL，并使用您在第 5 步中创建的用户的用户名和密码登录。选择“管理控制台”或“命令行或程序化访问”以获得对 AWS 账户的`PowerUserAccess`权限。

#### 提示

首次启用 AWS SSO 时会生成默认 URL。您可以通过单击 SSO 控制台中默认 URL 旁边的“自定义”链接，为您自己的目的创建自定义 URL。

### 清理

转到 AWS SSO 控制台，从左侧导航菜单中选择“设置”。向下滚动到设置页面底部，然后选择“删除 AWS SSO 配置”。通过选中复选框并单击“删除 AWS SSO”来确认删除。

转到 AWS Organizations，选择您的组织，打开组织的设置，然后选择“删除组织”。您需要验证组织 ID，并通过与 AWS 账户根用户关联的电子邮件地址确认删除组织。

## 讨论

当您启用 AWS Organizations 时，您最初创建组织的账户称为[管理账户](https://oreil.ly/UTrvA)。虽然您启用 AWS Organizations 是为了在本配方中使用 AWS Single Sign-On，但它主要用于大规模治理、组织和管理 AWS 账户。您可能感兴趣的一些账户管理功能包括但不限于：

*   合并多个 AWS 账户的账单
*   使用[组织单元](https://oreil.ly/ZLn6v)（OUs）管理组账户
*   向单个账户或 OUs 应用[服务控制策略](https://oreil.ly/DeR3h)（SCPs）
*   为组织内的所有账户[集中管理策略](https://oreil.ly/4s2Nx)，用于标记和备份
*   使用[资源访问管理器](https://oreil.ly/eiNlp)（RAM）跨账户共享资源
