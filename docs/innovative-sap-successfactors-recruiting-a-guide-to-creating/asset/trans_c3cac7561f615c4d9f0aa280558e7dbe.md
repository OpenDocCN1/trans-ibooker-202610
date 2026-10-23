

![](img/879ce8537ecb7481ad77093e6fcbe574_73_0.png)

## 图3-9：数据输出预览

为了添加 JobRequisition 和 JobReqScreeningQuestionChoice 字段，我们将使用“+ Add”按钮，然后从菜单中选择“Add Field”来添加列。字段将从左侧的 Entity Tree View 选项中选择。向下滚动到 JobRequisition 以添加 JobRequisition 字段，再滚动到 JobReqScreeningQuestionChoices 以显示多项选择的问题选项。选择某个实体后，该实体的字段或导航选项将出现在右侧。图3-10显示了通过 choices 导航添加 Option Label 字段的过程。要在此屏幕上添加多个字段而无需离开，请勾选左下角的“Add Another”，并在选择每个字段后点击“Add Association”。完成后点击 Cancel。

![](img/879ce8537ecb7481ad77093e6fcbe574_73_1.png)

## 图3-10：向集成输出添加字段

当查看图3-11中的输出时，我们会发现一些奇怪之处。每个多项选择题只有一行。考虑到有多个选项，我们本期望会有多行。Question Id 列显示与问题库中 GUID 匹配的 Question Id。每个问题都只有一行。

![](img/879ce8537ecb7481ad77093e6fcbe574_74_0.png)

## 图3-11：职位申请问题输出示例

Integration Center 专为简单输出而设计，而我们刚刚展示了一个限制。我们将使用 Postman 来更全面地了解 JobRequisitionScreeningQuestion 和 JobReqScreeningQuestionChoice 实体。

## 使用 Postman 请求职位申请问题选项

在第1章中，我们使用 Postman 向 SAP SuccessFactors 发出了一次 ODATA 调用，以检索一些职位申请信息。在本示例中，我们将拉取与该职位申请关联的实体。除了多项选择题的选项外，我们还将请求与该职位申请关联的问题。

登录 SAP SuccessFactors Recruiting 的测试或演示环境，找到一条关联了不止一个问题的职位申请。其中应至少有一个多项选择题。如果你没有这样的数据，可以新建一个带问题的职位申请，或向现有职位申请添加问题。记下该职位申请的 ID。

打开 Postman 应用，确保选择了你在第1章中创建的环境，如图3-12所示。在使用多个环境时，一个常见错误是以为数据或 HTTP 请求出了问题，而实际上只是选错了环境。

我的职位申请的 URI 是 {{Host}}/JobReqScreeningQuestion?$format=json&$select=jobReqId,jobReqContent,questionId,questionName,questionType&$filter=jobReqId eq 55。

让我们来检查一下上述参数：

$format=json

在第1章中，ODATA 响应的消息正文使用的是 XML 格式。JSON 格式是一种开放、轻量级的数据交换标准，具有“自描述”性，也就是说，机器和人都很容易阅读。它被 Web 应用程序与服务器通信时广泛使用。你会注意到，这种输出读起来容易得多。

$select=jobReqId,jobReqContent,questionId,questionName,questionType
$filter=jobReqId eq 55

这指定职位申请 ID 为55。对于你的 SAP SuccessFactors 系统，这个 ID 很可能不同。在第1章中，我们使用了 URI host/JobRequisition?internalStatus=Approved。那么现在为什么还需要使用 filter 参数呢？因为我们不是直接访问职位申请，而是通过 JobReqScreeningQuestion 访问。

点击 Send 运行查询，然后查看 JSON 格式的结果。结果应如图3-14所示。

![](img/879ce8537ecb7481ad77093e6fcbe574_76_0.png)

图3-14：JSON 格式的 ODATA GET 响应

![](img/879ce8537ecb7481ad77093e6fcbe574_78_0.png)

## 图3-15：JobReqScreeningQuestion 的 JSON 结果（包含选项）

在 results 标签内，每个问题由以下内容表示：

- URI – 直接访问该实体的 URI
- Type – ODATA 实体的类型
- 实体字段和值的列表
- 一个用于职位申请的 URI
- 每个问题的选项嵌套列表，包括选项的 URI、类型 JobReqScreeningQuestionChoice，以及每个选项的字段和值列表

你会注意到，选项 URI 中引用的实体是 JobReqScreeningQuestionChoice，而我们使用了 `$expand` 值“choices”，结果中也包含了选项的内容。这是因为，虽然 JobReqScreeningQuestionChoice 是实体，但从 JobReqScreeningQuestion 到 JobReqScreeningQuestionChoice 的导航值是“choices”。

在第一个示例中，我们使用的 URI 是 {{Host}}/JobReqScreeningQuestion?$format=json&$select=jobReqId,jobReqContent,questionId,questionName,questionType&$filter=jobReqId eq 55，结果中只包含了少量字段。第二个 URI `{{Host}}/JobReqScreeningQuestion?$expand=choices&$filter=jobReqId eq 55&$format=json` 除了包含每个选项的所有字段外，还包含问题的所有字段。既然第二个 URI 更简单，又能提供我们需要的所有信息，为什么不直接使用它呢？

在生产系统中，返回大量数据会降低响应速度。通过使用 `$select` 只返回足以应用业务逻辑的信息，随后便可用返回的 URI 拉取所需问题的数据。例如，如果只需要问题名称中包含“electronics”或“engineering”的题目的选项信息，就需要检索 questionName。原因是 questionName 不是可筛选字段。任何筛选都需要由中间件处理。

`$filter` 参数包含一些非常有用的运算符，例如 eq 表示等于、ne 表示不等于、gt 表示大于、lt 表示小于，此外还有 And、Or、Not 等。

ODATA V2 还有一些有用的筛选函数，例如 `substringof(string to match, field to match against)`。这方面的示例是 `/JobRequisitions$filter= substringof('Engineer',jobReqLocale/jobTitle) eq true`。这只会包含职位名称中含有字符串“Engineer”的职位申请。

有关运算符和函数的完整列表，请参见 www.odata.org/documentation/odata-version-2-0/uri-conventions/。

## 使用自定义配置向职位申请添加问题

正如我们在前言中所说，本书不会教你如何使用 SAP 云平台集成（CPI）等工具构建中间件集成。虽然集成开发人员会从本书中受益，但其目的是教客户和功能顾问如何构建概念验证，并正确描述接口所需规范。在这里，你将看到以下内容：

- 一个向职位申请添加问题的流程
- 一个用于配置在职位申请上创建问题的自定义元数据框架（MDF）对象示例
- 一个使用 Postman 调用向职位申请添加带多个选项的问题的示例

## 向职位申请添加问题的流程

- 触发向职位申请添加问题。
- 使用 MDF 对象配置将哪些问题添加到哪些职位申请中。
- 向职位申请添加问题。
- 更新职位申请，以表明更新成功。

## 触发向职位申请添加问题

任何需要添加到职位申请的问题都必须在职位申请发布前完成添加。我们不希望在问题添加之前，求职者就已经开始申请该职位。自动添加问题也应在招聘人员手动添加任何问题之前完成，否则他们可能会做无用功。

主要有两种方法可以触发向职位申请添加问题：

- 使用智能服务中心（ISC）中的“Update of Job Requisition”事件。
  - ISC 允许 SAP SuccessFactors 预定义的事件触发操作，例如发起出站 Integration Center 调用。
  - 由于每次创建或更新职位申请时都会触发该事件，因此可以使用 Integration Center 中的筛选器来确定哪些事件应触发添加问题。
  - 第12章将更详细地介绍 ISC 的使用。
- 使用来自云平台集成（CPI）的定期调度调用。该调用将检查尚未自动添加问题的职位申请。

在这两种方法中，我们更倾向于使用 ISC 的实时触发。它会在职位申请创建后的最早时间添加问题。它还免去了对 SAP SuccessFactors 的频繁调用，从而减少了 CPI 的开销。

为了简化集成并减少读取 JobReqScreeningQuestions 的开销，我们建议在职位申请中添加一个简单的布尔字段，用于表示问题已添加。然后，在确定是否需要添加问题时，可以将其用作筛选条件。由于该字段仅用于集成，因此不应让招聘用户看到。

