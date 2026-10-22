### 第 6 章 "供应链安全"

#### 1.

使用提供的 Dockerfile 构建的初始容器镜像大小为 998MB。可以使用以下命令生产并运行容器镜像。为验证是否可以到达应用程序公开的端点，快速执行`curl`命令：

```
$ docker build . -t node-app:0.0.1
...
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
node-app     0.0.1     7ba99d4ba3af   3 seconds ago   998MB
$ docker run -p 3001:3001 -d node-app:0.0.1
c0c8a301eeb4ac499c22d10399c424e1063944f18fff70ceb5c49c4723af7969
$ curl -L http://localhost:3001/
Hello World
```

其中一个改进是不使用大型基础镜像。可以替换为`alpine`版本的 node 基础镜像。此外，`latest`。选择实际要运行应用程序的 Node.js 版本。以下命令使用基础镜像`node:19-alpine`，将容器镜像大小缩小到 176MB 的 Dockerfile：

```
$ docker build . -t node-app:0.0.1
...
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
node-app     0.0.1     ef2fbec41a75   2 seconds ago    176MB
```

#### 2.

Kyverno 的安装可使用 Helm 或指向项目 GitHub 仓库中的 YAML 清单进行。这里使用 YAML 清单：

```
$ kubectl create -f https://raw.githubusercontent.com/kyverno/\
kyverno/main/config/install.yaml
```





`restrict-image-registries.yaml` という名前の YAML マニフェストファイルをセットアップする。このファイルに以下の内容を追加する。このマニフェストは、`gcr.io/` で始まるコンテナイメージの使用のみを許可する `ClusterPolicy` を表す。属性 `spec.validationFailureAction` に値 `Enforce` を代入する：

```
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-image-registries
  annotations:
    policies.kyverno.io/title: Restrict Image Registries
    policies.kyverno.io/category: Best Practices, EKS Best Practices
    policies.kyverno.io/severity: medium
    policies.kyverno.io/minversion: 1.6.0
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      Images from unknown, public registries can be of dubious quality \
      and may not be scanned and secured, representing a high degree of \
      risk. Requiring use of known, approved registries helps reduce \
      threat exposure by ensuring image pulls only come from them. This \
      policy validates that container images only originate from the \
      registry `eu.foo.io` or `bar.io`. Use of this policy requires \
      customization to define your allowable registries.
spec:
  validationFailureAction: Enforce
  background: true
  rules:
  - name: validate-registries
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "Unknown image registry."
      pattern:
        spec:
          containers:
          - image: "gcr.io/*"
```

以下のコマンドでマニフェストを適用する：

```
$ kubectl apply -f restrict-image-registries.yaml
```

以下のコマンドを実行して、ポリシーがアクティブになったことを確認する。接頭辞 `gcr.io/` を使用しないコンテナイメージ定義は拒否される：

```
$ kubectl run nginx --image=nginx:1.23.3
Error from server: admission webhook "validate.kyverno.svc-fail" \
denied the request:

policy Pod/default/nginx for resource violation:

restrict-image-registries:
  validate-registries: 'validation error: Unknown image registry. \
  rule validate-registries
    failed at path /spec/containers/0/image/'
$ kubectl run busybox --image=gcr.io/google-containers/busybox:1.27.2
pod/busybox created
```

#### 3.
Docker Hubの検索機能を使って、イメージ `nginx:1.23.3-alpine` のSHA256ハッシュを発見する。[検索](https://oreil.ly/a4o8E)結果はイメージのタグにつながる。ページの一番上に、ダイジェスト `sha256:c1b9fe3c0c015486cf1e4a0ecabe78d05864475e279638e9713eb55f013f907f` が見つかるはずだ。Pod 定義でタグの代わりにダイジェストを使用する。結果は以下のYAMLマニフェストになる：

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx@sha256:c1b9fe3c0c015486cf1e4a0ecabe78d05864475e279638e9713eb55f013f907f
```

Podの作成はうまくいくはずだ：

```
$ kubectl apply -f pod-validate-image.yaml
pod/nginx created
$ kubectl get pods nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          29s
```

SHA256ハッシュを何らかの形で変更し、Podを再作成しようとすると、Kubernetesはイメージを引き出すことを許可しない。

##### 4.
DockerコンテナでKubesecを実行すると、以下の出力に示すように、たくさんのサジェストが表示される：

```
$ docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin < pod.yaml
[
  {
    "object": "Pod/hello-world.default",
    "valid": true,
    "message": "Passed with a score of 0 points",
    "score": 0,
    "scoring": {
      "advise": [
        {
          "selector": "containers[] .securityContext .capabilities \
                       .drop | index(\"ALL\")",
          "reason": "Drop all capabilities and add only those \
                     required to reduce syscall attack surface"
        },
        {
          "selector": "containers[] .resources .requests .cpu",
          "reason": "Enforcing CPU requests aids a fair balancing \
                     of resources across the cluster"
        },
        {
          "selector": "containers[] .securityContext .runAsNonRoot \
                       == true",
          "reason": "Force the running image to run as a non-root \
                     user to ensure least privilege"
        },
        {
          "selector": "containers[] .resources .limits .cpu",
          "reason": "Enforcing CPU limits prevents DOS via resource \
                     exhaustion"
        },
        {
          "selector": "containers[] .securityContext .capabilities \
                       .drop",
          "reason": "Reducing kernel capabilities available to a \
                     container limits its attack surface"
        },
        {
          "selector": "containers[] .resources .requests .memory",
          "reason": "Enforcing memory requests aids a fair balancing \
                     of resources across the cluster"
        },
        {
          "selector": "containers[] .resources .limits .memory",
          "reason": "Enforcing memory limits prevents DOS via resource \
                     exhaustion"
        },
        {
          "selector": "containers[] .securityContext \
                       .readOnlyRootFilesystem == true",
          "reason": "An immutable root filesystem can prevent malicious \
                     binaries being added to PATH and increase attack \
                     cost"
        },
        {
          "selector": ".metadata .annotations .\"container.seccomp. \
                       security.alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure \
                     against unknown threats"
        },
        {
          "selector": ".metadata .annotations .\"container.apparmor. \
                       security.beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater \
                     protection from unknown threats. WARNING: NOT \
                     PRODUCTION READY"
        },
        {
          "selector": "containers[] .securityContext .runAsUser -gt \
                       10000",
          "reason": "Run as a high-UID user to avoid conflicts with \
                     the host's user table"
        },
        {
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access \
                     and should be configured with least privilege"
        }
      ]
    }
  }
]
```

修正したYAMLマニフェストは次のようになる：

```
apiVersion: v1
kind: Pod
metadata:
  name: hello-world
spec:
  serviceAccountName: default
  containers:
  - name: linux
    image: hello-world:linux
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    securityContext:
      readOnlyRootFilesystem: true
      runAsNonRoot: true
      runAsUser: 20000
      capabilities:
        drop: ["ALL"]
```

##### 5.
既存の `setup.yaml` マニフェストに対して `kubectl apply` コマンドを実行すると、名前付け `r61` に `backend`、`loop`、`logstash` という名前の Pod が作成される：

```
$ kubectl apply -f setup.yaml
namespace/r61 created
pod/backend created
pod/loop created
pod/logstash created
```

以下のコマンドで確認できる：

```
$ kubectl get pods -n r61
NAME       READY   STATUS    RESTARTS   AGE
backend    1/1     Running   0          115s
logstash   1/1     Running   0          115s
loop       1/1     Running   0          115s
```

`kubectl describe` コマンドを使用して、名前空間 `r61` 内の各 Pod の画像を確認する。使用されるイメージは `bmuschko/nodejs-hello-world:1.0.0`、`alpine:3.13.4`、`elastic/logstash:7.13.3` である：





```
$ kubectl describe pod backend -n r61
...
Containers:
  hello:
    Container ID:   docker://eb0bdefc75e635d03b625140d1e \
                    b229ca2db7904e44787882147921c2bd9c365
    Image:          bmuschko/nodejs-hello-world:1.0.0
    ...
```

使用 Trivy 执行文件检查所有镜像的漏洞：

```
$ trivy image bmuschko/nodejs-hello-world:1.0.0
$ trivy image alpine:3.13.4
$ trivy image elastic/logstash:7.13.3
```

仔细查看漏洞列表，可以发现所有镜像都包含严重程度为“CRITICAL”的问题。因此，删除所有 Pod：

```
$ kubectl delete pod backend -n r61
$ kubectl delete pod logstash -n r61
$ kubectl delete pod loop -n r61
```
