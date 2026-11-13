
# 使用 AWS CDK 和 Python 编写您的第一个模板

我们将从安装 CDK 的先决条件开始。尽管我们将使用 Python 编写代码，但仍然需要安装 `nodejs`。完成此操作后，我们可以安装 CDK：

```
$ npm install -g aws-cdk
```

之后，我们需要通过打印 CDK 版本来检查一切是否正确：

```
$ cdk --version
```

安装 CDK 并不意味着我们拥有所有的构建库。

首先，我们需要初始化环境：

```
$ mkdir app && cd app
$ cdk init app --profile personal --language=python
$ source .env/bin/activate
$ pip install -r requirements.txt
```

然后，我们安装 CDK 的核心组件：

```
$ pip install --upgrade aws-cdk.core
```

`cdk init` 命令将创建所有文件，因此我们可以立即开始开发基础设施。

## 准备构建

我们将构建与在*第 2 章 高级模板开发*中构建的相同基础设施。我们的基础设施将包含以下堆栈：

*   核心（网络、安全等）
*   Web 层
*   存储层

我们将从构建核心堆栈开始，因为许多资源将从中继承。

运行 `cdk init` 后，将创建多个文件和目录。

处理生成和部署的主文件称为 `app.py`，内容如下：

```
app.py
from aws_cdk import core
from app.app_stack import AppStack

app = core.App()
AppStack(app, "app")
app.synth()
```

此脚本初始化*构建* `app` 和 `AppStack` 类，最后运行 `synth()` 方法，该方法生成 CloudFormation 模板。

此外，我们还有一个实际包含 `AppStack` 类的文件：

```
app/app_stack.py
from aws_cdk import core

class AppStack(core.Stack):
    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)
        # The code that defines your stack goes here
```

这是我们必须在其中声明资源的类。

> **重要提示**
>
> CDK 创建的文件和对象命名可能看起来很奇怪。当我们运行 `cdk init` 时，它将创建一个 `app.py` 文件（此处 `app` 代表 *App*，即 CDK 中的主要构建）和一个目录 `X`，其中 `X` 是运行 `cdk init` 的当前工作目录的名称。在该新目录中，`cdk init` 将创建一个 `X_stack.py` 文件和一个 `XStack()` 类。为了开发方便和透明度，您应该花时间相应地重命名文件和对象。

CDK 创建了一个堆栈类，但此处的命名并非我们所需（此外，我们将需要多个类）。因此，我们将把 `app_stack.py` 重命名为 `core_stack.py`，将 `AppStack` 重命名为 `CoreStack`：

```
app/core_stack.py
from aws_cdk import core

class CoreStack(core.Stack):
    # ...
```

我们还将对 `app.py` 进行一些更改，以便导入正常工作：

```
app.py
from aws_cdk import core
from app.core_stack import CoreStack

app = core.App()
CoreStack(app, "core")
app.synth()
```

我们还在类初始化调用中做了一个更改，将 `"app"` 改为 `"core"`：

```
CoreStack(app, "core")
```

引号中的字符串代表 `Id`，即 CDK 内部堆栈的唯一标识符。部署期间，此 ID 将成为堆栈名称，但我们现在进行此更改是为了让我们更清晰。

我们已完成所有准备工作。现在，让我们开始编写核心模板。

## 渲染核心资源

正如我们所记得的，核心堆栈需要以下资源：

*   1 个 VPC
*   3 个公有子网
*   3 个私有子网
*   1 个互联网网关
*   1 个 NAT 网关
*   1 个公有路由表
*   1 个私有路由表
*   2 个 IAM 角色（供管理员和开发人员使用）

CDK 不会安装所有构建库，因此我们需要安装其中两个：`aws_cdk.aws_iam` 和 `aws_cdk.aws_ec2`：

```
$ pip install aws_cdk.aws_iam aws_cdk.aws_ec2
```

现在我们可以导入它们：

```
app/core_stack.py
from aws_cdk import core
from aws_cdk.core import Fn
import aws_cdk.aws_ec2 as ec2
import aws_cdk.aws_iam as iam
```

通过以下步骤，我们将使用 CDK 声明核心堆栈：

1.  让我们从 IAM 角色开始。我们通过在堆栈类中声明变量来创建它们：

    ```
    app/core_stack.py
    # imports...
    class CoreStack(core.Stack):
        def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
            super().__init__(scope, id, **kwargs)
            admin_role = iam.Role(self,
                                  "admin",
                                  assumed_by=iam.AccountPrincipal(Fn.ref("AWS::AccountId")))
            dev_role = iam.Role(self,
                                "developer",
                                assumed_by=iam.AccountPrincipal(Fn.ref("AWS::AccountId")))
    ```

    同样，我们需要编写的代码更少，因为 `iam.Role` 对象有一个 `assumed_by` 属性。这将自动转换为 `AssumeRolePolicyDocument`。

2.  为了检查一切是否按预期生成，我们可以运行 `cdk synth`：

    ```
    $ cdk synth
    Resources:
      admin81D8EBF0:
        Type: AWS::IAM::Role
        Properties:
          AssumeRolePolicyDocument:
            Statement:
              - Action: sts:AssumeRole
                Effect: Allow
                Principal:
                  AWS:
                    Fn::Join:
                      - ""
                      - - "arn:"
                        - Ref: AWS::Partition
                        - ":iam::"
                        - Ref: AWS::AccountId
                        - :root
            Version: "2012-10-17"
    # the rest of the output...
    ```

3.  模板看起来有效，因此我们可以继续为角色添加策略：

    ```
    app/core_stack.py
    class CoreStack(core.Stack):
        def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
            # ...
            admin_role.add_managed_policy(iam.ManagedPolicy.from_aws_managed_policy_name("AdministratorAccess"))
            dev_role.add_managed_policy(iam.ManagedPolicy.from_aws_managed_policy_name("ReadOnlyAccess"))
    ```

    我们只需添加托管策略，因此将使用 `add_managed_policy()` 方法。

4.  现在我们可以开始构建网络。我们需要一个公有子网和若干私有子网：

    ```
    class CoreStack(core.Stack):
        # ...
        # VPC section
        self.vpc = ec2.Vpc(self,
                           "vpc",
    ```
