

减慢向客户交付软件的速度。换句话说，如果你的软件供应链的第一部分缓慢或容易出错，那么依赖于这一关键部分的其余软件供应链就会出现瓶颈。

“它很慢”是分布式系统中最难调试的问题。“它不稳定”是支持内部工具的团队听到的最常见问题。两者共同的挑战是如何关联交互的高复杂度系统中的问题。例如，当基础设施或测试不稳定时，开发人员编写代码的能力会下降，他们对工具的信任也会被侵蚀，导致挫败感。这种类型的复杂性促使 Slack 投资构建共享可观测性管道。你将在第 18 章中看到有关可观测性管道的更多细节，该章由 Slack 的 Suman Karumuri 和 Ryan Katkov 撰写。在下一节中，你将了解维度如何帮助解决这个问题。

## Instrumentation: Shared Client Libraries and Dimensions

Slack CI 的主要挑战是复杂性。端到端测试中的一次失败可能是多个相互作用的代码库的结果，这些代码库需要在代码库更改、基础设施更改和平台运行时之间进行查看。对于 2020 年一名 Web 应用开发者的单次提交，我们的 CI 管道会执行 30 多个测试套件，这些套件依赖于 GitHub、三个平台团队（性能、后端和前端）构建的管道，跨越 20 个团队/服务，各自有不同的需求和专业领域。到 2020 年中期，我们的 CI 基础设施开始达到极限，因为测试执行量每月增长 10%，导致多个下游服务在满足测试执行带来的额外需求方面遇到扩展挑战。

Slack 试图通过分布式追踪来解决这一系列瓶颈。Slack 的 CI 基础设施在 2019 年大部分由 CTO 和早期员工编写，并且多年以来一直基本可用。但该

基础设施显示出成长的烦恼，并且对许多工作流程缺乏可观测性。

通过将追踪应用于多跳构建系统，我们的团队在添加 instrumentation 后数小时内就解决了 CI 工作流程中的多个挑战：

- 在 2019 年第二季度，我们用一个下午的原型对我们的 CI 运行器进行了 instrumentation。类似于其他追踪用例，在获得追踪数据几分钟后，我们发现了 Git checkout 的异常运行时间。通过查看底层主机，我们发现它们没有像其他主机一样在自动扩展组（ASG）中更新，并迅速将其停用。这解锁了一个简单的解决方案，用于那些没有导致错误或失败但仍呈现缓慢——因此是糟糕——用户体验的工作流程。
- 在 2019 年第三季度，我们的团队正处于一次多日/多团队的事故中。我们在 CI 运行器和测试环境之间实现了第一个跨服务追踪，并发现了一个减慢系统吞吐量的 Git 大文件存储（LFS）问题。最初，团队忙于将多个超载的系统控制在可控范围内，因为系统的一个部分将故障级联到其他系统。我们借用了 CI 运行器的简单 instrumentation，并通过发现一组无法从 Git LFS 检索工件的主机，在不到两个小时内解决了事故。

在图 14-2 中，你可以看到用户将提交推送到 GitHub 后一次测试运行的简化视图。此测试运行由 Checkpoint 编排，随后传递给构建步骤然后是测试步骤，每个步骤均由 Jenkins 执行器执行。在每个阶段，你都会看到额外的维度，这些维度为 CI 中的执行提供了上下文。Slack 工程师随后使用单个或组合的维度作为面包屑，在生产环境中出现性能或可靠性问题时探索执行情况。每个额外的维度都是问题调查过程中的一个方向和线索。你可以组合线索以迭代地深入挖掘已部署代码中的特定问题。

![](img/aaaa2d27e08e754554d501192f304d8f_240_0.png)

*Figure 14-2. A simplified view of a single end-to-end test run orchestrated by our CI orchestration layer, highlighting the common dimensions shared across our workflow*

客户端维度在每个追踪中配置。图 14-3 显示了示例维度。Slack 使用一个 TraceContext 单例来设置这些维度。每个 TraceContext 构建一个具有通用维度和新追踪的初始追踪。每个追踪包含多个 spans 和每个 span 的一组特定维度。单个 span 例如在图 14-4 中来自 `runner.test_execution` 可以包含原始请求的上下文并向根 span 添加感兴趣的维度。随着你添加更多维度，你为问题调查提供了更多上下文和丰富性。

NOTE
You can learn more about wide spans at “CircleCI: The Unreasonable Effectiveness of a Single Wide Event,” by Glen Mailer, a video posted on the Honeycomb website.

```
enum ServiceName: string as string (
CHECKPOINT_BG = 'ci_checkpoint_bg';
CHECKPOINT_APP = 'ci_checkpoint_aa';
CHECKPOINT_ENV_CONTROL = 'ci_checkpoint_env';
CIBOT_RUNNER = 'ci_cibot_runner';
CIBOT_BUILDER = 'ci_cibot_builder';
DB_CLIENT = 'ci_db';
DEFAULT = 'ci_defaut';
ROOT = 'ci_root';
)
```

```
// Build
ARTIFACT = 'artifact_id';
ARTIFACT_URL = 'artifact_url';
ARTIFACT_TYPE = 'artifact_type';
JENKINS_BUILDER_TYPE = 'jenkins_builder_type';
JENKINS_WORKER_LABEL = 'jenkins_worker_label';
```

```
// Test run
TEST_SUITE_NAME = 'test_suite';
TEST_SUITE_RESULT = 'test_suite_result';
TEST_RUN_REASON = 'run_reason';
```

```
enum ServiceName: string as string (
// Global
NAME = 'name';
SERVICE_NAME = 'service_name';
BACKTRACE = 'backtrace';
ERROR_MESSAGE = 'error';
IS_SYNTHETIC_SPAN = 'is_Synthetic_span';
METHOD = 'method';
STATUS_CODE = 'status';
)
```

```
// Test case
TEST_CASE_NAME = 'test_case_name';
TEST_CASE_RESULT = 'test_case_result';
TEST_CASE_ATTEMPTS = 'test_case_attempts';
```

```
// Orchestration
ENTRYPOINT = 'entrypoint';
REQUEST_ID = 'request_id';
PATH = 'path';
AUTH_METHOD = 'auth_method';
```

```
// Commit related
COMMIT_HEAD = 'commit_head';
COMMIT_MASTER = 'commit_master';
COMMIT_MERGED = 'commit_merged';
BRANCH = 'branch';
REPO = 'repo';
)
```

```
// Clients
DB_QUERY = 'query';
DB_TABLE = 'query_table';
GITHUB_METHOD = 'github_method';
GITHUB_PATH = 'github_path';
/* [... Other dimensions ...] */
)
```

```
// Host related
EVN_ID = 'env_id';
HOSTNAME = 'hostname';
ASG_ID = 'asg_id';
)
```

Figure 14-3. Common dimensions in our Hack codebase. You can use these as examples for structuring dimensions across spans and services in CI.

例如，Slack 工程师可能希望沿着常见维度识别并发问题（如主机名或 Jenkins 工作组）。TraceContext 已经提供了一个主机名标签。CI 运行器客户端随后附加一个 Jenkins 工作器标签的标签。使用这两种维度的组合，Slack 工程师可以对具有运行时问题的主机或 Jenkins 工作器组进行分组。

同样，Slack 工程师可能希望识别常见的构建失败。CI 运行器客户端附加一个提交头或提交主分支的标签。这种组合可以识别损坏的构建可能来自哪些提交。

图 14-3 中的维度随后在各种脚本和服务调用中使用，因为它们相互通信以完成一次测试运行图 14-4。

Figure 14-4. A trace for the CI execution of a backend test suite called backend-php-unit

在以下部分中，我将分享 Slack 如何使用追踪工具和查询来理解供应链，以及出处如何导致可操作的告警以解决问题的过程。

## Case Studies: Operationalizing the Supply Chain

通过分析由指标、事件、日志和追踪组成的遥测数据进行可观测性，是建模内部客户体验的关键组成部分。对 Slack 基础设施工具和人员而言，关键是持续学习并将可观测性嵌入到我们的工具中。本节介绍的案例研究将可观测性带入 Slack 开发人员

