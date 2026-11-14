
## 验证检查

替换任务开始服务 100% 的流量后，您可以访问之前观察 Blue 应用程序运行的同一 URL，该 URL 现已替换为 Green 版本的应用程序。

### 提示

您可能需要刷新浏览器或清除缓存才能看到更新后的 Green 应用程序。

## 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Containers) 中此配方文件夹内的步骤操作。

## 讨论

CodeDeploy 提供[多种部署策略](https://oreil.ly/18B4u)——金丝雀发布、AllAtOnce、蓝/绿部署等——您也可以创建自己的自定义部署策略。您可以自定义策略，以定义更长的切换窗口等待期，或定义在流量切换发生前必须满足的其他条件。在默认的蓝/绿部署策略中，CodeDeploy 会在将所有流量路由到新版本的同时，保持您的应用程序先前版本运行五分钟。如果您发现新版本表现不正常，可以快速将流量路由回原始版本，因为它仍在单独的 AWS Application Load Balancer 目标组中运行。

CodeDeploy 使用 ALB 目标组来管理哪个应用程序被视为“生产环境”。当您使用 AWS CDK 部署初始堆栈时，V1 Blue 容器已注册到与 ALB 上端口 8080 关联的目标组。在您启动新版本部署后，CodeDeploy 会启动一个全新的 ECS 服务版本，将其与您创建的 Green 目标组关联，然后平滑地将所有流量转移到 Green 目标组。最终结果是 Green V2 容器现在在 ALB 的端口 8080 上提供服务。之前的目标组现在已准备好执行下一次蓝/绿部署。

这是一个常用于 CI/CD 的模式。您的先前版本可以通过无缝回滚快速重新激活。如果不需要回滚，初始版本 (V1) 将被终止，您可以在下次部署时重复该过程，将 V3 放入 Blue 目标组，并在准备就绪时将流量切换到它。使用此策略有助于您将新应用程序版本对用户的影响降至最低，同时支持更频繁的部署。

### 提示

部署条件允许您定义部署成功标准。您可以将自定义部署策略与部署条件相结合，将自动化测试构建到 CodeDeploy 流程中。这将允许您确保在流量发送到新部署之前，所有测试均已运行并通过。

## 挑战

触发回滚到原始 V1 容器部署并观察结果。

# 6.6 在 Amazon ECS 上自动扩展容器工作负载

## 问题

您需要部署一个容器化服务，该服务在流量高峰期能够横向扩展以满足需求。

## 解决方案

为 ECS 服务配置 CloudWatch 警报和扩展策略，以便在 CPU 负载增加时服务添加更多容器，如[图 6-10](#ch06.html#ecs_service_with_a_cloudwatch_alarm_and)所示。

![](assets/awsc_0610.png)

### 准备工作

按照 [章节代码仓库](https://github.com/AWSCookbook/Containers) 中此配方文件夹内的步骤操作。

### 步骤

1.  使用 `curl` 命令（或您的 Web 浏览器）通过互联网访问 ECS 服务 URL，以验证部署是否成功：

    ```
    curl -v -m 3 $LOAD_BALANCER_DNS
    ```

2.  使用详细模式 (`-v`) 和 3 秒超时 (`-m 3`)，以确保您看到完整的连接过程并设置了超时。以下是示例命令和输出：

    ```
    curl -v -m 3 http://AWSCookbook.us-east-1.elb.amazonaws.com:80/
    *   Trying 1.2.3.4...
    * TCP_NODELAY set
    * Connected to AWSCookbook.us-east-1.elb.amazonaws.com (1.2.3.4) port 80<
    > GET / HTTP/1.1
    > Host: AWSCookbook.us-east-1.elb.amazonaws.com:80
    > User-Agent: curl/7.64.1
    > Accept: */*
    >
    < HTTP/1.1 200
    < Content-Type: application/json
    < Content-Length: 318
    < Connection: keep-alive
    <
    {
    
    "URL":"http://awscookbookloadtestloadbalancer-36821611.us-east-1.elb.amazonaws.com:80/",
      "ContainerLocalAddress":"10.192.2.179:8080",
      "ProcessingTimeTotalMilliseconds":"0",
      "LoadBalancerPrivateIP":"10.192.2.241",
      "ContainerHostname":"ip-10-192-2-179.ec2.internal",
      "CurrentTime":"1605724705176"
    }
    Closing connection 0
    ```

    ### 提示

    连续多次运行此相同的 `curl` 命令，您会注意到 `ContainerHostname` 和 `ContainerLocalAddress` 在两个地址之间交替。这表明 Amazon ECS 正在按照 ECS 服务的定义，在两个预期始终运行的容器之间进行负载均衡。

3.  您需要为自动扩展触发器创建一个执行角色；此文件在 [代码仓库](https://github.com/AWSCookbook/Containers) 的此解决方案目录中提供：

    ```
    aws iam create-role --role-name AWSCookbook606ECS \
        --assume-role-policy-document file://task-execution-assume-role.json
    ```

4.  附加自动扩展的托管策略：

    ```
    aws iam attach-role-policy --role-name AWSCookbook606ECS --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceAutoscaleRole
    ```

5.  注册自动扩展目标：

    ```
    aws application-autoscaling register-scalable-target \
        --service-namespace ecs \
        --scalable-dimension ecs:service:DesiredCount \
        --resource-id service/$ECS_CLUSTER_NAME/AWSCookbook606 \
        --min-capacity 2 \
        --max-capacity 4
    ```

6.  使用指定 50% 平均 CPU 目标的示例配置文件，为自动扩展目标设置自动扩展策略：

    ```
    aws application-autoscaling put-scaling-policy --service-namespace ecs \
        --scalable-dimension ecs:service:DesiredCount \
        --resource-id service/$ECS_CLUSTER_NAME/AWSCookbook606 \
        --policy-name cpu50-awscookbook-606 --policy-type TargetTrackingScaling \
        --target-tracking-scaling-policy-configuration file://scaling-policy.json
    ```

7.  现在，要触发容器内模拟高 CPU 负载的进程，请运行相同的 `curl` 命令，并在服务 URL 末尾追加 `cpu`：

    ```
    curl -v -m 3 $LOAD_BALANCER_DNS/cpu
    ```

    该命令将在三秒后超时，表明访问该 URL 导致容器正在运行 CPU 密集型进程（您使用 CDK 部署的 ECS 服务运行我们提供的 CPU 负载生成器以模拟高 CPU 使用率）。以下是示例命令和输出：
