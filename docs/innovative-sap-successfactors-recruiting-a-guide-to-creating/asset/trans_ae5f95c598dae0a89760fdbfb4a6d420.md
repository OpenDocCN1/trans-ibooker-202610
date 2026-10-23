

## 第1章 引言

![](img/879ce8537ecb7481ad77093e6fcbe574_18_0.png)

图1-2. ODATA数据字典中与职位申请（JobRequisition）相关的实体列表

选择“职位申请（JobRequisition）”，该实体将详细显示，各字段的定义如图1-3所示。

![](img/879ce8537ecb7481ad77093e6fcbe574_19_0.png)

图1-3. ODATA数据字典中职位申请（JobRequisition）实体的字段

除了展示职位申请（JobRequisition）实体内的字段外，还显示了支持的操作。这些操作包括：查询（Query）、插入（Insert）、合并（Merge）、替换（Replace）、更新或插入（Upsert）和删除（Delete）。并非所有实体都支持所有操作。某个实体可能支持的操作可以在ODATA数据字典和参考指南中找到。

我们探索数据模型的第二种方式是通过集成中心（Integration Center）。这将在第2章中介绍。

### 如何执行简单的ODATA调用

### 前提条件

我们可以快速获取所有实体的列表，同时确认我们可以使用系统管理员提供的用户名和密码访问ODATA API。您还需要SAP SuccessFactors公司ID。

访问ODATA API的端点URL取决于托管您的SAP SuccessFactors实例的数据中心。我们将在示例中使用一个演示系统。端点为 https://apisalesdemo8.successfactors.com/。

访问您的数据中心的数据中心URL可以在知识库文章（KBA）中找到，链接在此 <https://launchpad.support.sap.com/#/notes/2605498>。您需要一个SAP用户（例如S用户）才能访问此站点。

要查询实体列表，URL为 `<Endpoint>/odata/v2`。我们的URL是 `<https://apisalesdemo8.successfactors.com/odata/v2>`。

系统会提示您输入用户名和密码。用户名格式为 `<user@companyid>`。该用户应为集成用户，且对系统的访问权限有限。如果您负责创建角色和用户，请阅读“设置所需的用户和权限”部分。

### 设置所需的用户和权限

集成用户应仅拥有足以发出ODATA请求的权限，不应有更多权限。他们不应访问任何人工用户会执行的前端功能。他们的密码还应设置为永不过期，因为过期的密码会导致SuccessFactors收到的任何API调用失败。

应为集成用户创建一个单独基于角色的权限（RBP）角色和组。假设您在阅读本书时已了解基于角色的权限的基础知识。图1-4显示了为招聘集成用户推荐的权限。可能并非所有ODATA权限都是必需的。基础对象或MDF对象权限（如地点（Location））也可能需要读取访问权限。

| 角色权限部分 | 权限 | 备注 |
| :------------------------- | :------------------------- | :------------------------- |
| 常规用户权限 | 用户登录 | |
| | 登录方式（密码） | 如果启用了SSO |
| | 创建表单的权限（勾选任何职位申请模板） | 如果将通过集成创建职位申请 |
| 招聘权限 | 勾选所有ODATA权限 | |

![](img/879ce8537ecb7481ad77093e6fcbe574_20_0.png)

如果您在自己的计算机上执行API测试，并且您是唯一使用该集成用户的人，那么密码过期并不是什么大问题。在生产环境中，这对集成来说是灾难性的。需要对密码过期策略进行例外处理。

要设置密码过期例外，请在SuccessFactors屏幕右上角的操作搜索中键入“密码（Password）”。“密码和登录策略例外”将是选项之一。也可以使用Admin Center中的搜索工具，如图1-5所示。

Admin Center

![](img/879ce8537ecb7481ad77093e6fcbe574_21_0.png)

图1-5. 密码和登录策略设置屏幕

点击“设置API登录例外”。现在您可以通过点击“添加”按钮来添加例外。输入集成用户名，并将最大密码期限设置为“-1”以表示永不过期，或根据您组织的策略设置其他期限。需要设置IP地址限制。这将限制用户可以从哪些IP地址调用您的SuccessFactors实例。例如，调用SuccessFactors的评估解决方案的IP地址。

### 从浏览器调用SuccessFactors

打开您最喜欢的浏览器，输入您的SuccessFactors实例的ODATA端点。对于我们的演示实例，它是 https://apisalesdemo8.successfactors.com/odata/v2。系统会提示您输入用户名和密码。用户名格式为 username@companyid。点击登录后，您将看到所有ODATA实体的列表，如图1-6所示。恭喜！您已成功执行了第一个ODATA API请求。

此XML文件似乎没有任何相关的样式信息。文档树如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<workspaces>
  <collection title="Default" />
  <collection title="GoalComment_1" />
  <collection href="BenefitProgramEnrollmentDetail" title="BenefitProgramEnrollmentDetail" />
  <collection href="JobRequisitionPostingFieldControls" title="JobRequisitionPostingFieldControls" />
  <collection href="GoalPermission_101" title="GoalPermission_101" />
  <collection href="InnerMessage" title="InnerMessage" />
  <collection href="JobApplicationStatusLabel" title="JobApplicationStatusLabel" />
  <collection href="BenefitInsuranceDependentDetail" title="BenefitInsuranceDependentDetail" />
  <collection href="ReportSmartMapping" title="ReportSmartMapping" />
  <collection href="SpotAwardProgramAdvancedSettings" title="SpotAwardProgramAdvancedSettings" />
  <collection href="TodoAction" title="TodoAction" />
  <collection href="UserResult" title="UserResult" />
  <collection href="InterviewOverallAssessment" title="InterviewOverallAssessment" />
</workspaces>
```

图1-6. 简单ODATA请求的结果

我们究竟做了什么？我们执行了一个 *GET* 请求，这意味着我们请求数据，而不请求对SuccessFactors数据库进行任何更改。进行此类更改需要用到POST请求。我们没有指定想要查看的实体类型，例如职位申请（requisition）。每个ODATA请求都是通过http(s) URI（统一资源标识符）完成的。我们没有指定任何参数来表明我们想要返回哪些数据，因此返回了完整的数据模型。可以完全使用浏览器来执行请求。一旦您开始在不同的环境中工作，将请求串联起来使请求参数基于先前请求的结果，以及诸如自动化测试等其他需求，您就需要投资一个API应用程序，我们将在第1.3节中看到。熟悉一个应用程序所需的小小学习曲线与它带来的好处相比显得微不足道。

### 示例：使用Postman读取职位申请

我们的目标是检索所有状态为“已批准（Approved）”的职位申请。我们将读取 *JobRequisition* 实体，并针对标记为“内部状态”的字段进行筛选。该字段名为 *internalStatus*。该字段的值会在职位申请被创建、批准和关闭时发生变化。

### Postman入门

有许多用于测试和自动化HTTP API交互的桌面应用程序和基于浏览器的工具。我们打算使用Postman，因为它有一个免费版本，并且是较常用的工具之一。它还非常适合协作。您可能与同事（例如集成顾问或后端Web开发人员）一起工作。能够快速构建API调用并将其导出以供同事使用，可以极大地提高工作效率。

Postman应用程序可以在 www.postman.com/downloads/ 找到。它适用于Mac、Windows和Linux。Postman也有一个网页版本。我们将演示使用桌面应用程序。我们将只教授足以执行必要API调用的Postman知识，但建议您查看位于 https://learning.postman.com/docs/getting-started/introduction/ 的Postman学习中心。图1-7显示了Postman的初始视图。

![](img/879ce8537ecb7481ad77093e6fcbe574_23_0.png)

图1-7. Postman初始视图或点击新建（New）

我们将开始创建一个新环境（New Environment）。这意味着我们不必记住SuccessFactors API URI。将来，它还将使我们能够轻松地在演示环境或客户环境之间切换。

点击“环境（Environment）”，然后将名称从“新环境（New Environment）”更改为有意义的名字，例如“Mark的演示环境”。在其下方，添加一个变量名“Host”，并将初始值设置为您的数据中心对应的ODATA API URL，例如 https://apisalesdemo8.successfactors.com/odata/v2 。添加一个用户名字段变量，使其为 user@companyid。

