

自由函数，我真的不能把所有关于重载集的建议都用在上面，对吗？"这可能会让你感到惊讶，但这条建议仍然适用于你。因为最终目标是减少依赖，而虚拟函数可能会导致相当大的耦合，因此其中一个目标也会是“解放”它们。事实上，在后续的许多准则中，尤其是在“准则 19：使用策略来隔离事物是如何完成的”和“准则 31：对非侵入式运行时多态使用外部多态”中，我将讲述如何提取和分离虚拟函数，例如以自由函数的形式，但不限于此。

总之：函数重载是一种强大的编译时抽象机制，你不应该低估它。特别是，泛型编程大量利用了这种能力。然而，不要太轻视这种能力：请记住，就像基类和概念一样，重载集代表了一组语义要求，因此受 Liskov 替换原则的约束。重载集的预期行为必须得到遵守，否则事情将无法正常工作。

## 准则 8：理解重载集的语义要求

- 要意识到函数重载是一种编译时抽象机制；
- 记住重载集中函数的行为存在预期；
- 注意现有的名称和约定。

## 准则 9：注意抽象的所有权

正如“准则 2：为变化而设计”中所说，变化是软件开发中唯一不变的东西。意味着你的软件应该为变化做好准备。应对变化的基本要素之一是引入抽象（另见“准则 6：遵守抽象的预期行为”）。抽象有助于减少依赖，从而使在隔离状态下更改细节变得更容易。然而，引入抽象不仅仅是添加基类或模板。

### 依赖倒置原则

对抽象的需求也由 Robert Martin 表达：16

> 最灵活的系统是那些源代码依赖仅指向抽象，而非具体实现 的系统。
> 
> —Robert C. Martin

这段智慧通常被称为依赖倒置原则（DIP），它是 SOLID 原则中的第五条。简单来说，它建议为了依赖关系，你不应该依赖具体类型或实现细节，而应该依赖抽象。注意，这个说法并没有提到继承层次结构，而只是提到了抽象的一般概念。

让我们来看一下图 2-1 所示的情况。17 想象你正在为自动取款机（ATM）实现逻辑。ATM 机器提供几种操作：你可以取款、存款和转账。由于所有这些操作都涉及真实货币，它们要么完全完成，要么在任何错误情况下被中止并且所有更改都应回滚。这种行为（要么 100% 成功，要么完全回滚）是我们通常所说的事务。因此我们可以引入一个名为 Transaction 的抽象。所有抽象（即 Deposit、Withdrawal 和 Transfer）都继承自 Transaction 类（由 UML 继承箭头表示）。

高层
（稳定，依赖少）

Transaction
virtual execute() = 0

Deposit
execute() override

Withdrawal
execute() override

Transfer
execute() override

架构边界

低层
（易变，可塑，依赖多）

UI
requestDepositAmount()
requestWithdrawalAmount()
requestTransferAmount()
informInsufficientFunds()

图 2-1. 若干事务与用户界面（UI）之间初始的强依赖关系。

所有事务都需要银行客户通过用户界面输入的输入数据。此用户界面由 UI 类提供，该类提供许多不同的函数来查询输入的数据：
`requestDepositAmount()`、`requestWithdrawalAmount()`、`requestTransferAmount()`、`informInsufficientFunds()`，以及可能更多的函数。所有三个抽象在需要信息时都会直接调用这些函数。这种关系通过小实心箭头表示，表示抽象依赖于 UI 类。

虽然这种设置可能在一段时间内有效，但你训练有素的眼睛可能已经发现了一个潜在问题：如果有东西发生变化会怎样？例如，如果系统添加了新的事务会怎样？

假设我们需要为 VIP 客户添加一个 `SpeedTransfer` 事务。这可能需要我们更改并扩展 UI 类，增加几个新函数（例如 `requestSpeedTransferAmount()` 和 `requestVIPNumber()`）。反过来，这也会影响所有其他事务，因为它们直接依赖于 UI 类。在最好的情况下，这些事务只需要重新编译和重新测试（尽管这仍然需要时间！），在最坏的情况下，如果它们以单独的共享库形式交付，可能需要重新部署。

所有这些额外工作背后的根本原因是架构被破坏。所有事务通过对 UI 类的具体依赖而间接依赖于彼此。而这从架构角度来看是一个非常不幸的情况：事务类位于我们架构的高层，而 UI 类位于低层。因此在这个例子中，高层依赖于低层。而这就是错误的：在一个适当的架构中，这种依赖关系应该被倒置。¹⁸

所有事务由于依赖于 UI 类而间接相互依赖。此外，我们架构的高层依赖于低层。这确实是一个非常不幸的情况，我们应该妥善解决。“但这很简单！”，你说。“我们只要引入一个抽象就行了！”这正是 Robert Martin 在他的陈述中所表达的：我们需要引入一个抽象，以便不依赖于 UI 类中的具体实现。然而，单个抽象并不能解决问题。三种事务仍然会间接耦合。不，在图 2-2 所示的情况下，我们需要三个抽象：每个事务一个。¹⁹

高层
（稳定，依赖少）

Transaction
virtual execute() = 0

Deposit
execute() override

Withdrawal
execute() override

Transfer
execute() override

架构边界

DepositUI
virtual requestDepositAmount() = 0

WithdrawalUI
virtual requestWithdrawalAmount() = 0
virtual informInsufficientFunds() = 0

TransferUI
virtual requestTransferAmount() = 0
virtual informInsufficientFunds() = 0

低层
（易变，可塑，依赖多）

UI
requestDepositAmount() override
requestWithdrawalAmount() override
requestTransferAmount() override
informInsufficientFunds() override

图 2-2. 若干事务与用户界面（UI）之间松弛的依赖关系。

通过引入 DepositUI、WithdrawalUI 和 TransferUI 类，我们打破了三个事务之间的依赖关系。三个事务不再依赖于具体的 UI 类，而只依赖于一个轻量级抽象，该抽象仅表示相关事务真正需要的操作。如果我们现在引入 SpeedTransfer 事务，我们也会引入 SpeedTransferUI 抽象，因此其他事务不会受到 UI 类中更改的影响。

“哦，是的，我明白了！这样我们就实现了三个设计原则！”你听起来很印象深刻。“我们引入了一个抽象来切断对用户界面实现细节的依赖。这一定是 DIP。而且我们遵循了 ISP，从而移除了不同事务之间的依赖。作为奖励，我们还很好地将真正属于一起的东西分组在一起。这就是 SRP，对吧？太棒了！让我们庆祝吧！”

等等，等等，等等……在你现在去开瓶最好的香槟来庆祝解决这个依赖问题之前，让我们更仔细地看看这个问题。是的，你是对的，我们通过将 UI 类的关注点分离来遵循 ISP。通过将其细分为三个客户端特定的接口，我们解决了三个事务之间的依赖情况。这确实是 ISP。非常好！

不幸的是我们还没有解决架构问题，所以不，我们还没有遵循 DIP（还）。但我理解这种误解：看起来我们确实已经倒置了依赖关系。图 2-3 显示我们确实引入了依赖倒置：我们不再依赖具体的 UI 类，而是依赖抽象。

