
提醒原始作者代码的工作原理，如果在一段时间后需要对其进行修改。

许多软件产品都公开了应用程序编程接口（APIs）。其中一些 API 可能会被开发系统其他模块的工程师调用。适当的代码文档将促进外部开发人员使用 API，并且他们在调用 API 时犯错的可能性要小得多。许多产品都包含在软件开发工具包（SDKs）中的 API，旨在供客户使用。通常，这些 SDK 的文档由技术写作者编写。即使技术写作者是软件开发人员，如果代码有足够的注释，文档编写过程也会更容易、更高效。

注释块应包含在源代码文件的顶部以及文件中的每个方法或函数中。还应包含注释来描述变量、结构或有意义的代码流程。文件开头的注释块应包括：

- 代码文件中代码的整体目的和内容
- 如适用时的版权声明
- 代码文件的修订历史

每个方法或函数头的注释块应包括：

- 方法或函数（它做什么）的描述
- 方法或函数使用的概述
- 每个参数的描述
- 输出或返回代码的描述

有一些工具允许使用特定文档方案的注释被处理成外部用户文档。其中一些常用的是 Sandcastle、Doxygen 和 Natural Docs。其中最常见的两个可能是 Microsoft 标记格式和 Doxygen 格式。Microsoft 格式基于 XML 标签。包含 Microsoft XML 标签的注释可以轻松转换为 Microsoft 帮助文本或使用 IntelliSense（Microsoft Visual Studio 的一组功能）查看。Doxygen 是一款根据 GNU 通用公共许可证（GPL）免费提供的广泛使用的工具。包含 Doxygen 标记的注释可以提取为独立的 HTML 文档。

图 12.3 显示了一些经过良好文档说明的伪代码示例。

## 12.5 评审和检查

设计、代码和测试计划都应进行评审。举行评审的主要好处是，它是发现错误和缺陷的、迄今为止最经济的方法。在项目早期在评审期间发现问题，可以防止后期出现大量的返工甚至产品失败。考虑一个设计缺陷直到项目后期系统测试阶段才被发现的问题。在那时修复设计缺陷可能会因所需返工量而对进度产生重大影响。更糟糕的是，如果需求不正确并且在项目早期未被发现，整个产品可能毫无用处。

```
//
// This file contains code to read proximity sensors.
//
// Copyright (C) 2018 Jonathan Rodin. All rights reserved.
//
// Revision History:
// Date            Name        Notes
// 2018 Apr 11      J. Rodin    Refactored after code review
// 2018 Mar 27      J. Rodin    Initial creation
//
// read the ultrasonic proximity sensor to find range to nearest object
// Parameters:
//
//        eSensor - sensor to read
//
//        range - distance to detected object
//
// Output:
//
//        sets range to detected range in cm
//
//        if no object in range, sets range to -1
//
enum ESensor {Front=0, Right, Back, Left};
#define baseTriggerIO  10
#define baseEchoIO     13
#define errorLedIO     100

void proximitySensorCheck( ESensor eSensor, float range )
{
    float duration;

        // pulse sensor for 10 usecs
        digitalWrite(baseTriggerIO+eSensor, HIGH);
        delayuSecs(10);
        digitalWrite(baseTriggerIO+eSensor, LOW);

        // get the echo time
        duration = digitalRead(baseEchoIO+eSensor, HIGH);

        // returns -1 if nothing is in range
        range = convertToRange(duration)
}
```

### 图 12.3 代码注释示例。

评审的另一个主要优势是它们促进了技术人员之间的知识转移。如果更多工程师了解某事物的工作原理，那么在维护需要时支持该组件就更容易了。

没有一个人比精心挑选的团队更聪明。也没有任何单一的设计师或程序员熟悉每一种可能的设计模式、平台、技术等。团队总是有知识优势，并能看到作者忽略的事情。评审小组会发现作者遗漏的问题。更不用说作者或编码者很难在自己的工作中发现错误；第三方读者发现错误比作者更容易。评审的另一个好理由，正如前面所述，是向其他人解释一个主题会迫使作者思考计划。

每个技术工作产品的评审过程都有一些共同特征。评审应包括所有利益相关者的代表，并且评审应尽可能频繁和尽早举行。技术评审的利益相关者应代表几个不同的专业领域。他们应包括负责与被评审组件交互的任何其他组件的工程师。这包括如果被评审组件与硬件交互，则包括硬件工程师。反之，编写特定硬件组件代码的软件工程师应参加这些组件的设计评审。评审还应包括被评审技术或功能的主题专家。测试计划评审，尤其是系统测试计划，应包括一个或多个客户代表。根据组织和被测试组件的不同，这可能包括产品营销人员、产品负责人、软件质量保证（SQA）人员，甚至是实际客户。没有关于评审应涉及多少评审者的标准。

为了最大化生产力，评审应是迭代的，尽早开始，并分解为可管理的较小块。最好在可能需要签署的正式评审之前先进行非正式评审或检查。例如，早期设计评审可以只是白板会议，在那里可以收集反馈并就设计概念达成共识。最不productive的设计评审发生在工程师花费大量时间开发设计文档，然后在最后的评审中发现设计有缺陷时。进行迭代非正式评审以在途中进行方向修正，远比不这样做并到达错误目的地要好。

为代码评审创建狭窄的范围特别有益，以便在任何一次评审中评审少量代码。代码评审应尽早开始。在项目结束时等待并要求评审者阅读、理解并对大量代码提供反馈是非常困难且效率低下。不仅如此，要求评审者在项目后期检查大量代码几乎可以保证评审质量低下，因为评审者很可能匆忙并且可能没有全神贯注。

评审者应关注工作产品的功能方面。如果是设计评审，则重点应放在设计是否涵盖所有用例并满足需求，包括隐含需求如性能目标。代码评审者应重点确定代码是否实现了设计、代码是否涵盖所有错误和异常情况，以及代码是否正确实现。评审者有自然倾向花费太多时间纠正拼写错误、格式错误和编码标准违规。虽然修复格式问题和标准违规是好事，但检查更重要的是验证需求、设计和代码在功能上是否正确。尽管代码注释不是可执行代码的一部分，但评审者应确保存在足够的注释，并且这些注释充分描述了代码。

可能也有用的是对代码和设计进行安全评审。如果产品连接到网络或以其他方式可远程访问（例如
