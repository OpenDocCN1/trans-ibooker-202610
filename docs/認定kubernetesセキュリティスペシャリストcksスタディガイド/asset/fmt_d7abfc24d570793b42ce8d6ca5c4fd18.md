
etcdデータの暗号化の詳細については、[Kubernetesのドキュメントを](https://oreil.ly/uIylK)参照されたい。そこには、他の暗号化プロバイダ、復号化キーのローテーション方法、高可用性（HA）クラスタのセットアップで考慮すべきプロセスに関する追加情報が発見できる。

# コンテナランタイムサンドボックスを理解する

コンテナは、ホスト環境から隔離されたコンテナ・ランタイムで実行される。コンテナ内で実行されるプロセスやアプリケーションは、システムコールを行うことでカーネルと相互作用できる。現在、（Podによって制御される）複数のコンテナを単一Kubernetesクラスタノード上で実行させることができるため、同じカーネルを使用することができる。ある条件付きでは、脆弱性によって、コンテナを実行しているプロセスが、その基盤となるマシンから「抜け出して」、同じホストマシン上で実行されている別のコンテナにアクセスできる状況に陥る可能性がある。*コンテナランタイムのサンドボックスは*、通常のコンテナランタイムと並行して実行されるが、プロセスの分離を強化することでセキュリティのレイヤーを追加する。

コンテナランタイム・サンドボックスを使用することが理にかなっているユースケースがいくつかある。例えば、Kubernetesクラスタが異なる顧客のワークロードを同じインフラで処理する、いわゆるマルチテナント環境だ。より強力なコンテナ分離に頼りたいもう1つの理由は、コンテナイメージ内で実行されているプロセスやアプリケーションを信頼できない可能性があることだ。例えば、コンテナイメージをパブリックレジストリから取得した場合、作成者やそのランタイムの振る舞いを確認できないことがあるはずだ。

## シナリオ攻撃者が別のコンテナにアクセスする

このシナリオでは、開発者が、Pod から参照されるコンテナイメージを公開レジストリから取得する場面に直面する。コンテナは、セキュリティ脆弱性についてスキャンされていない。攻撃者は、悪意のあるコードを実行するコンテナイメージの新しいタグをプッシュすることができる。イメージからコンテナをインスタンス化した後、コンテナ 1 のカーネルグループで実行されている悪意のあるコードは、コンテナ 2 で実行されているプロセスにアクセスできる。[図 5-4 を](#ch05.html#container-runtime-attacker)見ればわかるように、どちらのコンテナもホスト・システムの同じカーネルを使用している。

![](assets/ckss_0504.png)

一般的に言って、公開されているコンテナイメージを盲目的に信用するのは得策ではない。そのようなコンテナ・イメージがより分離された状態で実行されることを保証する方法の1つが、コンテナ・ランタイム・サンドボックスである。次のセクションでは、カリキュラムで明示されている2つの実装を紹介する。

## 利用可能なコンテナランタイムサンドボックスの実装

本書では、[Kata Containersと](https://katacontainers.io) [gVisorという](https://gvisor.dev)2つのコンテナランタイム・サンドボックスの実装についてだけ説明したい。Kataコンテナは、軽量な仮想マシンでコンテナを実行することでコンテナの分離を実現している。gVisorは、ホストシステム上で動作するLinuxカーネルを効果的に実装している。そのため、ホストシステム上のすべてのコンテナでシステムコールが共有されることはない。

これらのコンテナランタイムサンドボックス実装の機能セットや特定のユースケースについて深く議論することは、本書の範囲を超えている。ここでは、1つのソリューションであるgVisorを例として、それをKubernetesに結びつける方法を学ぶだけだ。詳細な比較については、講演[「KataコンテナとgVisor：定量的比較](https://oreil.ly/PBfEn)」を参照されたい。

## gVisorのインストールと設定

以下の[手順では](https://oreil.ly/MlSET)、`apt` パッケージマネージャを使用して Linux に gVisor をインストールするために必要な手順を説明する。ワーカーノードとして宣言されたすべてのホストマシンで、この手順を繰り返すことになる。試験では、gVisor や Kata Containers をインストールする必要はない。コンテナランタイムのサンドボックスはすでにインストールされ、設定されていると考えてよい。

以下のコマンドでgVisorの依存関係をインストールすることから始める：

```
$ sudo apt-get update && \
  sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg
```

次に、アーカイブとリポジトリの署名に使用するキーを設定する。以下のコマンドでわかるように、gVisorはGoogleストレージでホストされている：

```
$ curl -fsSL https://gvisor.dev/archive.key | sudo gpg --dearmor -o /usr/share/\
keyrings/gvisor-archive-keyring.gpg
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/\
gvisor-archive-keyring.gpg] https://storage.googleapis.com/gvisor/releases \
release main" | sudo tee /etc/apt/sources.list.d/gvisor.list > /dev/null
```

gVisorには、`runsc`と呼ばれるOCI（Open Container Initiative）ランタイムが含まれている。`runsc`ランタイムは、DockerやKubernetesのようなツールと統合し、コンテナランタイム・サンドボックスを実行する。以下のコマンドは、リポジトリから実行ファイルをインストールする：

```
$ sudo apt-get update && sudo apt-get install -y runsc
```

コンテナランタイムとして`containerd`を使っていると仮定しよう。`containerd`に`runsc`を認識させるための設定を追加する必要がある。他のコンテナ・ランタイムについても同様の説明がgVisorのドキュメントで発見できる：

```
$ cat <<EOF | sudo tee /etc/containerd/config.toml
version = 2
[plugins."io.containerd.runtime.v1.linux"]
  shim_debug = true
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  runtime_type = "io.containerd.runc.v2"
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runsc]
  runtime_type = "io.containerd.runsc.v1"
EOF
```

最後に`containerd`を再起動して、変更を有効にする：

```
$ sudo systemctl restart containerd
```

gVisorのインストールに成功し、Podを設定できるようになった。

## ランタイム・クラスの作成と使用

コンテナ・ランタイム・サンドボックスをPodで使うには、2段階のアプローチがある。まず、ランタイムクラスを作成する必要がある。RuntimeClassは、コンテナランタイムの構成を定義するKubernetes APIリソースである。[例5-13は](#ch05.html#gvisor-runtimeclass)、コンテナランタイムのYAMLマニフェストを示しており、先ほど`containerd`構成ファイルで設定したハンドラとして`runsc`を点示している。

### 例 5-13. runsc ハンドラを使用してランタイムクラスを定義するための YAML マニフェスト

```
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

これで、Podの構成でランタイム・クラス名`gvisor` を参照できるようになった。[例5-14は](#ch05.html#nginx-pod-gvisor)、`spec.runtimeClassName` 属性を使用してランタイム・クラスを代入するPod定義を示している。

### 例 5-14. ランタイムクラスを使用する Pod の YAML マニフェスト

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  runtimeClassName: gvisor
  containers:
  - name: nginx
    image: nginx:1.23.2
```

`apply` 、ランタイム・クラスとPodオブジェクトを作成する：

```
$ kubectl apply -f runtimeclass.yaml
runtimeclass.node.k8s.io/gvisor created
$ kubectl apply -f pod.yaml
pod/nginx created
```

コンテナが実行されているかどうかは、コンテナランタイムサンドボックスで確認できる。単に`dmesg` コマンドを実行して、カーネルのリングバッファを調べる。コマンドの出力は、以下のようにgVisorについて言及しているはずである：

```
$ kubectl exec nginx -- dmesg
[    0.000000] Starting gVisor...
[    0.123202] Preparing for the zombie uprising...
[    0.415862] Rewriting operating system in Javascript...
[    0.593368] Reading process obituaries...
[    0.741642] Segmenting fault lines...
[    0.797360] Daemonizing children...
[    0.831010] Creating bureaucratic processes...
[    1.313731] Searching for needles in stacks...
[    1.455084] Constructing home...
[    1.834278] Gathering forks...
[    1.928142] Mounting deweydecimalfs...
[    2.109973] Setting up VFS...
[    2.157224] Ready!
```

# mTLSによるPod間暗号化を理解する

[ネットワークポリシーを使ってPod間通信を制限する」では](#ch02.html#network-policies)、Pod間通信について話した。大きな収穫の一つは、より制限的なネットワークポリシーを設定しない限り、どのPodもその仮想IPアドレスをターゲットにして他のどのPodとも通信できるということだ。2つのPod間の通信は、デフォルトでは暗号化されていない。

TLSはネットワーク通信の暗号化を提供するもので、HTTPプロトコルと組み合わせて使われることが多い。ブラウザからWebページへの呼び出しに HTTPSプロトコルを使用するのはそのためである。認証プロセスの一環として、クライアントは自分の身元を証明するためにクライアント証明書をサーバに提供する。しかし、サーバはクライアントを認証しない。

Webページをロードする際、クライアント（この場合はブラウザ）の身元は通常問題にならない。重要なのは、Webページがその身元を証明することである。相互TLS（mTLS）はTLSのようなものだが、双方が認証する必要がある。この方法には以下の利点がある。第一に、暗号化によって安全な通信を実現できる。第二に、クライアントの身元を確認できる。攻撃者は簡単に他のPodになりすますことができない。

## シナリオ攻撃者が2つのPod間の通信を聞く

攻撃者は、暗号化されていないデフォルトのPod間ネットワーク通信の振る舞いを利用することができる。[図5-5を](#ch05.html#pod-communication-attacker)見れば分かるように、攻撃者はPodに侵入する必要すらない。送信側または受信側になりすましてPod間通信を盗聴し、機密情報を引き出して、さらに高度な攻撃ベクトルに利用するだけでよい。

![](assets/ckss_0505.png)

mTLSをセットアップすることで、この状況を緩和することができる。次のセクションでは、そのためのオプションについて簡単に触れる。

## KubernetesでmTLSを採用する

KubernetesクラスタにmTLSを実装する際に厄介なのは、証明書の管理だ。ご想像の通り、マイクロサービスアーキテクチャでは多くの証明書を扱わなければならない。これらの証明書は通常、信頼できることを保証するために公式の認証局（CA）によって生成される。証明書を要求するには、証明書署名要求（CSR）をCAに送信する。CAがリクエストを承認すると、証明書が作成され、署名されて返される。証明書を再発行する前に、証明書に短い寿命を代入することが推奨される。このプロセスを証明書のローテーションと呼ぶ。

CKS試験でmTLSをどの程度詳しく理解する必要があるのかは、やや不明だ。証明書のリクエストと承認の一般的なプロセスは、[Kubernetesのドキュメントに](https://oreil.ly/QvtEQ)記載されている。

ほとんどの場合、Kubernetes管理者はmTLSを手動で実装するのではなく、Kubernetesサービスメッシュに頼って実装している。LinkerdやIstioといったKubernetesサービスメッシュは、可観測性やセキュリティといった横断的な機能をクラスタに追加するためのツールだ。

もうひとつの選択肢は、トラフィックが暗号化されないままワイヤー上を通過しないようにするために、透過的な暗号化を使用することだ。[Calicoや](https://oreil.ly/XZSpx) [Ciliumといった](https://oreil.ly/Qsqq_)人気のあるCNIプラグインの中には、[WireGuardの](https://www.wireguard.com)サポートを追加したものもある。WireGuardはオープンソースで軽量かつ安全な仮想プライベートネットワーク（VPN）ソリューションであり、暗号鍵や証明書の設定や管理を必要としない。管理が容易なため、多くのチームがサービスメッシュよりもWireGuardを好んでいる。

サービスメッシュとワイヤーガードは試験範囲外である。

# 概要

Podに対してセキュリティのベストプラクティスを実施することは重要だ。この章では、さまざまなオプションを検討した。セキュリティ・コンテキストと、それをPodやコンテナ・レベルで定義する方法について見てきた。例えば、コンテナのセキュリティコンテキストを設定して、非 root ユーザで実行し、特権モードの使用を防ぐことができる。通常、これらの設定を定義するのは開発者の責任だ。Pod Security Admissionは、Podのセキュリティ設定をさらに一歩進めたKubernetesの機能だ。Podが特定のセキュリティ標準に準拠する必要があるように、Podを一元的に設定することができる。設定されたセキュリティ標準は、強制することも、監査することも、標準出力にログを記録することもできる。Gatekeeperは、Kubernetes用のOpen Policy Agentの機能を実装したオープンソースプロジェクトだ。Podオブジェクトの設定を管理できるだけでなく、作成時に他の種類のオブジェクトにポリシーを適用することもできる。

Secretsで定義されたキーバリューペアはetcdにプレーンテキストで保存される。攻撃者がetcdから機密データを読み取れないようにするために、etcdの暗号化を設定すべきである。暗号化を有効にするには、EncryptionConfiguration用のYAMLマニフェストを作成し、それをコマンドラインオプション`--encryption-provider-config` でAPIサーバプロセスに渡す。

コンテナランタイムのサンドボックスは、通常のコンテナランタイムよりも強力にプロセスやアプリケーションを分離するのに役立つ。Kata ContainersとgVisorというプロジェクトは、このようなコンテナランタイム・サンドボックスの実装であり、Kubernetesにインストールして動作するように設定することができる。gVisorを試してみた。gVisorをインストールして設定した後、`runsc`を指すRuntimeClassオブジェクトを作成する必要がある。 Podの設定で、RuntimeClassオブジェクトを名前で指定する。

Pod間の通信は、デフォルトでは暗号化も認証もされていない。相互TLSはこのプロセスをより安全にする。互いに通信するPodは、身元を証明するために証明書を提供する必要がある。何百ものマイクロサービスでは、クラスタにmTLSを実装するのは面倒な作業だ。マイクロサービスを実行している各Podは、クライアント機関から承認された証明書を採用する必要がある。サービスメッシュは、Kubernetesクラスタに機能としてmTLSを追加するのに役立つ。

# 試験の要点

Kubernetesのコア機能と外部ツールを使用して、セキュリティ設定を管理する練習をする。
