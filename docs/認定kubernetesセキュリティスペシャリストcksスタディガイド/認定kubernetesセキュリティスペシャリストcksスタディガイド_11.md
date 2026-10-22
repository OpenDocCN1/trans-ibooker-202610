### 第 5 章 "最小化微服务的脆弱性"

#### 1.

在文件`busybox-security-context.yaml`中定义具有安全设置的 Pod。可以发现以下 YAML 清单的内容：

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox-security-context
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  volumes:
  - name: vol
    emptyDir: {}
  containers:
  - name: busybox
    image: busybox:1.28
    command: ["sh", "-c", "sleep 1h"]
    volumeMounts:
    - name: vol
      mountPath: /data/test
    securityContext:
      allowPrivilegeEscalation: false
```

使用以下命令创建 Pod：

```
$ kubectl apply -f busybox-security-context.yaml
$ kubectl get pod busybox-security-context
NAME                       READY   STATUS    RESTARTS   AGE
busybox-security-context   1/1     Running   0          54s
```

进入容器并创建文件。如安全上下文所定义，可以发现文件组为 2000：

```
$ kubectl exec busybox-security-context -it -- /bin/sh
/ $ cd /data/test
/data/test $ touch hello.txt
/data/test $ ls -l
total 0
-rw-r--r--    1 1000     2000             0 Nov 21 18:29 hello.txt
/data/test $ exit
```

#### 2.

在文件`psa-namespace.yaml`中指定名为`audited`的命名空间。将 PSA 标签设置为`baseline`级别和`warn`模式：

```
apiVersion: v1
kind: Namespace
metadata:
  name: audited
  labels:
    pod-security.kubernetes.io/warn: baseline
```

从 YAML 清单创建命名空间：

```
$ kubectl apply -f psa-namespace.yaml
```

使用文件`psa-pod.yaml`中的以下 Pod 配置即可生成错误。YAML 清单设置了`hostNetwork: true`属性，这在[ `baseline`级别下](https://oreil.ly/c8JEW)是不允许的：

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: audited
spec:
  hostNetwork: true
  containers:
  - name: busybox
    image: busybox:1.28
    command: ["sh", "-c", "sleep 1h"]
```

创建 Pod 时会显示警告消息。尽管如此，Pod 仍会被创建。通过将 Pod 设置为`restricted`级别，可以阻止 Pod 的创建：

```
$ kubectl apply -f psa-pod.yaml
Warning: would violate PodSecurity "baseline:latest": host namespaces \
(hostNetwork=true)
pod/busybox created
$ kubectl get pod busybox -n audited
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          2m21s
```

#### 3.

Gatekeeper 可通过以下命令安装：

```
$ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
gatekeeper/master/deploy/gatekeeper.yaml
```

Gatekeeper 库中描述了用于[限制副本数](https://oreil.ly/gyD1-)的 ConstraintTemplate。查看本页中描述的 YAML 清单。使用以下命令应用该清单：

```
$ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
gatekeeper-library/master/library/general/replicalimits/template.yaml
```

在此，使用名为`replica-limits-constraint.yaml`的文件中的 YAML 清单定义约束：

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sReplicaLimits
metadata:
  name: replica-limits
spec:
  match:
    kinds:
      - apiGroups: ["apps"]
        kinds: ["Deployment"]
  parameters:
    ranges:
    - min_replicas: 3
      max_replicas: 10
```

使用以下命令创建约束：

```
$ kubectl apply -f replica-limits-constraint.yaml
```

可以发现，只有在指定的副本数在约束范围内时，部署才可以创建：

```
$ kubectl create deployment nginx --image=nginx:1.23.2 --replicas=15
error: failed to create deployment: admission webhook \
"validation.gatekeeper.sh" denied the request: [replica-limits] \
The provided number of replicas is not allowed for deployment: nginx. \
Allowed ranges: {"ranges": [{"max_replicas": 10, "min_replicas": 3}]}
$ kubectl create deployment nginx --image=nginx:1.23.2 --replicas=7
deployment.apps/nginx created
```

##### 4.

如 etcd 数据加密中所述，设置 etcd 加密。接下来，使用以下必需命令创建新的 Secret：

```
$ kubectl create secret generic db-credentials \
  --from-literal=api-key=YZvkiWUkycvspyGHk3fQRAkt
```

存储在 etcd 中的 Secret 的加密值可通过以下命令确认：

```
$ sudo ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/\
etcd/server.key get /registry/secrets/default/db-credentials | hexdump -C
```

##### 5.

使用 Vagrant 打开对工作节点的交互式 shell：

```
$ vagrant ssh kube-worker-1
```

定义 RuntimeClass，使用以下 YAML 清单。内容存储在文件`runtime-class.yaml`中：

```
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: container-runtime-sandbox
handler: runsc
```

创建 RuntimeClass 对象：

```
$ kubectl apply -f runtime-class.yaml
```

使用`spec.runtimeClassName`属性将 RuntimeClass 的名称分配给 Pod。nginx Pod 在文件`pod.yaml`中定义：

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  runtimeClassName: container-runtime-sandbox
  containers:
  - name: nginx
    image: nginx:1.23.2
```

创建 Pod 对象。Pod 将过渡到“Running”状态：

```
$ kubectl apply -f pod.yaml
$ kubectl get pod nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          2m21s
```

退出节点：

```
$ exit
```
