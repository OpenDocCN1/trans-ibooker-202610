

```
interface IEventStore
{
    IEnumerable<Event> Fetch(Guid instanceId);
    void Append(Guid instanceId, Event[] newEvents, int expectedVersion);
}
```

Append 方法中的 expectedVersion 参数用于实现乐观并发管理：当你追加新事件时，同时指定了你所依据的实体的版本。如果该版本已过期，即在 expectedVersion 之后有新的事件被添加，事件存储应抛出并发异常。

在大多数系统中，实现 CQRS 模式还需要额外的端点，我们将在下一章讨论。

> NOTE
> 本质上，事件溯源模式并非新事物。金融行业使用事件来表示账本的变化。账本是一个只追加的日志，用于记录交易。当前状态（例如账户余额）始终可以通过“投影”账本的记录来推导。

## Event-Sourced Domain Model

原始领域模型维护其聚合体的状态表示并发出选定的领域事件。事件溯源领域模型则完全使用领域事件来建模聚合体的生命周期。对聚合体状态的任何更改都必须表示为领域事件。

对事件溯源聚合体的每次操作都遵循以下脚本：

- 加载聚合体的领域事件。
- 重建状态表示——将事件投影到可用于做出业务决策的状态表示中。
- 执行聚合体的命令以执行业务逻辑，并由此产生新的领域事件。
- 将新的领域事件提交到事件存储。

回到第 6 章中的 Ticket 聚合体的示例，让我们看看它将如何实现为事件溯源聚合体。

应用程序服务遵循前面描述的脚本：它加载相关票据的事件，重建聚合体实例，调用相关命令，并将更改持久化回数据库：

```
01 public class TicketAPI
02 {
03     private ITicketsRepository _ticketsRepository;
04     ...
05
06     public void RequestEscalation(TicketId id, EscalationReason reason)
07     {
08         var events = _ticketsRepository.LoadEvents(id);
09         var ticket = new Ticket(events);
10         var originalVersion = ticket.Version;
11         var cmd = new RequestEscalation(reason);
12         ticket.Execute(cmd);
13         _ticketsRepository.CommitChanges(ticket, originalVersion);
14     }
15
16     ...
17 }
```

Ticket 聚合体的重建逻辑在构造函数中（第 27 行至第 31 行）实例化状态投影器类 TicketState 的实例，并依次为票据的每个事件调用其 AppendEvent 方法：

```
18 public class Ticket
19 {
20     ...
21     private List<DomainEvent> _domainEvents = new List<DomainEvent>();
22     private TicketState _state;
23     ...
24
25     public Ticket(IEnumerable<IDomainEvents> events)
26     {
27         _state = new TicketState();
28         foreach (var e in events)
29         {
30             AppendEvent(e);
31         }
32     }
```

AppendEvent 将传入的事件传递给 TicketState 投影逻辑，从而生成票据当前状态的内存表示：

```
33     private void AppendEvent(IDomainEvent @event)
34     {
35         _domainEvents.Append(@event);
36         // Dynamically call the correct overload of the "Apply" method.
37         ((dynamic)state).Apply((dynamic)@event);
38     }
```

与上一章中看到的实现不同，事件溯源聚合体的 RequestEscalation 方法不会显式将 IsEscalated 标志设置为 true。相反，它实例化适当的事件并将其传递给 AppendEvent 方法（第 43 行和第 44 行）：

```
39     public void Execute(RequestEscalation cmd)
40     {
41         if (!_state.IsEscalated && _state.RemainingTimePercentage <= 0)
42         {
43             var escalatedEvent = new TicketEscalated(_id,
44         cmd.Reason);
45        AppendEvent(escalatedEvent);
46    }
47 }
48 
49 ...
50 }
```

添加到聚合体事件集合中的所有事件都会传递给 TicketState 类中的状态投影逻辑，其中相关字段的值会根据事件的数据进行变更：

```
50 public class TicketState
51 {
52     public TicketId Id { get; private set; }
53     public int Version { get; private set; }
54     public bool IsEscalated { get; private set; }
55     ...
56     public void Apply(TicketInitialized @event)
57     {
58         Id = @event.Id;
59         Version = 0;
60         IsEscalated = false;
61         ...
62     }
63 
64     public void Apply(TicketEscalated @event)
65     {
66         IsEscalated = true;
67         Version += 1;
68     }
69 
70     ...
71 }
```

现在让我们看看在实现复杂业务逻辑时利用事件溯源的一些优势。

## WHY “EVENT-SOURCED DOMAIN MODEL”?

我觉得有必要解释一下为什么我使用 *event-sourced domain model* 这个术语而不是仅仅使用 *event sourcing*。使用事件来表示状态转换——事件溯源模式——可以在有或没有领域模型构建块的情况下实现。因此，我更喜欢这个较长的术语，以明确表示我们正在使用事件溯源来表示领域模型聚合体生命周期中的变化。

## Advantages

与更传统的模型相比，聚合体的当前状态被持久化到数据库中，事件溯源领域模型需要更多的工作来建模聚合体。然而，这种方法带来了显著的优势，使得该模式在许多场景下值得考虑：

### *Time traveling*

正如领域事件可用于重建聚合体的当前状态，它们也可以用于恢复聚合体的所有过去状态。换句话说，你始终可以重建聚合体的所有过去状态。

这通常在分析系统行为、检查系统决策和优化业务逻辑时完成。

重建过去状态的另一个常见用例是追溯调试：你可以将聚合体恢复到观察到 bug 时的确切状态。

### *Deep insight*

在本书第一部分，我们看到优化核心子域对业务具有战略重要性。事件溯源提供了对系统状态和行为的深入洞察。正如你在本章前面学到的，事件溯源提供了灵活的模型，允许将事件转换为不同的状态表示——你始终可以添加新的投影，利用现有事件的数据提供额外的洞察。

### Audit log

持久化的领域事件代表了聚合体状态发生的一切的强一致性审计日志。法律要求某些业务域实现此类审计日志，而事件溯源开箱即用。

这种模型对于管理资金或货币交易的系统尤其方便。它使我们能够轻松追踪系统的决策以及资金在账户之间的流动。

### Advanced optimistic concurrency management

经典的乐观并发模型在读取数据在写入过程中变得陈旧——被另一个进程覆盖——时会抛出异常。

使用事件溯源时，我们可以更深入地了解在读取现有事件和写入新事件之间到底发生了什么。你可以查询并发追加到事件存储的确切事件，并做出基于业务域的决策，判断新事件是否与尝试的操作冲突，或者额外事件无关，可以安全继续。

## Disadvantages

到目前为止，事件溯源领域模型似乎是实现业务逻辑的终极模式，因此应尽可能频繁使用。当然，这与让业务域需求驱动设计决策的原则相矛盾。所以，让我们讨论一下该模式带来的一些挑战：

### Learning curve

该模式的明显缺点是它与传统的数据管理技术有很大差异。成功实施该模式需要对团队进行培训，并需要时间来适应新的思维方式。除非团队已有实施事件溯源系统的经验，否则必须考虑学习曲线。

### Evolving the model

演进事件溯源模型可能具有挑战性。事件溯源的严格定义说事件是不可变的。但如果需要调整事件的模式怎么办？这个过程并不像更改表模式那么简单。事实上，整本书都写在了这个主题上：*Versioning in an Event Sourced System* by Greg Young。

### Architectural complexity

事件源的实现引入了众多架构上的“活动部件”，使整体设计变得更加复杂。本主题将在下一章中更详细地介绍，当我们讨论 CQRS 架构时。
