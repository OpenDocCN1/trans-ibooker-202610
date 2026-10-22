

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  name: cluster-developer
rules:
- apiGroups:
  - '*'
  resources:
  - '*'
  verbs:
  - get
  - list
  - watch
- nonResourceURLs:
  - '*'
  verbs:
  - get
  - list
  - watch
```

最後に、[例2-12に](#ch02.html#restricted-user-clusterrolebinding)示すように、ファイル`restricted-user-clusterrolebinding.yaml`でServiceAccountをClusterRoleにマッピングする。

### 例2-12. 制限されたパーミッションのためのClusterRoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: developer-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-developer
subjects:
- kind: ServiceAccount
  name: developer-user
  namespace: kubernetes-dashboard
```

以下の宣言的コマンドですべてのオブジェクトを作成する：

```
$ kubectl create -f restricted-user-serviceaccount.yaml
serviceaccount/restricted-user created
$ kubectl create -f restricted-user-clusterrole.yaml
clusterrole.rbac.authorization.k8s.io/cluster-developer created
$ kubectl create -f restricted-user-clusterrolebinding.yaml
clusterrolebinding.rbac.authorization.k8s.io/developer-user created
```

以下のコマンドで、制限ユーザのベアラートークンを生成する：

```
$ kubectl create token developer-user -n kubernetes-dashboard
eyJhbGciOiJSUzI1NiIsImtpZCI6...
```

ログインしているユーザに許可されていない演算子は、GUIでは無効なオプションとして表示されない。ただし、エラーメッセージは表示される。[図 2-7](#ch02.html#dashboard-forbidden-operation)は、操作を実行する権限を持っていないユーザを使って Pod を削除しようとした場合のダッシュボードの振る舞いを示している。

![](assets/ckss_0207.png)

## 安全でないコンフィギュレーションの引数を避ける

本番環境でダッシュボードを保護するには、認証認可を適切に構成するために必要な[実行引数](https://oreil.ly/gS1hE)を使用する必要がある。デフォルトでは、ログイン機能は有効になっており、HTTPSエンドポイントはポート8443で公開される。TLS 証明書を自動生成させたくない場合は、`--tls-cert-file`および`--tls-cert-key`コマンドラインオプションで指定することができる。

コマンドライン引数`--insecure-port`、HTTPエンドポイントを公開し、`--enable-insecure-login`、HTTPSではなくHTTPでログイン・ページを提供できるようにする設定は避けること。さらに、ログイン画面のSkipボタンをクリックするだけで認証方法を回避できるようになるので、`--enable-skip-login`。

# Kubernetesプラットフォームのバイナリを検証する

Kubernetesプロジェクトは、リリースごとにクライアント、サーバのバイナリを公開している。クライアント・バイナリは、実行ファイル`kubectl`を指す。サーバ・バイナリには、`kubeadm`、APIサーバ、スケジューラ、kubeletの実行ファイルが含まれる。これらのファイルは、[Kubernetes GitHub](https://oreil.ly/vHpAV)リポジトリの "tags"セクションか、リリースページの[*https://dl.k8s.io。*](https://dl.k8s.io)

## シナリオ 攻撃者が悪意のあるコードをバイナリに注入した

実行ファイル`kubectl`と`kubeadm`は、Kubernetesとやりとりするのに欠かせない。`kubectl`を使うと、オブジェクトの管理など、サーバに対してコマンドを実行できる。`kubeadm`は、クラスタノードをあるバージョンから別のバージョンにアップグレードするのに必要だ。例えば、[クラスタのバージョンを](https://oreil.ly/hTJ57)1.23から1.24に[アップグレードして](https://oreil.ly/hTJ57)いるとしよう。そのプロセスの一環として、`kubeadm`のバイナリもアップグレードする必要がある。公式のアップグレードドキュメントには、バイナリのアップグレードにどのようなコマンドを使うべきか、非常に具体的に書かれている。

攻撃者がバージョン1.24の`kubeadm`実行ファイルを修正し、悪意のあるバイナリが置かれた場所からそのバイナリをダウンロードする必要があると思わせたとする。[図2-](#ch02.html#binary-attacker)8に示すように、変更された`kubeadm`実行可能ファイルを呼び出すたびに、悪意のあるコードを実行する危険にさらされることになる。例えば、クラスタ外のサーバに資格情報を送信している可能性があり、Kubernetes環境に侵入する新たな方法が開かれることになる。

![](assets/ckss_0208.png)

## ハッシュに対するバイナリの検証

バイナリの有効性は、MD5やSHAなどのハッシュコードの助けを借りて確認できる。Kubernetesは各バイナリのSHA256ハッシュコードを公開している。初めて使用する前に、個々のバイナリのハッシュ検証を実行する必要がある。生成されたハッシュコードがダウンロードしたものと一致しない場合は、バイナリに何か問題がある。バイナリがサードパーティによって変更されているか、正しいバイナリのタイプやバージョンに対応するハッシュコードを使用していない可能性がある。

バイナリに対応するハッシュコードは、[*https://dl.k8s.io*](https://dl.k8s.io)からダウンロードできる[*。*](https://dl.k8s.io)ハッシュコードの完全なURLは、バイナリのバージョン、オペレーティングシステム、アーキテクチャを反映している。以下のリストは、Linux AMD64と互換性のあるプラットフォーム・バイナリのURLの例を示している：

* `kubectl` https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubectl.sha256
* `kubeadm` https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256
* `kubelet` https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubelet.sha256
* `kube-apiserver` https://dl.k8s.io/v1.26.1/bin/linux/amd64/kube-apiserver.sha256

バイナリの正当性をチェックするには、オペレーティングシステム固有のハッシュコード検証ツールを使う必要がある。マシンにツールがない場合は、インストールする必要があるかもしれない。以下のコマンドは、[Kubernetesのドキュメントで](https://oreil.ly/2FmVm)説明されているように、異なるオペレーティングシステムでのツールの使い方を示している：

* Linux: `echo "$(cat kubectl.sha256) kubectl" | sha256sum --check`
* MacOSXである：`echo "$(cat kubectl.sha256) kubectl" | shasum -a 256 --check`
* PowerShellでWindows：`$($(CertUtil -hashfile .\kubectl.exe SHA256)[1] -replace " ", "") -eq $(type .\kubectl.exe.sha256)`

以下のコマンドは、バージョン1.26.1の`kubeadm`バイナリと、それに対応するSHA256ハッシュファイルのダウンロードを示している：

```
$ curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm"
$ curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256"
```

検証ツール`shasum`、チェックサムが一致するかどうかを検証できる：

```
$ echo "$(cat kubeadm.sha256)  kubeadm" | shasum -a 256 --check
kubeadm: OK
```

前のコマンドは "OK"メッセージを返した。バイナリファイルは改ざんされていない。それ以外のメッセージは、バイナリを実行する際に潜在的なセキュリティ・リスクがあることを示している。

# 概要

ドメイン「クラスタのセットアップ」では、Kubernetesクラスタのセットアップに関連するセキュリティの側面にダイアルしている。`kubeadm`を使ってゼロからクラスタを作成していても、必ずしもベストプラクティスに従っているとは限らない。kube-benchを使って潜在的なセキュリティリスクを検出するのは良いスタートだ。ツールによって報告された問題を1つずつ修正する。また、クライアント、サーバのバイナリをチェックサムと照合し、攻撃者によって変更されていないことを確認するのもよい。クラスタとそのオブジェクトを管理するためにDashboardを使用する組織もある。Dashboardの認証と認可によって、少数の関係者にアクセスを制限するようにする。

セキュリティ面で重要なのはネットワーク通信だ。Pod間の通信はデフォルトでは制限されていない。Kubernetesの内部で動作しているアプリケーションアーキテクチャをよく見てみよう。アーキテクチャの要件を満たすために、PodからPodへの指向性のあるネットワーク・トラフィックだけを許可する。それ以外のネットワークトラフィックはすべて拒否する。アプリケーションをクラスタの外部に公開する場合は、IngressオブジェクトがTLSターミネーションで設定されていることを確認する。これにより、クライアントとKubernetesクラスタ間で送信されるパスワードのような機密情報を攻撃者が観測できないように、データが双方向で暗号化される。

# 試験の要点

ネットワークポリシーの目的と効果を理解する

デフォルトでは、Pod間の通信は制限されていない。デフォルトの拒否規則をインスタンス化して、最小権限の原則でPod間のネットワーク・トラフィックを制限する。ネットワークポリシーの属性`spec.podSelector`は、ラベル選択に基づいて規則が適用される対象のPodを選択する。イングレス・ルールとイグレス・ルールでは、送受信トラフィックを許可するためのPod、ネームスペース、IPアドレス、ポートを定義する。ネットワーク・ポリシーは集約できる。デフォルトの拒否規則は、イングレスおよび/またはイグレスのトラフィックを拒否することができる。追加のネットワークポリシーは、より細かい定義でこれらの規則を開くことができる。

クラスタコンポーネントの脆弱性を検出するためのkube-benchの使い方を実践する

Kubernetes CIS Benchmarkは、本番Kubernetes環境で推奨されるセキュリティ設定のベストプラクティスのセットである。kube-benchというツールを使えば、セキュリティリスクを検出するプロセスを自動化できる。kube-benchの実行により生成されるレポートには、検出された問題を修正するための詳細な修正アクションが記載されている。結果をどのように解釈し、どのように問題を軽減するかを学ぶ。

TLSターミネーションによるIngressの設定方法を知る

Ingressは、HTTPSエンドポイントを公開することで、暗号化されたデータを送受信するように設定できる。これを動作させるには、TLS Secretオブジェクトを作成し、それにTLS証明書と鍵を代入する必要がある。このシークレットは、`spec.tls[]`という属性を使ってイングレスが消費できる。

安全なアクセスのためにGUI要素を設定する方法を知る

Kubernetes DashboardなどのGUI要素は、オブジェクトを管理する便利な方法を提供する。アプリケーションが不正アクセスから保護されていなければ、攻撃者はクラスタに危害を加える可能性がある。試験のためには、特定の関係者のためにRBACを適切にセットアップする方法を知っている必要がある。さらに、セキュリティ関連のコマンドライン引数を大まかに理解していることも求められる。Dashboardのインストール手順を練習し、そのコマンドライン引数を調整する方法を学び、異なるユーザに対する権限設定の効果を理解する。

変更されたプラットフォームのバイナリを検出する方法を知る

`kubectl`や`kubeadm`のようなプラットフォームバイナリは、対応するハッシュコードと照合して検証することができる。ハッシュファイルの発見場所と、バイナリが改ざんされていないかどうかを確認するための検証ツールの使い方を知っておくこと。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1. クラスタ外のドメインへのイグジット・トラフィックを拒否するネットワーク・ポリシーを作成する。このネットワークポリシーは、`app=backend`のラベルを持つPodに適用され、他のネームスペースにあるPodへのUDPおよびTCPのポート53のイグジットトラフィックも許可する。

2. ポート80で`busybox:1.36.0`イメージを実行する`allowed`という名前のPodを作成し、`app=frontend`というラベルを代入する。`http://google.com`に`curl`呼び出しを行う。ネットワークポリシーはPodには適用されないので、ネットワーク呼び出しは許可されるはずである。

3. ポート80で`busybox:1.36.0`イメージを実行する`denied`という名前の別のPodを作成し、`app=backend`というラベルを代入する。`http://google.com`に`curl`呼び出しを行う。ネットワーク呼び出しはブロックされるはずである。

4. Kubernetes Dashboardをインストールするか、すでにインストールされていることを確認する。名前空間`kubernetes-dashboard`に、`observer-user`という名前のServiceAccountを作成する。さらに、対応するClusterRoleとClusterRoleBindingを作成する。ServiceAccountにはデプロイの閲覧のみを許可する。他のすべての演算子は拒否されるべきである。例として、`default`名前付けに`deploy`というデプロイを以下のコマンドで作成する：`kubectl create deployment deploy --image=nginx --replicas=3`.

5. `observer-user`という名前のServiceAccount用トークンを作成し、期限切れにならないようにする。トークンを使用してダッシュボードにログインする。デプロイのみが表示され、他のタイプのリソースは表示されないことを確認する。

6. Linux AMD64上のバージョン1.26.1のAPIサーバーのバイナリファイルをダウンロードする。バージョン1.23.1のAPIサーバー実行ファイルのSH256チェックサムファイルをダウンロードする。OS固有の検証ツールを実行し、結果を観察する。

# 第3章. クラスタ・ハードニング

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

