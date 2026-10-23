
**表12-2.（续）**

| 事件名称 | 事件描述 | 事件类型 | OData 实体类型 | 实体键和数据类型 | 参数键和数据类型 |
|---|---|---|---|---|---|
| 候选档案更新 | 当候选档案发生更新时触发此事件。 | CandidateProfile.Updated | Candidate | candidateId (string) | None |
| 启动职位发布 | 当职位发布到外部招聘网站时触发此事件。 | JobPosting.Change | JobRequisitionPosting | jobPostingId (string) | jobReqId (string) jobPostingType (string) |
| 将候选人的国家/地区更新为俄罗斯 | 当候选人的国家/地区更改为俄罗斯时触发此事件。 | CandidateProfileUpdated.CountryUpdated | Candidate | candidateId (string) | isCountryChangeEvent (string) |

第1章介绍了OData API的基本内容。你也可以通过执行 GET <<endpoint>>odata/v2/getExtEventMetaDataDefinition 来获取客户实例（或任何实例）中的事件；但请记住，没有筛选条件，你将获得实例中的所有事件。

## 订阅事件

设置与智能服务集成的第一步是确定要订阅哪个事件。为此，请导航至管理中心 ➤ ISC（智能服务中心）。图12-2展示了本文档所述的七个招聘事件。本节将全程以“Update of Job Requisition”作为示例进行说明。

![](img/879ce8537ecb7481ad77093e6fcbe574_314_0.png)

*图12-2. 智能服务中心部分截图，展示招聘事件*

为了说明这一点，我们将重点关注 Update of Job Requisition 事件，并展示代码的编程示例。选择 Update of Job Requisition 后，将进入 ISC 主页面，如图12-3所示。

![](img/879ce8537ecb7481ad77093e6fcbe574_314_1.png)

*图12-3. Update of Job Requisition 事件的详细信息*

点击 Event Connector 打开定义连接器的页面。请注意，第10章展示了使用 Update of Job Application Status 的另一个示例。点击图12-3中的 Event Connector 以显示事件连接器。图12-4展示了事件连接器。

![](img/879ce8537ecb7481ad77093e6fcbe574_315_0.png)

*图12-4. 定义事件连接器*

![](img/879ce8537ecb7481ad77093e6fcbe574_316_0.png)

*图12-5. 定义虚构的事件连接器*

图12-5展示了一个未配置任何身份验证的完整事件连接器。请注意，Name 和 Endpoint URL 是必填字段，且 Endpoint URL 是虚构的。

SAP SuccessFactors 提供三种身份验证选项：

-   无（None）（如图12-5所示）
-   基本（Basic）（如图12-6所示）

![](img/879ce8537ecb7481ad77093e6fcbe574_317_0.png)

*图12-6. 使用基本身份验证的事件订阅*

必填字段为用户名和密码。

> **注意** 基本身份验证将于2021年底弃用，到2022年底将不再作为选项提供。因此，你应该提前规划，使用“None”或“OAuth2 with Client Credentials Grant”。

-   OAuth2 with Client Credentials Grant（如图12-7所示）。

![](img/879ce8537ecb7481ad77093e6fcbe574_318_0.png)

*图12-7. 使用 OAuth2 with Client Credentials Grant 的事件订阅*

必填字段为 Client ID、Client Secret 和 Token Endpoint，而 Scope 是可选的。这些属性在第三方平台上定义。无论选择哪种身份验证选项，都必须点击“Add”来注册订阅。最后一步是确保你的流程已保存，如图12-8所示。

![](img/879ce8537ecb7481ad77093e6fcbe574_318_1.png)

*图12-8. 保存你的最终流程*

在本章前面，我们描述了如何通过执行 GET <<endpoint>>odata/v2/getExtEventMetaDataDefinition（在 Postman 或浏览器中）以编程方式获取所有事件。以下是“Update of Job Requisition”事件的响应负载：

```json
{
  "description": "This event is raised when an approved or closed requisition is updated.",
  "effectiveDated": false,
  "entity": "JobRequisition",
  "entityKeys": {
    "results": [
      {
        "description": null,
        "descriptionMessageKey": null,
        "name": "jobReqId",
        "type": "STRING"
      },
      {
        "description": null,
        "descriptionMessageKey": null,
        "name": "startDate",
        "type": "DATE"
      }
    ]
  },
  "name": "Update of Job Requisition",
  "params": {
    "results": [
      {
        "description": null,
        "descriptionMessageKey": null,
        "hasValueAlways": false,
        "name": "updatedFieldsList",
        "type": "STRING"
      },
      {
        "description": null,
        "descriptionMessageKey": null,
        "hasValueAlways": false,
        "name": "isOnboardingInitiated",
        "type": "STRING"
      }
    ]
  },
  "publisher": "Recruiting",
  "topic": "com.successfactors.recruiting.JobRequisition.Updated",
  "type": "JobRequisition.Updated"
}
```

配置好订阅并成功连接外部系统后，负载将通过 SOAP 消息传递，如下所示：

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns6:ExternalEvent xmlns:ns6="http://notification.event.successfactors.com" xmlns:ns5="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns7="com.successfactors.event.notification" xmlns:ns2="com.successfactors.alert" xmlns:ns4="http://alert.successfactors.com" xmlns:ns3="http://www.boomi.com/connector/wss">
    <ns6:externalEventMeta>
        <ns6:externalEventId>b772edb5-9f59-4b53-8595-6e45d08c0756</ns6:externalEventId>
        <ns6:type>com.successfactors.recruiting.JobRequisition.Updated</ns6:type>
        <ns6:publishedAt>1621793760938</ns6:publishedAt>
        <ns6:publishedBy>sfadmin</ns6:publishedBy>
        <ns6:repost>false</ns6:repost>
    </ns6:externalEventMeta>
    <ns6:events>
        <ns6:event>
            <ns6:eventId>fa477244-08c2-4293-aa65-2bad255b75d9</ns6:eventId>
            <ns6:entityType>JobRequisition</ns6:entityType>
            <ns6:publishedAt>1621793760929</ns6:publishedAt>
            <ns6:publishedBy>sfadmin</ns6:publishedBy>
            <ns6:repost>false</ns6:repost>
            <ns6:entityKeys>
                <ns6:entityKey>
                    <name>jobReqId</name>
                    <value>2561</value>
                </ns6:entityKey>
            </ns6:entityKeys>
            <ns6:params>
                <ns6:param>
                    <name>updatedFieldsList</name>
                    <value>workHours[en_US], experienceReq</value>
                </ns6:param>
                <ns6:param>
                    <name>isOnboardingInitiated</name>
                    <value>false</value>
                </ns6:param>
            </ns6:params>
        </ns6:event>
    </ns6:events>
</ns6:ExternalEvent>
```

关于 SOAP 负载的说明：

1. 有效负载是在成功检索事件后从外部系统获取的。在此示例中，外部系统是 SAP 的业务技术平台（BTP），一个中间件。
2. 适用于外部实体的唯一命名空间是 ns7。但是，最佳实践是确保第三方软件设置为忽略命名空间。
3. externalEventId 是应用于整个数据中心的属性，而 eventId 特定于当前实例。
4. publishedAt 以相对于 UTC 的 Unix 纪元时间表示。注意，有两个 publishedAt 属性，相隔几毫秒。以 https://currentmillis.com 作为参考，我们可以确定 1621793760929 转换为 UTC 日期和时间是“Sun May 23 2021 18:16:00”，本地时间（太平洋夏令时间）是“Sun May 23 2021 11:16:00”。
5. publishedBy 是当前客户实例中用户的 userId。在此示例中，它是销售演示实例中的 twalker（Tessa Walker）。
6. updateFieldsList 参数集合表明，属性“workHours[en_US]”和“experienceReq”已在 JobRequisition 的“entityType”上发生更改。你可以使用这些参数来确定后续操作。
7. SOAP 负载中的“type”属性表示所涉及的基础事件，即 JobRequisition.Updated。

## 集成故障排除

集成设置完成后，接下来的合理步骤是确定集成是否正常工作。为此，请访问管理中心 ➤ ISC（智能服务中心）。你将看到 SuccessFactors 中触发的事件。

![](img/879ce8537ecb7481ad77093e6fcbe574_322_0.png)

*图12-9. 智能服务中心显示为 Update of Job Requisition 触发的事件*
