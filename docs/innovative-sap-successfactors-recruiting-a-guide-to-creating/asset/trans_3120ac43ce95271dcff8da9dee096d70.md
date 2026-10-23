

![](img/879ce8537ecb7481ad77093e6fcbe574_88_0.png)

# 图 4-1 候选人的来源示例

人才营销与寻源明显不同。在寻源中，寻源人员通过 Google 简历搜索、LinkedIn、社交媒体等渠道主动联系被动候选人（即没有在找工作的人）。申请人跟踪系统（ATS）中的候选人池是候选人的一个来源，但通常不会在其中找到被动候选人。招聘（Recruiting）是为特定招聘需求管理申请人招聘活动的过程。寻源和招聘的职责可能会混合在一起，尤其是在规模较小的组织中。

将候选人吸引到职位发布的初始环节只是候选人生命周期的一部分，如图 4-2 所示。从左到右可以看到候选人生命周期：

- 吸引（Attract）：职位发布被发布到不同渠道。
- 个性化（Personalize）：候选人可以选择订阅职位提醒和营销信息，以便收到与其兴趣相关的新职位发布通知。
- 互动（Engage）：候选人与招聘网站互动，通过聊天机器人、短信、电子邮件等各种渠道接收信息。
- 转化（Convert）：候选人通过主动表明对特定职位的兴趣而成为申请人的阶段。

• 招聘（Recruit）：这是传统意义上的申请人跟踪。

申请人会被筛选、面试、录用或淘汰。

先前寻源工作的努力会得到回报，因为可以邀请现有合格人才库中的候选人申请。此外，AI 搜索和匹配可以找到符合招聘需求的现有候选人。

招聘（Recruit）也成为未来人才互动的来源，因为那些合格但因其他候选人更胜一筹而被淘汰的候选人被视为“银牌获得者”，并被放入人才库，供未来的寻源和互动使用。

• 发展（Develop）：既支持内部流动，也利用员工推荐。

![](img/879ce8537ecb7481ad77093e6fcbe574_89_0.png)

# *候选人生命周期*

除了通过职位发布分发来开展人才营销外，候选人生命周期还包括候选人关系管理（CRM），即通过候选人自我兴趣识别、人才库管理和电子邮件活动自动与候选人互动，如前文“个性化”和“互动”阶段所述。

没有任何招聘解决方案能够解决所有问题。SAP SuccessFactors Recruiting 处于与它集成的互补解决方案生态系统之中。你可以在 SAP Store 上找到许多第三方解决方案（参见图 4-3）：https://store.sap.com/dcp/en/search/recruiting。请注意，第三方解决方案并不一定要在 SAP Store 上才能与 SAP SuccessFactors Recruiting 兼容。

![](img/879ce8537ecb7481ad77093e6fcbe574_90_0.png)

## 需要集成的候选人吸引相关业务场景

表 4-1 列出了常见的人才营销和寻源需求、可满足这些需求的第三方解决方案、与 SAP SuccessFactors Recruiting 集成所需的类型，以及示例供应商。请注意，所列供应商仅为示例，并非推荐。

## 第 4 章　候选人吸引

**表 4-1.** *需要集成的候选人吸引业务场景示例*

| 需求 | 解决方案 | 集成方式 | 示例供应商 |
|---|---|---|---|
| 校友寻源 | 校友解决方案集成 | 使用 Intelligent Services 和 Integration Center 导入候选人，通过 XML feed 发布职位 | Enterprise alumni, PeoplePath |
| 发布到小众招聘网站（如 OFCCP） | 招聘网站聚合器 | Recruiting Marketing XML feed | Direct employers |
| 早期职业/实习生招聘 | 大学招聘网站 | Recruiting Posting | |
| 通过短信进行互动式寻源 | AI 聊天机器人解决方案 | Integration Center 和 Intelligent Services | Paradox |

本章重点讨论如何通过将职位发布分享到所需人才经常访问的网站来吸引候选人。

尽管本书的主题是利用 SAP SuccessFactors 提供的广泛 API 进行创新，但大多数利用职位发布开展的候选人触达都是通过 SuccessFactors 预先提供的功能完成的，或者也可以提出请求。这通过两种方法实现：XML feed 和 Recruiting Posting 解决方案。

最常见的职位发布分发场景可以通过标准 XML feed 功能或使用 Recruiting Posting 来处理。

除了将职位发布到现有招聘网站之外，组织可能还希望在 Recruiting Marketing (RMK) 之外，将职位发布和搜索功能嵌入其现有网站。现有网站可能使用内容管理系统（CMS），如 Drupal 或 WordPress。乍一看，与组织的 Web 开发团队合作进行 ODATA 调用、拉取招聘需求和发布数据似乎顺理成章。然而，XML feed 是更好的选择。原因如下：

-   它省去了构建 ODATA REST 调用和解析 XML 或 JSON 结果的工作，因为 XML feed 被设计为只包含恰当信息的简单职位发布信息。
-   无需 API 用户即可调用。这是公开可用的 XML feed。这降低了复杂性，也减少了向 SAP SuccessFactors 发起调用的第三方数量。
-   如果发布需要按特定国家/地区、部门、职位职能等进行分类，则可以使用规则对 feed 进行预过滤。
-   申请 URL 可以为特定规则分配活动代码参数（UTM_Campaign 参数），以提供更准确的进阶分析信息。

## 案例研究：使用 WordPress 重新发布

一家企业软件咨询公司在其网站上有一个博客，涵盖许多主题，包括 SAP SuccessFactors Recruiting。该博客使用 WordPress 平台。当博客文章专注于 SAP SuccessFactors Recruiting 咨询主题时，公司希望将相关职位发布嵌入到博客文章中。职位列表需要自动更新，而无需编辑博客文章。我们的示例站点如图 4-4 所示。

![](img/879ce8537ecb7481ad77093e6fcbe574_92_0.png)

图 4-4. 包含博客文章的示例网站

## 第 4 章 候选人吸引

以下是一些关键要求：

-   面向顾问的博客文章应在底部自动包含相关职位发布列表。
-   每个职位发布都应在博客文章中作为带链接的内容呈现，并链接回 Recruiting Marketing 主招聘网站上的对应职位。我们的人才营销专家告诉我们，这些链接有利于搜索引擎优化（SEO）。
-   我们的首页通常会显示博客文章列表。我们不希望在这里包含职位发布，因为这会让网站看起来像招聘网站，而不是有实用内容的博客网站。

图 4-5 和图 4-6 展示了 Recruiting Command Center。

![](img/879ce8537ecb7481ad77093e6fcbe574_93_0.png)

**图 4-5.** *Recruiting Command Center Feed 列表*

## 第 4 章 候选人吸引

![](img/879ce8537ecb7481ad77093e6fcbe574_94_0.png)

# 图 4-6. Recruiting Command Center Feed 详情

职位发布 feed 的 XML 示例如下所示。请注意，你的 XML feed 可能会有所不同。

我们不希望所有职位发布都发送到 WordPress 网站。我们希望将博客读者与他们关心的职业相关的特定职位发布连接起来。在之前的示例中，我们使用了一条规则，只包含标题中包含“SuccessFactors Consultant”的职位发布。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<source>
  <publisher>myTestRMKSite</publisher>
  <publisherurl>jobs.myTestRMKSite.com</publisherurl>
  <jobs>
    <job>
      <title>Consultant</title>
      <date>Fri, 25 06 2021 00:00:00 GMT</date>
      <referencenumber>540-en_US</referencenumber>
      <url>https:// jobs.myTestRMKSite.com/job/Consultant-WA-98020/86338550/?feedId=18876&utm_source=WordPress&utm_campaign=SomeTestBlog</url>
    </job>
  </jobs>
</source>
```

