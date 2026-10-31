
# 案例研究：保真度健身函数

一个*保真度健身函数*，它允许团队比较新旧版本，展示了将指标与工程实践相结合的威力。 一个常见的困境是，许多现实世界的架构师面临的问题是：“我如何用新系统替换旧系统，同时确保新系统产生与旧系统相同的结果？” 换句话说，架构师如何保证两个实现之间的保真度？ 答案是一个保真度健身函数。

最后一个案例研究，“Move Fast and Fix Things,”^([2](#ch08.html#ch01fn71)) 来自 GitHub 工程博客，将进化式架构定义中的许多方面联系在一起。 这个案例研究也戳破了这样一种常见论点，即诸如持续部署等激进的敏捷工程实践会使风险增加到不可接受的程度。 实际上，现实表明，使用这些工程实践的团队找到了减轻风险的方法。

GitHub 是一个相当激进的工程组织。 它使用持续部署——当开发人员在其代码库中进行更改时，这些更改会通过部署管道进行，如果没有错误，就会进入生产环境。 GitHub 平均每天部署 60 次，但其运营规模如此之大，以至于边缘情况几乎会立即出现。

正如博客文章所述，GitHub 解决的问题与合并有关，过去合并是通过执行使用命令行 `Git` 合并文件的 shell 脚本完成的。 虽然这能完美运行，但扩展性并不特别好。 为提高性能而进行的替代方案是博客文章的主题。

该团队构建了新的内存中合并功能并对行为进行了测试，以确保其正确执行。 然而，在某个时候，他们必须将其部署到生产环境中，这就是可怕的部分：如果它破坏了某些东西会怎样？ 如果合并代码失败已经够糟糕了，但如果旧合并代码中存在一些以前未知的耦合点，导致更灾难性的故障会怎样？ 这就是让许多团队不敢采用现代技术的恐惧。

GitHub 团队所做的是（他们将其开源供大家受益）创建了工具 [Scientist](https://oreil.ly/0AF3j)，它允许团队在架构内安全地进行实验，而不会让用户暴露于错误中。

`Scientist` 工具允许开发人员创建实验，每个实验包含两个子句：`use` 和 `try`。 `use` 子句包含他们要替换的旧代码，而 `try` 子句包含新行为。 对于合并实验，实验被封装在 `create_merge_commit` 方法中，显示在 [示例 8-4](#ch08.html#example_eight_fourdot_the_commit_method)。

### 示例 8-4. 包含 `science` 块的 `commit` 方法

```
def create_merge_commit(author, base, head, options = {})
  commit_message = options[:commit_message] || "Merge #{head} into #{base}"
  now = Time.current

  science "create_merge_commit" do |e|
    e.context :base => base.to_s, :head => head.to_s, :repo => repository.nwo
    e.use { create_merge_commit_git(author, now, base, head, commit_message) }
    e.try { create_merge_commit_rugged(author, now, base, head, 
        commit_message) }
  end
end
```

在 [示例 8-4](#ch08.html#example_eight_fourdot_the_commit_method) 中，`science` 块充当两个子句 `use` 和 `try` 的调度器。 对于每次调用，`use` 子句始终执行，并且该输出始终返回给用户。 因此，用户永远不会意识到自己是实验的一部分。 架构师还配置框架以确定还应执行 `try` 块的频率——在合并实验中，它运行了 1% 的请求。 当 `use` 和 `try` 都执行时，框架：

* 随机化 `use` 和 `try` 的顺序以防止时序异常
* 比较两次调用的结果以检查保真度
* 吞掉但记录 `try` 块引发的任何异常
* 将结果发布到仪表板，显示在 [图 8-9](#ch08.html#dashboard_showing_the_results_for_the_m)

![](assets/sarm_0809.png)

[图 8-9](#ch08.html#dashboard_showing_the_results_for_the_m) 中的仪表板显示 GitHub 在 02:20 进行了略多于两千次合并。 然而，由于 GitHub 的运营规模如此之大，错误在该视图中并不容易出现。 [图 8-10](#ch08.html#the_errors_during_the_same_time_period) 显示同一时间段内的错误。

![](assets/sarm_0810.png)

如 [图 8-10](#ch08.html#the_errors_during_the_same_time_period) 所示，他们的新代码中存在错误。 然而，由于 scientist 框架，用户看不到这些错误。 相反，开发人员修复问题并重新部署——请记住，在此期间，持续部署对该实验和其他代码都继续进行。

实验的目标之一是提高性能，我们可以在 [图 8-11](#ch08.html#performance_metrics_during_the_scientis) 中的图表中看到成功。

GitHub 架构师运行了四天的实验，直到他们有 24 小时没有不匹配或缓慢的情况，此时他们移除了旧的合并代码并保留了新代码。 在这四天里，他们进行了超过 1000 万次实验，使他们对新代码的正确运行充满信心。

`Scientist` 是一个保真度健身函数，使用功能开关和性能指标实现。 这种方法展示了工程与指标之间的协同作用如何带来项目的超能力。

![](assets/sarm_0811.png)

## 结论

无论健身函数多么强大，架构师都应避免过度使用。 架构师不应结成小团体，退回到象牙塔中构建一个不可能复杂、相互交织的健身函数集合，只是让开发人员和团队感到沮丧。 相反，健身函数是架构师为软件项目构建可执行的重要但非紧急原则清单的一种方式。 许多项目淹没在紧迫性中，让一些重要原则被搁置。 这是技术债务的常见原因：“我们知道这很糟糕，但我们稍后再回来修复它”…而后来永远不会到来。 通过将关于代码质量、结构以及其他防止衰减的保护措施的规则编纂成持续运行的健身函数，架构师构建了一个开发人员无法跳过的质量清单。

Atul Gawande 的优秀著作 *A Checklist Manifesto*（Metropolitan Books，2009）强调了外科医生、航空飞行员等专业人士如何将清单作为他们工作的一部分（有时是法律强制的）。 这并不是因为他们不知道自己的工作或特别健忘——而是当专业人员反复执行相同的任务时，很容易在意外跳过时自欺欺人，而清单可以防止这种情况。 健身函数代表了由架构师定义并作为构建的一部分运行的重要原则清单，以确保开发人员不会意外地（或有意地，因为外部因素如进度压力）跳过它们。

指标（以及许多其他验证技术）与工程的结合使架构治理具有新的水平和能力，进一步推动了软件开发从神秘的手工艺向真正的工程学科的转变。

^([1](#ch08.html#ch01fn70-marker)) Ford、Parsons 和 Kua，“[第 2 章](#ch02.html#the_fitness_function_testing_pyramid_an)：健身函数”，载于 *Building Evolutionary Architectures*。

^([2](#ch08.html#ch01fn71-marker)) Vicent Martí，“Move Fast and Fix Things”，GitHub 博客，2015 年 12 月 15 日，*https://oreil.ly/JXuEu*。

# 第 9 章. 使用软件指标确保可维护性

Alexander von Zitzewitz
