
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述了第 2 级引擎中的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手的角度评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。在国际象棋游戏中，理论上总是存在最佳走法。可能走法的数量庞大，以及由此产生的令人难以置信的游戏局面数量，使得在实践中确定最佳走法是不可能的。常见的算法如 Minimax 仅探索到游戏树的特定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供其功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类使用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；如果找到更好的走法，调用方将通过观察者模式接收 `onNext` 消息。`Search` 通过 `onComplete` 消息指示其工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手的角度评估位置。结果是一个数字，0 表示平衡局面。正数描述玩家优势，负数表示劣势。数字越高，优势或劣势越大。因此该模块使得可以相互比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供其功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是一个非常简单的实现。接口包含典型评级的常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的一个方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有的棋子（材料）。每种棋子类型都有一个值（兵 1，马 3，…，后 9）。棋盘上的棋子相应地相加。玩家的棋子为正，对手的棋子为负。因此，在材料平衡时，结果为 0。如果失去一枚后，数值将下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
在纯材料评估中，棋子所在位置无关紧要。起始位置的兵与即将升变的兵价值相同。边缘的马相当于中心的马。改进空间很大，但未被利用，因为 DokChess 应该邀请他人进行实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。各个部分如何协同工作？

### 6.1 走法确定演练
在建立 `XBoard` 协议后，客户端（白方）通过指示走法开始游戏。下面的时序图显示了从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应的子系统级示例交互，输出为 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先，`Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例中的走法被识别为合法，随后在（有状态的）`Engine` 上执行（`performMove` 消息）。然后，`Text UI` 子系统要求 `Engine` 确定其走法。由于走法计算可能需要很长时间，但 DokChess 仍应继续响应输入，因此此调用是异步的。引擎会返回可能的走法。
`Engine` 首先检查开局库是否有提供。在示例中并非如此。`Engine` 必须自行计算走法。然后它访问 `Rules` 并确定所有有效走法作为候选。之后，它调查并对其进行评级，并逐步向调用方（`Text UI` 子系统）报告更好的走法（从 `Engine` 的角度来看更好）。这里使用了观察者模式（使用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到了两个走法（兵 `e7-e5` 和马 `b8-c6`），最后是搜索完成的[content]
**图 5.25：引擎，构建块视图，第 2 级**

下列列表描述第 2 级引擎的模块：

* `Search`：在特定条件下确定位置的最佳走法
* `Evaluation`：从对手视角评估位置。

### 5.2.1 Search（黑盒）

#### 意图/职责
该模块在特定条件下确定位置的最佳走法。国际象棋理论上总存在最佳走法。可能走法数量庞大，导致需考虑的局面不可思议多，实践中无法确定最佳走法。常见算法如 Minimax 仅探索游戏树到一定深度。

#### 接口
该模块通过 `org.dokchess.engine.search.Search` Java 接口提供功能。
`org.dokchess.engine.search.MinimaxAlgorithm` 类用 Minimax 算法实现该接口。`MinimaxParallelSearch` 类使用该算法并实现相同的 `Search` 接口。它并发检查多个子树；若找到更好走法，调用方通过观察者模式收到 `onNext` 消息。`Search` 用 `onComplete` 消息指示工作完成：
![图 5.26：Search 接口以及 MinimaxAlgorithm 和 MinimaxParallelSearch 类](image/B15546_05_26.jpg)
**图 5.26：`Search` 接口和 `MinimaxAlgorithm` 和 `MinimaxParallelSearch` 类**
以下是 `Search` 接口的方法：
![图 5.27：Search 接口的方法](image/B15546_05_27.jpg)
**图 5.27：`Search` 接口的方法**
以下是 `MinimaxAlgorithm` 类的方法：
![图 5.28：MinimaxAlgorithm 类的方法](image/B15546_05_28.jpg)
**图 5.28：`MinimaxAlgorithm` 类的方法**
#### 文件
实现位于包 `org.dokchess.engine.search.` 下。

### 5.2.2 Evaluation（黑盒）

#### 意图/职责
该模块从对手视角评估位置。结果为数字，0 为平衡。正数表示玩家优势，负数表示劣势。数值越高优势或劣势越大。该模块因此可比较位置。

#### 接口
`Evaluation` 模块通过 `org.dokchess.engine.eval.Evaluation` Java 接口提供功能。
`org.dokchess.engine.eval.StandardMaterialEvaluation` 类是简单实现。接口包含典型评级常量：
![图 5.29：Evaluation 接口和 StandardMaterialEvaluation 类](image/B15546_05_29.jpg)
**图 5.29：`Evaluation` 接口和 `StandardMaterialEvaluation` 类**
以下是 Evaluation 接口的方法：
`evaluatePosition`：返回指定玩家颜色视角下给定位置的评估值。越高越好。
`StandardMaterialEvaluation` 实现类仅考虑现有棋子（材料）。每种棋子有值（兵 1，马 3，…，后 9）。棋盘棋子相应相加。玩家棋子为正，对手棋子为负。材料平衡时结果为 0。失去一后数值下降 9。

#### 文件
实现位于包 `org.dokchess.engine.eval.` 下。

#### 开放问题
纯材料评估中棋子位置无关紧要。起始兵与将升变兵价值相同。边缘马相当于中心马。改进空间很大但未被利用，因 DokChess 应邀请他人实验。

## V.6 运行时视图
与静态构建块视图相反，本节可视化动态方面。部件如何协作？

### 6.1 走法确定演练
建立 `XBoard` 协议后，客户端（白）通过指示走法开始游戏。下时序图显示子系统级示例交互，从输入 "`e2e4`"（白兵 e2-e4）到 DokChess 响应输出 "move `b8c6`"（*黑马 b8-c6，即“尼姆佐维奇防御”*（[https://en.wikipedia.org/wiki/Nimzowitsch_Defence](https://en.wikipedia.org/wiki/Nimzowitsch_Defence)））：
![图 5.30：走法确定的示例交互](image/B15546_05_30.jpg)
**图 5.30：走法确定的示例交互**
首先 `Text UI` 子系统借助 `Rules` 子系统验证输入（参见 V 8.4，*合理性检查和验证*）。示例走法被识别为合法，随后在有状态的 `Engine` 上执行（`performMove` 消息）。然后 `Text UI` 子系统要求 `Engine` 确定其走法。因计算可能耗时但 DokChess 仍需响应输入，此调用为异步。引擎会返回可能走法。
`Engine` 首先检查开局库是否有提供。示例中没有。`Engine` 需自行计算走法。然后访问 `Rules` 确定所有有效走法为候选。之后调查并评级，逐步向调用方（`Text UI` 子系统）报告更好走法（从 `Engine` 视角更好）。这里使用观察者模式（用响应式扩展实现（[http://reactivex.io/intro.html](http://reactivex.io/intro.html)））。
示例图显示找到两走法（兵 `e7-e5` 和马 `b8-c6`），最后搜索完成消息，引擎不再提供更好走法。`Text UI` 子系统取 `Engine` 最后走法，按 `XBoard` 协议以字符串打印到标准输出："move `b8c6`"。

## V.7 部署视图
本视图描述 DokChess 运行。作为 Java 程序，命令行使用要求不高。但不便，若用户不能盲目下棋需实体带坐标棋盘。因此说明如何与图形前端配置 DokChess。

### 7.1 Windows 基础设施
下部署图显示无开局库在 Windows 上使用 DokChess。Arena 为示例前端（参见 V 9.1，*决策 如何使引擎与外部世界通信？*）：
![图 5.31：在 Windows PC 上部署 DokChess](image/B15546_05_31.jpg)
**图 5.31：在 Windows PC 上部署 DokChess**
PC 软件要求如下：
* Java Runtime Environment SE 7 或更高。
* JVM（`javaw.exe`）在路径中，否则调整 `dokchess.bat`。
* Arena（[http://www.playwitharena.de](http://www.playwitharena.de)）。
`DokChess.jar` 包含所有模块编译 Java 源代码及必要依赖（Uber-jar）。`dokchess.bat` 脚本启动 JVM 运行 DokChess。两者在同一目录，因 `dokchess.bat` 地址 JAR 文件。
在 Arena 中，脚本在菜单 `Engine` *|* `Install a new Engine` 声明。会显示文件选择。可限制为 `*.bat` 文件。然后将引擎类型设为 `WinBoard`。其他象棋前端声明引擎类似，详见对应文档。

## V.8 技术和横切概念
本节描述通用结构和系统范围方面。也介绍各种技术方案。

### 8.1 模块间依赖关系
