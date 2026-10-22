
次は、マルチステージ・アプローチを見てみよう。マルチステージ`Dockerfile`では、少なくとも2つのステージを定義する。[例6-](#ch06.html#dockerfile-multi-stage)2では、``build`` をエイリアス化したステージを指定し、テストを実行してバイナリをビルドしている。2番目のステージは、ステージ``build`` でプロデューサを生成したバイナリを専用のディレクトリにコピーする。ただし、実行には``alpine`` ベース・イメージを使用する。``docker build`` コマンドを実行すると、ステージ``build`` は最終的なコンテナ・イメージに含まれなくなる。

### 例6-2. 多段階Dockerfileの一部としてGoプログラムをビルドし、テストする。

```
FROM golang:1.19.4-alpine AS build 
RUN apk add --no-cache git
WORKDIR /tmp/go-sample-app
COPY go.mod .
COPY go.sum .
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 go test -v 
RUN go build -o ./out/go-sample-app . 

FROM alpine:3.17.0 
RUN apk add ca-certificates
COPY --from=build /tmp/go-sample-app/out/go-sample-app /app/go-sample-app 
CMD ["/app/go-sample-app"]
```

[![1](assets/1.png)](#ch06.html#co_supply_chain_security_CO2-1)

: 

``build`` という名前のステージで、プログラムのビルドとテストにGoのベースイメージを使用する。

[![2](assets/2.png)](#ch06.html#co_supply_chain_security_CO2-2)

: 

アプリケーション・コードに対してテストを実行する。

[![3](assets/3.png)](#ch06.html#co_supply_chain_security_CO2-3)

: 

Goアプリケーションのバイナリをビルドする。

[![4](assets/4.png)](#ch06.html#co_supply_chain_security_CO2-4)

: 

コンテナ内でアプリケーションを実行するために、より小さなベースイメージを使用する。

[![5](assets/5.png)](#ch06.html#co_supply_chain_security_CO2-5)

: 

``build`` ステージでプロデューサが生成したアプリケーションバイナリをコピーし、コンテナ起動時に実行するコマンドとして使用する。

その結果、``alpine`` ベース・イメージを使用した場合のコンテナ・イメージ・サイズは、わずか12MBと大幅に小さくなった。この結果は、``docker build`` コマンドを再度実行し、コンテナ・イメージのサイズをリストアップすることで確認できる：

```
$ docker build . -t go-sample-app:0.0.1
...
$ docker images
REPOSITORY      TAG     IMAGE ID       CREATED          SIZE
go-sample-app   0.0.1   88175f3ab0d3   44 seconds ago   12MB
```

サイズを小さくしただけでなく、依存関係を少なくすることで攻撃対象も減らした。``alpine`` ベース・イメージの代わりにディストロ・レス・ベース・イメージを組み込むことで、コンテナ・イメージのサイズをさらに小さくすることができる。

## レイヤー数を減らす

すべての`Dockerfile`は、順番に並んだ命令リストで構成されている。一部の命令だけが、作成されるコンテナイメージに読み取り専用レイヤーを作成する。これらの命令は、``FROM`` 、``COPY`` 、``RUN`` 、``CMD`` である。他のすべての命令は、一時的な中間イメージを作成するため、レイヤーを作成しない。コンテナ・イメージにレイヤーを追加すればするほど、ビルドの実行時間は遅くなり、またはコンテナ・イメージのサイズは大きくなる。したがって、コンテナイメージのフットプリントを最小化するために、`Dockerfile` で使用する命令には慎重を期す必要がある。

複数のコマンドを連続して実行することはよくある。[例6-](#ch06.html#dockerfile-multiple-run-instructions)3に示すように、``RUN`` 命令のリストを個々の行に使って、それらのコマンドを定義することができる。

### 例6-3. 複数の``RUN`` 命令を指定するDockerfile

```
FROM ubuntu:22.10
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y curl
```

各``RUN`` 命令はレイヤーを作成し、コンテナ・イメージのサイズを増やす可能性がある。``&&`` で文字列化し、単一のレイヤーのみを確実に生成する方が効率的である。[例6-4は](#ch06.html#dockerfile-combined-run-instructions)、この最適化テクニックの例を示している。

### 例6-4. 複数の``RUN`` 命令を指定するDockerfile

```
FROM ubuntu:22.10
RUN apt-get update -y && apt-get upgrade -y && apt-get install -y curl
```

## コンテナイメージ最適化ツールを使う

先に述べた最適化のプラクティスを忘れてしまいがちだ。オープンソースコミュニティは、プロデューサのコンテナイメージを検査するのに役立つツールをいくつか開発した。それらの関数は、不要なレイヤー、ファイル、フォルダーを減らすための有用なヒントを提供してくれる：

ドッカースリム

: 

`DockerSlim`は、アプリケーションとその依存関係を分析することで、コンテナイメージを最適化し、セキュリティを確保する。詳細はツールの[GitHubリポジトリで](https://oreil.ly/ZbeZl)発見できる。

ダイブ

: 

`Dive`は、コンテナイメージにベイクされたレイヤーを探索するためのツールである。不要なレイヤーを簡単に特定し、さらに最適化することができる。`Dive`のコードとドキュメントは[GitHubリポジトリで](https://oreil.ly/UBqqj)公開されている。

これはコンテナイメージ最適化ツールのほんの短いリストに過ぎない。[ワークロードの静的解析](#ch06.html#static-analysis-workload)」では、`Dockerfile`とKubernetesマニフェストの静的解析に焦点を当てた他のツールを見ていく。

# サプライチェーンの確保

サプライチェーンは、コンテナ・イメージのプロデューサと、それを実行環境（この場合はKubernetes）で演算子するプロセスを自動化する。コンテナイメージの最適化をサポートするためにサプライチェーンに統合できるツールについては、すでにいくつか触れた。このセクションでは、セキュリティの側面を対象としたプラクティスについて説明する。詳しくは、Liz Rice著『*Container Security*』（O'Reilly）を参照されたい。

## コンテナイメージに署名する

コンテナイメージをコンテナレジストリにプッシュする前に、署名することができる。署名には``docker trust sign`` コマンドを使用する。 コマンドは、コンテナイメージに署名、いわゆるイメージダイジェストを追加する。イメージ・ダイジェストはコンテナ・イメージの内容から導き出され、一般的にSHA256の形式で表される。コンテナイメージを消費する際、Kubernetesはイメージダイジェストとイメージの内容を比較し、改ざんされていないことを確認できる。

## シナリオ攻撃者がコンテナイメージに悪意のあるコードを注入する

イメージダイジェストを検証するKubernetesコンポーネントは`kubelet`である。[イメージのプルポリシーを](https://oreil.ly/0wSjy) ``Always`` に設定した場合、``kubelet``は、コンテナイメージを以前にダウンロードして検証していても、コンテナレジストリからイメージダイジェストをクエリする。

攻撃者は、[図6-](#ch06.html#no-image-digest-validation-attacker)2に示すように、既存のコンテナ・イメージの内容を変更し、悪意あるコードを注入して、同じタグを持つコンテナ・レジストリにアップロードしようとすることができる。コンテナ内で実行されている悪意のある関係者は、攻撃者がアクセス可能なサードパーティ・サーバに機密情報を送信する可能性がある。

![](assets/ckss_0602.png)

# 画像のチェックサム検証は自動ではない

イメージダイジェストの検証は、Kubernetesのオプトイン機能である。Podを定義する際には、すべてのコンテナイメージに対してイメージダイジェストを明示するようにしよう。

## コンテナイメージを検証する

Kubernetesでは、コンテナの仕様にSHA256イメージダイジェストを追加することができる。例えば、これは``spec.containers[].image`` という属性で実現できる。イメージダイジェストは一般にコンテナレジストリで利用できる。例えば、[図6-3は](#ch06.html#dockerhub-alpine-image-digest) [Docker Hub上の](https://oreil.ly/ZAV_H)コンテナイメージ[``alpine:3.17.0``](https://oreil.ly/ZAV_H) のイメージダイジェストを示している。この例では、イメージダイジェストは``sha256:c0d488a800e4127c334ad20d61d7bc21b4097540327217dfab52262adc02380c`` である。

![](assets/ckss_0603.png)

画像ダイジェストの動きを見てみよう。[例6-5では](#ch06.html#container-valid-image-digest)、タグを使う代わりに、イメージ・ダイジェストを付加してコンテナ画像を指定している。

### 例6-5. 有効なコンテナ・イメージ・ダイジェストを使用するPod

```
apiVersion: v1
kind: Pod
metadata:
  name: alpine-valid
spec:
  containers:
  - name: alpine
    image: alpine@sha256:c0d488a800e4127c334ad20d61d7bc21b40 \
           97540327217dfab52262adc02380c
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10; done"]
```

Podの作成は期待通りに動作する。イメージ・ダイジェストが検証され、コンテナは「Running」ステータスに移行する：

```
$ kubectl apply -f pod-valid-image-digest.yaml
pod/alpine-valid created
$ kubectl get pod alpine-valid
NAME           READY   STATUS    RESTARTS   AGE
alpine-valid   1/1     Running   0          6s
```

[例6-](#ch06.html#container-invalid-image-digest)6は同じPod定義を示すが、イメージダイジェストはコンテナイメージの内容と一致しないように変更されている。

### 例 6-6. 無効なコンテナ・イメージ・ダイジェストを使用するPod

```
apiVersion: v1
kind: Pod
metadata:
  name: alpine-invalid
spec:
  containers:
  - name: alpine
    image: alpine@sha256:d006a643bccb6e9adbabaae668533c7f2e5 \
           111572fffb5c61cb7fcba7ef4150b
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10; done"]
```

KubernetesはまだPodオブジェクトを作成できるが、コンテナイメージのハッシュを適切に検証できないことがわかるだろう。その結果、"ErrImagePull "というステータスになる。イベントログからわかるように、コンテナイメージはコンテナレジストリからプルすることすらできなかった：

```
$ kubectl get pods
NAME             READY   STATUS         RESTARTS   AGE
alpine-invalid   0/1     ErrImagePull   0          29s
$ kubectl describe pod alpine-invalid
...
Events:
  Type     Reason     Age   From               Message
  ----     ------     ----  ----               -------
  Normal   Scheduled  13s   default-scheduler  Successfully assigned default \
  /alpine-invalid to minikube
  Normal   Pulling    13s   kubelet            Pulling image "alpine@sha256: \
  d006a643bccb6e9adbabaae668533c7f2e5111572fffb5c61cb7fcba7ef4150b"
  Warning  Failed     11s   kubelet            Failed to pull image \
  "alpine@sha256:d006a643bccb6e9adbabaae668533c7f2e5111572fffb5c61cb7fcba7ef4 \
  150b": rpc error: code = Unknown desc = Error response from daemon: manifest \
  for alpine@sha256:d006a643bccb6e9adbabaae668533c7f2e5111572fffb5c61cb7fcba7e \
  f4150b not found: manifest unknown: manifest unknown
  Warning  Failed     11s   kubelet            Error: ErrImagePull
  Normal   BackOff    11s   kubelet            Back-off pulling image \
  "alpine@sha256:d006a643bccb6e9adbabaae668533c7f2e5111572fffb5c61cb7fcba7ef415 \
  0b"
  Warning  Failed     11s   kubelet            Error: ImagePullBackOff
```

## 公開画像レジストリを利用する

コンテナ・ランタイム・エンジンは、Podが作成されるたびに、宣言されたコンテナ・イメージがまだローカルで利用可能でなければ、コンテナ・レジストリからダウンロードする。このランタイムの振る舞いは、[イメージのプルポリシーを使って](https://oreil.ly/ZBMnH)さらに微調整できる。

イメージ名のプレフィックスは、レジストリのドメイン名を宣言する。例えば、``gcr.io/google-containers/debian-base:v1.0.1`` は、[Google Cloud コンテナレジストリの](https://oreil.ly/QFxfY)コンテナイメージ``google-containers/debian-base:v1.0.1`` を指し、``gcr.io`` と表記する。コンテナイメージの宣言でドメイン名を省略すると、コンテナランタイムは[Docker Hubコンテナレジストリの](https://hub.docker.com) ``docker.io`` 。

## シナリオ攻撃者が悪意のあるコンテナイメージをアップロードする

公開コンテナ・レジストリからコンテナ・イメージを解決するのは便利だが、リスクがないわけではない。これらのコンテナ・レジストリにログインしていれば、誰でも新しいイメージをアップロードできる。コンテナ・イメージの消費には通常、アカウントすら必要ない。

[図 6-4](#ch06.html#public-registries-attacker) に示すように、攻撃者は盗んだ資格情報を使って、悪質なコードを含むコンテナ・イ メージを公開レジストリにアップロードすることができる。そのレジストリからコンテナイメージを参照するコンテナは、自動的に悪意のあるコードを実行することになる。

![](assets/ckss_0604.png)
