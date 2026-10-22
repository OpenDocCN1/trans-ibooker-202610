## 附录 复习问题解答

### 章节，“集群的设置”

1. 创建名为 `deny-egress-external.yaml` 的文件以定义网络策略。网络策略应将 Pod 选择器设置为 `app=backend`，并定义 `Egress` 策略类型。确保允许协议 UDP 和 TCP 的端口 53。Egress 策略的命名空间选择器应使用 `{}` 选择所有命名空间：

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-egress-external
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector: {}
    ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```

执行 `apply` 命令以从 YAML 文件实例化网络策略对象：

```
$ kubectl apply -f deny-egress-external.yaml
```

2. 与网络策略的标签选择不匹配的 Pod 可以调用集群外的 URL。在此情况下，标签分配为 `app=frontend`：

```
$ kubectl run web --image=busybox:1.36.0 -l app=frontend --port=80 -it \
  --rm --restart=Never -- wget http://google.com --timeout=5 --tries=1
Connecting to google.com (142.250.69.238:80)
Connecting to www.google.com (142.250.72.4:80)
saving to /'index.html'
index.html           100% |**| 13987 \
0:00:00 ETA
/'index.html' saved
pod "web" deleted
```

3. 与网络策略的标签选择匹配的 Pod 不能调用集群外的 URL。在此情况下，标签分配为 `app=backend`：

```
$ kubectl run web --image=busybox:1.36.0 -l app=backend --port=80 -it \
  --rm --restart=Never -- wget http://google.com --timeout=5 --tries=1
wget: download timed out
pod "web" deleted
pod default/web terminated (Error)
```

4. 首先，检查 Dashboard 是否已安装。可以检查 Dashboard 通常创建的命名空间：

```
$ kubectl get ns kubernetes-dashboard
NAME                   STATUS   AGE
kubernetes-dashboard   Active   109s
```

如果命名空间不存在，则认为 Dashboard 尚未安装。使用以下命令安装：

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/\
dashboard/v2.6.0/aio/deploy/recommended.yaml
```

创建 ServiceAccount、ClusterRole、ClusterRoleBinding。确保 ClusterRole 仅允许列出部署对象。以下 YAML 清单保存在文件 `dashboard-observer-user.yaml` 中：





```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: observer-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  name: cluster-observer
rules:
- apiGroups:
  - 'apps'
  resources:
  - 'deployments'
  verbs:
  - list
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: observer-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-observer
subjects:
- kind: ServiceAccount
  name: observer-user
  namespace: kubernetes-dashboard
```

以下コマンドでオブジェクトを作成する：

```
$ kubectl apply -f dashboard-observer-user.yaml
```

5. 以下のコマンドを実行して、`ServiceAccount`用のトークンを作成する。オプション`--duration 0s` 、トークンの有効期限が切れることはない。コマンドのコンソール出力に表示されたトークンをコピーする：

```
$ kubectl create token observer-user -n kubernetes-dashboard \
  --duration 0s
eyJhbGciOiJSUzI1NiIsImtpZCI6Ik5lNFMxZ1...
```

`Proxy`コマンドを実行し、[*http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy のリンクをブラウザで*](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy)開く：

```
$ kubectl proxy
```

トークン」認証メソッドを選択し、先ほどコピーしたトークンを貼り付ける。ダッシュボードにサインインする。デプロイ・オブジェクトだけがリスト可能であることがわかるはずである（図 A-1 参照）。

それ以外のオブジェクトは "There is nothing to display here." と表示される。図 A-2はPodのリストをレンダリングしたものである。

![](img/ckss_aa01.png)![](img/ckss_aa02.png)

6. 以下のコマンドでAPIサーバーのバイナリをダウンロードする：

```
$ curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kube-apiserver"
```

次に、同じバイナリで異なるバージョンのSHA256ファイルをダウンロードする。次のコマンドは、バージョン1.23.1のファイルをダウンロードする：

```
$ curl -LO "https://dl.k8s.io/v1.23.1/bin/linux/amd64/\
kube-apiserver.sha256"
```

バイナリファイルとチェックサムファイルを比較すると、バージョンが一致しないため失敗となる：

```
$ echo "$(cat kube-apiserver.sha256)  kube-apiserver" | shasum -a 256 \
  --check
kube-apiserver: FAILED
shasum: WARNING: 1 computed checksum did NOT match
```

### 章、"クラスタ・ハードニング"

1. `openssl` 実行ファイルを使用して秘密鍵を作成する。 のように、表現力豊かなファイル名を指定する。 オプションは、ユーザー名（CN）とグループ（O）を指定する。以下のコマンドでは、ユーザー名を 、グループ名を とする：`jill.key` `-subj` `jill` `observer`

```
$ openssl genrsa -out jill.key 2048
$ openssl req -new -key jill.key -out jill.csr -subj \
  "/CN=jill/O=observer"
```

`CSR`ファイル・コンテンツのbase64エンコード値を以下のコマンドで取得する。次のステップで`CertificateSigningRequest`オブジェクトを作成する際に必要になる：

```
$ cat jill.csr | base64 | tr -d "\n"
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tL...
```

以下のスクリプトは、`CertificateSigningRequest`オブジェクトを作成する：

```
$ cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: jill
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tL...
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400
  usages:
  - client auth
EOF
```

`certificate approve` コマンドを使用して署名要求を承認し、発行された証明書をエクスポートする：

```
$ kubectl certificate approve jill
$ kubectl get csr jill -o jsonpath={.status.certificate}| base64 \
  -d > jill.crt
```

`kubeconfig`ファイルにユーザを追加し、ユーザのコンテキストを追加する。ここで使用しているクラスタ名は`minikube` 。 Kubernetes 環境では異なるかもしれない：

```
$ kubectl config set-credentials jill --client-key=jill.key \
  --client-certificate=jill.crt --embed-certs=true
$ kubectl config set-context jill --cluster=minikube --user=jill
```

2. `Role`と`RoleBinding`を作成する。以下の命令形代入コマンドは、`Pod`、`ConfigMaps`、および `Secrets` に関する動詞`get` 、`list` 、および`watch` を、`group` 型の`observer` という名前のサブジェクトに代入する。ユーザ`jill` はグループの一部である：

```
$ kubectl create role observer --verb=create --verb=get --verb=list \
  --verb=watch --resource=pods --resource=configmaps --resource=secrets
$ kubectl create rolebinding observer-binding --role=observer \
  --group=observer
```

3. ユーザのコンテキストに切り替える：

```
$ kubectl config use-context jill
```

ここでは、`ConfigMap`オブジェクトをリストする、許可された操作を1つ選ぶ。ユーザは呼び出しのマッピングを許可されている：

```
$ kubectl get configmaps
NAME               DATA   AGE
kube-root-ca.crt   1      16m
```

リストノードが認証されない。ユーザが適切な権限を持っていない：

```
$ kubectl get nodes
Error from server (Forbidden): nodes is forbidden: User "jill" cannot \
list resource "nodes" in API group "" at the cluster scope
```

管理者コンテキストに戻る：

```
$ kubectl config use-context minikube
```

4. 名前空間`t23` を作成する：

```
$ kubectl create namespace t23
```

ネームスペースにサービスアカウント`api-call` を作成する：

```
$ kubectl create serviceaccount api-call -n t23
```

`pod.yaml` という名前の YAML マニフェストファイルを定義する。ファイルの内容は、`default` 名前空間内のサービスのリストを取得するために API サーバに HTTPS GET 呼び出しを行う Pod を定義する：

```
apiVersion: v1
kind: Pod
metadata:
  name: service-list
  namespace: t23
spec:
  serviceAccountName: api-call
  containers:
  - name: service-list
    image: alpine/curl:3.14
    command: ['sh', '-c', 'while true; do curl -s -k -m 5 \
              -H "Authorization: Bearer $(cat /var/run/secrets/\
              kubernetes.io/serviceaccount/token)" https://kubernetes.\
              default.svc.cluster.local/api/v1/namespaces/default/\
              services; sleep 10; done']
```

以下のコマンドでPodを作成する：

```
$ kubectl apply -f pod.yaml
```

Podのログを確認する。以下のログ出力に示されているように、API 呼び出しは許可されていない：

```
$ kubectl logs service-list -n t23
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "services is forbidden: User \"system:serviceaccount:t23 \
              :api-call\" cannot list resource \"services\" in API \
              group \"\" in the namespace \"default\"",
  "reason": "Forbidden",
  "details": {
    "kind": "services"
  },
  "code": 403
}
```

5. YAML マニフェストをファイル`clusterrole.yaml` に作成する：

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: list-services-clusterrole
rules:
- apiGroups: [""]
  resources: ["services"]
  verbs: ["list"]
```

ファイル`rolebinding.yaml` で定義された`RoleBinding`で`ClusterRole`を参照する。サブジェクトは、名前空間`t23` のサービスアカウント`api-call` をリストしなければならない：

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: serviceaccount-service-rolebinding
subjects:
- kind: ServiceAccount
  name: api-call
  namespace: t23
roleRef:
  kind: ClusterRole
  name: list-services-clusterrole
  apiGroup: rbac.authorization.k8s.io
```

YAMLマニフェストから両方のオブジェクトを作成する：

```
$ kubectl apply -f clusterrole.yaml
$ kubectl apply -f rolebinding.yaml
```

これで、コンテナ内で実行されているAPI 呼び出しが認証され、`default` 名前空間内のサービスオブジェクトのリストが許可されるはずである。以下の出力に示すように、ネームスペースは現在、少なくとも1つのサービスオブジェクト（`kubernetes.default` Service）をホストしている：

```
$ kubectl logs service-list -n t23
{
  "kind": "ServiceList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "1108"
  },
  "items": [
     {
       "metadata": {
         "name": "kubernetes",
         "namespace": "default",
         "uid": "30eb5425-8f60-4bb7-8331-f91fe0999e20",
         "resourceVersion": "199",
         "creationTimestamp": "2022-09-08T18:06:52Z",
         "labels": {
           "component": "apiserver",
           "provider": "kubernetes"
       },
       ...
     }
  ]
}
```

6. 以下のコマンドを使用して、サービス・アカウントのトークンを作成する：

```
$ kubectl create token api-call -n t23
eyJhbGciOiJSUzI1NiIsImtpZCI6IjBtQkJzVWlsQjl...
```




ライブ・オブジェクトを削除して再作成し、既存のPod 定義を変更する。以下のように、トークンの自動マウントを無効にする属性を追加する：

```
apiVersion: v1
kind: Pod
metadata:
  name: service-list
  namespace: t23
spec:
  serviceAccountName: api-call
  automountServiceAccountToken: false
  containers:
  - name: service-list
    image: alpine/curl:3.14
    command: ['sh', '-c', 'while true; do curl -s -k -m 5 \
              -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6Ij \
              BtQkJzVWlsQjl" https://kubernetes.default.svc.cluster. \
              local/api/v1/namespaces/default/services; sleep 10; \
              done']
```

APIサーバは、サービスアカウントのトークンを使って実行されたHTTPSリクエストを認証認可する：

```
$ kubectl logs service-list -n t23
{
  "kind": "ServiceList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "81194"
  },
  "items": [
     {
       "metadata": {
         "name": "kubernetes",
         "namespace": "default",
         "uid": "30eb5425-8f60-4bb7-8331-f91fe0999e20",
         "resourceVersion": "199",
         "creationTimestamp": "2022-09-08T18:06:52Z",
         "labels": {
           "component": "apiserver",
           "provider": "kubernetes"
       },
       ...
     }
  ]
}
```

7. このサンプル練習の解答は、多くの手動ステップを必要とする。以下のコマンドは出力を表示しない。

Vagrantを使ってコントロールプレーンノードへのインタラクティブシェルを開く：

```
$ vagrant ssh kube-control-plane
```

`kubeadm` をバージョン1.26.1にアップグレードし、適用する：

```
$ sudo apt-mark unhold kubeadm && sudo apt-get update && sudo apt-get \
  install -y kubeadm=1.26.1-00 && sudo apt-mark hold kubeadm
$ sudo kubeadm upgrade apply v1.26.1
```

ノードの水を抜き、kubeletと`kubectl` をアップグレードし、kubeletを再起動し、ノードのコードを解除する：

```
$ kubectl drain kube-control-plane --ignore-daemonsets
$ sudo apt-get update && sudo apt-get install -y \
  --allow-change-held-packages kubelet=1.26.1-00 kubectl=1.26.1-00
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
$ kubectl uncordon kube-control-plane
```

ノードのバージョンはv1.26.1と表示されているはずだ。ノードを終了する：

```
$ kubectl get nodes
$ exit
```

Vagrantを使って、最初のワーカーノードへの対話型シェルを開く。ワーカーノードに対して、以下のステップを全て繰り返す：

```
$ vagrant ssh kube-worker-1
```

`kubeadm` をバージョン1.26.1にアップグレードし、ノードに適用する：

```
$ sudo apt-get update && sudo apt-get install -y \
  --allow-change-held-packages kubeadm=1.26.1-00
$ sudo kubeadm upgrade node
```

ノードの水を抜き、kubeletと`kubectl` をアップグレードし、kubeletを再起動し、ノードのコードを解除する：

```
$ kubectl drain kube-worker-1 --ignore-daemonsets
$ sudo apt-get update && sudo apt-get install -y \
  --allow-change-held-packages kubelet=1.26.1-00 kubectl=1.26.1-00
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
$ kubectl uncordon kube-worker-1
```

ノードのバージョンはv1.26.1と表示されているはずだ。ノードを終了する：

```
$ kubectl get nodes
$ exit
```
