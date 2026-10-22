### 第 7 章 "监控、日志、运行时安全"

#### 1.

使用以下命令进入工作节点 Shell：

```
$ vagrant ssh kube-worker-1
```

检查正在运行的 `malicious` Pod 的命令和参数。可以看到它试图向 `/etc/threat` 文件添加消息：

```
$ kubectl get pod malicious -o jsonpath='{.spec.containers[0].args}'
...
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do echo "attacker intrusion" >> /etc/threat; \
      sleep 5; done
...
```

Falco 的默认规则之一是监控尝试写入 `/etc` 目录的文件操作。标准输出中可以找到所有写入尝试的消息：

```
$ sudo journalctl -fu falco
Jan 24 23:40:18 kube-worker-1 falco[8575]: 23:40:18.359740123: Error \
File below /etc opened for writing (user=<NA> user_loginuid=-1 \
command=sh -c while true; do echo "attacker intrusion" >> /etc/threat; \
sleep 5; done pid=9763 parent=<NA> pcmdline=<NA> file=/etc/threat \
program=sh gparent=<NA> ggparent=<NA> gggparent=<NA> \
container_id=e72a6dbb63b8 image=docker.io/library/alpine)
...
```

搜索字符串 "etc opened for writing "，找到生成 `/etc/falco/falco_rules.yaml` 中消息的规则。规则如下：

```
- rule: Write below etc
  desc: an attempt to write to any file below /etc
  condition: write_etc_common
  output: "File below /etc opened for writing (user=%user.name \
           user_loginuid=%user.loginuid command=%proc.cmdline \
           pid=%proc.pid parent=%proc.pname pcmdline=%proc.pcmdline \
           file=%fd.name program=%proc.name gparent=%proc.aname[2] \
           ggparent=%proc.aname[3] gggparent=%proc.aname[4] \
           container_id=%container.id image=%container.image.repository)"
  priority: ERROR
  tags: [filesystem, mitre_persistence]
```

将此规则复制到文件 `/etc/falco/falco_rules.local.yaml`，并将输出定义修改为如下：

```
- rule: Write below etc
  desc: an attempt to write to any file below /etc
  condition: write_etc_common
  output: "%evt.time,%user.name,%container.id"
  priority: ERROR
  tags: [filesystem, mitre_persistence]
```

重启 Falco 服务，并在 Falco 日志中发现更改后的输出：

```
$ sudo systemctl restart falco
$ sudo journalctl -fu falco
Jan 24 23:48:18 kube-worker-1 falco[17488]: 23:48:18.516903001: \
Error 23:48:18.516903001,<NA>,e72a6dbb63b8
...
```

编辑文件 `/etc/falco/falco.yaml`，更改输出通道。禁用标准输出，启用文件输出，并将 `file_output` 属性指向文件 `/var/log/falco.log`。结果配置如下：

```
file_output:
  enabled: true
  keep_alive: false
  filename: /var/log/falco.log

stdout_output:
  enabled: false
```

日志文件会追加 Falco 的日志：

```
$ sudo tail -f /var/log/falco.log
00:10:30.425084165: Error 00:10:30.425084165,<NA>,e72a6dbb63b8
...
```

退出虚拟机：

```
$ exit
```

#### 2.

从 `setup.yaml` 文件创建名为 `hash` 的 Pod。其容器中运行的命令会无限循环地向 `/var/config/hash.txt` 文件添加哈希：

```
$ kubectl apply -f setup.yaml
pod/hash created
$ kubectl get pod hash
NAME   READY   STATUS    RESTARTS   AGE
hash   1/1     Running   0          27s
$ kubectl exec -it hash -- /bin/sh
/ # ls /var/config/hash.txt
/var/config/hash.txt
```

要使容器不可变，需要在现有 Pod 定义中添加配置。需要将根文件系统设置为只读，并将路径 `/var/config` 挂载为卷以允许对 `hash.txt` 文件进行写入。结果 YAML 清单如下：

```
apiVersion: v1
kind: Pod
metadata:
  name: hash
spec:
  containers:
  - name: hash
    image: alpine:3.17.1
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: hash-vol
      mountPath: /var/config
    command: ["sh", "-c", "if [ ! -d /var/config ]; then mkdir -p \
              /var/config; fi; while true; do echo $RANDOM | md5sum \
              | head -c 20 >> /var/config/hash.txt; sleep 20; done"]
  volumes:
  - name: hash-vol
    emptyDir: {}
```

#### 3.

使用以下命令进入控制平面节点 Shell：

```
$ vagrant ssh kube-control-plane
```

编辑 `/etc/kubernetes/audit/rules/audit-policy.yaml` 中现有的审计策略文件。添加说明中要求的规则。最终审计策略文件内容如下：

```
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"
rules:
  - level: RequestResponse
    resources:
    - group: ""
      resources: ["pods"]
  - level: Metadata
    resources:
    - group: ""
      resources: ["secrets", "configmaps"]
  - level: Request
    resources:
    - group: ""
      resources: ["services"]
```

编辑文件 `/etc/kubernetes/manifests/kube-apiserver.yaml`，以配置 API 服务器使用审计策略文件。根据要求指定额外的选项。所需配置如下：

```
...
spec:
  containers:
  - command:
    - kube-apiserver
    - --audit-policy-file=/etc/kubernetes/audit/rules/audit-policy.yaml
    - --audit-log-path=/var/log/kubernetes/audit/logs/apiserver.log
    - --audit-log-maxage=5
    ...
    volumeMounts:
    - mountPath: /etc/kubernetes/audit/rules/audit-policy.yaml
      name: audit
      readOnly: true
    - mountPath: /var/log/kubernetes/audit/logs/
      name: audit-log
      readOnly: false
  ...
  volumes:
  - name: audit
    hostPath:
      path: /etc/kubernetes/audit/rules/audit-policy.yaml
      type: File
  - name: audit-log
    hostPath:
      path: /var/log/kubernetes/audit/logs/
      type: DirectoryOrCreate
```

记录资源之一是 `Metadata` 级别的 ConfigMap。以下命令创建 ConfigMap 对象的示例：

```
$ kubectl create configmap db-user --from-literal=username=tom
configmap/db-user created
```

审计日志文件将包含此事件的条目：

```
$ sudo cat /var/log/kubernetes/audit/logs/apiserver.log
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata", \
"auditID":"1fbb409a-3815-4da8-8a5e-d71c728b98b1","stage": \
"ResponseComplete","requestURI":"/api/v1/namespaces/default/configmaps? \
fieldManager=kubectl-create\u0026fieldValidation=Strict","verb": \
"create","user":{"username":"kubernetes-admin","groups": \
["system:masters","system:authenticated"]},"sourceIPs": \
["192.168.56.10"], "userAgent":"kubectl/v1.24.4 (linux/amd64) \
kubernetes/95ee5ab", "objectRef":{"resource":"configmaps", \
"namespace":"default", "name":"db-user","apiVersion":"v1"}, \
"responseStatus":{"metadata": {},"code":201}, \
"requestReceivedTimestamp":"2023-01-25T18:57:51.367219Z", \
"stageTimestamp":"2023-01-25T18:57:51.372094Z","annotations": \
{"authorization.k8s.io/decision":"allow", \
"authorization.k8s.io/reason":""}}
```

退出虚拟机：

```
$ exit
```
