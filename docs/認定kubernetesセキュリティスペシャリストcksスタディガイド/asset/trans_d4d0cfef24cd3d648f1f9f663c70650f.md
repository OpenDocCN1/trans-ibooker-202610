

```
$ kubectl create -f psa-namespace.yaml
namespace/psa created
$ kubectl apply -f psa-violating-pod.yaml
Error from server (Forbidden): error when creating "psa-pod.yaml": pods \
"busybox" is forbidden: violates PodSecurity "restricted:latest": \
allowPrivilegeEscalation != false (container "busybox" must set \
securityContext.allowPrivilegeEscalation=false), unrestricted \
capabilities (container "busybox" must set securityContext. \
capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container \
"busybox" must set securityContext.runAsNonRoot=true), seccompProfile \
(pod or container "busybox" must set securityContext.seccompProfile. \
type to "RuntimeDefault" or "Localhost")
$ kubectl get pod -n psa
No resources found in psa namespace.
```

为了遵循非常严格的标准，需要配置 Pod 的安全上下文设置。[例5-8是](#ch05.html#psa-non-violating-pod)展示了一个不违反 Pod 安全标准示例 Pod 定义的示例。

### 例5-8. 遵循 PSS 的 Pod

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: psa
spec:
  containers:
  - image: busybox:1.35.0
    name: busybox
    command: ["sh", "-c", "sleep 1h"]
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
      runAsNonRoot: true
      runAsUser: 2000
      runAsGroup: 3000
      seccompProfile:
        type: RuntimeDefault
```

Pod 对象的创建按预期正常工作：

```
$ kubectl apply -f psa-non-violating-pod.yaml
pod/busybox created
$ kubectl get pod busybox -n psa
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          10s
```

PSA 是 Kubernetes 版本 `1.23` 及以后默认启用的内置功能。引入简单，可以选择合适的策略标准，也可以设置为强制执行，也可以仅记录违规日志。

不幸的是，PSA 仅适用于预先描述了策略集的 Pod。无法编写自己的自定义规则、更改消息传递或在 Pod 对象不符合 PSS 时对其进行变异。下一节将介绍超出 PSA 功能的工具。

## 理解 Open Policy Agent（OPA）和 Gatekeeper

[Open Policy Agent（OPA](https://oreil.ly/oK9pI)）是一个用于应用规则的开源通用策略引擎。OPA 并非专门针对 Kubernetes，也可在其他技术栈中使用。其优点之一是可以以非常灵活的方式定义策略。在[Rego](https://oreil.ly/0_mA8)查询语言的帮助下，可以编写自己的规则。用 Rego 编写的验证逻辑决定是否接受或拒绝对象。

[Gatekeeper](https://oreil.ly/AyVjP)是 Kubernetes 的扩展，使用 OPA。Gatekeeper 使能够为各种 Kubernetes API 原语定义和执行自定义策略。因此它比 PSA 通用得多，但需要更复杂的知识来创建这些规则。Gatekeeper 参与了[“请求处理”](#ch03.html#processing-api-request)中描述的*准入控制*阶段[。](#ch03.html#processing-api-request)下面列出了 Gatekeeper 可用的策略：

* 所有服务对象必须定义带有以下键的标签：`team`
* Pod 中定义的所有容器镜像必须从内部注册表中获取。
* 确保部署管理至少 3 个副本。

截至本文撰写时，Gatekeeper 可以通过在未满足要求时拒绝对象创建来强制执行策略。未来版本可能还会提供在对象创建时修改对象的机制。例如，可能希望向创建的对象添加特定标签的键值对。在这种情况下，变异将自动添加标签。

## Gatekeeper 的安装

Gatekeeper 的安装相对简单。只需从 Gatekeeper 项目提供的 YAML 清单创建 Kubernetes 对象即可。正确安装 Gatekeeper 需要集群管理员权限。以下命令是用于应用最新发布的 `kubectl` 命令。详情请参阅[安装手册](https://oreil.ly/CyZ1c)：

```
$ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
gatekeeper/master/deploy/gatekeeper.yaml
```

Gatekeeper 对象已安装到命名空间 `gatekeeper-system` 中。在使用 Gatekeeper 之前，请确保命名空间中的所有 Pod 都已迁移到“Running”状态：

```
$ kubectl get namespaces
NAME                STATUS   AGE
default             Active   29h
gatekeeper-system   Active   4s
...
```

## OPA 策略的实现

通过具体的用例示例，展示定义自定义 OPA 策略所需的组件。在“[使用网络策略限制 Pod 到 Pod 通信](#ch02.html#network-policies)”中，说明了如何为命名空间分配标签以便从网络策略中进行选择。核心自定义 OPA 策略决定命名空间必须至少定义一个带有表示托管应用程序的键 `app` 的标签。

在 Gatekeeper 中，需要实现两个组件来用于自定义策略：*约束模板*和*约束*。简单来说，约束模板用 Rego 定义规则并描述约束的模式。[例 5-9](#ch05.html#opa-constraint-template)是用于强制标签分配的约束模板定义。

### 例 5-9. 需要至少一个标签定义的 OPA 约束模板

```
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels 
      validation:
        openAPIV3Schema: 
          type: object
          properties:
            labels:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: | 
        package k8srequiredlabels

        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("you must provide labels: %v", [missing])
        }
```

[![1](assets/1.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO1-1)

: 

声明约束所使用的类型。

[![2](assets/2.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO1-2)

: 

指定约束的验证模式。在此情况下，可以传递名为 `labels` 的属性来捕获所需的标签键。

[![3](assets/3.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO1-3)

: 

使用 Rego 检查标签是否存在并与所需键列表进行比较。

约束本质上是约束模板的实现。它使用约束模板定义的类型并输入由终端用户提供的数据。[例5-10](#ch05.html#opa-constraint)中，类型为 `K8sRequiredLabels`，我们在约束模板中定义。我们正在命名空间上匹配，并期望它们定义带有键 `app` 的标签。

### 例 5-10. 定义策略“数据”的 OPA 约束

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels 
metadata:
  name: ns-must-have-app-label-key
spec:
  match: 
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters: 
    labels: ["app"]
```

[![1](assets/1.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO2-1)

: 

使用约束模板定义的类型。

[![2](assets/2.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO2-2)

: 

定义约束模板应适用的 API 资源。

[![3](assets/3.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO2-3)

: 

`labels` 属性声明期望键 `app` 存在。

相关 YAML 清单已就位，现在让我们创建约束模板和约束对象。假设约束模板已写入文件 `constraint-template-labels.yaml`，约束已写入文件 `constraint-ns-labels.yaml`：

```
$ kubectl apply -f constraint-template-labels.yaml
constrainttemplate.templates.gatekeeper.sh/k8srequiredlabels created
$ kubectl apply -f constraint-ns-labels.yaml
k8srequiredlabels.constraints.gatekeeper.sh/ns-must-have-app-label-key created
```

验证行为可以通过立即执行的命令式命令进行确认。以下命令尝试在没有标签分配的情况下创建新命名空间。Gatekeeper 将显示错误消息并阻止对象创建：

```
$ kubectl create ns governed-ns
Error from server (Forbidden): admission webhook "validation.gatekeeper.sh" \
denied the request: [ns-must-have-app-label-key] you must provide labels: {"app"}
```

让我们确认可以实际创建具有预期标签分配的命名空间。[例 5-11](#ch05.html#namespace-app-label)展示了此类命名空间的 YAML 清单。

### 例 5-11. 带有标签分配的命名空间 YAML 清单

```
apiVersion: v1
kind: Namespace
metadata:
  labels:
    app: orion
  name: governed-ns
```

以下命令从名为 `namespace-app-label.yaml` 的 YAML 清单文件创建对象：

```
$ kubectl apply -f namespace-app-label.yaml
namespace/governed-ns created
```

这个简单的示例展示了 OPA Gatekeeper 的使用方法。还可以发现更多示例，[OPA Gatekeeper Library](https://oreil.ly/1VV5e)。CKS 课程中未明确提及，但最近在 Kubernetes 社区备受关注的项目[Kyverno](https://kyverno.io)也值得查看。

# 管理密钥

关于 Kubernetes 安全功能的讨论如果不涉及 Secrets 主题就无法结束。你可能已经很熟悉用于定义机密数据的 API 原语 Secret 以及在 Pod 中消费它的各种选项。该主题已是 CKA 考试的一部分，因此此处不再赘述。详情请参阅[*Certified Kubernetes Administrator (CKA) Study Guide*](https://oreil.ly/cka-study-guide)的相关章节，或[Kubernetes 文档](https://oreil.ly/1afoc)。关于在容器内消费 ConfigMap 或 Secret 的安全方面，请参阅[“使用 ConfigMap 或 Secret 配置容器”](#ch07.html#configuring-container-configmap-secret)。

