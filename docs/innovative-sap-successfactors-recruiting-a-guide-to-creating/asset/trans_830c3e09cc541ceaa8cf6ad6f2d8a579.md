

+   4. `orderStatus` 作为响应的一部分被发送。可接受的值只有两个：OK 和 ERROR。通常，如果供应商无法对同步请求作出响应，则设置为 ERROR。
+   5. `responseDetails` 和 `responseCode` 也由供应商设置。这些仅用于信息目的。

## 步骤 18：为选项 2 创建 JobApplicationBackgroundCheckRequest 实体

当背景调查通过 Intelligent Services 发起时，适用此步骤。由于控制完全在供应商手中，该记录可以通过 API 调用创建。如果没有匹配的背景调查请求，系统将插入一条新记录，响应字段 `editStatus` 显示为 INSERTED，否则为 UPDATED。

实际的 API 调用为：POST https://apisalesdemo4.successfactors.com:443/odata/v2/upsert?$format=json

```
{
  "__metadata": {
    "uri": "JobApplicationBackgroundCheckRequest",
    "type": "SFOData.JobApplicationBackgroundCheckRequest"
  },
  "vendorCode": "MYTHICAL",
  "requestId": 140,
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "orderStatus": "OK",
  "applicationId": 3040,
  "responseCode": "200",
  "responseDetail": "Order received and acknowledged."
}
```

响应载荷：

```json
{
  "d": [
    {
      "key": "JobApplicationBackgroundCheckRequest/requestId=140",
      "status": "OK",
      "editStatus": "UPDATED",
      "message": "JobApplicationBackgroundCheckRequest Updated successfully for request 140",
      "index": 0,
      "httpCode": 204,
      "inlineResults": null
    }
  ]
}
```

步骤 17 中的注意事项同样适用于此操作。

## 步骤 19：背景调查 – 获取候选人详细信息

本节介绍了如何构建您的集成。以下示例使用 JobApplicationBackgroundCheckRequest 作为顶级实体，并导航到其他实体以获取应用程序的额外信息。关于是否使用 JobApplication 或 Candidate 实体来构建查询，没有具体指导。选择权在您。响应仅显示部分结果。

```http
GET: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckRequest?$format=json&$select=vendorCode,applicationId,jobApplication/firstName,jobApplication/country,jobApplication/lastName,jobApplication/contactEmail,jobApplication/dateOfBirth,jobApplication/address,jobApplication/address2,jobApplication/city,jobApplication/gender,jobApplication/zip,jobApplication/state,jobApplication/status,jobApplication/state/picklistLabels/label&$filter=requestId eq '140'&$expand=jobApplication,jobApplication/state/picklistLabels
```

响应：

```json
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckRequest(140L)",
          "type": "SFOData.JobApplicationBackgroundCheckRequest"
        },
        "applicationId": "3040",
        "vendorCode": "MYTHICAL",
        "jobApplication": {
          "__metadata": {
            "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplication(3040L)",
            "type": "SFOData.JobApplication"
          },
          "zip": "94123",
          "firstName": "Theon",
          "country": "United States",
          "lastName": "Greyjoy",
          "address": "100 Main Street",
          "gender": "Male",
          "contactEmail": "athanur+24@gmail.com",
          "address2": null,
          "city": "San Francisco",
          "dateOfBirth": "/Date(534574800000+0000)/",
          "status": "Open",
          "state": {
            "results": [
              {
                "__metadata": {
```
```
"uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistOption(11645L)",
"type": "SFData.PicklistOption"
},
"id": "11645",
"minValue": "-1",
"externalCode": "state_United_States_California",
"maxValue": "-1",
"optionValue": "-1",
"sortOrder": 20,
"mdfExternalCode": "state_United_States_California",
"status": "ACTIVE",
"parentPicklistOption": {
"__deferred": {
"uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistOption(11645L)/parentPicklistOption"
}
}
},
"picklistLabels": {
"results": [
{
"__metadata": {
"uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistLabel(locale='en_US',optionId=11645L)",
"type": "SFData.PicklistLabel"
```

```
},
"optionId": "11645",
"locale": "en_US",
"id": "12076",
"label": "California",
"picklistOption": {
  "__deferred": {
    "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistLabel(locale='en_US',optionId=11645L)/picklistOption"
  }
}
},
{
  "__metadata": {
    "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistLabel(locale='fr_FR',optionId=11645L)",
    "type": "SFOData.PicklistLabel"
  },
  "optionId": "11645",
  "locale": "fr_FR",
  "id": "120541",
  "label": "Californie",
```

```
"picklistOption": {
  "__deferred": {
    "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/PicklistLabel(locale='fr_FR',optionId=11645L)/picklistOption"
  }
}
},
}
(...only partial response shown...)
]
}
}
```

## 步骤 20：向 JobApplicationBackgroundCheckResult 插入记录

注意：SuccessFactors 并不隐含特定的包构建结构，因为该解决方案必须适用于所有供应商。规范越严格，第三方供应商实施就越困难。在本示例中，我们将模拟一个背景调查请求，它是完全任意的。

结构如下：

综合背景调查（主包）– 与此相关的 API 调用将包含一个显示在 UI 中的占位 reportUrl：
- 驾照（子包）
- 雇佣记录（子包）

包之间没有隐含的层级关系。因此，我们建立了一种伪父子关系。您可以将此作为示例，或者简单地在每个背景调查中使用一个包。

POST: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult?$format=json

请求载荷：

```
{
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "vendorCode": "MYTHICAL",
  "stepName": "Driver's License",
  "stepStatus": "Received",
  "stepMessage": "Background Check is in process. Please check back later",
  "finalStep": false
}
```

响应载荷：

```
{
  "d": {
    "__metadata": {
      "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult(140L)",
      "type": "SFODdata.JobApplicationBackgroundCheckResult"
    },
    "vendorOrderNo": "MYTHICAL_12112019_87654",
    "vendorCode": "MYTHICAL",
    "stepName": "Driver's License",
    "stepStatus": "Received",
    "stepMessage": "Background Check is in process. Please check back later",
    "finalStep": false,
    "statusId": "140"
  }
}
```

**注意事项：**

+   1.  `stepName` 和 `stepStatus` 被视为键值对，并在 UI 中可见。如何构建完全由供应商定义。
+   2.  系统仅检查 `stepName` 是否在上一次调用中存在。请密切注意此值。“Driver’s License”与“Drivers License”不同。系统将在 UI 中创建两条条目，一条用于“Driver’s License”，另一条用于“Drivers License”。
+   3.  `stepMessage` 仅在 ODATA 审计日志中可见，仅供信息参考。
+   4.  `finalStep` 旨在显示结果已完成，尽管在技术上对将值设置为 true 或 false 的次数没有限制。
+   5.  `statusId` 是系统生成的值，无法更新。

图 10-32 展示了一个确认 API 调用结果的示例 UI 屏幕。

![](img/879ce8537ecb7481ad77093e6fcbe574_280_0.png)

**图 10-32.** *API 调用的 UI 确认*

以下是更新先前调用结果的示例：

POST: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult?$format=json

请求载荷：

```
{
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "vendorCode": "MYTHICAL",
  "stepName": "Driver's License",
  "stepStatus": "Completed - no discrepancies",
  "stepMessage": "Background Check is in process. Please check back later",
  "finalStep": false
}
```

响应：

```
{
  "d": {
    "__metadata": {
      "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult(141L)",
      "type": "SFOData.JobApplicationBackgroundCheckResult"
    },
    "vendorOrderNo": "MYTHICAL_12112019_87654",
    "vendorCode": "MYTHICAL",
    "stepName": "Driver's License",
    "stepStatus": "Completed - no discrepancies",
    "stepMessage": "Background Check is in process. Please check back later",
    "finalStep": false,
    "statusId": "141"
  }
}
```

