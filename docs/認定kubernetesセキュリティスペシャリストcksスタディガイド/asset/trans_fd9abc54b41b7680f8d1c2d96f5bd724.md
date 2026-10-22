

*   プラットフォームのバイナリをチェックサムと照合する

# ネットワークポリシーを使ってPod間通信を制限する

マイクロサービスアーキテクチャがKubernetesで機能するためには、Podがネットワークアドレス変換（NAT）なしで、同じノードまたは異なるノードで動作する別のPodに到達できる必要がある。Kubernetesは、すべてのPodに対して、そのノードのPod CIDR範囲から、作成時に一意のIPアドレスを代入する。このIPアドレスはエフェメラル（一時的）なものであるため、長期的に安定しているとは言えない。Podを再起動するたびに、新しいIPアドレスがリースされる。一貫したネットワーク・インタフェースに依存できるように、Pod間通信よりもPod-to-Service間通信を使用することを推奨する。

Podに代入されるIPアドレスは、すべてのノードとネームスペースで一意である。これは、各ノードの登録時に専用のサブネットを代入することで実現される。ノード上に新しいPodを作成する際、IPアドレスは割り当てられたサブネットからリースされる。これはContainer Network Interface（CNI）プラグインによって処理される。その結果、ノード上のPodは、クラスタの他のどのノード上で実行されている他のすべてのPodと通信できる。

ネットワーク・ポリシーはファイアウォール・ルールに似ているが、Pod間の通信に対して機能する。規則には、1つのネームスペース内または異なるネームスペースにまたがる1つまたは複数のPodのネットワークトラフィックの方向（ingressおよび/またはegress）、およびそれらのターゲットポートを含めることができる。ネットワークポリシーの基本について深く掘り下げた内容については、書籍『[*Certified Kubernetes Application Developer (CKAD) Study Guide*](https://learning.oreilly.com/library/view/certified-kubernetes-application/9781492083726)』（O'Reilly）または[Kubernetesのドキュメントを](https://oreil.ly/WChde)参照されたい。CKS試験では、主にネットワークポリシーによるクラスタレベルのアクセス制限に焦点が当てられている。

ネットワーク・ポリシーの規則を正しく定義することは難しい。[`networkpolicy.io`](https://networkpolicy.io)というページは、ネットワーク・ポリシーのためのビジュアル・エディターを提供し、ブラウザ上でグラフィカルに表示する。

## シナリオ攻撃者がPodにアクセスする

あなたが、3つのワーカーノードでKubernetesクラスタを運用している会社で働いているとする。ワーカーノード1は現在、マイクロサービスアーキテクチャの一部として2つのPodを実行している。KubernetesのPod間ネットワーク通信のデフォルトの振る舞いを考えると、Pod 1はPod 2と無制限に話すことができ、逆も同様だ。

[図2-](#ch02.html#pod-to-pod-communication-attacker)1でわかるように、攻撃者はPod 1にアクセスした。ネットワーク・ポリシーを定義しなければ、攻撃者は単純にPod 2に話しかけてさらなる損害を与えることができる。この脆弱性は単一のネームスペースに限定されるものではない。Pod3と4にもアクセスし、侵害することができる。

![](assets/ckss_0201.png)

## デフォルトの振る舞いを観察する

3つのPodをセットアップして、制限のないPod間ネットワーク通信を実際に試してみよう。[例 2-1](#ch02.html#setup-network-policy) を見るとわかるように、YAML マニフェストは`backend` と`frontend` という名前の Pod を名前空間`g04` で定義しています。`other` Pod は`default` 名前空間に存在する。名前空間とPodのラベル代入に注目してほしい。この章の後半でネットワークポリシーを定義するときに、これらを少し参照することになる。

### 例 2-1. 異なる名前空間にある 3 つの Pod の YAML マニフェスト

```
apiVersion: v1
kind: Namespace
metadata:
  labels:
    app: orion
  name: g04
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    tier: backend
  name: backend
  namespace: g04
spec:
  containers:
  - image: bmuschko/nodejs-hello-world:1.0.0
    name: hello
    ports:
    - containerPort: 3000
  restartPolicy: Never
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    tier: frontend
  name: frontend
  namespace: g04
spec:
  containers:
  - image: alpine
    name: frontend
    args:
    - /bin/sh
    - -c
    - while true; do sleep 5; done;
  restartPolicy: Never
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    tier: outside
  name: other
spec:
  containers:
  - image: alpine
    name: other
    args:
    - /bin/sh
    - -c
    - while true; do sleep 5; done;
  restartPolicy: Never
```

宣言的な`kubectl apply` コマンドを使用して、既存の YAML マニフェストからオブジェクトを作成することから始める：

```
$ kubectl apply -f setup.yaml
namespace/g04 created
pod/backend created
pod/frontend created
pod/other created
```

ネームスペース`g04` 、正しいPodが実行されていることを確認しよう。`-o wide` CLIオプションを使用して、Podに代入されている仮想IPアドレスを確認する。`backend` PodはIPアドレス10.0.0.43を使用し、`frontend` PodはIPアドレス10.0.0.193を使用する：

```
$ kubectl get pods -n g04 -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP           NODE     \
  NOMINATED NODE   READINESS GATES
backend    1/1     Running   0          15s   10.0.0.43    minikube \
  <none>           <none>
frontend   1/1     Running   0          15s   10.0.0.193   minikube \
  <none>           <none>
```

 `default` 名前空間は単一のPodを扱う：

```
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
other   1/1     Running   0          4h45m
```

`frontend` Podは、通信制限がかけられていないため、`backend` Podと話すことができる：

```
$ kubectl exec frontend -it -n g04 -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
remote file exists
/ # exit
```

`default` 名前空間に存在する`other` Podは、`backend` Podと問題なく通信できる：

```
$ kubectl exec other -it -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
remote file exists
/ # exit
```

次のセクションでは、deny-allネットワークポリシールールの助けを借りて、Pod間のネットワーク通信を最大レベルに制限することについて説明する。そして、マイクロサービスアーキテクチャが適切に機能するために必要な種類のネットワーク通信に対してのみ、イングレス通信やイグレス通信を開放する。

## 指向性ネットワークトラフィックを拒否する

Pod間のネットワーク・トラフィックを制限する最善の方法は、最小権限の原則を用いることである。最小特権とは、Podはネットワーク通信において最も低い特権で通信すべきであるということだ。通常は、あらゆる方向のトラフィックを許可しないことから始めて、アプリケーションアーキテクチャが必要とするトラフィックを開放する。

[Kubernetesのドキュメントには](https://oreil.ly/PZOGf)、役に立つYAMLマニフェストの例がいくつか用意されている。[例 2-2](#ch02.html#deny-ingress-network-policy)は、名前空間`g04` にあるすべての Pod へのイングレス・トラフィックを拒否するネットワーク・ポリシーを示している。

### 例2-2. デフォルトのdeny-allイングレスネットワークポリシー

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: g04
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

すべてのPodを選択することは、`spec.podSelector` 属性に割り当てられた値`{}` で示される。値属性`spec.policyTypes` はトラフィックの拒否方向を定義する。着信トラフィックに対しては、`Ingress` を配列に追加できる。発信トラフィックは、値`Egress` で指定できる。この例では、すべての流入トラフィックを拒否する。退出トラフィックは依然として許可されている。

deny-all」ネットワークポリシーの内容は、ファイル`deny-all-ingress-network-policy.yaml` に保存されている。次のコマンドはファイルからオブジェクトを作成する：

```
$ kubectl apply -f deny-all-ingress-network-policy.yaml
networkpolicy.networking.k8s.io/default-deny-ingress created
```

これでPod間ネットワーク通信のランタイムの振る舞いがどう変わったか見てみよう。先ほど使ったのと同じ`wget` コマンドを実行して観察すると、`frontend` Podは`backend` Podと通信できなくなった。CLIオプション`--timeout` で定義されているように、ネットワーク呼び出しは1秒後にタイムアウトする：

```
$ kubectl exec frontend -it -n g04 -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
wget: download timed out
/ # exit
```

さらに、別のネームスペースで動作しているPodも、`backend` Podに接続できなくなる。次の`wget` ・コマンドは、`default` ・ネームスペースで動作している`other` ・Podから、`backend` ・PodのIPアドレスに呼び出しを行う：

```
$ kubectl exec other -it -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
wget: download timed out
```

この呼び出しもタイムアウトになる。

## きめ細かな着信トラフィックを許可する

Networkポリシーは加法的である。ネットワーク通信に対してより多くの許可を与えるには、より細かい規則を持つ別のネットワーク・ポリシーを作成すればよい。`backend` Podへのイングレス・トラフィックを、同じネームスペースにある`frontend` Podからのみ許可したいとする。他のすべてのPodからのイングレス・トラフィックは、それらが実行されている名前空間とは無関係に拒否する必要がある。

ネットワーク・ポリシーは、ラベル選択を使って規則を定義することが多い。`g04` 名前空間と、同じ名前空間で動作しているPodオブジェクトのラベルを特定し、ネットワーク・ポリシーで使用できるようにする：

```
$ kubectl get ns g04 --show-labels
NAME   STATUS   AGE   LABELS
g04    Active   12m   app=orion,kubernetes.io/metadata.name=g04
$ kubectl get pods -n g04 --show-labels
NAME       READY   STATUS    RESTARTS   AGE     LABELS
backend    1/1     Running   0          9m46s   tier=backend
frontend   1/1     Running   0          9m46s   tier=frontend
```

名前空間`g04` のラベル代入は、キーと値のペア`app=orion` を含む。Pod`backend` のラベルセットにはキー・バリューのペア`tier=backend` が含まれ、`frontend` Pod にはキー・バリューのペア`tier=frontend` が含まれる。

`frontend` Podがポート3000でのみ`backend` Podと通信できるように、新しいネットワークポリシーを作成する。それ以外の通信は許可しない。[例 2-3](#ch02.html#ingress-network-policy)の YAML マニフェスト表現は、ネットワークポリシーの完全な定義を示している。

### 例2-3. イングレス・トラフィックを許可するネットワーク・ポリシー

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-ingress
  namespace: g04
spec:
  podSelector:
    matchLabels:
      tier: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          app: orion
      podSelector:
        matchLabels:
          tier: frontend
    ports:
    - protocol: TCP
      port: 3000
```

ネットワーク・ポリシーの定義はファイル`backend-ingress-network-policy.yaml` に格納されている。ファイルからオブジェクトを作成する：

```
$ kubectl apply -f backend-ingress-network-policy.yaml
networkpolicy.networking.k8s.io/backend-ingress created
```

`frontend` Podは`backend` Podと会話できるようになった：

```
$ kubectl exec frontend -it -n g04 -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
remote file exists
/ # exit
```

 `g04` ネームスペースの外で実行されているPodは、まだ Podに接続できない。 コマンドはタイムアウトする：`backend` `wget`

```
$ kubectl exec other -it -- /bin/sh
/ # wget --spider --timeout=1 10.0.0.43:3000
Connecting to 10.0.0.43:3000 (10.0.0.43:3000)
wget: download timed out
```

# Kubernetesコンポーネントのセキュリティ・ベスト・プラクティスを適用する

