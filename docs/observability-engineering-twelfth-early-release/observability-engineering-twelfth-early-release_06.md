# Part II. Fundamentals of Observability

在本書的第一部分中，我们检查了“可观测性”的定义、其在现代系统中的必要性、其从传统实践的演变，以及它目前在实践中的使用方式。本第二部分深入探讨技术方面，并详细说明为什么在可观测系统中需要特定的要求。

[Chapter 5](#) 介绍了构建可观测系统所需的基本数据类型——任意宽度的结构化事件。这正是遥测的基础数据类型，使得本部分后面描述的分析成为可能。

[Chapter 6](#) 介绍了分布式追踪概念。它分解了追踪系统的工作原理，以说明追踪数据只是一系列相互关联的任意宽度结构化事件。本章指导你通过代码示例手动创建最小追踪。

[Chapter 7](#) 介绍了 OpenTelemetry 项目。虽然 [Chapter 6](#) 中的手动代码示例有助于说明概念，但你更有可能从开始使用仪器库。 rather than choosing a proprietary library or agent that locks you into one vendor’s particular solution, we recommend starting with an open source and vendor-neutral instrumentation framework that allows you to easily switch between observability tools of your choice.

[Chapter 8](#) 介绍了核心分析循环。生成和收集遥测数据只是第一步。分析该数据有助于你实现可观测性。本章介绍了筛选遥测数据以呈现相关模式并快速定位问题来源所需的工作流程。它还涵盖了自动化核心分析循环的方法。

[Chapter 9](#) 重新介绍了指标作为数据类型的作用，以及何时以及在何处最好使用基于指标的传统监控方法。本章还展示了传统监控实践如何与可观测性实践共存。

本部分关注与向基于可观测性的调试实践转变所需工作流程相关的技术要求。在 [Part III](#) 中，我们将检查这些个人实践如何影响团队动态以及如何应对采用挑战。
