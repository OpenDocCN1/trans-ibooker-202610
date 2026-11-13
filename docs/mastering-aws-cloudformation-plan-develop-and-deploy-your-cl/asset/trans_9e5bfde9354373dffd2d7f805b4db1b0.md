

```python
matches.append(RuleMatch(path, message))

return matches
```

正如你所见，我们正在执行前面（在 linter 规则工作流中）解释过的操作。我们获取所有数据库资源，并检查它们每一个是否都有合适的 `Deletion Policy`。

现在我们可以检查这个 linter 规则了！这是一个示例，展示了我们如何检查模板是否符合我们自己的规则：

```bash
(venv)$ deactivate

$ cfn-lint database_failing.yaml -a custom_rules
```

```text
W9001 资源 TestDatabase 没有 Deletion Policy！

database_failing.yaml:42:3

W9001 资源 ProdDatabase 的 Deletion Policy 未设置为 Snapshot 或 Retain！

database_failing.yaml:65:3
```

`cfn-lint` 和模板验证是堆栈部署过程的重要组成部分。我们将在下一章讨论持续交付时回到它们。但在那之前，让我们看看如何预配置我们的资源。

# 预配置我们的堆栈

在第一章中，我们用一个简单的命令创建了堆栈：

```bash
$ aws cloudformation create-stack --stack-name ... --template-body file://...
```

该命令调用 CloudFormation API `CreateStack`。CloudFormation 的 API 在请求体中接收模板和参数，并开始创建堆栈。

当我们想要更新堆栈时（例如，更改现有资源或添加新资源），我们调用另一个 API `UpdateStack`：

```bash
$ aws cloudformation update-stack --stack-name ... --template-body file://...
```

在这两种情况下，AWS CLI 都会调用 `ListStacks` API 方法，以检查堆栈是否存在。

如果在创建操作期间堆栈已存在，你将收到一个错误：

```text
An error occurred (AlreadyExistsException) when calling the CreateStack operation: Stack [...] already exists
```

如果你尝试更新一个不存在的堆栈，你将收到另一个错误：

```text
An error occurred (ValidationError) when calling the UpdateStack operation: Stack [foo] does not exist
```

请注意这是一个 `ValidationError`。正如你可能记得的，在资源开始预配置之前，CloudFormation 总是会验证模板，以检查模板是否可用。

很久以前，当 CloudFormation 仅支持 YAML 格式的模板时，`CreateStack` 和 `UpdateStack` API 方法是预配置和更新资源的唯一途径。使用这些方法存在（且仍然存在）几个问题。首先，当我们希望使 CI/CD 流水线通用且可复用时，很难将 `create-stack` 和 `update-stack` 子命令集成到 CI/CD 流水线中，因为我们不知道是在预配置新堆栈还是更新现有堆栈。我们不得不通过执行 `list-stacks` 子命令来检查堆栈是否存在，然后决定运行哪个操作，这算是一种变通方法。另一种变通方法是先手动创建堆栈，然后只在 CI/CD 流水线中使用 `update-stack`，但那样我们就会失去开发人员的自主权。

其次，当我们以这种方式更新堆栈时，可能会有大量更改，很难预测将应用哪些更改，或者这些更改是否会销毁重要资源。我们不得不进行小幅甚至粒度极细的更改，或者在代码审查上花费大量时间，从而降低开发速度。

为了解决这个问题，CloudFormation 开发团队引入了一个称为 `Change Sets`（更改集）的功能。

## 使用更改集部署堆栈

更改集（顾名思义）是在部署期间将应用于堆栈的一组更改。

我们可以创建、审查、执行，甚至删除更改集，具体取决于将要应用的更改是否符合预期。

在本例中，我们将部署核心模板的一部分，为该堆栈创建一个更改集（包含额外资源），然后审查并执行它：

```bash
$ aws cloudformation create-stack \
    --stack-name core \
    --template-body file://core_partial.yaml \
    --parameters-file file://testing.json
```

堆栈创建完成后，我们可以检查 **Change Sets** 选项卡并验证它是空的：

![Figure 3.1 – CloudFormation console](image/B10533_03_001.jpg)

Figure 3.1 – CloudFormation 控制台

让我们创建一个更改集：

```bash
$ aws cloudformation create-change-set \
    --stack-name core \
    --change-set-name our-change-set \
    --template-body file://core_full.yaml \
    --parameters file://testing.json \
    --capabilities CAPABILITY_IAM
```

我们将收到以下输出（你的会有所不同）：

```json
{
    "Id": "arn:aws:cloudformation:REGION:ACCT_ID:changeSet/our-change-set/bd04aeb3-386b-44d7-a25c-5fe626c24aed",
    "StackId": "arn:aws:cloudformation:REGION:ACCT_ID:stack/core/00697420-1123-11ea-9d40-02433c861a1c"
}
```

我们可以在 AWS 控制台或 AWS CLI 中审查此更改集。

通过 AWS CLI，它看起来如下（同样，你的可能不同，因为 AWS 账户和/或 AWS 区域不同）：

```bash
$ aws cloudformation describe-change-set \
    --change-set-name arn:aws:cloudformation:REGION:ACCT_ID:changeSet/our-change-set/bd04aeb3-386b-44d7-a25c-5fe626c24aed
```

输出会很长，因为有很多更改，所以我只包含其中一部分：

```json
{
    "Changes": [
        {
            "Type": "Resource",
            "ResourceChange": {
                "Action": "Add",
                "LogicalResourceId": "AdminRole",
                "ResourceType": "AWS::IAM::Role",
                "Scope": [],
                "Details": []
            }
        },
        {
            "Type": "Resource",
            "ResourceChange": {
                "Action": "Add",
                "LogicalResourceId": "DevRole",
                "ResourceType": "AWS::IAM::Role",
                "Scope": [],
                "Details": []
            }
        }
        # The rest of the output...
    ]
}
```

在下半部分，我们有更改集的元数据，但目前我们关注的是这部分：

```json
{
    "Type": "Resource",
    "ResourceChange": {
        "Action": "Add",
        "LogicalResourceId": "AdminRole",
        "ResourceType": "AWS::IAM::Role",
        "Scope": [],
        "Details": []
    }
}
```

因为我们添加了一个新资源，所以此更改集中的 `Action` 设置为 `Add`。如果我们从模板中移除资源，将看到 `Remove`，如果模板中的资源已更改，我们将看到 `Modify`。

让我们在 AWS 控制台中检查此更改集：

![Figure 3.2 – CloudFormation console – Change Sets tab](image/B10533_03_002.jpg)

Figure 3.2 – CloudFormation 控制台 – Change Sets 选项卡

当我们点击 **our-change-set** 时，将看到其内部详情：

![Figure 3.3 – CloudFormation console – our-change-set review](image/B10533_03_003.jpg)

Figure 3.3 – CloudFormation 控制台 – our-change-set 审查

总而言之，结果是一样的，但更便于人工审查。

审查完更改后，我们可以部署此更改集：

```bash
$ aws cloudformation execute-change-set --change-set-name our-change-set --stack-name core
```

最后但同样重要的是，另一个有用的子命令 `deploy` 使用方式如下：

```bash
$ aws cloudformation deploy --stack-name foo --template-file bar
```

`deploy` 与 `create-stack` 或 `update-stack` 的区别在于，此命令是幂等的。如果你在新堆栈上运行 `deploy`，CloudFormation 将创建它。如果堆栈已存在，CloudFormation 将更新它。

主要区别如下：

- `deploy` 使用更改集运行堆栈预配置。
- `deploy` 使用 `--template-file` 参数代替 `--template-body`。

