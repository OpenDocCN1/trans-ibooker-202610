## 23 应用软件质量程序

一旦所有技术都得到理解，它们就会被整合到一个流程中，使软件组织能够生产高质量的软件。组织应采用两种通用类型的流程。第一种是软件开发流程。第二种是组织流程改进流程。软件开发流程是用于以足够质量交付特定软件项目的指标和技术集合。组织流程改进流程使团队能够从一次发布到下一次发布持续改进软件质量。

项目期间流程的概述包含七个要素：

- 1. 开发需求，包括质量目标。
- 2. 估算工作量。
- 3. 创建缺陷运行图，显示软件相对于质量目标的状况。
- 4. 使用失效模式和影响分析（FMEAs）、评审、检查和防御性编程技术来避免缺陷注入。
- 5. 采用各种测试技术来发现缺陷。
- 6. 测量项目期间发现的缺陷并保持缺陷运行图最新。
- 7. 使用缺陷运行图确定何时达到软件质量目标。

上面描述的通用流程可用于瀑布和敏捷软件生命周期。在瀑布生命周期中，这些活动相当清晰地映射到项目的各个阶段。在敏捷流程中，每个活动都可以在冲刺期间使用。然而，在敏捷中，其中一些活动可能在冲刺之间用于回顾，或应用于整个史诗。例如，每个冲刺都会有估算、需求、评审、代码检查和测试。每个冲刺都可以有一个缺陷运行图来跟踪该冲刺的质量。然而，缺陷运行图也可以应用于史诗而不是冲刺。如果每个冲刺都会发布给客户，则更合适为每个冲刺设置缺陷运行图。另一方面，如果史诗代表实际的客户发布，则缺陷运行图可能更适合应用于史诗。同样，流程改进方法，如根本原因分析（RCA）和 FMEAs，可能适合在冲刺之间进行的回顾。然而，这些机制通常需要几天才能很好地执行，因此完全可以将其应用于史诗，并在冲刺流程之外作为整体流程改进流程的一部分执行。敏捷中流程改进机制的另一种选择是创建涵盖它们的用户故事，并在各个冲刺期间实际执行 RCA 和 FMEAs 等任务作为交付成果。

长期质量改进流程通常执行以下操作：

- 1. 主要使用逃逸缺陷率设定软件质量目标。
- 2. 测量每次发布的逃逸缺陷率。
- 3. 在发布之间（或作为发布期间的正交活动），使用 RCA 和 FMEAs 等分析技术来识别技术、培训和流程方面的薄弱环节。
- 4. 解决上一步中确定的弱点。
- 5. 重复此程序。

### 23.1 使用缺陷模型创建缺陷运行图

从最简单的缺陷模型开始，其中仅估计注入的缺陷总数。要创建此简单模型，请通过将缺陷密度估计值乘以代码行数（LOC）大小估计值来计算项目中预计注入的缺陷总数。

对于此计算，最好使用开发组历史上测量的缺陷密度数据。如果您没有来自组织的历史数据，您可以使用第 9.3 节关于缺陷密度中讨论的行业数据；即低级语言每千行代码 10 个缺陷，高级语言每千行代码 20 个缺陷。

缺陷不会在项目过程中以稳定速率注入。缺陷也不会以稳定速率被发现。缺陷会在项目的每个活动中注入，即需求开发、设计、编码，甚至在修复其他缺陷时。缺陷会在评审、代码检查和各种测试活动中被发现。通常，大多数缺陷会在编码期间注入，大多数缺陷会在测试期间被发现并修复。更复杂的缺陷模型可能会估计项目过程中随时间推移注入、发现和修复的缺陷数量，甚至可能考虑到在任何给定时间开发、测试和修复代码的工程师数量。对于刚开始使用缺陷运行图的组织，足以仅创建一个简单的燃尽图，该图绘制预计的缺陷总数以及找到它们的进展，如第 9.5 节关于缺陷运行图中所示。

### 23.2 使用缺陷运行图了解何时达到质量目标

软件开发期间发现的所有缺陷都必须计数。这应包括在评审、检查和开发人员测试期间发现的缺陷，以及在传统软件质量保证（SQA）类型测试期间发现的缺陷。同样，每个修复的缺陷也必须计数。拥有准确的缺陷发现和关闭


counts is critical to determining if the software is meeting its quality goals and is ready to release.

The defect discovered and fixed counts are used to update the defect run chart. The cadence of the defect run chart updates could vary, particularly depending on the type of software development lifecycle used. For a long waterfall project, it might be sufficient to update the defect run chart on a weekly basis. For an Agile process, where iterations are short, it might work better to update the defect run chart on a daily basis.

The defect run chart is both a burn-up and a burn-down chart. The burn-up part is the line that shows the number of discovered defects as it approaches the model number. The burn-down part of the chart shows the open defects as that count decreases toward the number of allowable escapes determined in the quality goal. Both of these are useful for taking management action to ensure that the software quality achieves its goals.

If the number of discovered defects is not approaching the estimated number of injected defects in a timely fashion, then this should be examined and mitigation strategies should be executed if appropriate. The same is true if the number of open defects is not decreasing to the target as expected by the schedule. Before deploying mitigation activities, the data should be examined to determine why the defect metrics are not converging as expected. There could be a number of factors (including simply that the software is better than expected) and a number of different mitigation tactics.

Possible reasons why the number of discovered and repaired defects is not conforming to the model as expected include the following:

- If the discovered defect counts are too low, it could be because some of the software quality methods have not been adequately executed. It should be determined if inspections, reviews, and various testing have been thoroughly carried out as expected. If these techniques have not been adequately performed, then remedial action should be taken to perform them as soon as possible.
- If the discovered and/or closed defect counts are too low, it might have been caused by the project having gotten off to a late start and is simply behind in overall execution. In this case, there are several options available to achieve the quality goals. The project schedule can be pushed out to make up for the delay. Or more staff can be added to get more work done. Or scope/content can be removed from the project to decrease the overall amount of work to be performed. A note on adding staff: This should be done very judiciously. Throwing bodies at a late project often will just be disruptive and make the project even later.
- Sometimes the discovered defect rate is higher than the model. This is a concern for the quality of a particular release if the closure rate is not keeping up. Even if the defect closure rate is keeping up with the incoming rate, it is still worth determining the root cause of the higher than expected discovery rate. In some cases, it might be nothing more than an opportunity to better calibrate the defect prediction model. However, in other cases, it might represent a particularly problematic piece of code that could benefit from additional attention.
- It is possible that the estimation used for the model is wrong. This would obviously require no mitigation at all.

## 23.3 Using Root Cause Analysis on Defects to Improve Organizational Quality Delivery

Before any process improvement processes are undertaken, it must be determined where to start improving. Pareto charts are a particularly useful means of identifying targets for improvement.

Ideally, when a defect is discovered, it should be categorized by what kind of defect and when it was discovered. The defect type categories include requirements, design, coding, testing, and, of course, escapes. The activity that discovered the defect could be different than the type of the discovered defect. For instance, a requirements defect could be discovered during testing or it could be discovered during requirements development. Multiple Pareto charts should be created to show where in the code defects are found (i.e. which components), where in the development process defects are injected, and where in the process defects are not being adequately discovered. These Pareto charts will show where to focus the improvement efforts.

The details on when defects are discovered and their type are both useful inputs into the quality process improvement process. There are numerous examples of how to use this categorization data:

- If it is determined that the number of defects is about right, but that many defects are found downstream from when they are introduced, then a target for improvement activities would be to find more of those defects when they are injected. This could have a major impact to project cost since the earlier defects are discovered, the less costly it is to fix them. For instance, if many requirements defects are discovered during system testing activities, it would be beneficial to focus on improving the requirements procedures to find those defects sooner.
- If it is found that a particular development activity generates more defects than the model expects, then there could be a deficiency in the defect prevention activities for that activity. For example, if it is determined that the software design process discovers many more defects than are expected during design, then the design process should be examined with the intent on improving it.
- If a particular software component or technology has a higher-than-expected defect rate, then investigation might focus on discovering the root causes. It could be that the code is old and unstable, or it could be that this area of the code is particularly complex.

Once it has been determined where to focus the improvement efforts, RCA can be used to find the reasons why the quality was not as expected.

## 23.4 Continuous Integration and Test

In previous sections of the book, there are recommendations to automate testing, to keeping up with the defect backlog, and to start testing early and often. One mechanism to accomplish this is continuous integration and test. With continuous integration and test, the build and test environments are automated such that each time there is a code delivery, the software is automatically built and the tests are automatically run.

This process uncovers defects (and build issues) at the fastest possible rate, allowing the software development organization to stay on top of the defect backlog and to help identify defect root causes in the most efficient way.

It is very difficult to determine which code introduced which defects when large amounts of new code are allowed to accumulate before being delivered and tested. Also, just the instability introduced by the amassed defects in that accumulated code decreases tester and debugger productivity. Another common problem is that a code change might break functionality in some other related area. If many code changes are delivered at the same time, this kind of ancillary breakage is extremely difficult to debug.

Continuous integration and test is a process where developers deliver their code as soon as it has been unit tested and code reviewed, and then each delivery initiates an automated build and test cycle. If a new defect is introduced in a particular delivery, it becomes obvious which code change caused the problem, because only a very small number of code changes are introduced at any given point in time.

The continuous integration and test process improves software development productivity in two ways. First, the debugging process is streamlined by improving the ability of developers to determine which code introduced a defect. Second, the overall defect density of the product, at any point in time, is kept lower, thus making testing of the entire product more efficient by providing a more stable platform to test.

## Further Reading

Booch, G. (1994). *Object-Oriented Analysis and Design with Applications, 2e.* Addison Wesley Longman, Inc.

Humble, J. and Farley, D. (2011). *Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation.* Pearson Education, Inc.

Vandermark, M.A. (2003). *Defect Escape Analysis: Test Process Improvement.* IBM Corporation.




