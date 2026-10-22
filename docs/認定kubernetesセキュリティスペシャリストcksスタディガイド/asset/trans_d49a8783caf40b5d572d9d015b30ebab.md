

これらのポートの多くは設定可能である。例えば、構成ファイル`/etc/kubernetes/manifests/kube-apiserver.yaml`のフラグ`--secure-port`で異なる値を指定することで、クラスタ・コンポーネントの[ドキュメントにある](https://oreil.ly/TTzAz)ようにAPIサーバ・ポートを変更できる。他のすべてのクラスタ・コンポーネントについては、対応するドキュメントを参照のこと。

[表2-](#ch02.html#worker-node-ports)2はワーカーノードのデフォルトインバウンドポートのリストである。

### 表2-2. インバウンドワーカーノードポート

| ポート範囲 | | 目的 |
| --- | --- | --- |
| 10250 | | Kubelet API |
| --- | --- | --- |
| 30000-32767 | | ノードポート・サービス |

クラスタコンポーネントが使用するポートを保護するために、攻撃対象領域を最小化するファイアウォール規則を設定する。例えば、APIサーバをイントラネット外の誰にも公開しないように決めることができる。`kubectl`を使用するクライアントは、VPNにログインした場合のみKubernetesクラスタに対してコマンドを実行できるようになり、クラスタが攻撃を受けにくくなる。

クラウドプロバイダのKubernetesクラスタ（AWS、Azure、Google Cloudなど）は、いわゆるメタデータサービスを公開している。メタデータサービスは、認証トークンのような機密データを仮想マシンやPodから消費するために、追加の認可なしで提供できるAPIだ。CKS試験のためには、これらのノードエンドポイントとクラウドプロバイダのメタデータサービスを認識しておく必要がある。さらに、不正アクセスからそれらを保護する方法についても、高レベルで理解しておく必要がある。

### シナリオ 侵害されたPodがメタデータサーバにアクセスできる

[図2-3は](#ch02.html#metadata-server-attacker)、クラウドプロバイダーのKubernetesクラスタ内のノードで実行されているPodにアクセスした攻撃者を示している。

![](assets/ckss_0203.png)

メタデータサーバへのアクセスはいかなる形でも制限されていない。攻撃者は機密情報を取得することができ、他の侵入の可能性を開く可能性がある。

### ネットワークポリシーでメタデータサーバへのアクセスを保護する

メタデータのエンドポイントを公開しているクラウドプロバイダーを1つ選んでみよう。AWSの場合、[AWSのドキュメントに](https://oreil.ly/6DsIx)記載されているように、メタデータサーバにはIPアドレス`169.254.169.254`でアクセスできる。公開されたエンドポイントは、EC2インスタンスのメタデータへのアクセスを提供できる。例えば、インスタンスのローカルIPアドレスを取得して外部アプリケーションへの接続を管理したり、スクリプトを使用してインスタンスに接続したりできる。`curl`コマンドラインツールを使用したエンドポイントの呼び出しについては、対応する[ドキュメントのページを](https://oreil.ly/Bwdej)参照のこと。

ネームスペース内のどのPodもメタデータ・サーバのIPアドレスに到達しないようにするには、`169.254.169.254`以外のすべてのIPアドレスへのイグジット・トラフィックを許可するネットワーク・ポリシーを設定する。[例 2-7](#ch02.html#deny-egress-metadata-server)は、このような規則セットを持つYAMLマニフェストを示している。

#### 例2-7. IPアドレス169.254.169.254へのデフォルトdeny-all egressネットワークポリシー

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress-metadata-server
  namespace: a12
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 169.254.169.254/32
```

ネットワークポリシーが作成されると、名前空間`a12`のPodはメタデータエンドポイントに到達できなくなるはずである。`curl`経由でエンドポイントを使用する詳細な例については、関連する[AWS ドキュメントを](https://oreil.ly/fQ07b)参照のこと。

# GUI要素を保護する

クラスタを管理するためのユーザインタフェース（UI）は、`kubectl`ツールだけではない。`kubectl`ではきめ細かな操作が可能だが、ほとんどのユーザはクラスタのオブジェクトを管理するためにより便利なグラフィカル・ユーザ・インターフェイス（GUI）を好む。さまざまなオプションから選択できる。[Kubernetes Dashboardは](https://oreil.ly/ABDQo)無料のウェブベースのアプリケーションだ。[Portainerの](https://oreil.ly/i_FJv)ようなKubernetes用の他のGUIダッシュボードは、イベントのトレースやハードウェアリソース消費の可視化を追加することで、基本的な機能を超えている。このセクションでは、インストールと設定が簡単なKubernetes Dashboardに焦点を当てる。

## シナリオ 攻撃者がダッシュボード機能にアクセスする

Kubernetes Dashboardはクラスタ内部のPodとして実行される。Dashboardをインストールすると、`ClusterIP`、クラスタ内部からエンドポイントへのアクセスのみを許可するタイプのServiceも作成される。エンドユーザがDashboardにアクセスできるようにするには、クラスタの外部にServiceを公開する必要がある。たとえば、`NodePort`Serviceタイプに切り替えるか、Ingressを立ち上げる。[図 2-4](#ch02.html#dashboard-attacker)に、Dashboardのデプロイとアクセスに関する高レベルのアーキテクチャを示す。

![](assets/ckss_0204.png)

ダッシュボードを外界に公開した時点で、攻撃者はダッシュボードにアクセスできる可能性がある。適切なセキュリティ設定がなければ、オブジェクトが削除されたり、変更されたり、悪意のある目的に使用されたりする可能性がある。このような攻撃の最も顕著な被害者はテスラで、2018年に暗号通貨を採掘するために無防備なDashboardにアクセスしたハッカーの餌食になった。それ以来、Dashboardの新しいバージョンではデフォルト設定が変更され、最初からより安全になっている。

## Kubernetes Dashboardをインストールする

Kubernetes Dashboardのインストールは簡単だ。プロジェクトのGitHubリポジトリで利用可能なYAMLマニフェストの助けを借りて、関連するオブジェクトを作成できる。以下のコマンドは、必要なオブジェクトをすべてインストールする：

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/\
v2.6.0/aio/deploy/recommended.yaml
```

# ダッシュボードにメトリックをレンダリングする

また、ダッシュボード機能の一部としてリソース消費メトリクスの検査に興味がある場合は、[メトリクサーバを](https://oreil.ly/3Rtkl)インストールするとよい。

マニフェストによって作成されたオブジェクトは、`kubernetes-dashboard`名前空間で発見できる。その中には、デプロイ、Pod、およびサービスが含まれる。次のコマンドは、これらすべてのリストを表示する：

```
$ kubectl get deployments,pods,services -n kubernetes-dashboard
NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           11m
deployment.apps/kubernetes-dashboard        1/1     1            1           11m

NAME                                             READY   STATUS    RESTARTS   AGE
pod/dashboard-metrics-scraper-78dbd9dbf5-f8z4x   1/1     Running   0          11m
pod/kubernetes-dashboard-5fd5574d9f-ns7nl        1/1     Running   0          11m

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP \
  PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.98.6.37       <none>      \
  8000/TCP   11m
service/kubernetes-dashboard        ClusterIP   10.102.234.158   <none>      \
  80/TCP     11m
```

## Kubernetesダッシュボードにアクセスする

`kubectl proxy`コマンドは、ブラウザでダッシュボードを開くためのプロキシを一時的に作成するのに役立つ。この機能はトラブルシューティングを目的としており、本番環境向けではない。コマンドに関する情報は`proxy`[ドキュメントで](https://oreil.ly/gGsqX)発見できる：

```
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

URL `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy`でブラウザを開く。Dashboardが認証方法と資格情報の提供を求める。Dashboardを構成する推奨の方法は、ベアラートークンを使用することである。

## 管理者権限を持つユーザの作成

ログイン画面で認証する前に、管理者権限を与えるServiceAccountとClusterRoleBindingオブジェクトを作成する必要がある。まず、`admin-user-serviceaccount.yaml`ファイルを作成し、[例2-](#ch02.html#admin-user-serviceaccount)8に示す内容でそれを埋めることから始める。

#### 例2-8. 管理者権限を持つサービスアカウント

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

次に、`cluster-admin`という名前のClusterRoleをServiceAccountにマッピングするために、[例2-9の](#ch02.html#admin-user-clusterrolebinding)内容をファイル`admin-user-clusterrolebinding.yaml`に格納する。

#### 例2-9. 管理者権限用のClusterRoleBinding

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

以下の宣言的コマンドで両方のオブジェクトを作成する：

```
$ kubectl create -f admin-user-serviceaccount.yaml
serviceaccount/admin-user created
$ kubectl create -f admin-user-clusterrolebinding.yaml
clusterrolebinding.rbac.authorization.k8s.io/admin-user created
```

次のコマンドで管理者ユーザのベアラートークンを作成できる。このコマンドは、指定されたServiceAccountオブジェクトのトークンを生成し、コンソールに表示する：

```
$ kubectl create token admin-user -n kubernetes-dashboard
eyJhbGciOiJSUzI1NiIsImtpZCI6...
```

# サービス・アカウント・トークンの失効

デフォルトでは、このトークンは24時間後に失効する。つまり、「有効期間」（TTL）が過ぎると、トークン・オブジェクトは自動的に削除される。コマンドラインオプション`--ttl`を指定することで、トークンのTTLを変更することができる。たとえば、`40h`を指定すると、トークンは40時間後に失効する。`0`を指定すると、トークンは期限切れにならない。

[図2-](#ch02.html#dashboard-login-screen-token)5に示すように、コマンドの出力をコピーし、ログイン画面の「Enter token」フィールドにペーストする。

![](assets/ckss_0205.png)

Sign in "ボタンを押すと、[図2-6に](#ch02.html#dashboard-overview)示すダッシュボードが表示される。

![](assets/ckss_0206.png)

エンドユーザやクラスタのオブジェクトを制限なく管理できるようになった。

## 制限された権限を持つユーザの作成

前のセクションでは、クラスタ全体の管理者権限を持つユーザを作成する方法を学んだ。しかし、Dashboardのほとんどのユーザは、制限されたパーミッションのセットしか必要としない。たとえば、クラウドネイティブなアプリケーションを実装して運用する開発者は、Kubernetesクラスタ上でタスクを実行するために管理者権限のサブセットしか必要としない可能性が高い。制限された権限を持つDashboardのユーザを作成するには、3つのステップからなる：

1. ServiceAccountオブジェクトを作成する。
2. パーミッションを定義するClusterRoleオブジェクトを作成する。
3. ClusterRoleをServiceAccountにマッピングするClusterRoleBindingを作成する。

見てわかるように、このプロセスは管理者ユーザに対して行ったものと非常によく似ている。ステップ2が新しく、付与したいパーミッションを特定する必要がある。この後に続くYAMLマニフェストは、読み取り専用パーミッション(たとえば、リソースの取得、リスト表示、監視)だけを許可されるべき開発者として働くユーザをモデル化する。

まず、`restricted-user-serviceaccount.yaml`ファイルを作成し、[例2-10に](#ch02.html#restricted-user-serviceaccount)示す内容を入力する。

#### 例2-10. 制限された権限のサービスアカウント

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: developer-user
  namespace: kubernetes-dashboard
```

[例2-11の](#ch02.html#restricted-user-clusterrole)ClusterRoleでは、リソースの取得、リスト、ウォッチのみが許可されている。それ以外の演算子は許可されていない。内容をファイル`restricted-user-clusterrole.yaml`に格納する。

#### 例2-11. 制限されたパーミッションのためのClusterRole

