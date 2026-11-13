
你可能已经注意到，`cfn-lint` 会显示其发现错误的行号。然而，当你摆弄内部函数的参数时，你会看到错误出现在第一行，这并非总是如此。

此外，该错误并非 linter 指出的实际错误，而是一个 Python 异常。

Linter 不仅根据内置规则检查模板，还支持调整和自定义。

## 针对特定区域进行 Lint 检查

运行 `cfn-lint` 时，它会根据默认区域（即 `us-east-1`）检查模板。要针对其他区域进行检查，可以手动指定：

```bash
$ cfn-lint core.yaml --regions 'eu-west-1'
```

```bash
# 或...
$ cfn-lint core.yaml --regions 'eu-west-1','us-east1'
```

```bash
# 甚至...
$ cfn-lint core.yaml --regions 'ALL_REGIONS'
```

在最后一个示例中，你将得到更有趣的结果：

```text
E3001 Invalid or unsupported Type AWS::SSM::Parameter for resource MiddlewareAsgMaxSizeParameter in us-gov-west-1
core.yaml:393:5

E3001 Invalid or unsupported Type AWS::SSM::Parameter for resource MiddlewareAsgMaxSizeParameter in us-gov-east-1
core.yaml:393:5

E3001 Invalid or unsupported Type AWS::SSM::Parameter for resource MiddlewareAsgMaxSizeParameter in eu-north-1
core.yaml:393:5

E3001 Invalid or unsupported Type AWS::SSM::Parameter for resource MiddlewareAsgMaxSizeParameter in ap-east-1
core.yaml:393:5
```

此错误触发的原因是上述区域不支持该资源类型。如果你计划将模板部署到各个区域，明智的做法是将所有或部分区域纳入检查，以确保你想要预配的服务和资源确实受 CloudFormation 支持。

另一种包含待检查区域的方法是在模板的元数据中添加它们：

```yaml
# core.yaml
Metadata:
  cfn-lint:
    regions:
      - us-east-1
      - eu-west-1
      - eu-central-1
```

随后，运行检查时 `cfn-lint` 会自动选取这些区域。

### 重要提示

请注意，模板的元数据值始终会被命令行参数覆盖。

## 忽略特定规则

如果你在 CI/CD 流水线中包含 `cfn-lint`，当 linter 返回码非 `0` 时将步骤标记为失败，你会遇到麻烦。如果 `cfn-lint` 发出警告（不一定是问题），你可能希望在其不适用于你的堆栈时忽略它。

你可以通过提供 `--ignore-checks` 参数来忽略特定规则：

```bash
cfn-lint core.yaml --ignore-checks 'W2001'
```

或者也可以在元数据中包含它们：

```yaml
Metadata:
  cfn-lint:
    regions:
      - us-east-1
      - eu-west-1
      - eu-central-1
    ignore-checks:
      - W2001
```

虽然不建议忽略 linter 规则，但仅针对特定规则（如不存在的服务）在针对所有区域进行 lint 检查时使用是可以接受的。

## 创建自定义规则

`cfn-lint` 拥有丰富的检查和规则库。然而，你的组织可能有自己的模板要求（例如必要的 `Tags`），这些要求并非开箱即用。

`cfn-lint` 支持创建自定义规则。自定义规则必须用与 `cfn-lint` 相同的语言编写（例如 Python），并在 linter 运行时追加。

在添加自定义规则之前，让我们先开发一个！我们的场景是：确保 RDS 资源（如 DB 实例和 DB 集群）具有 `DeletionPolicy`，且该 `DeletionPolicy` **不** 设置为 `Delete`。

部分工作已完成：`cfn-lint` 拥有内置模板解析器，我们只需匹配特定资源并检查其是否具有所需属性。

Linter 规则的流程如下：

1.  规则检查模板中是否存在类型为 `AWS::RDS::DBInstance` 或 `AWS::RDS::DBCluster` 的资源。
2.  规则检查该资源是否具有 `DeletionPolicy`。
3.  规则检查 `DeletionPolicy` 是 `Retain` 还是 `Snapshot`。
4.  如果未设置 `DeletionPolicy` 或其设置为 `Delete`，规则返回 `Warning`。

我们将创建一个名为 `custom_rules` 的目录，并在其中创建一个名为 `rdsdeletionpolicy.py` 的 Python 文件。

### 重要提示

为了让 `cfn-lint` 正确处理自定义规则，必须确保类名与规则文件名完全相同。

- 可行：`myrule.py` - `class MyRule(CloudFormationLintRule)`
- 不可行：`my_rule.py` - `class MyRule(CloudFormationLintRule)`

首先，我们需要确定一个规则编号。`cfn-lint` 有一系列类别规则。我们可以使用 `W9001` 类别（`W` 代表警告，`9XXX` 保留给用户规则）。

每个自定义规则都是一个继承自 `CloudFormationLintRule` 类的 Python 类。由于该类对象属于 `cfnlint` Python 模块，我们需要使用 `pip` 安装它，并且为了不弄乱系统库，我们将创建一个 Python 虚拟环境 `Virtualenv`：

```bash
$ pip install virtualenv
$ virtualenv venv
$ source venv/bin/activate
(venv)$ pip install cfnlint
```

我们从导入所需模块和创建规则类骨架开始：

```python
# rdsdeletionpolicy.py
from cfnlint.rules import CloudFormationLintRule
from cfnlint.rules import RuleMatch

class RdsDeletionPolicy(CloudFormationLintRule):
    """This rule is used to verify if resources AWS::RDS::DBInstance
    and AWS::RDS::DBCluster have a deletion policy set to Snapshot or Retain"""

    id = 'W9001'
    shortdesc = 'Check RDS deletion policy'
    description = 'This rule checks DeletionPolicy on RDS resources to be Snapshot or Retain'

    def match(self, cfn):
        matches = []
        # Your Rule code goes here
        return matches
```

我们初始化了一个名为 `matches` 的空数组。该数组将包含符合我们规则的资源。

现在我们需要解析模板，获取仅想要评估的 RDS 资源，并检查它们是否有 `DeletionPolicy`。我们需要将代码添加到 `match` 函数中：

```python
# rdsdeletionpolicy.py (partial)
    def match(self, cfn):
        matches = []
        resources = cfn.get_resources(["AWS::RDS::DBInstance", "AWS::RDS::DBCluster"])
        for resource_name, resource in resources.items():
            deletion_policy = resource.get("DeletionPolicy")
            path = ['Resources', resource_name]
            if not deletion_policy:
                message = f"Resource {resource_name} does not have Deletion Policy!"
                matches.append(RuleMatch(path, message))
            elif deletion_policy not in ["Snapshot", "Retain"]:
                message = f"Resource {resource_name} does not have Deletion Policy set to Snapshot or Retain!"
                matches.append(RuleMatch(path, message))
        return matches
```

现在让我们看看我们的规则是什么样子的：

```python
# rdsdeletionpolicy.py
from cfnlint import CloudFormationLintRule
from cfnlint import RuleMatch

class RdsDeletionPolicy(CloudFormationLintRule):
    # ...

    def match(self, cfn):
        matches = []
        resources = cfn.get_resources(["AWS::RDS::DBInstance", "AWS::RDS::DBCluster"])
        for resource_name, resource in resources.items():
            deletion_policy = resource.get("DeletionPolicy")
            path = ['Resources', resource_name]
            if not deletion_policy:
                message = f"Resource {resource_name} does not have Deletion Policy!"
                matches.append(RuleMatch(path, message))
            elif deletion_policy not in ["Snapshot", "Retain"]:
                message = f"Resource {resource_name} does not have Deletion Policy set to Snapshot or Retain!"
```
