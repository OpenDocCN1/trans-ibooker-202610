

如果由于某种原因税率发生变化会怎样？如果 15% 的税率被下调到 12% 呢？或者上调到 16% 呢？我仍然能听到当初将初始设计提交到代码库时那天的争论：“不，这永远不会发生！”好吧，即使是最意想不到的事情也可能发生。例如，在德国，2021 年下半年税率曾从 19% 下调到 16%。这当然意味着我们必须在代码库中更改税率。我们在哪里应用更改？在当前情况下，更改几乎会影响从 Item 类派生的每个类。更改会在整个代码库中蔓延！

正如 SRP 建议分离变化点一样，我们也应注意不要在代码库中重复信息。正如一切都应只有一个职责（即只有一个变更原因）一样，每种职责在系统中也应只存在一次。这个想法通常被称为不要重复自己，或简称为 DRY 原则。该原则建议我们不要在多处重复某些关键信息，而应设计系统，使我们只需在一个地方进行更改。在最佳情况下，税率应仅在唯一一处表示，以便你能轻松更改。

通常 SRP 和 DRY 原则配合得非常好。遵循 SRP 通常也会导致遵守 DRY，反之亦然。然而有时，如给出的示例中，同时遵循两者需要一些额外步骤。我知道你迫不及待想学习这些额外步骤是什么以及如何解决问题，但在这一点上，让我们仅指出 SRP 和 DRY 的一般思想就足够了。我承诺会重访这个问题并向你展示如何解决它（参见“Guideline 35: Use Decorators to Add Customization Hierarchically”）。

## Avoid Premature Separation of Concerns

到目前为止，我希望已经说服你遵循 SRP 和 DRY 是一个非常合理的想法。你甚至可能已经如此坚定地承诺，以至于计划分离一切，将所有类和函数分离成最微小的功能单元。毕竟，这就是目标，对吧？如果你现在正在这样想，那么请停下来！深呼吸。再深一次。然后请仔细聆听 Katerina Trajchevska 的智慧：29

> *Don't try to achieve SOLID, use SOLID to achieve maintainability.*
> —Katerina Trajchevska

*SRP* 和 *DRY* 都是你实现更好可维护性的工具，是简化变更的工具。它们不是你的目标。虽然两者从长远来看都至关重要，但在没有明确了解将影响你的变更类型的情况下分离实体可能会非常适得其反。
为变更而设计通常有利于某一特定类型的变更，但不幸的是可能会使其他类型的变更变得更困难。这种哲学是众所周知的 *YAGNI 原则*（*You Aren’t Gonna Need It*）的一部分，它警告你不要过度设计（参见“Guideline 5: Design for Extension”）。如果你有一个明确的计划，如果你知道会期望哪种类型的变更，那么就应用 *SRP* 和 *DRY* 使这种变更变得简单。
然而，如果你不知道会期望哪种类型的变更，那么不要猜测，而只需等待。等待直到你对期望的变更类型有清晰的想法，然后重构以使变更尽可能容易。

TIP
请不要忘记，轻松更改事物的一个方面是拥有单元测试，以便确认更改没有破坏预期行为。

总之，变更在 *software* 中是预期的，因此设计以适应变更至关重要。分离关注点并最小化重复，以便你能够在不担心破坏其他正交方面情况下轻松更改事物。

## GUIDELINE 2: DESIGN FOR CHANGE

- Expect change in *Software*;
- Design for easy change and make software more adaptable;
- Avoid combining unrelated, orthogonal aspects to prevent coupling;
- Understand that coupling increases the likelihood for change and makes changes harder;
- Adhere to the *Single-Responsibility Principle (SRP)* to separate concerns;
- Follow the *Don’t Repeat Yourself (DRY)* principle to minimize duplication;
- Avoid premature abstraction if you are not sure about the next change.

## Guideline 3: Separate Interfaces to Avoid Artificial Coupling

让我们重温“Guideline 2: Design for Change”中的 `Document` 示例。我知道，到现在你觉得已经看过足够多的文档了，但相信我，我们还没完。还有一个重要的耦合方面需要解决。这次我们不关注 `Document` 类中的单个函数，而是关注整个接口：

```
class Document
{
public:
    // ...
    virtual ~Document() = default;

    virtual void exportToJSON( /*...*/ ) const = 0;
```

```
virtual void serialize( ByteStream& bs, /*...*/ ) const = 0;
// ...
};
```

### Segregate Interfaces to Separate Concerns

Document 要求派生类同时处理 JSON 导出和序列化。虽然从文档的角度来看，这似乎是合理的（毕竟，所有文档都应可导出为 JSON 且可序列化），但它不幸导致了另一种耦合。想象以下用户代码：

```
void exportDocument( Document const& doc )
{
    // ...
    doc.exportToJSON( /* pass necessary arguments */ );
    // ...
}
```

exportDocument() 函数只对将给定文档导出为 JSON 感兴趣。换句话说，exportDocument() 函数不关心文档的序列化也不关心 Document 提供的任何其他方面。尽管如此，由于 Document 接口的定义，将许多正交方面耦合在一起，exportDocument() 函数依赖于远不止 JSON 导出。这些依赖都是不必要的依赖，人工依赖：更改其中任何一个，例如 ByteStream 类或 serialize() 函数的签名，都会影响 Document 的所有用户，即使那些不需要序列化的用户。对于任何更改，所有用户，包括 exportDocument() 函数，都需要重新编译、重新测试，在最坏情况下，如果例如交付在单独的库中，还需要重新部署。同样，如果 Document 类被扩展为另一个函数，例如导出到另一种文档类型，也会发生同样的事情。并且问题会变得更大，Document 中耦合的正交功能越多：任何更改都可能带来在代码库中产生连锁反应的风险。这确实令人遗憾：接口应该帮助解耦，而不是引入人工耦合。

这种耦合是由 *Interface Segregation Principle (ISP)* 的违反造成的，它是 *SOLID* 首字母缩写中的 *I*：³⁰

> *Clients should not be forced to depend on methods that they do not use.*
> 
> —Robert C. Martin

*ISP* 建议通过分离接口（即解耦接口）来分离关注点。在我们的案例中应有两个单独的接口来表示 JSON 导出和序列化的两个正交方面：

```
class JSONExportable
{
public:
    // ...
    virtual ~JSONExportable() = default;

    virtual void exportToJSON( /*...*/ ) const = 0;
    // ...
};

class Serializable
{
public:
    // ...
    virtual ~Serializable() = default;

    virtual void serialize( ByteStream& bs, /*...*/ ) const = 0;
    // ...
};

class Document
    : public JSONExportable
  , public Serializable
{
public:
    // ...
};
```

这种分离并不会使 Document 类过时。相反，Document 类仍然代表对所有文档提出的要求。然而，这种关注点的分离现在使你能够将依赖最小化为实际所需的一组函数：

```
void exportDocument( JSONExportable const& exportable )
{
    // ...
    exportable.exportToJSON( /* pass necessary arguments */ );
    // ...
}
```

以这种形式，通过仅依赖于分离的 JSONExportable 接口，exportDocument() 函数不再依赖于序列化功能，因此也不再依赖于 ByteStream 类。因此，接口的分离有助于减少耦合。

“但这不就是关注点的分离吗？”你在问。“这不就是单一职责原则的另一个例子吗？”是的，确实如此。我同意我们本质上识别了两个正交方面，将它们分开，从而将 SRP 应用于 Document 接口。因此，我们可以说 ISP 和 SRP 是相同的。或者至少 ISP 是 SRP 的特例，因为 ISP 专注于接口。

这种态度似乎是社区中的普遍观点，我也同意。然而，我仍然认为谈论 ISP 是有价值的。尽管 ISP 可能只是特例，我认为它是一个重要的特例。不幸的是，将不相关、正交的方面聚合到接口中往往非常诱人。你甚至可能会将单独的方面耦合到接口中。当然我绝不会暗示你是有意这样做的，但无意中、偶然地。我们经常没有足够关注这些细节。当然你会争辩说“我永远不会那样做”。然而，在“Guideline 19: Use Strategy to Isolate How Things are Done”中你将看到一个例子，可能会说服你这有多容易发生。由于之后更改接口可能极其困难，我相信提高对这个接口问题的认识是值得的。出于这个原因，我没有放弃 ISP，而是将其作为 SRP 的一个重要且值得注意的特例包含在内。

