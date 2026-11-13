# 在云中部署微服务

现在，我们可以将所学内容串联起来，在下一章部署我们的示例商店后端。

## 问题

回答以下问题，以确保你理解 Vapor 应用的部署：

1. 为什么 Docker 常用于部署 Vapor 应用？
2. Docker 镜像和 Docker 容器有什么区别？
3. Kubernetes 如何参与部署？

好了；前几章的内容都为这一章做铺垫。我们想要最终部署我们开发的后端应用程序。我们还想设置自动**持续部署**（**CD**），这样只需按一下按钮，一切就会更新。

我们将为示例应用准备一套完整的生产就绪设置。这将使用 AWS 完成——相同的原则也可用于所有其他云服务提供商。

在本章中，我们将介绍以下要点：

*   设置 **Amazon Web Services** (**AWS**) 和 Docker 仓库：让我们设置 AWS 和 Docker 以部署应用程序。
*   设置 **Elastic Container Service** (**ECS**)：ECS 将运行我们的容器，所以我们将在这里设置它。
*   使用 CodePipeline 进行 CD：我们想要自动部署服务，所以 CodePipeline 将在此帮助我们。

让我们开始吧！

## 技术要求

本节介绍你需要在机器上运行的内容，以便编写此微服务。需要安装以下组件：

*   Vapor 4
*   Swift 5
*   Xcode 11+
*   Docker
*   macOS/Linux
*   AWS **命令行界面** (**CLI**)
*   一个 AWS 账户

我们还假设你已经学习了前几章，并知道如何编写 Vapor 微服务。本章中的所有代码都将在 macOS 和 Linux 上运行。

本章的 GitHub URL 是 `https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 15`。

## 设置 AWS 和 Docker 仓库

在上一章，第 14 章 *Docker 和云* 中，我们主要通过 CLI 部署应用；这次，我们想更多地使用 AWS 管理控制台，因为它提供了一些便利。此处所示步骤假设此 AWS 账户刚刚开通，没有预先存在的服务。请按以下步骤设置账户：

1. 登录 `console.aws.amazon.com`。你应该看到以下屏幕：

![](img/b196d6bd-4c49-4ac9-b352-8bd6d81eb2c5.png)

2. 在“查找服务”字段中输入 `ecs` 并转到 ECS 页面，如下截图所示：

![](img/a7c1f9cd-3b8f-48e5-be21-88f3548c12f0.png)

3. 接下来，从 Amazon ECR 中选择 Repositories，如下截图所示：

![](img/d93b11e1-2833-44f0-9ff8-defc644769b6.png)

4. 然后，单击 Create repository 创建新仓库，如下截图所示：

![](img/7d59b412-5b03-4c92-864d-ddb9d4069fba.png)

5. 在文本字段中填入 `users`，然后继续：

![](img/6bcddbd0-0d83-404b-bb21-450dd0abcd11.png)

现在，我们创建了第一个仓库。

6. 对 `products` 和 `orders` 重复该步骤。我们现在应该有三个仓库，如下截图所示：

![](img/b274dc64-81cb-4d5f-b771-7284a2142d86.png)

在上一章，第 14 章 *Docker 和云* 中，我们做了同样的事情，只是通过命令行。

7. 接下来，我们需要设置负载均衡器。点击顶栏的 Services，然后点击 EC2，如下截图所示：

![](img/43c815f3-47a7-4979-a401-7706e75f38dd.png)

8. 在 LOAD BALANCING 下选择 Load Balancers，如下所示：

![](img/b607463f-f975-46c8-93df-82903d822d9b.png)

9. 点击 Application Load Balancer，然后点击 Create，如下截图所示：

![](img/43e833b5-dd5b-45c4-a128-8978c358520b.png)

10. 在 Name 字段中输入 `ecommerce`；保持 Load Balancer Protocol 设置为 HTTP，然后在屏幕底部选择至少两个监听器，如下截图所示：

![](img/382390eb-0007-4c93-b28c-7f02253be52f.png)

你还需要选择可用区，如下所示：

![](img/21d8516c-2a35-419e-8935-0531f0c10bff.png)

下一步，步骤 2：配置安全设置，将提示我们未选择安全监听器。安全监听器需要证书，证书与域名绑定。我们现在可以保留端口 `80`，稍后为特定域名配置证书时再调整。

11. 确认并进入步骤 3，我们需要创建一个安全组，如下所示：

![](img/44aa3c8c-4bff-438d-b8a2-f12a2936265b.png)

12. 将其命名为 `ecommerce access`，然后配置两个端口 `80` 和 `8080`，如上截图所示。

13. 点击 Next: Configure Routing，并调整设置如下所示：

![](img/fff5e843-fde6-4ce7-a668-5b502f99fb3e.png)

这是默认组，将接收所有与路由组不匹配的流量。

14. 点击 Next: Register Targets。

15. 选择 Add a function later 并点击 Next: Review，如下所示：

![](img/7db33bff-c08c-4ddc-95f3-b63afa4bf720.png)

16. 点击 Review，如下所示：

![](img/7cbab061-3d8a-4138-ac16-990a37667538.png)

现在，负载均衡器正在创建中。几秒钟后，你应该会看到以下屏幕：

![](img/3843a959-28b6-4a45-9f27-f105ba82e18e.png)

至此，负载均衡器的设置就完成了。我们现在已经设置了仓库以及负载均衡器。负载均衡器将作为访问的中心点，相应地重定向并平衡流量。

现在，我们可以进入下一节设置 ECS 和服务。

## 设置 ECS

我们已经设置了负载均衡器和仓库。现在，我们需要设置服务。按如下步骤操作：

1. 转到 ECS 并选择 Clusters。

2. 然后，选择 Create Cluster，如下截图所示：

![](img/70d5968d-55c2-4fdf-b9de-59d6d053ac7a.png)

3. 选择 Networking only，因为我们将使用 AWS `Fargate` 进行部署，如下截图所示：

![](img/fcb86643-575e-418c-90f0-7149e9f30361.png)

4. 点击 Next step，并输入 `ecommerce` 作为集群名称。其余留空，如下截图所示：

![](img/6fc46f04-19ce-484d-ad26-df74bf3474c1.png)

你现在应该会看到以下成功消息：

![](img/14f67a6c-3b77-49b8-bdb4-56ccd66bb3b2.png)

5. 接下来，我们需要创建任务定义。点击 Task Definitions 并选择 Create new Task Definition，如下所示：

![](img/0899e3b9-cf0d-41a1-ba10-ef7fd91c5087.png)

6. 选择 `FARGATE` 作为类型，然后点击 Next，如下所示：

![](img/0265eb6a-7c1b-4624-931f-ed96930b4261.png)

7. 现在，输入 `users` 作为名称，其余留空，如下截图所示：

![](img/87bc7ba3-c1dc-4d18-bc8b-dca92e13f8b9.png)

8. 向下滚动，为 RAM 选择 `0.5GB`，为 vCPU 选择 `0.25`。然后点击 Add container，如下截图所示：

![](img/4061ad17-5475-44ae-98c6-e32a20aa9198.png)

9. 现在，你需要获取我们之前为 users 创建的统一资源标识符 (`URI`) 仓库。将其填入 Image，将 Container name 设为 `users`。将 Container port 设置为 `8080`，如下截图所示：

![](img/c3c81adc-8792-4acd-9a36-fe464ef2cb45.png)

10. 点击 Add Container，然后点击 Create Task Definition，结果如下截图所示：

![](img/edc40365-7d70-4f66-ae15-155b01c960d5.png)

11. 现在，点击 View task definition 并导航回集群。




## 配置 ECS 服务

12. 选择 `ecommerce` 集群，然后在 **Services（服务）** 选项卡中点击 **Create（创建）**。现在，我们正在配置我们的服务。

13. 启动类型选择 **Fargate**，并选择我们之前创建的任务定义 `users`，修订版本 1。

14. **Service name（服务名称）** 输入 `users`，其余保持默认。

15. **Number of tasks（任务数量）** 输入 `1`。

16. 其余保持默认，点击 **Next Step（下一步）**。

17. 在下一个屏幕上，选择我们拥有的唯一 **virtual private cloud（虚拟私有云）**（**VPC**），以及至少两个子网。选择我们之前创建的安全组，并将 **Auto-assign public IP（自动分配公有 IP）** 保留为 **ENABLED（已启用）**，如下截图所示：

![](img/b92f40d2-fb40-4aa4-bb28-6718a191a35f.png)

18. 在 **Load balancing（负载均衡）** 下选择 **Application Load Balancer（应用负载均衡器）**，如下截图所示：

![](img/1852a9f4-b55e-4f7b-ae7e-254cc28f9976.png)

19. 选择唯一可用的负载均衡器，然后选择唯一可用的监听端口（`80`）。其余保持现状，但在 **Path pattern（路径模式）** 中写入 `/v1/users\*`，在 **Evaluation order（评估顺序）** 中写入 `1`，如下截图所示：

![](img/1f010d74-efc4-4169-a4b4-cd18bdd3b42c.png)

当你为其他服务重复此步骤时，第二个服务需要写入 `2`，第三个服务写入 `3`。这意味着优先级是一个唯一字段，虽然我们所有的服务都有不同的任务，但负载均衡器仍然期望一个优先顺序。

20. 最后，在 **Health check path（健康检查路径）** 中写入 `/v1/users/health`，并取消勾选 **Enable service discovery integration（启用服务发现集成）**，因为我们现在不需要它。然后，点击 **Next step（下一步）**。

21. 在 **Service Auto Scaling（服务自动扩缩容）** 处选择 **Do not adjust the service's desired count（不调整服务的期望数量）**，然后点击 **Next Step（下一步）**（我们将在下一章介绍此内容）。

22. 在下一个屏幕上，你将看到此服务的摘要。点击 **Create Service（创建服务）**。服务现已创建，如下截图所示：

![](img/95098bf3-27ce-4cf1-87be-51c732c26224.png)

现在，你需要为 `orders` 和 `products` 重复相同的步骤。请记得正确使用名称。请注意，服务正尝试启动但不会成功；我们尚未推送 Docker 镜像。

现在，我们已经完成了设置服务的整个过程。你已经了解了如何在 AWS 中设置仓库、服务、集群和任务。接下来，我们将在下一节中了解如何使用 AWS CodePipeline 进行自动部署。

## 使用 CodePipeline 进行 CD

因此，我们已经设置好了三个服务，一旦我们上传 Docker 镜像，它们 *would*（本将） 运行。你在上一章 第 14 章 *Docker and the Cloud（Docker 与云）* 中学习了如何通过命令行执行此操作；然而这一次，我们希望 AWS 直接从我们的源代码进行构建。这种自动部署的过程称为 **CD（持续部署）**。

让我们快速了解一下 CodePipeline 的工作原理。CodePipeline 分阶段工作。在每个阶段，它都会对我们的代码执行某些操作，并通过管道将其发送到下一阶段。然而，这些阶段通常由其他服务组成，例如 CodeBuild。CodeBuild 是一种本质上使用 EC2 单元为我们编译代码的工具。CodeBuild 和 CodePipeline 都非常灵活且可定制。花一些时间深入了解如何使用它们是值得的。

要开始，你需要在 GitHub 中设置三个仓库。它们可以是私有或公开的；两者都可以正常工作。我们需要它们用于我们的三个服务，如下所示：

*   User Management Service（用户管理服务）
*   Product Management Service（产品管理服务）
*   Order Management Service（订单管理服务）

在本特定情况下，应使用 GitHub。其他 Git 仓库托管服务也可以工作，但本章假设使用 GitHub，因为它原生集成在 AWS 中。

一旦你的 GitHub 仓库准备就绪，请按以下步骤操作：

1.  打开每个文件夹中的 `buildspec.json` 文件，并将 `<REP_URL>` 调整为正确的仓库 URL。

2.  在 `https://console.aws.amazon.com/codesuite/codepipeline/pipelines` 下打开 AWS CodePipeline。

3.  点击 **New Pipeline（新建管道）**，在 **Pipeline name（管道名称）** 下输入 `users`。其余保持默认。然后，点击 **Next（下一步）**，如下截图所示：

![](img/f84e2360-c5d5-4911-a6aa-30ceb433df22.png)

当你再次执行此过程时，请确保使用在此步骤中创建的服务角色。这意味着它将显示 `New service role（新建服务角色）`，这在这里没问题。创建 orders 和 products 的管道时，你可以复用该服务角色。

4.  在下一个屏幕上，选择 **GitHub** 作为源。然后，点击 **Connect to GitHub（连接到 GitHub）**，如下截图所示：

![](img/e5cddb42-25f1-4ecf-b146-571e85e4e8d6.png)

5.  AWS 现在将引导你通过一个窗口授权 AWS 访问你的 GitHub 文件。允许 AWS 执行此操作，完成后，你可以选择仓库和分支。

6.  在下一个屏幕上，选择 **AWS CodeBuild** 作为 **Build provider（构建提供商）**，然后点击 **Create project（创建项目）**，如下截图所示：

![](img/18864f49-a0b9-4ed4-9cf5-40815b0197fd.png)

7.  在下一个屏幕上，填写详细信息，如下所示：

*   **Project name（项目名称）**：`users`
*   **Environment image（环境镜像）**：`Managed Image（托管镜像）`
*   **Operating system（操作系统）**：`Amazon Linux 2`
*   **Runtime（运行时）**：`Standard（标准）`
*   **Image（镜像）**：`AWS/codebuild/amazonlinux2-x86_64-standard:2.0`
*   **Privileged（特权）**：`Yes（是）`
*   **Service Role（服务角色）**：`New service role（新建服务角色）`

8.  其余保持默认，点击 **Continue to CodePipeline（继续到 CodePipeline）**。

9.  你的屏幕现在应该如下所示：

![](img/f4ef0cee-2666-4d19-9233-6fbff0804456.png)

10. 点击 **Next（下一步）**，选择 **Amazon ECS** 作为你的 **Deploy provider（部署提供商）**。然后，输入 `ecommerce` 作为集群，`users` 作为服务。你的屏幕应该如下所示：

![](img/6ae5d41a-bc3b-4bee-946f-ecf2998ccf00.png)

11. 点击 **Next（下一步）**，然后在查看屏幕上，点击 **Create Pipeline（创建管道）**。

12. 你现在将被重定向到 CodePipeline 屏幕，该屏幕显示你的代码当前正在经历的所有步骤。

13. 你会注意到该过程无法通过 CodeBuild 阶段，并且报错。我们需要调整一些角色。通过点击 **Services（服务）** 然后选择 **IAM** 来进入 **Identity and Access Management（身份和访问管理）**（**IAM**）。然后，选择 **Roles（角色）**，并选择新创建的角色。它应该包含 **CodeBuild**，看起来像 `codebuild-users-service-role`。选择 **Attach Policy（附加策略）**，然后搜索 `AmazonEC2ContainerRegistryFullAccess` 并将其添加到角色中。

14. 一旦你为 `users` 完成此操作且一切看起来良好，请为 `products` 和 `orders` 重复本节。

管道的最终屏幕如下所示：

![](img/0df98b73-2562-4262-ad2a-e9b61c78b969.png)

在底部，我们有 **Deploy（部署）** 部分，现在已成功，如下截图所示：

![](img/4620ed18-85cd-46b8-99f1-2b4579143b57.png)

耶！我们做到了！我们的服务现在应该已经编译并部署完成。请注意，你只需提交并推送到 GitHub，它就会自动部署到你的系统。

## 总结

在本章中，你学习了如何设置 AWS 以微服务架构运行 Vapor 应用程序。我们完成了设置负载均衡器和创建可运行服务的集群的过程。

然后我们完成了设置自动 CD 的过程，并将其连接到 GitHub 仓库，这意味着推送代码到该仓库将导致我们的服务自动部署到 AWS。最后，我们拥有一个全功能的、正在运行的 API。

值得注意的是，我个人曾与亚马逊工程师交谈，确认亚马逊本身使用这种确切的方法来部署他们的微服务。虽然这种方法可能有点复杂，但它已被亚马逊等公司测试并证明是可靠的。

## 问题

为确保你的理解正确，请回答以下问题：



