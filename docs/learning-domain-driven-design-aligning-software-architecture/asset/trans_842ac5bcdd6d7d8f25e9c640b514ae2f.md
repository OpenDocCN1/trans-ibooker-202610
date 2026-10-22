

```
"event-type": "followup-set",
"followup-on": "2020-05-27T12:00:00.00Z",
"timestamp": "2020-05-20T12:32:08.24Z"
},
{
  "lead-id": 12,
  "event-id": 3,
  "event-type": "contact-details-updated",
  "first-name": "Casey",
  "last-name": "Davis",
  "phone-number": "555-8101",
  "timestamp": "2020-05-20T12:32:08.24Z"
},
{
  "lead-id": 12,
  "event-id": 4,
  "event-type": "contacted",
  "timestamp": "2020-05-27T12:02:12.51Z"
},
{
  "lead-id": 12,
  "event-id": 5,
  "event-type": "order-submitted",
  "payment-deadline": "2020-05-30T12:02:12.51Z",
  "timestamp": "2020-05-27T12:02:12.51Z"
},
{
  "lead-id": 12,
  "event-id": 6,
  "event-type": "payment-confirmed",
  "status": "converted",
  "timestamp": "2020-05-27T12:38:44.12Z"
}
```

列表中的事件讲述了客户的故事。线索在系统中创建（事件 0），两小时后被销售代理联系（事件 1）。通话期间，双方约定销售代理一周后回电（事件 2），但使用不同的电话号码（事件 3）。销售代理还修正了姓氏中的拼写错误（事件 3）。在约定日期和时间，线索被联系到（事件 4）并提交了订单（事件 5）。订单需要在三天内付款（事件 5），但付款在半小时后就收到了（事件 6），线索被转化为新客户。

正如我们之前看到的，客户的状态可以很容易地从这些领域事件中投影出来。我们所要做的就是将简单的转换逻辑依次应用于每个事件：

```
public class LeadSearchModelProjection
{
    public long LeadId { get; private set; }
    public HashSet<string> FirstNames { get; private set; }
    public HashSet<string> LastNames { get; private set; }
    public HashSet<PhoneNumber> PhoneNumbers { get; private set; }
    public int Version { get; private set; }

    public void Apply(LeadInitialized @event)
    {
        LeadId = @event.LeadId;
        FirstNames = new HashSet<string>();
        LastNames = new HashSet<string>();
        PhoneNumbers = new HashSet<PhoneNumber>();
        FirstNames.Add(@event.FirstName);
        LastNames.Add(@event.LastName);
        PhoneNumbers.Add(@event.PhoneNumber);
        Version = 0;
    }

    public void Apply(ContactDetailsChanged @event)
    {
        FirstNames.Add(@event.FirstName);
        LastNames.Add(@event.LastName);
        PhoneNumbers.Add(@event.PhoneNumber);
        Version += 1;
    }

    public void Apply(Contacted @event)
    {
        Version += 1;
    }

    public void Apply(FollowupSet @event)
    {
        Version += 1;
    }
}
```

```
public void Apply(OrderSubmitted @event)
{
    Version += 1;
}

public void Apply(PaymentConfirmed @event)
{
    Version += 1;
}
}
```

对聚合的事件进行迭代并依次将其馈送到 Apply 方法的适当重写中，将精确地产生表 7-1 中建模的状态表示。

请注意在应用每个事件后递增的 Version 字段。它的值表示对业务实体所做的修改总数。此外，假设我们应用事件的子集。在这种情况下，我们可以“穿越时间”：我们可以通过仅应用相关事件来投影实体在其生命周期中的任何点的状态。例如，如果我们需要实体在版本 5 的状态，我们只需应用前五个事件即可。

最后，我们并不局限于仅投影事件的单一状态表示！请考虑以下场景。

# Search

你需要实现一个搜索功能。然而，由于线索的联系信息可能会被更新——名字、姓氏和电话号码——销售代理可能不知道其他代理所做的更改，并可能希望使用其联系信息，包括历史值，来定位线索。我们可以轻松地投影历史信息：

```
public class LeadSearchModelProjection
{
    public long LeadId { get; private set; }
```

```
public HashSet<string> FirstNames { get; private set; }
public HashSet<string> LastNames { get; private set; }
public HashSet<PhoneNumber> PhoneNumbers { get; private set; }
public int Version { get; private set; }

public void Apply(LeadInitialized @event)
{
    LeadId = @event.LeadId;
    FirstNames = new HashSet<string>();
    LastNames = new HashSet<string>();
    PhoneNumbers = new HashSet<PhoneNumber>();

    FirstNames.Add(@event.FirstName);
    LastNames.Add(@event.LastName);
    PhoneNumbers.Add(@event.PhoneNumber);

    Version = 0;
}

public void Apply(ContactDetailsChanged @event)
{
    FirstNames.Add(@event.FirstName);
    LastNames.Add(@event.LastName);
    PhoneNumbers.Add(@event.PhoneNumber);

    Version += 1;
}

public void Apply(Contacted @event)
{
Version += 1;
}

public void Apply(FollowupSet @event)
{
Version += 1;
}

public void Apply(OrderSubmitted @event)
{
Version += 1;
}

public void Apply(PaymentConfirmed @event)
{
```

```
Version += 1;
}
}
```

投影逻辑使用 LeadInitialized 和 ContactDetailsChanged 事件来填充线索个人详细信息的相应集合。其他事件被忽略，因为它们不影响特定模型的状态。

将此投影逻辑应用于前面示例中 Casey Davis 的事件将产生以下状态：

```
LeadId: 12
FirstNames: ['Casey']
LastNames: ['David', 'Davis']
PhoneNumbers: ['555-2951', '555-8101']
Version: 6
```

# Analysis

你的商业智能部门要求你提供更便于分析的线索数据表示。对于他们当前的研究，他们希望获取不同线索所安排的后续电话数量。稍后他们将筛选已转化和已关闭的线索数据，并使用该模型来优化销售流程。让我们投影他们要求的数据：

```
public class AnalysisModelProjection
{
    public long LeadId { get; private set; }
    public int Followups { get; private set; }
    public LeadStatus Status { get; private set; }
    public int Version { get; private set; }

    public void Apply(LeadInitialized @event)
    {
        LeadId = @event.LeadId;
        Followups = 0;
        Status = LeadStatus.NEW_LEAD;
        Version = 0;

```
}
public void Apply(Contacted @event)
{
    Version += 1;
}

public void Apply(FollowupSet @event)
{
    Status = LeadStatus.FOLLOWUP_SET;
    Followups += 1;
    Version += 1;
}

public void Apply(ContactDetailsChanged @event)
{
    Version += 1;
}

public void Apply(OrderSubmitted @event)
{
    Status = LeadStatus.PENDING_PAYMENT;
    Version += 1;
}

public void Apply(PaymentConfirmed @event)
{
    Status = LeadStatus.CONVERTED;
    Version += 1;
}
}
```

前述逻辑维护了一个计数器，用于统计后续事件在线索事件中出现的次数。如果我们将此投影应用于聚合事件的示例，它将生成以下状态：

```
LeadId: 12
Followups: 1
Status: Converted
Version: 6
```

前述示例中实现的逻辑在内存中投影了搜索优化和分析优化的模型。然而，要实际实现所需的功能，我们必须将投影的模型持久化到数据库中。在第 8 章中，你将学习一种允许我们这样做的模式：命令查询职责分离（CQRS）。

# Source of Truth

要使事件溯源模式发挥作用，对对象状态的所有更改都应表示为事件并作为事件持久化。这些事件成为系统的事实来源（因此得名）。该过程如图 7-1 所示。

![](img/b742a71dea5d5dd509847f82600a6ad1_165_0.png)

图 7-1. 事件溯源聚合

存储系统事件的数据库是唯一强一致的存储：系统的事实来源。用于持久化事件的数据库的通用名称是事件存储。

# Event Store

事件存储不应允许修改或删除事件²，因为它是一个仅追加存储。为了支持事件溯源模式的实现，事件存储至少必须支持以下功能：获取属于特定业务实体的所有事件并追加事件。例如：

