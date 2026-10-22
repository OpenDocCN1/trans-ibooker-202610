![](assets/cover.png)

![](assets/cover.png)

TITLE 

# 認定Kubernetesセキュリティスペシャリスト（CKS）スタディガイド

AUTHOR 

byBenjamin  Muschko

 COPYRIGHT  

著作権 © 2023 Automated Ascent, LLC.無断複写・転載を禁ずる。

アメリカで印刷された。

PUBLISHER 

オライリー・メディア社（1005 Gravenstein Highway North, Sebastopol, CA 95472）発行。

オライリーの書籍は、教育用、ビジネス用、販売促進用として購入することができる。ほとんどのタイトルでオンライン版も利用できる[*（https://oreilly.com）。*](https://oreilly.com)詳しくは、法人・機関投資家向け営業部：800-998-9938または*corporate@oreilly.com。*

STAFF LIST 

*   アクイジション・エディター ジョン・デヴィンズ
*   デベロップメント・エディター ミシェル・クローニン
*   プロダクション・エディター： ベス・ケリー
*   コピーエディター リズ・ウィーラー
*   校正者 アムネット・システムズLLC
*   インデクサ Potomac Indexing, LLC
*   インテリア・デザイナー デヴィッド・フタト
*   カバーデザイナー カレン・モンゴメリー
*   イラストレーター ケイト・ダレア

 PRINTINGS  

*   2023年6月初版

 REVISIONS  

# 初版の改訂履歴

*   2023-06-08:ファースト・リリース

 ERRATA  

リリースの詳細は[https://oreilly.com/catalog/errata.csp?isbn=9781098132972 を参照の](https://oreilly.com/catalog/errata.csp?isbn=9781098132972)こと。

LEGAL 

O'ReillyのロゴはO'Reilly Media, Inc.の登録商標である。*Certified Kubernetes Security Specialist (CKS) Study Guide*、表紙画像、および関連するトレードドレスは、O'Reilly Media, Inc.の商標である。

本作品で表明された見解は著者のものであり、出版社の見解を代表するものではない。出版社および著者は、本作品に含まれる情報および指示が正確であるよう誠意をもって努力しているが、本作品の使用または本作品に依拠した結果生じた損害に対する責任を含むがこれに限定されない、エラーまたは脱落に対する一切の責任を否認する。本著作物に含まれる情報および指示の使用は、自己責任で行うものとする。本作品に含まれる、または記述されているコードサンプルやその他の技術が、オープンソースライセンスや他者の知的財産権の対象である場合、その使用がそのようなライセンスおよび/または権利に準拠していることを確認するのは、あなたの責任である。PROD: Uncomment the following sentence if appropriate and add it to the above para:This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

978-1-098-13297-2

\[LSI］

# 序文

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

Kubernetes認定プログラムは2018年から、つまりこの記事を書いている時点で5年が経過している。 この間、Kubernetesの世界を含め、あらゆる場所でセキュリティの重要性が高まっている。最近になって、[認定Kubernetesセキュリティ・スペシャリスト（CKS](https://www.cncf.io/certification/cks)）という役割が認定トラックに追加され、その必要性に対応するようになった。セキュリティにはさまざまな側面があり、それらの懸念に対処する方法は非常に多様である。そこでKubernetesのエコシステムが活躍する。Kubernetesに組み込まれたセキュリティ機能とは別に、セキュリティリスクの特定と修正に役立つ多くのツールが進化してきた。Kubernetes管理者として、クラスタとアプリケーションを強固にするために、幅広い概念とツールに精通する必要がある。

CKS認定プログラムは、セキュリティベースのトピックに関する能力を確認するために作成されたもので、登録する前に[Certified Kubernetes Administrator（CKA）](https://www.cncf.io/certification/cka)試験に合格する必要がある。もしあなたがKubernetes認定プログラムを全く知らないのであれば、まずはCKAか[Certified Kubernetes Application Developer（CKAD）](https://www.cncf.io/certification/ckad)プログラムを検討することをお勧めする。

この学習ガイドでは、認定試験に合格するための準備を万全にするために、CKS試験でカバーされるトピックを探っていく。Kubernetesと外部ツールのコア概念を、クラスタコンポーネント、クラスタ構成、およびPodで実行されるアプリケーションのセキュリティ確保にいつ、どのように適用すべきかを判断することについて見ていく。また、試験の準備をより円滑に進めるためのヒントを提供し、試験のあらゆる側面に備えるための個人的な経験を共有する。

CKSは、他の資格によくある多肢選択形式とは異なる。完全に成果主義で、膨大な時間的プレッシャーの中で、目の前のタスクに関する深い知識を実証することが求められる。一発合格の準備はできているだろうか？

# この本は誰のためにあるのか？

本書はすでにCKA試験に合格し、セキュリティ分野の知識を深めたい人のためのものである。CKSに申し込む前にCKA試験に合格する必要があることから、試験の出題形式や環境についてはすでに熟知しているはずである。[第1章では](#ch01.html#exam-details-and-resources)、試験カリキュラムの一般化について簡単にまとめただけであるが、CKS試験に特化した情報を強調している。まだCKA試験を受験したことがない場合は、「[*Certified Kubernetes Administrator (CKA) Study Guide」*](https://oreil.ly/cka-study-guide)（オライリー）を読み取り、一歩を踏み出すことをお勧めする。この本は、CKSを始めるために必要な基礎を提供してくれるだろう。

# 何を学ぶか

本書の内容は、CKS試験に関連する最も重要な点を凝縮している。AKSやGKEのようなクラウドプロバイダー固有のKubernetes実装を考慮する必要はない。Kubernetesで利用可能な設定オプションの多さを考えると、公式ドキュメントと重複することなく、すべてのユースケースとシナリオをカバーすることはほとんど不可能だ。受験者は、[Kubernetesのドキュメントを](https://kubernetes.io/docs/home)、より広範な知識を得るための大要として参照することが推奨される。TrivyやFalcoのようなCKS試験に関連する外部ツールは、高いレベルでしかカバーされていない。より多くの関数、機能、設定オプションを調べるには、それらのドキュメントを参照すること。

# 本書の構成

一般化されたKubernetesを学ぶには、より自然で学問的な構造があるかもしれないが、カリキュラムのアウトラインは、特定のトピックに焦点を当てることで、受験者が試験に備えるのに役立つだろう。その結果、既存の知識レベルに応じて、本書の他の章を相互参照することになるだろう。

本書では、CKS試験に関連する概念のみを扱っていることに留意してほしい。Kubernetesの基礎的な概念やプリミティブについては触れていない。より深く学びたい場合は、Kubernetesのドキュメントや他の書籍を参照すること。

Kubernetesの実践経験が試験合格の鍵となる。各章には「サンプル演習」という名前のセクションがあり、練習問題が掲載されている。これらの問題の解答は[付録で](#app01.html#appendix-a)発見できる。

# 本書で使用されている慣例

本書では、以下の表記規則を用いている：

*イタリック体*
: 

新しい用語、URL、メールアドレスを示す。

`Constant width`
: 

変数名や関数名、データベース、データ型、環境変数、文、キーワードなどのプログラム要素を指すために、段落内だけでなく、ファイル名、ファイル拡張子、プログラムリストに使用される。

**`Constant width bold`**
: 

ユーザが文字通り入力すべきコマンドやその他のテキストを表示する。

###### チップ

この要素は、ヒントや提案を意味する。

###### 注

この要素は一般的な注釈を意味する。

###### 警告

この要素は警告や注意を示す。

# コード例を使う

この本の一部のコード・スニペットでは、バックスラッシュ文字（`\` ）を使って1行を複数行に分割し、ページに収まるようにしている。本の内容からターミナルやエディターに直接コピーペーストする場合は、手作業でコードを修正する必要がある。より良い選択は、コードブックの[GitHubリポジトリを](https://github.com/bmuschko/cks-study-guide)参照することで、すでに適切な形式になっている。

GitHubリポジトリはApache License 2.0で配布されている。コードは、商用およびオープンソースプロジェクトで自由に使用できる。ソースコードで問題が発生した場合、または質問がある場合は、[GitHubのイシュー・トラッカーに](https://oreil.ly/YTRzJ)イシューをオープンしてほしい。私は喜んで会話をし、発生するかもしれない問題を修正する。

本書は、あなたが仕事をこなすのを助けるためにある。一般化として、本書とともにサンプルコードが提供されている場合、あなたはそれをあなたのプログラミングやドキュメントに使用することができる。コードのかなりの部分を複製するのでなければ、私たちに許可を求める必要はない。例えば、本書からいくつかのコードの塊を使用したプログラミングを書く場合、許可は必要ない。オライリー本のサンプルを販売したり配布したりする場合は、許可が必要である。本書を引用し、サンプルコードを引用して質問に答えることは、許可を必要としない。本書からかなりの量のサンプルコードをあなたの製品のドキュメントに組み込むことは、許可を必要とする。帰属表示はありがたいが、一般化する必要はない。帰属表示には通常、タイトル、著者、出版社、ISBNが含まれる。例えば例えば、「*Certified Kubernetes Security Specialist (CKS) Study Guide*by Benjamin Muschko (O'Reilly).Copyright 2023 Automated Ascent, LLC, 978-1-098-13297-2"。

コード例の使用がフェアユースや上記の許可から外れていると思われる場合は、遠慮なく[*permissions@oreilly.com*](mailto:permissions@oreilly.com) までご連絡いただきたい。

# オライリー・オンライン・ラーニング

###### 注

[*オライリー・メディアは*](http://oreilly.com)40年以上にわたり、テクノロジーとビジネスに関するトレーニング、知識、見識を提供し、企業の成功を支援してきた。

オライリー独自のエキスパートとイノベーターのネットワークは、書籍、記事、オンライン学習プラットフォームを通じて、その知識と専門知識を共有している。オライリーのオンライン学習プラットフォームでは、ライブトレーニングコース、詳細な学習パス、インタラクティブなコーディング環境、オライリーおよび200以上の出版社からのテキストやビデオの膨大なコレクションにオンデマンドでアクセスできる。詳細については、[*http://oreilly.com。*](http://oreilly.com)

# 問い合わせ先

本書に関するコメントや質問は、出版社までお寄せいただきたい：

*   オライリー・メディア社
*   グラベンスタイン・ハイウェイ北1005
*   セバストポール, CA 95472
*   800-889-8969（米国またはカナダ国内）
*   707-829-7019（国際線または市内線）
*   707-829-0104（ファックス）
*   [*support@oreilly.com*](mailto:support@oreilly.com)
*   [*https://www.oreilly.com/about/contact.html*](https://www.oreilly.com/about/contact.html)

この本のWebページがあり、正誤表、例題、追加情報をリストアップしている。このページは[*https://oreil.ly/cks-study-guide。*](https://oreil.ly/cks-study-guide)

書籍やコースに関するニュースや情報は、[*http://oreilly.com。*](http://oreilly.com)

LinkedInで発見: https:[*//linkedin.com/company/oreilly-media*](https://linkedin.com/company/oreilly-media)

Twitterでフォローする: http:[*//twitter.com/oreillymedia*](http://twitter.com/oreillymedia)

YouTubeで見る: http:[*//youtube.com/oreillymedia*](http://youtube.com/oreillymedia)

ツイッターで著者をフォロー：[*https://twitter.com/bmuschko*](https://twitter.com/bmuschko)

GitHubで作者をフォローする: https:[*//github.com/bmuschko*](https://github.com/bmuschko)

著者のブログをフォローする: https:[*//bmuschko.com*](https://bmuschko.com)

# 謝辞

すべての書籍プロジェクトは長い旅路であり、編集スタッフと技術査読者の助けなしには不可能であった。詳細な技術指導とフィードバックをしてくれたRobin Smorenburg、Werner Dijkerman、Michael Kehoe、Liz Riceには特に感謝する。また、オライリー・メディアの編集者であるジョン・デヴィンズとミケーレ・クローニンの継続的な支援と励ましにも感謝したい。

# 第1章. 試験の詳細とリソース

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

この入門章では、[認定Kubernetesセキュリティスペシャリスト（CKS）試験の](https://www.cncf.io/certification/cks)準備をする際に受験者が最も差し迫った質問を取り上げる。認定の対象者、カリキュラム、試験環境のほか、ヒントやコツ、追加の学習リソースについても説明する。すでに認定プログラムに精通している場合は、技術概念をカバーするどの章にも直接ジャンプできる。

# Kubernetes認定資格ラーニングパス

CNCFは4種類のKubernetes認定資格を提供している。[図1-1は](#ch01.html#kubernetes-certification-learning-path)、それぞれを対象者別に分類したものだ。CKSが最も高度な認定資格であることが発見できるだろう。最初に他の認定資格に合格することが前提条件となっているのはこの資格だけで、他はすべて独立したプログラミングとなっている。

![](assets/ckss_0101.png)

CKSが自分に合っているかどうかを確認するために、各資格の詳細をごく簡単に見てみよう。

## KubernetesおよびCloud Native Associate (KCNA)

KCNAは、クラウドネイティブなアプリケーション開発、ランタイム環境、ツールに関心のある人向けのエントリレベルの認定プログラムだ。試験はKubernetesをカバーしているが、実際に実践的に問題を解決することは期待されて*いない*。この試験は、エコシステムに広く触れ、このトピックに関心のある受験者に適している。

## KubernetesおよびCloud Native Security Associate (KCSA)

この認定資格は、セキュリティ概念に関する基本的な知識と、Kubernetesクラスタにおけるそのアプリケーションに焦点を当てている。ハンズオンで問題を解決する必要がないため、プログラムの広さと深さはKCNAに匹敵する。

## 認定Kubernetesアプリケーション開発者(CKAD)

CKAD試験は、マイクロサービスベースのアプリケーションをKubernetesにビルド、設定、デプロイする能力を確認することに重点を置いている。実際にアプリケーションを実装することは求められていないが、アプリケーションアーキテクチャ、ランタイム、プログラミング言語などのトピックに精通した開発者に適した試験となっている。

## 認定Kubernetesアドミニストレーター(CKA)

CKA試験の対象者は、DevOps実務者、システム管理者、サイトの信頼性エンジニアである。この試験では、トラブルシューティングのシナリオに重点を置きながら、クラスタ、ネットワーク、ストレージ、初級レベルのセキュリティ管理などのタスクを含むKubernetes管理者の役割を果たす能力がテストされる。

## 認定Kubernetesセキュリティスペシャリスト（CKS）

CKS試験は、CKA試験で確認されたトピックを発展させたものである。CKA試験に合格することは、CKS試験に申し込む前の前提条件である。この資格の取得には、Kubernetesのセキュリティ面に関するより深い知識が求められる。カリキュラムは、コンテナ化アプリケーション構築のベストプラクティスの適用や、安全なKubernetes実行環境の確保といったトピックをカバーしている。

# 試験オブジェクト

ソフトウェアやITインフラの脆弱性が悪用されれば、組織にとって大きな脅威となる。CNCF（Cloud Native Computing Foundation）は、Kubernetesクラスタとその中で運用されるクラウドネイティブソフトウェアを保護するKubernetes管理者の熟練度を確認するために、認定Kubernetesセキュリティスペシャリスト（CKS）資格を開発した。CKS試験の一環として、Kubernetesのコアセキュリティ機能に加え、アプリケーションとインフラを保護するためのサードパーティツールや確立されたプラクティスを理解することが求められる。

# 試験中に使用されたKubernetesのバージョン

執筆時点では、試験はKubernetes 1.26に基づいている。本書の内容はすべて、その特定バージョンの機能、API、コマンドラインサポートに従う。将来のバージョンで後方互換性が破られる可能性は確かにある。資格取得の準備をしながら、[Kubernetesのリリースノートを](https://oreil.ly/u3REo)確認し、試験で使用されるKubernetesのバージョンで練習して、不愉快な驚きを避けること。

本書では、具体的なユースケースを示しながら、それぞれのセキュリティ脅威について説明する。まず、攻撃者がクラスタにアクセスしたり、悪意のあるコードを注入したり、脆弱性を利用してシステムに侵入したりするシナリオについて説明する。そして、そのような状況を防ぐための概念、プラクティス、ツールについて触れる。このアプローチにより、セキュリティリスクの重大性とセキュリティ対策の実装の必要性を評価できるようになる。

# カリキュラム

以下の概要は、CKS試験のハイレベル・セクション（ドメインとも呼び出し）とその得点重みのリストである：

*   10%:クラスターのセットアップ
    
*   15%:クラスター・ハードニング
    
*   15%:システム・ハードニング
    
*   20%:マイクロサービスの脆弱性を最小限に抑える
    
*   20%:サプライチェーンのセキュリティ
    
*   20%:モニタリング、ロギング、ランタイム・セキュリティ
    

# 本の仕組み

一般化されたKubernetesを学ぶには、より自然で学問的な組織構成があるかもしれないが、カリキュラムのアウトラインは、特定のトピックに焦点を当てることで、受験者の試験準備に役立つだろう。その結果、既存の知識レベルに応じて、本書の他の章を相互参照することになる。

次のセクションでは、各ドメインについて詳しく説明しよう。

## クラスター設定

このセクションでは、CKA試験ですでにカバーされているKubernetesの概念をカバーする。しかし、CKA試験はあなたがすでに基本を理解していることを前提としており、より深く掘り下げることができることを期待している。ここでは、ネットワークポリシーと、同じネームスペース内および複数のネームスペースにまたがるPod間のネットワーク通信の禁止と許可に対するそれらの効果についてテストされる。攻撃対象領域を最小化するために通信を制限することに主眼が置かれる。さらに、「クラスタのセットアップ」ドメインでは、トランスポート・レイヤー・セキュリティ（TLS）終端を持つIngressオブジェクトのセットアップに関する知識を確認する。

クラスタのセットアップを検査することで、セキュリティの脆弱性を特定し、修正することに大きな重点を置いている。kube-benchのような外部ツールは、このプロセスの自動化に役立つ。クラスタに対してツールを実行した結果、脆弱性の実用的なリストが得られる。推奨事項に従ってクラスタのコンフィギュレーション設定を変更すれば、セキュリティリスクを大幅に低減できる。

最後に、クラスタノードのエンドポイント、ポート、グラフィカル・ユーザ・インタフェース（GUI）をロックダウンすることで、攻撃者がクラスタを制御しにくくすることができる。可能な限りアクセスを制限できるように、デフォルトのクラスタ設定を把握しておく必要がある。`kubectl` 、`kubeadm` 、kubeletのようなKubernetesのバイナリや実行ファイルは、第三者によって改ざんされていないことを確認するためにチェックサムと照合する必要がある。バイナリのチェックサムファイルを取得する方法と、それを使って実行ファイルの妥当性を検証する方法を理解する必要がある。

## クラスター・ハードニング

ほとんどの組織は、開発者と管理者が同様にKubernetesのインストール、設定、あらゆるオブジェクトの管理を行えるクラスタから始める。これはKubernetesに慣れ親しんでいるチームにとっては便利なアプローチだが、攻撃者にとって門戸を開く可能性があるため、安全で健全な状況とは言えない。ひとたびクラスタにアクセスできれば、どんな悪意のある演算子でも実行できてしまうからだ。

役割ベースのアクセス制御（RBAC）は、ユーザやプロセスに権限をマッピングする。この試験では、APIリソースに関する深い知識が要求される。また、「クラスタの堅牢化」というドメインでは、クラスタのバージョンを最新に保ち、最新のバグ修正を確実に反映させるというトピックに焦点が当てられている。Kubernetesはエンドポイントを介してAPIサーバを公開する。外界への露出を最小限に抑えるための戦略を知っておく必要がある。

## システム・ハードニング

この領域では、ホストシステムと外部ネットワークへのアクセスを最小限に抑え、攻撃対象領域を減らす方法を理解することが重要だ。AppArmorやseccompのようなOSレベルのツールが活躍する場である。要件を満たすためには、これらの使用方法を実証する必要がある。このドメインでは、特にAmazonのクラウド環境で動作するクラスタに対するAWS IAMロールの使用についても触れている。

## マイクロサービスの脆弱性を最小限に抑える

セキュリティ・コンテキストは、コンテナの特権とアクセス制御を定義する。プラットフォームチームとセキュリティチームは、組織レベルで必要なセキュリティ対策を管理し、実施することができる。試験では、Pod セキュリティ・ポリシーとそのための OPA ゲートキーパーを理解することが求められる。さらに、機密性の高いランタイム情報を注入するために、さまざまなタイプのシークレットを定義し、Podからシークレットを消費することを実演することも求められる。

検証されていないソースや潜在的に安全でないオリジンからのコンテナイメージを使って実験したい場合もあるだろう。gVisorやKata ContainersのようなコンテナランタイムサンドボックスをKubernetesに設定することで、非常に制限されたパーミッションでコンテナイメージを実行することができる。このようなコンテナランタイムサンドボックスの設定と使用は、この領域の一部である。さらに、mTLSによるPod間暗号化の利点とその設定方法についても知っておく必要がある。

## サプライチェーンのセキュリティ

コンテナのセキュリティはベースイメージから始まる。コンテナイメージを構築する際のベストプラクティスを知っておくことで、最初からセキュリティ脆弱性を導入するリスクを最小限に抑えることができる。最適には、組織内部のコンテナレジストリから信頼できるコンテナイメージを引き出すことのみを許可し、そのレジストリは、イメージを使用する前に、すでに脆弱性をスキャンしている。そのようなレジストリのみを許可することが最も重要であり、この領域で重要なトピックの1つになるだろう。Trivyのようなツールは、イメージの脆弱性をスキャンする作業を支援することができ、試験に合格するための要件としてリストアップされている。

## 監視、ログ、ランタイム・セキュリティ

このドメインの焦点の1点は、振る舞い分析、つまり異常で悪意のあるイベントを観察するプロセスである。Falco は、このセクションで熟知すべき主要なツールである。攻撃者に新たなバックドアを開けないようにするため、コンテナは起動後に可変性であってはならない。ベストプラクティスを認識し、コンテナのコンフィギュレーションに適用する能力を示す必要がある。

監査ロギングは、クラスタイベントのリアルタイムビューやデバッグの目的に役立つ。Kubernetesクラスタの監査ロギングの設定は、試験の一部である。

# 関与するKubernetesプリミティブ

試験オブジェクトの一部は、関連するKubernetesのコアプリミティブを理解することでカバーできる。試験では、1つの問題に複数の概念が組み合わされることが予想される。該当するKubernetesリソースとその関係性の大まかなガイドとして、[図1-2を](#ch01.html#kubernetes-primitives)参照すること。

![](assets/ckss_0102.png)

Kubernetesのコアプリミティブに加えて、オープンソースプロジェクトが提供する特定のカスタムリソース定義（CRD）も把握しておく必要がある。例えば、Open Policy Agent (OPA) Gatekeeperは、プリミティブのConstraintTemplateを提供している。

# 関係する外部ツール

試験のかなりの部分で、外部のセキュリティ関連ツールの専門知識を証明することが求められる。ツールの中には、カリキュラムで明示されているものもあるが、同じ機能カテゴリに分類されるツールもある。少なくとも、以下のツールには精通している必要がある：

*   [キューブ・ベンチ](https://oreil.ly/y3mbO)
    
*   [AppArmor](https://apparmor.net)
    
*   [セコンプ](https://oreil.ly/vxdZV)
    
*   [バイザー](https://gvisor.dev)
    
*   [加太コンテナ](https://oreil.ly/vmCZH)
    
*   [トリビー](https://oreil.ly/RsWka)
    
*   [ファルコ](https://oreil.ly/LGT8B)
    

# ドキュメンテーション

試験中、リファレンスとして定義されたWebページのリストを開くことが許可されている。それらのページを自由に閲覧し、試験端末にコードをコピーペーストすることができる。CKSの[よくある質問（FAQ）には](https://oreil.ly/ZcKNf)、許可されたURLのリストが明記されている。

Kubernetesの公式ドキュメントには、リファレンスマニュアル、GitHubサイト、ブログなどがある：

*   参考マニュアル*[*：https://kubernetes.io/docs*](https://kubernetes.io/docs)*
    
*   GitHub: https:*[*//github.com/kubernetes*](https://github.com/kubernetes)*
    
*   ブログ*[*：https://kubernetes.io/blog*](https://kubernetes.io/blog)*
    

外部ツールについては、以下のURLを開いて閲覧することが許可されている：

*   トリビー*[*：https://github.com/aquasecurity/trivy*](https://github.com/aquasecurity/trivy)*
    
*   ファルコ*[*：https://falco.org/docs*](https://falco.org/docs)*
    
*   AppArmor: https:*[*//gitlab.com/apparmor/apparmor/-/wikis/Documentation*](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation)*
    

# 候補者のスキル

CKS認定資格は、Kubernetesの管理者レベルの理解がすでにあることを前提としている。CNCFは、前提条件としてCKA資格の取得を求めている。これらの資格情報がなければ、CKS試験に申し込むことはできない。まだCKA試験に合格していない場合や、トピックをブラッシュアップしたい場合は、拙著『[*Certified Kubernetes Administrator (CKA) Study Guide*](https://oreil.ly/cka-study-guide)』を一読することをお勧めする。

この本の残りの部分は、あなたがすでにCKAに必要な知識を持っていると仮定する。したがって、重複するトピックについて基本的なことを繰り返すことはもうしない。便宜上、必要に応じてCKAの本の関連情報を紹介する。CKAの本にある試験環境と時間管理のセクションをもう一度見直してほしい。それらはCKS試験にも同様に当てはまる。

# 練習と模擬試験

試験に合格するためには、実践的な練習が非常に重要だ。そのためには、機能するKubernetesクラスタ環境が必要だ。以下の選択肢が目立つ：

*   [Vagrantと](https://oreil.ly/2jLJS) [VirtualBoxを使って](https://oreil.ly/3BhDj)、1つまたは複数の仮想マシンを実行するのが便利だと発見した。これらのツールは、ブートストラップやオンデマンドでの破棄が容易な、分離されたKubernetes環境の作成に役立つ。本書の練習問題のいくつかは、このセットアップを出発点としている。
    
*   開発者マシンにシンプルなKubernetesクラスタをインストールするのは比較的簡単だ。Kubernetesのドキュメントには、オペレーティングシステムに応じてさまざまな[インストールオプションが](https://oreil.ly/JrBUh)用意されている。Minikubeは、Ingressやストレージクラスなど、より高度な機能を試す場合に便利で、必要な機能をアドオンとして提供しており、単一のコマンドでインストールできる。
    
*   [オライリーの学習プラットフォームに](https://oreil.ly/xOtTT)加入していれば、[Kubernetes環境を実行するラボに](https://oreil.ly/gYiVj)無制限にアクセスできる。さらに、[インタラクティブなラボ形式のCKS模擬試験で](https://oreil.ly/9Jp6j)自分の知識を試すことができる。
    

また、以下の市販の学習・練習リソースを試してみるのもいいだろう：

*   本書『[*Certified Kubernetes Administrator (CKA) Study Guide*](https://oreil.ly/cka-study-guide)』は、CKA認定資格のカリキュラムを網羅している。基礎を再確認するために、この書籍の資料を再確認する。
    
*   [Killer Shellは](https://killer.sh)、すべてのKubernetes認定資格のためのサンプル演習を備えたシミュレータだ。
    
*   [Study4examのCKS模擬試験は](https://oreil.ly/c3Y7_)、市販のウェブベースの試験環境を提供し、あなたの知識レベルを評価する。
    

# 概要

CKS試験は、Kubernetesにおけるセキュリティ関連の実践的な知識を検証する。RBAC、ネットワークポリシー、Ingressなど、セキュリティ要件を満たすことができるKubernetesのコアプリミティブと概念を理解することが求められる。試験には、役に立つサードパーティのセキュリティツールも含まれる。それらのツールの効果的な使い方を実証する必要がある。CKA試験に合格することは、CKSの取得に必須である。まだCKAに合格していない場合は、まずCKAに合格していることを確認すること。

以下の章は、試験カリキュラムに沿った内容となっているため、学習目標と内容をマッピングすることができる。各章の終わりには、知識を練習するためのサンプル問題が発見できる。各領域の議論は、学ぶべき最も重要な側面の短い要約で締めくくられている。

# 第2章. クラスターのセットアップ

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

試験の最初の領域は、Kubernetesクラスタのセットアップと設定に関する問題を扱う。この章では、Kubernetes管理者の標準的なレスポンスではなく、セキュリティに特化した側面のみを掘り下げていく。

この章では、高いレベルで以下の概念をカバーする：

*   ネットワークポリシーを使ってPod間通信を制限する
    
*   CIS ベンチマークツールを実行してクラスタコンポー ネントのセキュリティリスクを特定する
    
*   TLSをサポートするIngressオブジェクトをセットアップする
    
*   ノードポート、APIエンドポイント、GUIアクセスの保護
    
*   プラットフォームのバイナリをチェックサムと照合する
    

# ネットワークポリシーを使ってPod間通信を制限する

マイクロサービスアーキテクチャがKubernetesで機能するためには、Podがネットワークアドレス変換（NAT）なしで、同じノードまたは異なるノードで動作する別のPodに到達できる必要がある。Kubernetesは、すべてのPodに対して、そのノードのPod CIDR範囲から、作成時に一意のIPアドレスを代入する。このIPアドレスはエフェメラル（一時的）なものであるため、長期的に安定しているとは言えない。Podを再起動するたびに、新しいIPアドレスがリースされる。一貫したネットワーク・インタフェースに依存できるように、Pod間通信よりもPod-to-Service間通信を使用することを推奨する。

Podに代入されるIPアドレスは、すべてのノードとネームスペースで一意である。これは、各ノードの登録時に専用のサブネットを代入することで実現される。ノード上に新しいPodを作成する際、IPアドレスは割り当てられたサブネットからリースされる。これはContainer Network Interface（CNI）プラグインによって処理される。その結果、ノード上のPodは、クラスタの他のどのノード上で実行されている他のすべてのPodと通信できる。

ネットワーク・ポリシーはファイアウォール・ルールに似ているが、Pod間の通信に対して機能する。規則には、1つのネームスペース内または異なるネームスペースにまたがる1つまたは複数のPodのネットワークトラフィックの方向（ingressおよび/またはegress）、およびそれらのターゲットポートを含めることができる。ネットワークポリシーの基本について深く掘り下げた内容については、書籍『[*Certified Kubernetes Application Developer (CKAD) Study Guide*](https://learning.oreilly.com/library/view/certified-kubernetes-application/9781492083726)』（O'Reilly）または[Kubernetesのドキュメントを](https://oreil.ly/WChde)参照されたい。CKS試験では、主にネットワークポリシーによるクラスタレベルのアクセス制限に焦点が当てられている。

ネットワーク・ポリシーの規則を正しく定義することは難しい。[networkpolicy.ioという](https://networkpolicy.io)ページは、ネットワーク・ポリシーのためのビジュアル・エディターを提供し、ブラウザ上でグラフィカルに表示する。

## シナリオ攻撃者がPodにアクセスする

あなたが、3つのワーカーノードでKubernetesクラスタを運用している会社で働いているとする。ワーカーノード1は現在、マイクロサービスアーキテクチャの一部として2つのPodを実行している。KubernetesのPod間ネットワーク通信のデフォルトの振る舞いを考えると、Pod 1はPod 2と無制限に話すことができ、逆も同様だ。

[図2-](#ch02.html#pod-to-pod-communication-attacker)1でわかるように、攻撃者はPod 1にアクセスした。ネットワーク・ポリシーを定義しなければ、攻撃者は単純にPod 2に話しかけてさらなる損害を与えることができる。この脆弱性は単一のネームスペースに限定されるものではない。Pod3と4にもアクセスし、侵害することができる。

![](assets/ckss_0201.png)

## デフォルトの振る舞いを観察する

3つのPodをセットアップして、制限のないPod間ネットワーク通信を実際に試してみよう。[例 2-1](#ch02.html#setup-network-policy) を見るとわかるように、YAML マニフェストは`backend` と`frontend` という名前の Pod を名前空間`g04` で定義しています。`other` Pod は`default` 名前空間に存在する。名前空間とPodのラベル代入に注目してほしい。この章の後半でネットワークポリシーを定義するときに、これらを少し参照することになる。

##### 例 2-1. 異なる名前空間にある 3 つの Pod の YAML マニフェスト

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

##### 例2-2. デフォルトのdeny-allイングレスネットワークポリシー

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

##### 例2-3. イングレス・トラフィックを許可するネットワーク・ポリシー

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

オンプレミスのKubernetesクラスタを管理すると、APIサーバ、etcd、kubeletなどのクラスタコンポーネントに適用される構成オプションを完全に制御できる。`kubeadm` 、クラスタノードの作成時に使用されるデフォルトのコンフィギュレーション設定に単純に従うことは珍しくない。こうしたデフォルト設定の中には、クラスタ・コンポーネントを不要な攻撃の機会にさらすものもある。

クラスタのセキュリティ対策を強化することは、攻撃ベクトルを最小化しようとするKubernetes管理者にとって極めて重要な活動だ。ベストプラクティスを知っていれば手動でこの活動を行うこともできるし、自動化されたプロセスを使うこともできる。

[Center for Internet Security（CIS](https://www.cisecurity.org)）は、サイバーセキュリティのベストプラクティスを公開している非営利団体だ。彼らのベスト・プラクティス・ポートフォリオの一部に、[Kubernetes](https://oreil.ly/CUe_D)環境のベスト・プラクティスのカタログである[Kubernetes CIS Benchmarkが](https://oreil.ly/CUe_D)ある。彼らのWebページには、クラスターコンポーネントの推奨セキュリティ設定の詳細なリストが発見できる。

# クラウドプロバイダーKubernetes環境のCISベンチマーク

Kubernetes CIS Benchmarkは、Kubernetesのセルフマネージドインストールを対象としている。Amazon Elastic Kubernetes Service（EKS）やGoogle Kubernetes Engine（GKE）のようなクラウドプロバイダのKubernetes環境は、独自のコマンドラインツールを伴う管理されたコントロールプレーンを提供する。そのため、Kubernetes CIS Benchmarkが推奨するセキュリティは、あまり当てはまらないかもしれない。次に説明するkube-benchのように、クラウドプロバイダーに特化した検証チェックを提供するツールもある。

## kube-benchを使う

[kube-benchという](https://oreil.ly/y3mbO)ツールを使えば、KubernetesクラスタのコンポーネントをCIS Benchmarkのベストプラクティスに照らして自動でチェックできる。Kube-benchはさまざまな方法で実行できる。例えば、RPMやDebianファイルの形でプラットフォーム固有のバイナリとしてインストールできる。検証プロセスを実行する最も便利で直接的な方法は、Kubernetesクラスタ上で直接Podでkube-benchを実行することだ。そのためには、ツールのGitHubリポジトリにチェックインしたYAMLマニフェストの助けを借りてジョブ・オブジェクトを作成する。

コントロールプレーンノードかワーカーノードのどちらを検査したいかによって、`job-master.yaml` 、または`job-node.yaml` というファイルからジョブを作成することから始める。次のコマンドは、コントロールプレーンノードに対して検証チェックを実行する：

```
$ kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/\
main/job-master.yaml
job.batch/kube-bench-master created
```

ジョブを実行すると、検証プロセスを実行している対応するPodは、`default` 名前付け空間内の名前によって識別できる。Podの名前はプレフィックス`kube-bench` で始まり、最後にノードのタイプとハッシュが付加される。以下の出力では、`kube-bench-master-8f6qh` という名前の Pod を使用している：

```
$ kubectl get pods
NAME                      READY   STATUS      RESTARTS   AGE
kube-bench-master-8f6qh   0/1     Completed   0          45s
```

Podが「Completed」ステータスに遷移するまで待ち、すべての検証チェックが終了したことを確認する。Podのログをダンプすれば、ベンチマーク結果を見ることができる：

```
$ kubectl logs kube-bench-master-8f6qh
```

検証結果をファイルに書き出した方が便利な場合もある。`kubectl logs` コマンドの出力をファイルにリダイレクトするには、例えば`kubectl logs kube-bench-master-8f6qh > control-plane-kube-bench-results.txt`.

## kube-benchの検証結果

作成される検証結果は、長くて詳細なものになることもあるが、以下の主要要素で構成される：検査されたノードの型、検査されたコンポーネント、合格したチェックのリスト、不合格だったチェックのリスト、警告のリスト、およびハイレベルの要約：

```
[INFO] 1 Control Plane Security Configuration 
[INFO] 1.1 Control Plane Node Configuration Files
[PASS] 1.1.1 Ensure that the API server pod specification file permissions are \
set to 644 or more restrictive (Automated) 
...
[INFO] 1.2 API Server
[WARN] 1.2.1 Ensure that the --anonymous-auth argument is set to false \
(Manual) 
...
[FAIL] 1.2.6 Ensure that the --kubelet-certificate-authority argument is set \
as appropriate (Automated) 

== Remediations master ==
...
1.2.1 Edit the API server pod specification file /etc/kubernetes/manifests/ \
kube-apiserver.yaml on the control plane node and set the below parameter.
--anonymous-auth=false
...
1.2.6 Follow the Kubernetes documentation and setup the TLS connection between 
the apiserver and kubelets. Then, edit the API server pod specification file 
/etc/kubernetes/manifests/kube-apiserver.yaml on the control plane node and \ 
set the --kubelet-certificate-authority parameter to the path to the cert \ 
file for the certificate authority. 
--kubelet-certificate-authority=<ca-string> 

...
== Summary total == 
42 checks PASS
9 checks FAIL
11 checks WARN
0 checks INFO
```

[![1](assets/1.png)](#ch02.html#co_cluster_setup_CO1-1)
: 

検査されるノード、この場合はコントロールプレーンノード。

[![2](assets/2.png)](#ch02.html#co_cluster_setup_CO1-2)
: 

パスしたチェック。ここでは、APIサーバ構成ファイルのファイルパーミッションを示す。

[![3](assets/3.png)](#ch02.html#co_cluster_setup_CO1-3)
: 

APIサーバー実行ファイルに提供された引数の値を手動でチェックするよう促す警告メッセージ。

[![4](assets/4.png)](#ch02.html#co_cluster_setup_CO1-4)
: 

チェックに失敗した。例えば、APIサーバ実行ファイルに`--kubelet-certificate-authority` フラグをセットする。

[![5](assets/5.png)](#ch02.html#co_cluster_setup_CO1-5)
: 

問題を解決するために取るべき改善措置。障害や警告の番号（例えば1.2.1）は、改善措置に代入された番号に対応する。

[![6](assets/6.png)](#ch02.html#co_cluster_setup_CO1-11)
: 

合格したチェックと不合格だったチェックのサマリー、および警告と情報メッセージ。

## 検出されたセキュリティ問題を修正する

報告された警告や故障のリストは、最初は少し圧倒されるかもしれない。それらすべてを一度に修正する必要はないことを覚えておいてほしい。いくつかのチェックは、コンフィギュレーションに代入された値を確認するための単なるガイドラインや代入を促すものである。以下のステップでは、警告メッセージを取り除くプロセスを説明する。

コントロールプレーンコンポーネントの構成ファイルは、コントロールプレーンノードのホストシステム上のディレクトリ`/etc/kubernetes/manifests` 。kube-benchで報告された警告1.2.12を修正したいとする：

```
[INFO] 1.2 API Server
...
[WARN] 1.2.12 Ensure that the admission control plugin AlwaysPullImages is \
set (Manual)

== Remediations master ==
...
1.2.12 Edit the API server pod specification file /etc/kubernetes/manifests/ \
kube-apiserver.yaml
on the control plane node and set the --enable-admission-plugins parameter \
to include AlwaysPullImages.
--enable-admission-plugins=...,AlwaysPullImages,...
```

改善措置で提案されているように、APIサーバーの構成ファイルを編集し、入場プラグインのリストに値`AlwaysPullImages` を追加することになっている。ファイル`kube-apiserver.yaml` を編集する：

```
$ sudo vim /etc/kubernetes/manifests/kube-apiserver.yaml
```

引数`--enable-admission-plugins` に値`AlwaysPullImages` を追加すると、結果は以下のようになる：

```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: \
    192.168.56.10:6443
  creationTimestamp: null
  labels:
    component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
    - --advertise-address=192.168.56.10
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction,AlwaysPullImages
...
```

変更をファイルに保存する。`kube-system` ネームスペースでAPIサーバを実行しているPodが自動的に再起動される。起動プロセスには数秒かかることがある。そのため、以下のコマンドを実行すると、成功するまでしばらく時間がかかることがある：

```
$ kubectl get pods -n kube-system
NAME                           READY   STATUS    RESTARTS   AGE
...
kube-apiserver-control-plane   1/1     Running   0          71m
...
```

変更された結果を確認する前に、既存のジョブオブジェクトを削除する必要がある：

```
$ kubectl delete job kube-bench-master
job.batch "kube-bench-master" deleted
```

検証チェック1.2.12は、パスした結果を報告するようになった：

```
$ kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/\
main/job-master.yaml
job.batch/kube-bench-master created
$ kubectl get pods
NAME                      READY   STATUS      RESTARTS   AGE
kube-bench-master-5gjdn   0/1     Completed   0          10s
$ kubectl logs kube-bench-master-5gjdn | grep 1.2.12
[PASS] 1.2.12 Ensure that the admission control plugin AlwaysPullImages is \
set (Manual)
```

# TLSターミネーションによるイングレスの作成

Ingressは、一致するURLコンテキストパスに基づいて、クラスタ外部からのHTTPおよび/またはHTTPSトラフィックを1つまたは複数のサービスにルーティングする。[図 2-2](#ch02.html#ingress_traffic_routing) にその機能を示す。

![](assets/ckss_0202.png)

Ingressは、クラスタ外部からのHTTPおよびHTTPSトラフィックを受け入れるように設定 されている。発呼側がコンテキストパス(`/app`)を提供した場合、トラ フィックはサービス1にルーティングされる。発呼側がコンテキストパス`/api` を提供した場合、トラフィックはサービス2にルーティングされる。重要な点は、通信は通常、Ingressを通過するとすぐに暗号化されていないHTTPネットワーク通信を使用することである。

Ingress APIリソースはCKADとCKA試験の一部であるため、ここでは基本的な説明は省略する。詳細な議論については、[*Certified Kubernetes Administrator (CKA) Study Guide*](https://oreil.ly/cka-study-guide)または[Kubernetesドキュメントの](https://oreil.ly/wmk2s)情報を参照すること。

# イングレス・コントローラーの役割

Ingressは、Ingressコントローラなしでは機能しないことを覚えておくこと。Ingressコントローラは、トラフィックのルーティングを決定するIngressで定義された規則の集合を評価する。プロダクショングレードのIngressコントローラーの一例として、[F5 NGINX Ingress Controllerや](https://oreil.ly/jOo6P) [AKS Application Gateway Ingress Controllerが](https://oreil.ly/ckuqf)ある。[Kubernetesのドキュメントに](https://oreil.ly/BXx8e)リストされている他のオプションも発見できる。minikubeを使用している場合は、[Ingressアドオンを有効にして](https://oreil.ly/11QAA)おくこと。

CKS の主な焦点は、TLS 終端を持つ Ingress オブジェクトのセットアップにある。HTTPS 通信用に Ingress を設定することで、サービスレベルでのネットワーク通信のセキュリ ティ確保から解放される。このセクションでは、TLS 証明書と鍵の作成方法、証明書と鍵を TLS タイプの Secret オブジェクトに渡す方法、Ingress オブジェクトが HTTPS 通信をサポートするように設定する方法を学ぶ。

## Ingressバックエンドのセットアップ

Ingressのコンテキストでは、*バックエンドは*サービス名とポートの組み合わせである。Ingressを作成する前に、サービス、デプロイ、そしてnginxが動作するPodを用意し、後で実際のアプリケーションへのHTTPSトラフィックのルーティングを実演できるようにする。これらのオブジェクトはすべて名前空間`t75` に存在する。[例2-4は](#ch02.html#ingress-setup)、Ingressバックエンドを素早く作成する手段として、これらすべてのリソースを単一のYAMLマニフェストファイル`setup.yaml` で定義している。

##### 例 2-4. サービスを通して nginx を公開するための YAML マニフェスト

```
apiVersion: v1
kind: Namespace
metadata:
  name: t75
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: t75
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: accounting-service
  namespace: t75
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

以下のコマンドでYAMLファイルからオブジェクトを作成する：

```
$ kubectl apply -f setup.yaml
namespace/t75 created
deployment.apps/nginx-deployment created
service/accounting-service created
```

オブジェクトが正しく作成され、Podが「Running」ステータスに移行していることを素早く確認してみよう。`get all` コマンドを実行すると、3つのレプリカを制御する`nginx-deployment` という名前のデプロイと、`ClusterIP` タイプの`accounting-service` という名前のサービスが表示されるはずだ：

```
$ kubectl get all -n t75
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-6595874d85-5rdrh   1/1     Running   0          108s
pod/nginx-deployment-6595874d85-jmhvh   1/1     Running   0          108s
pod/nginx-deployment-6595874d85-vtwxp   1/1     Running   0          108s

NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S) \
  AGE
service/accounting-service   ClusterIP   10.97.101.228   <none>        80/TCP \
  108s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           108s
```

同じノード上で動作している別のPodからサービスエンドポイントを呼び出すと、nginx Podからのレスポンスが成功するはずだ。ここでは、`wget` コマンドを使って振る舞いを検証している：

```
$ kubectl run tmp --image=busybox --restart=Never -it --rm \
  -- wget 10.97.101.228:80
Connecting to 10.97.101.228:80 (10.97.101.228:80)
saving to 'index.html'
index.html           100% |**|   612  0:00:00 ETA
'index.html' saved
pod "tmp" deleted
```

これらのオブジェクトを配置し、期待通りに機能させれば、あとはTLS終端を持つIngressの作成に集中できる。

## TLS証明書と鍵の作成

TLSシークレットを作成する前に、TLS証明書と鍵を生成する必要がある。そのためにOpenSSLコマンドを使う。出来上がったファイルの名前は`accounting.crt` と`accounting.key` ：

```
$ openssl req -nodes -new -x509 -keyout accounting.key -out accounting.crt \
  -subj "/CN=accounting.tls"
Generating a 2048 bit RSA private key
...........................+
..........................+
writing new private key to 'accounting.key'
-----
$ ls
accounting.crt accounting.key
```

本番環境で使用する場合は、鍵ファイルを生成し、それを使って認証局（CA）からTLS証明書を取得する。TLS証明書と鍵の作成の詳細については、[OpenSSLのドキュメントを](https://oreil.ly/sETSb)参照のこと。

## TLS型シークレットを作成する

シークレットを作成する最も簡単な方法は、命令型コマンドの助けを借りることだ。この方法では、証明書と鍵の値を手動でbase64エンコーディングする必要はない。エンコーディングはオブジェクトの作成時に自動的に行われる。次のコマンドはシークレットオプション`tls` を使用し、証明書と鍵のファイル名をオプション`--cert` と`--key` で代入している：

```
$ kubectl create secret tls accounting-secret --cert=accounting.crt \
  --key=accounting.key -n t75
secret/accounting-secret created
```

[例2-](#ch02.html#tls-secret)5は、オブジェクトを宣言的に作成したい場合のTLSシークレットのYAML表現を示 している。

##### 例2-5. 型を使ったシークレット`kubernetes.io/tls`

```
apiVersion: v1
kind: Secret
metadata:
  name: accounting-secret
  namespace: t75
type: kubernetes.io/tls
data:
  tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk...
  tls.key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk...
```

属性`tls.crt` と`tls.key` の代入は、必ず1行のbase64エンコード値として行うこと。base64エンコード値を生成するには、`base64` コマンドを、内容を変換したいファイル名に向けるだけでよい。次の例は、ファイル`accounting.crt` の内容をbase64エンコードしたものである：

```
$ base64 accounting.crt
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNyakNDQ...
```

## イングレスの作成

次のスニペットに示すワンライナーコマンドを使えば、命令形メソッドでIngressを作成できる。`--rule` 引数の値を正しく作るのは難しい。`create ingress` コマンドには特定の式が必要なので、`--help` オプションを参照する必要があるだろう。IngressオブジェクトとTLSシークレットの接続作成に関連する情報は、`tls=accounting-secret` の引数である：

```
$ kubectl create ingress accounting-ingress \
  --rule="accounting.internal.acme.com/*=accounting-service:80, \
  tls=accounting-secret" -n t75
ingress.networking.k8s.io/accounting-ingress created
```

[例2-](#ch02.html#ingress-tls-termination)6はIngressのYAML表現である。TLS情報を定義する属性は`spec.tls[]` である。

##### 例2-6. TLS 終端 Ingress を定義するための YAML マニフェスト

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: accounting-ingress
  namespace: t75
spec:
  tls:
  - hosts:
    - accounting.internal.acme.com
    secretName: accounting-secret
  rules:
  - host: accounting.internal.acme.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: accounting-service
            port:
              number: 80
```

命令的または宣言的アプローチでIngressオブジェクトを作成したら、名前空間`t75` 。以下の出力でわかるように、"PORT "列には443番ポートがリストされており、TLS終端が有効になっていることがわかる：

```
$ kubectl get ingress -n t75
NAME                 CLASS   HOSTS                          ADDRESS       \
  PORTS     AGE
accounting-ingress   nginx   accounting.internal.acme.com   192.168.64.91 \
  80, 443   55s
```

Ingressオブジェクトを記述すると、バックエンドはパス`/` にマッピングされ、`accounting-service` という名前のサービスを経由してPodにトラフィックをルーティングすることがわかる：

```
$ kubectl describe ingress accounting-ingress -n t75
Name:             accounting-ingress
Labels:           <none>
Namespace:        t75
Address:          192.168.64.91
Ingress Class:    nginx
Default backend:  <default>
TLS:
  accounting-secret terminates accounting.internal.acme.com
Rules:
  Host                          Path  Backends
  ----                          ----  --------
  accounting.internal.acme.com
                                /   accounting-service:80 \
                                (172.17.0.5:80,172.17.0.6:80,172.17.0.7:80)
Annotations:                    <none>
Events:
  Type    Reason  Age               From                      Message
  ----    ------  ----              ----                      -------
  Normal  Sync    1s (x2 over 31s)  nginx-ingress-controller  Scheduled for sync
```

## イングレスの呼び出し

自分のマシン上のローカルKubernetesクラスタでの振る舞いをテストするには、まずノードのIPアドレスを発見する必要がある。以下のコマンドでminikube環境でのIPアドレスが分かる：

```
$ kubectl get nodes -o wide
NAME       STATUS   ROLES           AGE     VERSION   INTERNAL-IP   \
  EXTERNAL-IP   OS-IMAGE               KERNEL-VERSION   CONTAINER-RUNTIME
minikube   Ready    control-plane   3d19h   v1.24.1   192.168.64.91 \
  <none>        Buildroot 2021.02.12   5.10.57          docker://20.10.16
```

次に、IPアドレスとホスト名のマッピングを`/etc/hosts` ファイルに追加する必要がある：

```
$ sudo vim /etc/hosts
...
192.168.64.91   accounting.internal.acme.com
```

代入されたドメイン名を使ってIngressにHTTPSリクエストを送信し、HTTPレスポンスコード200を受信することができるようになった：

```
$ wget -O- https://accounting.internal.acme.com --no-check-certificate
--2022-07-28 15:32:43--  https://accounting.internal.acme.com/
Resolving accounting.internal.acme.com (accounting.internal.acme.com)... \
192.168.64.91
Connecting to accounting.internal.acme.com (accounting.internal.acme.com) \
|192.168.64.91|:443... connected.
WARNING: cannot verify accounting.internal.acme.com's certificate, issued \
by ‘CN=Kubernetes Ingress Controller Fake Certificate,O=Acme Co’:
  Self-signed certificate encountered.
WARNING: no certificate subject alternative name matches
	requested host name ‘accounting.internal.acme.com’.
HTTP request sent, awaiting response... 200 OK
```

# ノードのメタデータとエンドポイントを保護する

Kubernetesクラスタは、クラスタ・コンポーネントとの通信に使用されるポートを公開する。例えば、APIサーバはデフォルトでポート6443を使用し、`kubectl` のようなクライアントがコマンドを実行する際に通信できるようにしている。

Kubernetesのドキュメントには、これらのポートが[「ポートとプロトコル](https://oreil.ly/iN993)」にリストされている[。](https://oreil.ly/iN993)以下の2つの表は、ノードごとのデフォルトのポート代入を示している。

[表2-](#ch02.html#control-plane-node-ports)1にクラスタ・ノードのデフォルトのインバウンド・ポートを示す。

表2-1. インバウンドコントロールプレーンノードポート

  
| ポート範囲 || 目的 |
| --- | --- |
| 6643 || Kubernetes APIサーバ |
| --- | --- |
| 2379-2380 || etcdサーバクライアントAPI |
| 10250 || Kubelet API |
| 10259 || キューブ・スケジューラー |
| 10257 || キューブコントローラマネージャ |

これらのポートの多くは設定可能である。例えば、構成ファイル`/etc/kubernetes/manifests/kube-apiserver.yaml` のフラグ`--secure-port` で異なる値を指定することで、クラスタ・コンポーネントの[ドキュメントにある](https://oreil.ly/TTzAz)ようにAPIサーバ・ポートを変更できる。他のすべてのクラスタ・コンポーネントについては、対応するドキュメントを参照のこと。

[表2-](#ch02.html#worker-node-ports)2はワーカーノードのデフォルトインバウンドポートのリストである。

表2-2. インバウンドワーカーノードポート

  
| ポート範囲 || 目的 |
| --- | --- |
| 10250 || Kubelet API |
| --- | --- |
| 30000-32767 || ノードポート・サービス |

クラスタコンポーネントが使用するポートを保護するために、攻撃対象領域を最小化するファイアウォール規則を設定する。例えば、APIサーバをイントラネット外の誰にも公開しないように決めることができる。`kubectl` を使用するクライアントは、VPN にログインした場合のみ Kubernetes クラスタに対してコマンドを実行できるようになり、クラスタが攻撃を受けにくくなる。

クラウドプロバイダのKubernetesクラスタ（AWS、Azure、Google Cloudなど）は、いわゆるメタデータサービスを公開している。メタデータサービスは、認証トークンのような機密データを仮想マシンやPodから消費するために、追加の認可なしで提供できるAPIだ。CKS試験のためには、これらのノードエンドポイントとクラウドプロバイダのメタデータサービスを認識しておく必要がある。さらに、不正アクセスからそれらを保護する方法についても、高レベルで理解しておく必要がある。

## シナリオ侵害されたPodがメタデータサーバにアクセスできる

[図2-3は](#ch02.html#metadata-server-attacker)、クラウドプロバイダーのKubernetesクラスタ内のノードで実行されているPodにアクセスした攻撃者を示している。

![](assets/ckss_0203.png)

メタデータサーバへのアクセスはいかなる形でも制限されていない。攻撃者は機密情報を取得することができ、他の侵入の可能性を開く可能性がある。

## ネットワークポリシーでメタデータサーバへのアクセスを保護する

メタデータのエンドポイントを公開しているクラウドプロバイダーを1つ選んでみよう。AWSの場合、[AWSのドキュメントに](https://oreil.ly/6DsIx)記載されているように、メタデータサーバにはIPアドレス169.254.169.254でアクセスできる。公開されたエンドポイントは、EC2インスタンスのメタデータへのアクセスを提供できる。例えば、インスタンスのローカルIPアドレスを取得して外部アプリケーションへの接続を管理したり、スクリプトを使用してインスタンスに接続したりできる。curlコマンドラインツールを使用したエンドポイントの呼び出しについては、対応する[ドキュメントのページを](https://oreil.ly/Bwdej)参照のこと。

ネームスペース内のどのPodもメタデータ・サーバのIPアドレスに到達しないようにするには、169.254.169.254以外のすべてのIPアドレスへのイグジット・トラフィックを許可するネットワーク・ポリシーを設定する。[例 2-7](#ch02.html#deny-egress-metadata-server)は、このような規則セットを持つ YAML マニフェストを示している。

##### 例2-7. IPアドレス169.254.169.254へのデフォルトdeny-all egressネットワークポリシー

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

ネットワークポリシーが作成されると、名前空間`a12` のPodはメタデータエンドポイントに到達できなくなるはずである。`curl` 経由でエンドポイントを使用する詳細な例については、関連する[AWS ドキュメントを](https://oreil.ly/fQ07b)参照のこと。

# GUI要素を保護する

クラスタを管理するためのユーザインタフェース（UI）は、`kubectl` ツールだけではない。`kubectl` ではきめ細かな操作が可能だが、ほとんどのユーザはクラスタのオブジェクトを管理するためにより便利なグラフィカル・ユーザ・インターフェイス（GUI）を好む。さまざまなオプションから選択できる。[Kubernetes Dashboardは](https://oreil.ly/ABDQo)無料のウェブベースのアプリケーションだ。[Portainerの](https://oreil.ly/i_FJv)ようなKubernetes用の他のGUIダッシュボードは、イベントのトレースやハードウェアリソース消費の可視化を追加することで、基本的な機能を超えている。このセクションでは、インストールと設定が簡単なKubernetes Dashboardに焦点を当てる。

## シナリオ攻撃者がダッシュボード機能にアクセスする

Kubernetes Dashboardはクラスタ内部のPodとして実行される。Dashboardをインストールすると、`ClusterIP` 、クラスタ内部からエンドポイントへのアクセスのみを許可するタイプのServiceも作成される。エンドユーザがDashboardにアクセスできるようにするには、クラスタの外部にServiceを公開する必要がある。たとえば、`NodePort` Serviceタイプに切り替えるか、Ingressを立ち上げる。[図 2-4](#ch02.html#dashboard-attacker)に、Dashboard のデプロイとアクセスに関する高レベルのアーキテクチャを示す。

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

マニフェストによって作成されたオブジェクトは、`kubernetes-dashboard` 名前空間で発見できる。その中には、デプロイ、Pod、およびサービスが含まれる。次のコマンドは、これらすべてのリストを表示する：

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

 `kubectl proxy` コマンドは、ブラウザでダッシュボードを開くためのプロキシを一時的に作成するのに役立つ。この機能はトラブルシューティングを目的としており、本番環境向けではない。 コマンドに関する情報は`proxy` [ドキュメントで](https://oreil.ly/gGsqX)発見できる：

```
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

URL[*http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy*](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy) でブラウザを開く。Dashboard が認証方法と資格情報の提供を求める。Dashboard を構成する推奨の方法は、ベアラートークンを使用することである。

## 管理者権限を持つユーザの作成

ログイン画面で認証する前に、管理者権限を与えるServiceAccountとClusterRoleBindingオブジェクトを作成する必要がある。まず、`admin-user-serviceaccount.yaml` ファイルを作成し、[例2-](#ch02.html#admin-user-serviceaccount)8に示す内容でそれを埋めることから始める。

##### 例2-8. 管理者権限を持つサービスアカウント

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

次に、`cluster-admin` という名前のClusterRoleをServiceAccountにマッピングするために、[例2-9の](#ch02.html#admin-user-clusterrolebinding)内容をファイル`admin-user-⁠clusterrole​bind⁠ing.yaml` 。

##### 例2-9. 管理者権限用のClusterRoleBinding

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

デフォルトでは、このトークンは24時間後に失効する。つまり、「有効期間」（TTL）が過ぎると、トークン・オブジェクトは自動的に削除される。コマンドラインオプション`--ttl` を指定することで、トークンのTTLを変更することができる。たとえば、`40h` を指定すると、トークンは40時間後に失効する。`0` を指定すると、トークンは期限切れにならない。

[図2-](#ch02.html#dashboard-login-screen-token)5に示すように、コマンドの出力をコピーし、ログイン画面の「Enter token」フィールドにペーストする。

![](assets/ckss_0205.png)

Sign in "ボタンを押すと、[図2-6に](#ch02.html#dashboard-overview)示すダッシュボードが表示される。

![](assets/ckss_0206.png)

エンドユーザやクラスタのオブジェクトを制限なく管理できるようになった。

## 制限された権限を持つユーザの作成

前のセクションでは、クラスタ全体の管理者権限を持つユーザを作成する方法を学んだ。しかし、Dashboardのほとんどのユーザは、制限されたパーミッションのセットしか必要としない。たとえば、クラウドネイティブなアプリケーションを実装して運用する開発者は、Kubernetesクラスタ上でタスクを実行するために管理者権限のサブセットしか必要としない可能性が高い。制限された権限を持つDashboardのユーザを作成するには、3つのステップからなる：

1.  ServiceAccountオブジェクトを作成する。
    
2.  パーミッションを定義するClusterRoleオブジェクトを作成する。
    
3.  ClusterRoleをServiceAccountにマッピングするClusterRoleBindingを作成する。
    

見てわかるように、このプロセスは管理者ユーザに対して行ったものと非常によく似ている。ステップ2が新しく、付与したいパーミッションを特定する必要がある。この後に続くYAMLマニフェストは、読み取り専用パーミッション(たとえば、リソースの取得、リスト表示、監視)だけを許可されるべき開発者として働くユーザをモデル化する。

まず、`restricted-user-serviceaccount.yaml` ファイルを作成し、[例2-10に](#ch02.html#restricted-user-serviceaccount)示す内容を入力する。

##### 例2-10. 制限された権限のサービスアカウント

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: developer-user
  namespace: kubernetes-dashboard
```

[例2-11の](#ch02.html#restricted-user-clusterrole)ClusterRoleでは、リソースの取得、リスト、ウォッチのみが許可されている。それ以外の演算子は許可されていない。内容をファイル`restricted-user-clusterrole.yaml` に格納する。

##### 例2-11. 制限されたパーミッションのためのClusterRole

```
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

最後に、[例2-12に](#ch02.html#restricted-user-clusterrolebinding)示すように、ファイル`restricted-user-clusterrolebinding.yaml` でServiceAccountをClusterRoleにマッピングする。

##### 例2-12. 制限されたパーミッションのためのClusterRoleBinding

```
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

本番環境でダッシュボードを保護するには、認証認可を適切に構成するために必要な[実行引数を](https://oreil.ly/gS1hE)使用する必要がある。デフォルトでは、ログイン機能は有効になっており、HTTPSエンドポイントはポート8443で公開される。TLS 証明書を自動生成させたくない場合は、`--tls-cert-file` および`--tls-cert-key` コマンドラインオプションで指定することができる。

コマンドライン引数`--insecure-port` 、HTTPエンドポイントを公開し、`--enable-insecure-login` 、HTTPSではなくHTTPでログイン・ページを提供できるようにする設定は避けること。さらに、ログイン画面のSkipボタンをクリックするだけで認証方法を回避できるようになるので、`--enable-skip-login` *。*

# Kubernetesプラットフォームのバイナリを検証する

Kubernetesプロジェクトは、リリースごとにクライアント、サーバのバイナリを公開している。クライアント・バイナリは、実行ファイル`kubectl` を指す。サーバ・バイナリには、`kubeadm` 、APIサーバ、スケジューラ、kubeletの実行ファイルが含まれる。これらのファイルは、[Kubernetes GitHub](https://oreil.ly/vHpAV)リポジトリの "tags "セクションか、リリースページの[*https://dl.k8s.io。*](https://dl.k8s.io)

## シナリオ攻撃者が悪意のあるコードをバイナリに注入した

実行ファイル`kubectl` と`kubeadm` は、Kubernetesとやりとりするのに欠かせない。`kubectl` を使うと、オブジェクトの管理など、サーバに対してコマンドを実行できる。`kubeadm` は、クラスタノードをあるバージョンから別のバージョンにアップグレードするのに必要だ。例えば、[クラスタのバージョンを](https://oreil.ly/hTJ57)1.23から1.24に[アップグレードして](https://oreil.ly/hTJ57)いるとしよう。そのプロセスの一環として、`kubeadm` のバイナリもアップグレードする必要がある。公式のアップグレードドキュメントには、バイナリのアップグレードにどのようなコマンドを使うべきか、非常に具体的に書かれている。

攻撃者がバージョン1.24の`kubeadm` 実行ファイルを修正し、悪意のあるバイナリが置かれた場所からそのバイナリをダウンロードする必要があると思わせたとする。[図2-](#ch02.html#binary-attacker)8に示すように、変更された`kubeadm` 実行可能ファイルを呼び出すたびに、悪意のあるコードを実行する危険にさらされることになる。例えば、クラスタ外のサーバに資格情報を送信している可能性があり、Kubernetes環境に侵入する新たな方法が開かれることになる。

![](assets/ckss_0208.png)

## ハッシュに対するバイナリの検証

バイナリの有効性は、MD5やSHAなどのハッシュコードの助けを借りて確認できる。Kubernetesは各バイナリのSHA256ハッシュコードを公開している。初めて使用する前に、個々のバイナリのハッシュ検証を実行する必要がある。生成されたハッシュコードがダウンロードしたものと一致しない場合は、バイナリに何か問題がある。バイナリがサードパーティによって変更されているか、正しいバイナリのタイプやバージョンに対応するハッシュコードを使用していない可能性がある。

バイナリに対応するハッシュコードは、[*https://dl.k8s.io*](https://dl.k8s.io) からダウンロードできる[*。*](https://dl.k8s.io)ハッシュコードの完全なURLは、バイナリのバージョン、オペレーティングシステム、アーキテクチャを反映している。以下のリストは、Linux AMD64と互換性のあるプラットフォーム・バイナリのURLの例を示している：

*   `kubectl`h[*ttps://*](https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubectl.sha256)dl.k8s.io/v1.26.1/bin/linux/amd64/kubectl.sha256
    
*   `kubeadm`h[*ttps://*](https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256)dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256
    
*   `kubelet`h[*ttps://*](https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubelet.sha256)dl.k8s.io/v1.26.1/bin/linux/amd64/kubelet.sha256
    
*   `kube-apiserver`h[*ttps://*](https://dl.k8s.io/v1.26.1/bin/linux/amd64/kube-apiserver.sha256)dl.k8s.io/v1.26.1/bin/linux/amd64/kube-apiserver.sha256
    

バイナリの正当性をチェックするには、オペレーティングシステム固有のハッシュコード検証ツールを使う必要がある。マシンにツールがない場合は、インストールする必要があるかもしれない。以下のコマンドは、[Kubernetesのドキュメントで](https://oreil.ly/2FmVm)説明されているように、異なるオペレーティングシステムでのツールの使い方を示している：

*   Linux:`echo "$(cat kubectl.sha256) kubectl" | sha256sum --check`
    
*   MacOSXである：`echo "$(cat kubectl.sha256) kubectl" | shasum -a 256 --check`
    
*   PowerShellでWindows：`$($(CertUtil -hashfile .\kubectl.exe SHA256)[1] -replace " ", "") -eq $(type .\kubectl.exe.sha256)`
    

以下のコマンドは、バージョン1.26.1の`kubeadm` バイナリと、それに対応するSHA256ハッシュファイルのダウンロードを示している：

```
$ curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm"
$ curl -LO "https://dl.k8s.io/v1.26.1/bin/linux/amd64/kubeadm.sha256"
```

検証ツール`shasum` 、チェックサムが一致するかどうかを検証できる：

```
$ echo "$(cat kubeadm.sha256)  kubeadm" | shasum -a 256 --check
kubeadm: OK
```

前のコマンドは "OK "メッセージを返した。バイナリファイルは改ざんされていない。それ以外のメッセージは、バイナリを実行する際に潜在的なセキュリティ・リスクがあることを示している。

# 概要

ドメイン「クラスタのセットアップ」では、Kubernetesクラスタのセットアップに関連するセキュリティの側面にダイアルしている。`kubeadm` を使ってゼロからクラスタを作成していても、必ずしもベストプラクティスに従っているとは限らない。kube-benchを使って潜在的なセキュリティリスクを検出するのは良いスタートだ。ツールによって報告された問題を1つずつ修正する。また、クライアント、サーバのバイナリをチェックサムと照合し、攻撃者によって変更されていないことを確認するのもよい。クラスタとそのオブジェクトを管理するためにDashboardを使用する組織もある。Dashboardの認証と認可によって、少数の関係者にアクセスを制限するようにする。

セキュリティ面で重要なのはネットワーク通信だ。Pod間の通信はデフォルトでは制限されていない。Kubernetesの内部で動作しているアプリケーションアーキテクチャをよく見てみよう。アーキテクチャの要件を満たすために、PodからPodへの指向性のあるネットワーク・トラフィックだけを許可する。それ以外のネットワークトラフィックはすべて拒否する。アプリケーションをクラスタの外部に公開する場合は、IngressオブジェクトがTLSターミネーションで設定されていることを確認する。これにより、クライアントとKubernetesクラスタ間で送信されるパスワードのような機密情報を攻撃者が観測できないように、データが双方向で暗号化される。

# 試験の要点

ネットワークポリシーの目的と効果を理解する
: 

デフォルトでは、Pod間の通信は制限されていない。デフォルトの拒否規則をインスタンス化して、最小権限の原則でPod間のネットワーク・トラフィックを制限する。ネットワークポリシーの属性`spec.podSelector` は、ラベル選択に基づいて規則が適用される対象の Pod を選択する。イングレス・ルールとイグレス・ルールでは、送受信トラフィックを許可するための Pod、ネームスペース、IP アドレス、ポートを定義する。ネットワーク・ポリシーは集約できる。デフォルトの拒否規則は、イングレスおよび/またはイグレスのトラフィックを拒否することができる。追加のネットワークポリシーは、より細かい定義でこれらの規則を開くことができる。

クラスタコンポーネントの脆弱性を検出するためのkube-benchの使い方を実践する
: 

Kubernetes CIS Benchmarkは、本番Kubernetes環境で推奨されるセキュリティ設定のベストプラクティスのセットである。kube-benchというツールを使えば、セキュリティリスクを検出するプロセスを自動化できる。kube-benchの実行により生成されるレポートには、検出された問題を修正するための詳細な修正アクションが記載されている。結果をどのように解釈し、どのように問題を軽減するかを学ぶ。

TLSターミネーションによるIngressの設定方法を知る
: 

Ingressは、 HTTPSエンドポイントを公開することで、暗号化されたデータを送受信するように設定できる。これを動作させるには、TLS Secretオブジェクトを作成し、それにTLS証明書と鍵を代入する必要がある。このシークレットは、`spec.tls[]` という属性を使ってイングレスが消費できる。

安全なアクセスのためにGUI要素を設定する方法を知る
: 

Kubernetes DashboardなどのGUI要素は、オブジェクトを管理する便利な方法を提供する。アプリケーションが不正アクセスから保護されていなければ、攻撃者はクラスタに危害を加える可能性がある。試験のためには、特定の関係者のためにRBACを適切にセットアップする方法を知っている必要がある。さらに、セキュリティ関連のコマンドライン引数を大まかに理解していることも求められる。Dashboard のインストール手順を練習し、そのコマンドライン引数を調整する方法を学び、異なるユーザに対する権限設定の効果を理解する。

変更されたプラットフォームのバイナリを検出する方法を知る
: 

`kubectl` や`kubeadm` のようなプラットフォームバイナリは、対応するハッシュコードと照合して検証することができる。ハッシュファイルの発見場所と、バイナリが改ざんされていないかどうかを確認するための検証ツールの使い方を知っておくこと。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1.  クラスタ外のドメインへのイグジット・トラフィックを拒否するネットワーク・ポリシーを作成する。このネットワークポリシーは、`app=backend` のラベルを持つ Pod に適用され、他のネームスペースにある Pod への UDP および TCP のポート 53 のイグジットトラフィックも許可する。
    
2.  ポート80で`busybox:1.36.0` イメージを実行する`allowed` という名前のPodを作成し、`app=frontend` というラベルを代入する。`http://google.com` に`curl` 呼び出しを行う。ネットワークポリシーはPodには適用されないので、ネットワーク呼び出しは許可されるはずである。
    
3.  ポート80で`busybox:1.36.0` イメージを実行する`denied` という名前の別のPodを作成し、`app=backend` というラベルを代入する。`http://google.com` に`curl` 呼び出しを行う。 ネットワーク呼び出しはブロックされるはずである。
    
4.  Kubernetes Dashboardをインストールするか、すでにインストールされていることを確認する。名前空間`kubernetes-dashboard` に、`observer-user` という名前のServiceAccountを作成する。さらに、対応するClusterRoleとClusterRoleBindingを作成する。ServiceAccountにはデプロイの閲覧のみを許可する。他のすべての演算子は拒否されるべきである。例として、`default` 名前付けに`deploy` というデプロイを以下のコマンドで作成する：`kubectl create deployment deploy --image=nginx --replicas=3`.
    
5.  `observer-user` という名前の ServiceAccount 用トークンを作成し、期限切れにならないようにする。トークンを使用してダッシュボードにログインする。デプロイのみが表示され、他のタイプのリソースは表示されないことを確認する。
    
6.  Linux AMD64上のバージョン1.26.1のAPIサーバーのバイナリファイルをダウンロードする。バージョン1.23.1のAPIサーバー実行ファイルのSH256チェックサムファイルをダウンロードする。OS固有の検証ツールを実行し、結果を観察する。
    

# 第3章. クラスタ・ハードニング

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

クラスタの堅牢化」という領域では、クラスタの初期設定と構成が完了したら、クラスタを可能な限り安全に保つために重要なトピックに触れる。この章の議論の一環として、通常Kubernetes管理者の手に落ちる概念やプラクティスを参照することに気づくかもしれない。必要に応じて、CKA試験ですでにカバーされているトピックへのリンクを提供する。

この章では、高いレベルで以下の概念をカバーする：

*   Kubernetes APIへのアクセスを制限する
    
*   役割ベースのアクセス制御（RBAC）を設定し、露出を最小限に抑える
    
*   サービスアカウントの使用に注意を払う
    
*   Kubernetesを頻繁に更新する
    

# Kubernetes APIと対話する

APIサーバはKubernetesクラスタへのゲートウェイである。人間のユーザ、クライアント（例：`kubectl` ）、クラスタコンポーネント、サービスアカウントは、HTTPS経由でRESTful API呼び出しを行うことでAPIサーバにアクセスする。Podの作成やServiceの削除といった演算子を実行するための中心*点*だ。

このセクションでは、APIサーバに関連するセキュリティ特有の側面にのみ焦点を当てる。APIサーバの内部構造とKubernetes APIの使用法に関する詳細な議論については、Brendan Burns氏とCraig Tracey氏による書籍『[*Managing Kubernetes*](https://learning.oreilly.com/library/view/managing-kubernetes/9781492033905)』（O'Reilly）を参照されたい。

## リクエストを処理する

[図3-](#ch03.html#api-server-request-processing)1は、APIサーバへの呼び出し時にリクエストが通過する段階を示している。参考までに、[Kubernetesのドキュメントに](https://oreil.ly/DuLdf)詳細な情報が発見できる。

![](assets/ckss_0301.png)

リクエスト処理の最初の段階は*認証*である。認証は、クライアント証明書またはベアラートークンを検査する ことで、呼び出し元のIDを検証する。ベアラートークンがサービスアカウントに関連付け られている場合、それはここで検証される。

第2ステージは、第1ステージで提供されたIDが動詞とHTTPパスリクエストにアクセスできるかどうかを判断する。そのため、第2ステージではリクエストの*認可を*扱うが、これは標準的なKubernetes RBACモデルで実装される。ここでは、リクエストされた内容であれば、サービスアカウントがPodのリストや新しいサービスオブジェクトの作成を許可されていることを確認する。

リクエスト処理の第三段階は、*アドミッション制御を*扱う。アドミッションコントロールは、リクエス トが処理される前に、リクエストが整形式であるかどうか、潜在的に修正さ れる必要があるかどうかを検証する。アドミッション制御ポリシーは、例えば、Pod作成リクエストが特定のラベルの 定義を含むことを保証することができる。もしラベルが定義されていなければ、リクエストは拒否される。

プロセスの最後のステージは、リクエストに含まれるリソースが有効であるこ とを保証する。リクエストの*バリデーションは*アドミッション制御の一部として実装できるが、実装する必要はない。例えば、このステージでは、サービスオブジェクトの名前が、提供されるDNS名に対する標準的なKubernetesの名前付け規則に準拠していることを保証する。

## APIサーバに接続する

APIサーバーのエンドポイントは、以下を実行すれば簡単に特定できる：

```
$ kubectl cluster-info
Kubernetes control plane is running at https://172.28.40.5:6443
...
```

指定されたKubernetesクラスタでは、APIサーバはURL[*https://172.28.40.5:6443*](https://172.28.40.5:6443) を介して公開されている。あるいは、APIサーバの構成ファイルにあるコマンドラインオプション`--advertise-address` と`--secure-port` を見て、エンドポイントを決定することもできる。APIサーバーの構成ファイルは`/etc/kubernetes/manifests/kube-apiserver.yaml` で発見できる。

# APIサーバに安全でないポートを設定する

Kubernetes 1.10では、安全でないポート（例えば80）を使用するようにAPIサーバを設定する機能は廃止された。バージョン1.24では、安全でないポートフラグ`--port` と`--insecure-port` が完全に削除されたため、APIサーバの設定に使用できなくなった。詳細は[リリースノートを](https://oreil.ly/OTsmV)参照のこと。

### Kubernetesサービスを使う

Kubernetesは、APIサーバへのアクセスを特定のユースケースで少し便利にする。例えば、PodからKubernetes APIにリクエストを送りたい場合がある。APIサーバのIPアドレスとポートを使う代わりに、`kubernetes.default.svc` という名前のServiceを参照すればいい。この特殊化Serviceは`default` ネームスペースに存在し、クラスタによって自動的に立ち上げられる。サービスを削除すると、自動的に再作成される。このServiceは以下のコマンドで簡単に発見できる：

```
$ kubectl get service kubernetes
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32s
```

このサービスのエンドポイントを調べると、APIサーバーのIPアドレスとポートを指していることがわかる：

```
$ kubectl get endpoints kubernetes
NAME         ENDPOINTS          AGE
kubernetes   172.28.40.5:6443   4m3s
```

サービスのIPアドレスとポートも、環境変数を通じてPodに公開される。環境変数の値は、コンテナ内で実行されているプログラムから読み取ることができる。サービスのIPアドレスは、環境変数`KUBERNETES_SERVICE_HOST` 。ポートには、環境変数`KUBERNETES_SERVICE_PORT` を使ってアクセスできる。環境をレンダリングするには、一時Podで`env` コマンドを使って環境変数にアクセスするだけでよい：

```
$ kubectl run kubernetes-envs --image=alpine:3.16.2 -it --rm --restart=Never \
  -- env
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
```

[サービスアカウントの権限を最小化する」の](#ch03.html#minimizing-permissions-service-account)セクションで、`kubernetes` サービスを使用する[。](#ch03.html#minimizing-permissions-service-account)

### 匿名アクセス

以下のコマンドは、`curl` コマンドラインツールを使用してAPIを匿名で呼び出し、すべてのネームスペースをリストする。オプション`-k` 、サーバのTLS証明書の検証を回避する：

```
$ curl https://172.28.40.5:6443/api/v1/namespaces -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "namespaces is forbidden: User \"system:anonymous\" cannot list \
              resource \"namespaces\" in API group \"\" at the cluster scope",
  "reason": "Forbidden",
  "details": {
    "kind": "namespaces"
  },
  "code": 403
}
```

JSON形式のHTTPレスポンスボディからわかるように、匿名呼び出しはAPIサーバによって受け入れられるが、演算子に対する適切なパーミッションを持っていない。内部的には、Kubernetesはこの呼び出しを[ユーザー名`system:anonymous`](https://oreil.ly/_HrbF) にマッピングしており、事実上この操作を実行する権限がない。

### クライアント証明書によるアクセス

認可ユーザとしてリクエストを行うには、新しいユーザを作成するか、`kubernetes-admin` という管理者権限を持つ既存のデフォルトユーザを使用する必要がある。ここでは、新規ユーザの作成プロセスは説明しない。ユーザ作成の詳細については、[「ユーザ権限の制限](#ch03.html#restricting-user-permissions)」を参照のこと[。](#ch03.html#restricting-user-permissions)

以下のコマンドは、クライアント証明書と鍵を含め、利用可能なすべてのユーザをリストする：

```
$ kubectl config view --raw
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tL... 
    server: https://172.28.132.5:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin 
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tL... 
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktL... 
```

[![1](assets/1.png)](#ch03.html#co_cluster_hardening_CO1-1)
: 

Base64エンコードされた認証局の値。

[![2](assets/2.png)](#ch03.html#co_cluster_hardening_CO1-2)
: 

デフォルトで作成される管理者権限を持つユーザエントリ。

[![3](assets/3.png)](#ch03.html#co_cluster_hardening_CO1-3)
: 

ユーザのクライアント証明書をbase64エンコードした値。

[![4](assets/4.png)](#ch03.html#co_cluster_hardening_CO1-4)
: 

ユーザの秘密鍵をbase64エンコードした値。

ユーザ`kubernetes-admin` を使用して呼び出しを行うには、CA、クライアント証明書、秘密鍵のbase64エンコード値をbase64デコード値としてファイルに抽出する必要がある。以下のコマンドは、base64エンコードされた値をコピーし、`base64` ツールを使用して、ファイルに書き込む前にデコードする。CA値はファイル`ca` に、クライアント証明書値は`kubernetes-admin.crt` に、秘密鍵は`kubernetes-admin.key` に格納される：

```
$ echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tL... | base64 -d > ca
$ echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tL... | base64 -d > kubernetes-admin.crt
$ echo LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktL... | base64 - \
> kubernetes-admin.key
```

これで、関連するコマンドラインオプションを指定して、`curl` コマンドをこれらのファイルに向けることができる。APIサーバへのリクエストは、`kubernetes-admin` が適切なパーミッションを持っているため、適切に認証され、既存のすべてのネームスペースが返されるはずである：

```
$ curl --cacert ca --cert kubernetes-admin.crt --key kubernetes-admin.key \
  https://172.28.132.5:6443/api/v1/namespaces
{
  "kind": "NamespaceList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "2387"
  },
  "items": [
    ...
  ]
}
```

# APIサーバへのアクセスを制限する

APIサーバをインターネットに公開する場合、それが必要かどうか自問してみよう。一部のクラウドプロバイダは、APIサーバへのパブリックアクセスを制限または完全に無効にするプライベートクラスタを作成するオプションを提供している。詳細については、[EKSと](https://oreil.ly/W4Oma)[GKEの](https://oreil.ly/c7G-g)ドキュメントページを参照のこと。

オンプレミスのKubernetesクラスタを運用している場合は、APIサーバへのアクセスを防ぐファイアウォールルールをインスタンス化する必要がある。ファイアウォールルールのセットは試験範囲外なので、本書では説明しない。

## シナリオ攻撃者はインターネットからAPIサーバを呼び出すことができる。

クラウドプロバイダーはAPIサーバをインターネットに公開し、管理アクセスを簡素化することがある。攻撃者はクライアント証明書やベアラートークンの提供を拒否することで、APIサーバのエンドポイントに匿名でリクエストを試みることができる。攻撃者が幸運にもユーザ資格情報を取得できれば、認証された呼び出しが実行できる。ユーザに代入された演算子によって、悪意のある操作が実行される可能性がある。[図3-](#ch03.html#api-server-attacker)2は、攻撃者がインターネットからAPIサーバを呼び出している様子を示している。

![](assets/ckss_0302.png)

この章では、API サーバへのアクセスを制限する方法と、権限を制限した RBAC を実装する方法を、例を挙げて説明する。[「オープン・ポリシー・エージェント(OPA)とゲートウェイを理解する」では](#ch05.html#opa-gatekeeper)、OPA ゲートウェイを利用したアドミッション・コントロールについて説明する。

## ユーザ権限を制限する

Kubernetes APIへの呼び出しに`kubernetes-admin` ユーザの資格情報を使用できることを見てきた。このユーザ資格情報の使用は非常に控えめであるべきで、また資格情報を多くの人間と共有すべきではない。資格情報が悪人の手に渡ると、多くの損害が発生する可能性がある。このユーザはクラスタ管理を担当する人間専用に確保すること。

Kubernetesクラスタの他の関係者については、限定された権限セットを持つ専用ユーザを設定する必要がある。組織内でマッピングできる特定のロールがあるかもしれない。例えば、デプロイ、Pod、ConfigMaps、Secrets、Servicesの管理は許可するが、それ以外の管理は許可しない開発者ロールがあるかもしれない。新しいユーザを作成して関連するRBAC権限を代入するには、[Kubernetesのドキュメントを](https://oreil.ly/n8EMD)参照する。簡単に言うと、4つのステップがある：

1.  秘密鍵を作成する。
    
2.  CertificateSigningRequest を作成し、承認する。
    
3.  RoleとRoleBindingを作成する。
    
4.  ユーザを[kubeconfigファイルに](https://oreil.ly/OKs9g)追加する。
    

このプロセスについて詳しく説明するが、サービスアカウントに関するRBAC概念については、[「サービスアカウントの権限を最小化する](#ch03.html#minimizing-permissions-service-account)」で詳しく説明する[。](#ch03.html#minimizing-permissions-service-account)

### 秘密鍵の作成

 `openssl` 実行ファイルを使用して秘密鍵を作成する。 などの式でファイル名を指定する：`<username>.key`

```
$ openssl genrsa -out johndoe.key 2048
Generating RSA private key, 2048 bit long modulus
...+
......................................................................+
e is 65537 (0x10001)
```

証明書署名要求（CSR）を、拡張子`.csr` のファイルに作成する。前のステップで秘密鍵を提供する必要がある。以下のコマンドでは、"Common Name "値の入力を求められた場合、ユーザー名`johndoe` を使用する。その他の入力要求はすべて任意であり、必要に応じて入力できる：

```
$ openssl req -new -key johndoe.key -out johndoe.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:
State or Province Name (full name) []:
Locality Name (eg, city) []:
Organization Name (eg, company) []:
Organizational Unit Name (eg, section) []:
Common Name (eg, fully qualified host name) []:johndoe
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
```

CSRファイル・コンテンツのbase64エンコード値を以下のコマンドで取得する。次のステップでCertificateSigningRequestオブジェクトを作成する際に必要になる：

```
$ cat johndoe.csr | base64 | tr -d "\n"
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tL...
```

### CertificateSigningRequestの作成と承認

以下のスクリプトは、CertificateSigningRequestオブジェクトを作成する。[CertificateSigningRequest リソースは](https://oreil.ly/ltFbE)、指定された署名者による証明書の署名を要求するために使用される：

```
$ cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: johndoe
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tL...
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400
  usages:
  - client auth
EOF
certificatesigningrequest.certificates.k8s.io/johndoe created
```

属性`spec.signerName` の`kubernetes.io/kube-apiserver-client` の値は、APIサーバによってクライアント証明書として承認される証明書に署名する。前のステップでbase64エンコードされた値を使用し、それを属性`spec.request` の値として代入する。最後に、オプションの属性`spec.expirationSeconds` 、証明書の寿命を決定する。代入された値`86400` は、証明書の有効期限を1日にする。証明書の有効期間をどの程度にしたいかによって、有効期間を長くするか、単に属性を追加しないようにする。

CertificateSigningRequest オブジェクトの作成後、条件付きは "Pending" となる。24時間以内に署名要求を承認しないと、クラスタ内の不要なオブジェクトをガベージコレクションする手段として、オブジェクトは自動的に削除される：

```
$ kubectl get csr johndoe
NAME        AGE   SIGNERNAME                            REQUESTOR     \
  REQUESTEDDURATION   CONDITION
johndoe     6s    kubernetes.io/kube-apiserver-client   minikube-user \
  24h                 Pending
```

 `certificate approve` 、署名要求を承認する。その結果、条件付きは "Approved,Issued "に変わる：

```
$ kubectl certificate approve johndoe
certificatesigningrequest.certificates.k8s.io/johndoe approved
$ kubectl get csr johndoe
NAME        AGE   SIGNERNAME                            REQUESTOR     \
  REQUESTEDDURATION   CONDITION
johndoe     17s   kubernetes.io/kube-apiserver-client   minikube-user \
  24h                 Approved,Issued
```

最後に、承認されたCertificateSigningRequestオブジェクトから発行された証明書をエクスポートする：

```
$ kubectl get csr johndoe -o jsonpath={.status.certificate}| base64 \
  -d > johndoe.crt
```

### 役割とRoleBindingの作成

いよいよRBAC権限を代入する。このステップでは、ユーザのRoleとRoleBindingを作成する。Roleは、組織内の「アプリケーション開発者」の役割をモデル化する。開発者には、Podの取得、リスト表示、更新、削除のみが許可されるべきである。以下の命令コマンドでRoleオブジェクトを作成する：

```
$ kubectl create role developer --verb=create --verb=get --verb=list \
  --verb=update --verb=delete --resource=pods
role.rbac.authorization.k8s.io/developer created
```

次に、Roleを`johndoe` という名前のユーザにバインドする。そのためには、命令コマンド`create rolebinding` ：

```
$ kubectl create rolebinding developer-binding-johndoe --role=developer \
  --user=johndoe
rolebinding.rbac.authorization.k8s.io/developer-binding-johndoe created
```

### kubeconfigファイルにユーザを追加する

この最後のステップでは、kubeconfigファイルにユーザを追加し、ユーザ用のコンテキストを作成する必要がある。minikubeのインストールで試しているため、以下のコマンドではクラスタ名が`minikube` ：

```
$ kubectl config set-credentials johndoe --client-key=johndoe.key \
  --client-certificate=johndoe.crt --embed-certs=true
User "johndoe" set.
$ kubectl config set-context johndoe --cluster=minikube --user=johndoe
Context "johndoe" created.
```

### パーミッションを確認する

 `johndoe` という名前のユーザコンテキストに切り替える時だ：

```
$ kubectl config use-context johndoe
Switched to context "johndoe".
```

APIサーバへの呼び出しを行うクライアントとして`kubectl` 、その操作が許可されているはずであることを検証する。`default` ネームスペース内のすべてのPodをリストアップするAPI呼び出しは認証認可された：

```
$ kubectl get pods
No resources found in default namespace.
```

このコマンドの出力は、`default` ネームスペースには現時点ではPodオブジェクトが含まれていないが、呼び出しは成功したことを示している。否定的なケースもテストしてみよう。名前空間のリストはユーザにとって許可されていない演算子である。該当する`kubectl` コマンドを実行すると、エラーメッセージが返ってくる：

```
$ kubectl get namespaces
Error from server (Forbidden): namespaces is forbidden: User "johndoe" cannot \
list resource "namespaces" in API group "" at the cluster scope
```

権限の確認が終わったら、管理者権限を持つコンテキストに戻るといいだろう：

```
$ kubectl config use-context minikube
Switched to context "minikube".
```

## シナリオ攻撃者はサービスアカウントからAPIサーバを呼び出すことができる。

ユーザは、`kubectl` 実行ファイルまたは UI ダッシュボードを使用して Kubernetes クラスタと対話するのが一般的な実在の人物を表す。まれな条件付きではあるが、Podのコンテナ内で実行されるアプリケーションがKubernetes APIと対話する必要がある。そのような要件の典型的な例は、パッケージマネージャの[Helm](https://helm.sh)だ。Helmは、HelmチャートにバンドルされたYAMLマニフェストに基づいてKubernetesリソースを管理する。Kubernetesは、認証トークンを介してAPIサーバとHelmサービスプロセスを認証するためにサービスアカウントを使用する。このサービスアカウントはPodに代入でき、RBAC規則にマッピングできる。

Podへのアクセス権を得た攻撃者は、[図3-](#ch03.html#service-account-attacker)3に示すように、サービスアカウントを悪用してKubernetes APIへの呼び出しも行える可能性が高い。

![](assets/ckss_0303.png)

## サービスアカウントの権限を最小化する

アプリケーションを機能させるために本当に必要なサービスアカウントのみに権限を制限することが重要である。次のセクションでは、潜在的な攻撃対象領域を最小化するために、これを実現する方法を説明する。

このシナリオを機能させるには、ServiceAccountオブジェクトを作成し、それをPodに代入する必要がある。サービスアカウントはRBACと連携させることができ、RoleとRoleBindingを割り当てて、実行を許可する演算子を定義することができる。

### サービスアカウントをPodにバインドする

手始めに、Kubernetes APIを呼び出し、名前空間`k97` 、すべてのPodとデプロイメントをリストアップするPodをセットアップする。呼び出しは10秒ごとに無限ループの一部として行われる。API呼び出しからのレスポンスは、Podのログからアクセスできる標準出力に書き出される。

APIサーバに対して認証を行うために、Podが使用するサービスアカウントに関連付けられたベアラートークンを送信する。サービスアカウントのデフォルトの振る舞いは、API資格情報をパス`/var/run/secrets/kubernetes.io/serviceaccount/token` に自動マウントすることである。`cat` コマンドラインツールを使ってファイルの内容を取得し、HTTP リクエストのヘッダとして送信する。[例 3-1 では](#ch03.html#service-account-pod-setup)、名前空間、サービスアカウント、および Pod を 1 つの YAML マニフェストファイル`setup.yaml` で定義している。

##### 例 3-1. サービスアカウントを Pod に代入するための YAML マニフェスト

```
apiVersion: v1
kind: Namespace
metadata:
  name: k97
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sa-api
  namespace: k97
---
apiVersion: v1
kind: Pod
metadata:
  name: list-objects
  namespace: k97
spec:
  serviceAccountName: sa-api
  containers:
  - name: pods
    image: alpine/curl:3.14
    command: ['sh', '-c', 'while true; do curl -s -k -m 5 -H \
              "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/ \
              serviceaccount/token)" https://kubernetes.default.svc.cluster. \
              local/api/v1/namespaces/k97/pods; sleep 10; done']
  - name: deployments
    image: alpine/curl:3.14
    command: ['sh', '-c', 'while true; do curl -s -k -m 5 -H \
              "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/ \
              serviceaccount/token)" https://kubernetes.default.svc.cluster. \
              local/apis/apps/v1/namespaces/k97/deployments; sleep 10; done']
```

以下のコマンドでYAMLファイルからオブジェクトを作成する：

```
$ kubectl apply -f setup.yaml
namespace/k97 created
serviceaccount/sa-api created
pod/list-objects created
```

### デフォルトのパーミッションを確認する

 `list-objects` という名前のPodがAPIサーバに呼び出しを行い、専用コンテナ内のPodとデプロイのリストを取得する。コンテナ は Pods をリストする呼び出しを実行する。コンテナ は、デプロイメントのリストを取得するリクエストを API サーバに送信する。`pods` `deployments`

[Kubernetesのドキュメントで](https://oreil.ly/gBp30)説明されているように、デフォルトのRBACポリシーは、`kube-system` 名前空間外のサービスアカウントにいかなる権限も付与しない。コンテナ`pods` と`deployments` のログは、サービスアカウント`sa-api` がリソースをリストする権限がないことを示すエラーメッセージを返す：

```
$ kubectl logs list-objects -c pods -n k97
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "pods is forbidden: User \"system:serviceaccount:k97:sa-api\" \
              cannot list resource \"pods\" in API group \"\" in the \
              namespace \"k97\"",
  "reason": "Forbidden",
  "details": {
    "kind": "pods"
  },
  "code": 403
}
$ kubectl logs list-objects -c deployments -n k97
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "deployments.apps is forbidden: User \
              \"system:serviceaccount:k97:sa-api\" cannot list resource \
              \"deployments\" in API group \"apps\" in the namespace \
              \"k97\"",
  "reason": "Forbidden",
  "details": {
    "group": "apps",
    "kind": "deployments"
  },
  "code": 403
}
```

次は、必要な呼び出しを実行するために必要なAPI権限を持つClusterRoleとRoleBindingオブジェクトを立ち上げる。

### ClusterRoleの作成

[例3-](#ch03.html#list-pods-clusterrole)2に示す`list-pods-clusterrole` という名前の ClusterRole をファイル`clusterrole.yaml` で定義することから始める。規則のセットはPodリソースと動詞`list` を追加するだけである。

##### 例 3-2. PodをリストできるようにするClusterRoleのYAMLマニフェスト

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: list-pods-clusterrole
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list"]
```

対応するYAMLマニフェストファイルをポイントしてオブジェクトを作成する：

```
$ kubectl apply -f clusterrole.yaml
clusterrole.rbac.authorization.k8s.io/list-pods-clusterrole created
```

### RoleBindingの作成

[例3-3は](#ch03.html#rolebinding-service-account)RoleBindingのYAMLマニフェストをファイル`rolebinding.yaml` で定義している。RoleBinding は ClusterRole`list-pods-clusterrole` を`sa-pod-api` という名前のサービスアカウントにマッピングし、名前空間`k97` にのみ適用される。

##### 例 3-3. サービスアカウントにアタッチされた RoleBinding の YAML マニフェスト

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: serviceaccount-pod-rolebinding
  namespace: k97
subjects:
- kind: ServiceAccount
  name: sa-api
roleRef:
  kind: ClusterRole
  name: list-pods-clusterrole
  apiGroup: rbac.authorization.k8s.io
```

 `apply` 、RoleBindingオブジェクトを作成する：

```
$ kubectl apply -f rolebinding.yaml
rolebinding.rbac.authorization.k8s.io/serviceaccount-pod-rolebinding created
```

### 付与された権限を確認する

付与された`list` パーミッションにより、サービス・アカウントは`k97` ネームスペース内のすべてのPodを適切に取得できるようになった。以下の出力に示すように、`pods` コンテナ内の`curl` コマンドは成功する：

```
$ kubectl logs list-objects -c pods -n k97
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "628"
  },
  "items": [
      {
        "metadata": {
          "name": "list-objects",
          "namespace": "k97",
          ...
      }
  ]
}
```

サービスアカウントには、他のリソースに対するパーミッションは与えていない。`k97` ネームスペースのデプロイメントのリストはまだ失敗している。以下の出力は、`deployments` ネームスペースでの`curl` コマンドのレスポンスを示している：

```
$ kubectl logs list-objects -c deployments -n k97
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "deployments.apps is forbidden: User \
              \"system:serviceaccount:k97:sa-api\" cannot list resource \
              \"deployments\" in API group \"apps\" in the namespace \
              \"k97\"",
  "reason": "Forbidden",
  "details": {
    "group": "apps",
    "kind": "deployments"
  },
  "code": 403
}
```

ClusterRoleオブジェクトを自由に変更して、デプロイオブジェクトもリストできるようにする。

### サービスアカウントトークンの自動マウントを無効にする

前のセクションで説明したPodは、APIサーバーに対する認証手段としてサービス・アカウントのトークンを使用した。トークン・ファイルを`/var/run/secrets/kubernetes.io/serviceaccount/token` にマウントするのは、どのサービス・アカウントでも標準的な振る舞いだ。このファイルの内容が本当に必要になるのは、Podが実際にKubernetes APIとやりとりする場合だけだ。それ以外の場合、この振る舞いは、Podへのアクセスが攻撃者をトークンに直接導くことになるため、潜在的なセキュリティリスクをもたらす。

[例3-](#ch03.html#service-account-disable-automount)4に示すように、属性`automountServiceAccountToken` に値`false` を代入することで、サービスアカウントオブジェクトのオートマウントの振る舞いを無効にすることができる。

##### 例3-4. サービスアカウントのトークン自動マウントの振る舞いをオプトアウトする

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sa-api
  namespace: k97
automountServiceAccountToken: false
```

個々の Pod に対して自動マウントの振る舞いを無効にしたい場合は、Pod 定義で属性`spec.automountServiceAccountToken` を使用する。[例 3-5](#ch03.html#pod-serviceaccount-disable-automount)に、Pod の YAML マニフェストを示す。

##### 例 3-5. Pod内のサービスアカウントのトークン自動マウントを無効にする

```
apiVersion: v1
kind: Pod
metadata:
  name: list-objects
  namespace: k97
spec:
  serviceAccountName: sa-api
  automountServiceAccountToken: false
  ...
```

### サービスアカウントトークンを生成する

トークンの自動マウントを*無効に*するサービスアカウントを作成したい理由には、様々なユースケースがある。例えば、外部ツールや継続的デリバリ・パイプラインからKubernetes APIにアクセスし、既存のオブジェクトに関する情報をクエリする必要があるかもしれない。このようなシナリオでAPIサーバに対して認証を行う場合にも、トークンが必要となる。リストにあるシナリオは、必ずしも代入されたサービスアカウントでPodを実行するわけではなく、`curl` のようなツールからRESTful API呼び出しを実行するだけである。

トークンを手動で作成するには、`create token` コマンドを実行し、引数にサービスアカウント名を指定する。コマンドの出力がトークンを表示する：

```
$ kubectl create token sa-api
eyJhbGciOiJSUzI1NiIsImtpZCI6IjBtQkJzVWlsQjl...
```

トークンはパスワード・マネージャーなど安全な場所に保管する必要がある。トークンを紛失した場合、再度取り出すことはできない。同じコマンドでトークンを再作成することしかできないが、その場合、以前のトークンは自動的に無効になる。トークンを使用している参照はすべて変更しなければならない。

自動化されたプロセスでは、トークンの有効期限を設定しておくと便利である。`--duration` 、「タイム・トゥ・ライフ」が過ぎるとトークンは自動的に無効になる：

```
$ kubectl create token sa-api --duration 10m
eyJhbGciOiJSUzI1NiIsImtpZCI6IjBtQkJzVWlsQjl...
```

### サービスアカウントのシークレットを作成する

Kubernetes 1.24では、ServiceAccountオブジェクトがトークンを含む対応するSecretオブジェクトを自動的に作成しなくなった。詳細は[リリースノートを](https://oreil.ly/MSPuX)参照のこと。ServiceAccountオブジェクトをリストすると、Secretsの数が0になる。また、オブジェクトのYAML表現に`secrets` 属性が含まれなくなった：

```
$ kubectl get serviceaccount sa-api -n k97
NAME     SECRETS   AGE
sa-api   0         42m
```

[サービスアカウントトークンの作成](#ch03.html#generating-serviceaccount-token)」で説明されているように、`create token` コマンドを使ってトークンを生成するか、対応するシークレットを手動で作成することができる。[例 3-6](#ch03.html#serviceaccount-secret)はそのようなシークレットの YAML マニフェストを示している。

##### 例3-6. サービスアカウントのシークレットを手動で作成する

```
apiVersion: v1
kind: Secret
metadata:
  name: sa-api-secret
  namespace: k97
  annotations:
    kubernetes.io/service-account.name: sa-api
type: kubernetes.io/service-account-token
```

シークレットにサービスアカウントを代入するには、`kubernetes.io/service-account.name` というキーでアノテーションを追加する。以下のコマンドでSecretオブジェクトを作成する：

```
$ kubectl create -f secret.yaml
secret/sa-api-secret created
```

トークンは、シークレットオブジェクトを説明する際の "データ "セクションで発見することができる：

```
$ kubectl describe secret sa-api-secret -n k97
...
Data
====
ca.crt:     1111 bytes
namespace:  3 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IjBtQkJzVWlsQjl...
```

# Kubernetesを頻繁にアップデートする

特定のバージョンでKubernetesクラスタをインストールすることは、1回限りのFire and Forget演算子ではない。インストール時に最新のLTS（Long-Term Support）リリースを使用したとしても、クラスタにセキュリティ脆弱性がないことを保証するものではない。

時間が経つにつれて、セキュリティ関連のバグや弱点が発見されるだろう。この文には、基盤となるオペレーティングシステムやクラスタノードが実行する依存関係が含まれる。攻撃者は、一般に公開されている[CVE（Common Vulnerabilities and Exposures）データベースで](https://oreil.ly/FHhXD)セキュリティ脆弱性を簡単に調べ、それを悪用することができる。

## バージョニング・スキーム

すべてのノードでKubernetesのバージョンを定期的に更新するのはクラスタ管理者次第だ。Kubernetesは[セマンティック・バージョニング・スキームに従って](https://semver.org)いる。セマンティックバージョンは、メジャーバージョン、マイナーバージョン、パッチバージョンで構成される。例えば、Kubernetesバージョン1.24.3の場合、メジャーバージョンは1、マイナーバージョンは24、パッチバージョンは3となる。

バージョンの各部分は特定の意味を持つ。メジャーバージョン部分の変更は破壊的変更を意味する。マイナーバージョン部分のインクリメントは、新しい関数が後方互換性をもって追加されたことを意味する。パッチバージョン部分は、単にバグを修正したものである。

# Kubernetesのマイナーバージョンアップに伴う破壊的変更

Kubernetesが常にセマンティック・バージョニングの厳密な解釈に固執するわけではないことに触れておくことは重要だ。例えば、PodSecurityPolicy（PSP）アドミッションコントローラーは、[バージョン1.25.](https://oreil.ly/JE-i8)0でPod Security Admission概念に置き換えられた。従来であれば、これらの変更はメジャーバージョンの更新でのみ発生するはずだ。API、フラグ、または機能がどのように段階的に廃止されるかをよりよく理解するために、[Kubernetesの非推奨](https://oreil.ly/on9lu)ポリシーを参照すること。

## リリース・ケイデンス

Kubernetesの[新しいマイナーバージョンリリースは](https://oreil.ly/LGIn5)3カ月ごとに行われる。リリースには新機能や追加のバグ修正が含まれることがある。Kubernetesの最新リリースでは必要に応じてセキュリティ修正が実装される可能性があり、その前の2つのマイナーリリースにバックポートされる。自分のクラスタのために常に最新リリースへのアップデートを続けるには、かなりの人的パワーが必要だ。それに応じて、それらの活動のための時間を確保する必要がある。

## アップグレードプロセスを実行する

マイナーバージョンから次の上位バージョンへ（例えば1.23から1.24へ）、またはパッチバージョンからより新しいバージョンへ（例えば1.24.1から1.24.3へ）アップグレードすることが推奨される。予期せぬ副作用を避けるため、複数のマイナーバージョンをジャンプアップすることは控えること。

[アップグレード手順の](https://oreil.ly/RxC9j)詳細は、Kubernetesの公式ドキュメントで発見できる。[図3-4は](#ch03.html#cluster_version_upgrade_process)、アップグレードのプロセスを高いレベルで示している。

クラスタのバージョンアッププロセスは、すでにCKA試験の一部である。前提条件としてCKAに合格する必要があることを考えると、このプロセスを実行する方法はすでに知っていると考えられる。詳しい説明は、[*Certified Kubernetes Administrator (CKA) Study Guideを*](https://oreil.ly/cka-study-guide)参照されたい。

![](assets/ckss_0304.png)

# 概要

ユーザ、クライアントアプリケーション（`kubectl` や`curl` など）、サービスアカウントを使用するPod、クラスタコンポーネントはすべて、オブジェクトを管理するためにAPIサーバと通信する。悪意のあるアクセスを防ぐために、APIサーバをセキュアにすることが最も重要である。

攻撃対象領域を最小化するために、ファイアウォールルールを使用してAPIサーバをインターネットに公開しないようにする。すべてのユーザorサービスアカウントに対して、RBAC規則を使用してKubernetes APIに対する演算実行権限を必要最低限に制限する。パーミッションが最小化されていれば、攻撃者が資格情報にアクセスできたとしても、被害ははるかに少なくなる。

Kubernetesクラスタのバージョンを必ずアップグレードすること。バグやセキュリティの修正を取り入れることで、攻撃者が利用できる不要な脆弱性をさらすリスクを減らすことができる。

# 試験の要点

Kubernetes APIとのやりとりを練習する。
: 

この章では、Kubernetes APIと通信するさまざまな方法を示した。ユーザコンテキストに切り替えたり、`curl` を使用した RESTful API 呼び出しの助けを借りて API リクエストを実行した。APIサーバのエンドポイントを決定する方法と、クライアント資格情報やベアラートークンなどの異なる認証メソッドを使用する方法を理解する必要がある。Kubernetes APIとそのエンドポイントを各自で探索し、より広い範囲に触れる。

ユーザとサービスアカウントのRBAC規則を定義することの意味を理解する。
: 

匿名ユーザによるKubernetes APIへのリクエストは、実質的な演算子を許可しない。ユーザやサービスアカウントからのリクエストについては、対象者に付与されたパーミッションを注意深く分析する必要がある。パーミッションを制御するための関連オブジェクトを作成することで、RBAC規則を定義する裏技を学ぼう。サービスアカウントは、Podで使用されるとトークンを自動マウントする。PodからAPI呼び出しを行う場合のみ、トークンをVolumeとして公開する。

Kubernetesのリリース・ケイデンスとクラスタのアップグレードの必要性を認識しておく。
: 

Kubernetesクラスタは、セキュリティ上の理由から長期にわたって手入れをする必要がある。攻撃者は、古いKubernetesバージョンの既知の脆弱性を利用しようとするかもしれない。バージョンのアップグレードプロセスはすべての管理者の仕事の一部であり、無視すべきではない。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1.  グループ`observer` のユーザ`jill` のクライアント証明書と鍵を作成する。adminコンテキストで、ユーザ`jill` のコンテキストを作成する。
    
2.  グループ（ユーザではない！）に対して、`default` 名前空間に Role と RoleBinding を定義し、リソース Pods、ConfigMaps、Secrets に対して、`get` 、`list` 、`watch` という動詞を許可する。オブジェクトを作成する。
    
3.  ユーザコンテキストに切り替え、許可された演算子の1つを許可する`kubectl` 、許可すべきでない`kubectl` コマンドを1つ実行する。`admin` 。
    
4.  名前空間`t23` に`service-list` という名前のPodを作成する。コンテナはイメージ`alpine/curl:3.14` を使用し、`default` 名前空間のサービスオブジェクトを無限ループでリスト作成する Kubernetes API への`curl` 呼び出しを行う。サービスアカウント`api-call` を作成し、アタッチする。Podが開始された後、コンテナのログを検査する。`curl` コマンドからどのようなレスポンスが期待できるか。
    
5.  Podが必要とする演算子のみを許可するClusterRoleとRoleBindingをサービスアカウントに代入する。`curl` コマンドのレスポンスを見てみよう。
    
6.  サービス・アカウント・トークンの自動マウントが無効になるようにPodを設定する。トークンの値を取得し、`curl` コマンドで直接使用する。`curl` コマンドがまだ演算子を認可できることを確認する。
    
7.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ)ディレクトリ*a/ch03/upgrade-versionに*移動する。`vagrant up` コマンドを使用して、クラスタを実行している仮想マシンを起動する。クラスタの全ノードをKubernetes 1.25.6から1.26.1にアップグレードする。このクラスタは、`kube-control-plane` という単一のコントロールプレーンノードと、`kube-worker-1` という単一のワーカーノードで構成されている。完了したら、`vagrant destroy -f` を使用してクラスタをシャットダウンする。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    

# 第4章. システム・ハードニング

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

システムハードニング」の領域では、Kubernetesクラスタノードを実行する基盤となるホストシステムに関連するセキュリティの側面を扱う。ここで取り上げる関数は、基本的にLinuxのコア機能であるテクニックや設定オプションに触れている。これには、サービスの無効化とパッケージの削除、ユーザとグループの管理、ポートの無効化、ファイアウォールルールの設定などが含まれる。最後に、この章では、コンテナ内で動作するプロセスがホストレベルで実行できる演算子を制限できるLinuxカーネルハードニングツールについて説明する。

この章では、高いレベルで以下の概念をカバーする：

*   ホストOSのフットプリントを最小限に抑える
    
*   IAMの役割を最小限にする
    
*   ネットワークへの外部アクセスを最小限に抑える
    
*   AppArmorやseccompのようなカーネルハードニングツールを使用する。
    

# ホストOSのフットプリントを最小限に抑える

クラスタノードは物理マシンまたは仮想マシン上で動作する。ほとんどの場合、これらのマシン上の演算子はLinuxディストリビューションである。明らかに、オペレーティングシステムはセキュリティの脆弱性をさらす可能性がある。

時間をかけて、オペレーティング・システムのバージョンを最新のセキュリティ修正に対応させる必要がある。このプロセスには、例えばノードのオペレーティング・システムをUbuntu 18から22にアップグレードすることが含まれる。オペレーティングシステムのアップグレードは本書の範囲外である。詳細については、関連するLinuxのドキュメントを確認してほしい。

Ubuntuなど多くのLinuxディストリビューションには、Kubernetesクラスタの運用には必ずしも必要ではない追加のツール、アプリケーション、サービスが付属している。セキュリティリスクを特定し、脆弱性を暴露する可能性のあるオペレーティングシステム固有の機能を無効化または削除し、最新のセキュリティ修正を組み込むためにオペレーティングシステムにパッチを適用し続けることは、演算子としてのあなたの仕事だ。オペレーティングシステムの機能が少なければ少ないほど、リスクは小さくなる。

# Ubuntu Linux用CISベンチマーク

リファレンス・ガイドとして、[Ubuntu LinuxのCISベンチマークと](https://oreil.ly/AeAAE)あなたのオペレーティング・システムの構成を比較するとよいだろう。

## シナリオ攻撃者がパッケージの脆弱性を悪用する

[図4-](#ch04.html#os-package-attacker)1は、攻撃者がシステムにインストールされたパッケージの脆弱性を悪用する様子を示している。例えば、アプリケーションはパッケージマネージャの[snapd](https://oreil.ly/ZOFTj) である。この攻撃者は、機密情報を攻撃者に暴露する可能性のある既知の脆弱性[USN-5292-1](https://oreil.ly/lw_MV)を利用していると仮定する 。

![](assets/ckss_0401.png)

以下では、Kubernetesの演算子としてあまり必要のないサービスやパッケージについて、無効化or削除するだけでセキュリティリスクを最小化する方法を説明する。

## サービスを無効にする

Linuxでは、多くのアプリケーションがバックグラウンドでサービスとして実行される。サービスは、コマンドラインツール`systemctl` を使って管理することができる。次の`systemctl` コマンドは、実行中のサービスをすべてリストアップする：

```
$ systemctl | grep running
...
snapd.service   loaded active running   Snap Daemon
```

クラスタ・ノードの運用に不要なサービスの1つに、パッケージ・マネージャのsnapdがある。このサービスの詳細については、`status` サブコマンドを使用してステータスを取得する：

```
$ systemctl status snapd
● snapd.service - Snap Daemon
     Loaded: loaded (/lib/systemd/system/snapd.service; enabled; vendor \
     preset: enabled)
     Active: active (running) since Mon 2022-09-19 22:49:56 UTC; 30min ago
TriggeredBy: ● snapd.socket
   Main PID: 704 (snapd)
      Tasks: 12 (limit: 2339)
     Memory: 45.9M
     CGroup: /system.slice/snapd.service
             └─704 /usr/lib/snapd/snapd
```

`systemctl` サブコマンド`stop` を使ってサービスを停止することができる：

```
$ sudo systemctl stop snapd
Warning: Stopping snapd.service, but it can still be activated by:
  snapd.socket
```

 `disable` サブコマンドを実行し、システム再起動時にサービスが再び開始されないようにする：

```
$ sudo systemctl disable snapd
Removed /etc/systemd/system/multi-user.target.wants/snapd.service.
```

このサービスは現在停止され、無効になっている：

```
$ systemctl status snapd
● snapd.service - Snap Daemon
     Loaded: loaded (/lib/systemd/system/snapd.service; disabled; vendor \
     preset: enabled)
     Active: inactive (dead) since Mon 2022-09-19 23:22:22 UTC; 4min 4s ago
TriggeredBy: ● snapd.socket
   Main PID: 704 (code=exited, status=0/SUCCESS)
```

## 不要なパッケージを削除する

サービスが無効になった今、パッケージを残しておく意味はもうない。パッケージを削除することで、追加のディスクスペースとメモリーを確保することができる。`apt purge` コマンドを使って、パッケージとその関連パッケージを削除することができる：

```
$ sudo apt purge --auto-remove snapd
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages will be REMOVED:
  snapd* squashfs-tools*
0 upgraded, 0 newly installed, 2 to remove and 116 not upgraded.
After this operation, 147 MB disk space will be freed.
Do you want to continue? [Y/n] y
...
```

パッケージがサービスによって管理されていなくても、同じコマンドを使うことができる。不要なパッケージを特定し、単純に削除する。システムのフットプリントはずっとスリムになるはずだ。

潜在的な攻撃者は、システムを悪用するためにsnapdサービスを使用できなくなる。不要なサービスについては、このプロセスを繰り返す。その結果、snapdサービスはシステム上に存在しなくなる：

```
$ systemctl status snapd
Unit snapd.service could not be found.
```

# IAMロールを最小限にする

システム・レベルでのアイデンティティおよびアクセス管理（IAM）には、Linuxユーザ、所属グループ、およびユーザに付与されたパーミッションの管理が含まれる。どのようなディレクトリやファイルにも、ユーザに対してファイル・パーミッションが代入される。

ユーザとアクセスの適切な管理は、すべてのシステム管理者の古典的な責任である。Kubernetes管理者としてのあなたの役割は、システムレベルのIAMには直接関わらないかもしれないが、セキュリティへの影響を理解することは重要だ。あなたはおそらく、Kubernetesクラスタを実行しているシステムを堅牢化するために、ピアと協力しなければならないだろう。

このセクションでは、ユーザとグループの管理方法について簡単に紹介する。また、アクセスをできるだけ少なくするために、ファイルのパーミッションと所有権をどのようにセットするかについても説明する。本書では、このトピックの表面をかすめるにすぎない。より詳細な情報については、お好きなLinuxのドキュメントを参照されたい。

## シナリオ攻撃者が資格情報を使ってファイルにアクセスする

セキュリティ侵害は、ユーザ資格情報の盗難につながる可能性がある。有効なユーザ資格情報へのアクセスを得ることで、さらなる攻撃ベクトルへの扉が開かれる。[図4-](#ch04.html#iam-attacker)2は、盗んだユーザ資格情報でクラスタ・ノードにログインし、ユーザに付与されたパーミッションですべてのファイルとディレクトリを対話できるようになった攻撃者を示している。

![](assets/ckss_0402.png)

最小権限の原則に従うことが推奨される。限られたグループのユーザにのみ管理者権限を与える。それ以外のユーザには、自分の仕事を遂行するのに必要な演算子だけを許可する。

## ユーザ管理を理解する

すべてのユーザは、システムを利用するために認証を受けなければならない。認証されたユーザは、代入された権限に基づいてリソースにアクセスできる。このセクションでは、ユーザを管理するために必要な主な演算子について説明する。

### リストユーザ

システム上の全ユーザをリストアップするには、ファイル`/etc/passwd` の内容をレンダリングする。すべてのエントリは、一般的なパターン`username:password:UID:GID:com⁠ment:​home:shell` に従っている。パターン内のフィールドのいくつかは空かもしれない：

```
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
...
```

コマンド出力は、出力の最初の位置にユーザrootを表示する。`root` ユーザーの文字列の最後の部分、`/bin/bash` は、そのユーザーが`bash` シェルでシステムにログインすることを許可されていることを示す。他のユーザは、ログインがまったく許可されていないかもしれない。そのようなユーザの場合、`shell` フィールドに`/usr/sbin/nologin` という文字列が代入されていることを発見する。

任意の時点で、どのプロセスがユーザによって起動されたかを見ることができる。次のコマンドは、`bash` の全プロセスを、それを起動したユーザーを含めて表示する：

```
$ ps aux | grep bash
root         956  0.0  0.4  22512 19200 pts/0    Ss   17:57   0:00 -bash
root        7064  0.0  0.0   6608  2296 pts/0    S+   18:08   0:00 grep \
--color=auto bash
```

### ユーザを追加する

ある時点で、クラスタ・ノードが稼動しているマシンへのアクセス権を制限付きでチーム・メンバーに与えたいと思うかもしれない。`adduser` コマンドで新しいユーザをシステムに追加できる。フラグ`--shell /sbin/nologin` を追加して、ユーザのシェル・アクセスを無効にする。次のコマンドはユーザ`ben` を作成する：

```
$ sudo adduser ben
Adding user ‘ben’ ...
Adding new group ‘ben’ (1001) ...
Adding new user ‘ben’ (1001) with group ‘ben’ ...
Creating home directory ‘/home/ben’ ...
Copying files from ‘/etc/skel’ ...
New password:
Retype new password:
...
```

ユーザエントリがファイル`/etc/passwd` に追加された：

```
$ cat /etc/passwd
...
ben:x:1001:1001:,,,:/home/ben:/bin/bash
```

### ユーザを切り替える

シェルのユーザを変更するには、`su` コマンドを使う。次のコマンドは、先ほど作成したユーザ`ben` に切り替える。ユーザのパスワードの入力を求められる：

```
$ su ben
Password:
ben@controlplane:/root$ pwd
/root
```

シェルはプロンプトで現在のユーザを示す。`su` コマンドを実行したときに使用したアカウントの環境変数を継承する。新しい環境を作成するには、`su` コマンドでハイフンを追加する：

```
$ su - ben
ben@controlplane:~$ pwd
/home/ben
```

ユーザを一時的に切り替えるもう一つの方法は、`sudo` コマンドを使うことである。このコマンドを実行するには、昇格権限が必要である。従って、`sudo` コマンドは「管理者としてこのコマンドを実行する」と等価性である：

```
$ sudo -u ben pwd
/root
```

### ユーザを削除する

システム内のユーザに代表されるチームメンバーは、他のチームに移ったり、単に会社を去ったりする。資格情報の不正使用を防ぐために、ユーザへのアクセス権を失効させたい。以下のコマンドは、ユーザのホーム・ディレクトリを含め、ユーザを削除する：

```
$ sudo userdel -r ben
```

## グループ経営を理解する

システム管理者にとっては、同様のアクセス要件を持つユーザをグループ化し、個々のユーザ・レベルでパーミッションをコントロールする方が便利である。Linuxシステムは、チームや特定の組織的役割に基づいてユーザを編成する方法として、グループという概念を提供している。ここでは、グループ管理の最も重要な側面について簡単に触れる。

### リストグループ

グループのリストは、ファイル`/etc/group` の中身を見ることで確認できる。どのエントリも一般的なパターン`groupname:password:GID:group members` に従っている：

```
$ cat /etc/group
root:x:0:
plugdev:x:46:packer
nogroup:x:65534:
...
```

出力でわかるように、いくつかのフィールドは空かもしれない。代入メンバーがいる唯一のグループは、`plugdev` で、その名前は`packer` である。

### グループを追加する

新しいグループを追加するには、`groupadd` というコマンドを使う。以下の例では、グループ`kube-developers` を追加している：

```
$ sudo groupadd kube-developers
```

これでグループは、ファイル`/etc/group` にリストされる。グループ識別子は1004である：

```
$ cat /etc/group
...
kube-developers:x:1004:
```

### ユーザをグループに代入する

ユーザにグループを代入するには、`usermod` コマンドを使用する。以下のコマンドは、ユーザ`ben` をグループ`kube-developers` に追加する：

```
$ sudo usermod -g kube-developers ben
```

グループ識別子1004は、グループ`kube-developers` の代用として機能する：

```
$ cat /etc/passwd | grep ben
ben:x:1001:1004:,,,:/home/ben:/bin/bash
```

### グループを削除する

グループを完全に削除したい場合もある。もしかしたら、もう存在しないLinuxグループを参照している組織ロールがあるかもしれない。グループを削除するには、`groupdel` コマンドを使用する。メンバーがまだグループに属している場合は、エラーメッセージを受信する：

```
$ sudo groupdel kube-developers
groupdel: cannot remove the primary group of user ben
```

グループを削除する前に、`usermod` コマンドを使用して、グループメンバーを別のグループに再割り当てする必要がある。以下のコマンドは、グループを`kube-developers` から`kube-admins` に変更する。グループ`kube-admins` は以前に作成されていると仮定する：

```
$ sudo usermod -g kube-admins ben
$ sudo groupdel kube-developers
```

## ファイルのパーミッションと所有権を理解する

可能な限り最小限のアクセスでファイル・パーミッションを代入することは、セキュリティを最大化する上で極めて重要である。ここでLinuxのファイル・パーミッションとオーナーシップが登場する。ここでは、関連する演算子について高いレベルで述べるにとどめる。詳しくは[Linux FoundationのLinuxファイル・パーミッションに関するブログ記事を](https://oreil.ly/3IpRT)参照されたい。

### ファイルのパーミッションと所有権を表示する

ユーザは誰でも新しいディレクトリやファイルを作成できる。例えば、`touch` コマンドを使って空のファイルを作成することができる。次のコマンドは、カレント・ディレクトリに`my-file` という名前のファイルを作成する：

```
$ touch my-file
```

ディレクトリの内容を "long "形式で見るには、`ls` コマンドを使用する。`-l` コマンドラインパラメーターで要求される長い形式の出力は、ファイルのパラメータとファイルの所有者を表示する：

```
$ ls -l
total 0
-rw-r--r-- 1 root root 0 Sep 26 17:53 my-file
```

出力の重要な部分は`-rw-r--r--` である。最初の文字は、システムごとに異なる特殊化許可文字であり、その後に`rwx` という表記の3つのグループ分けが続く。最初の3文字は所有者のパーミッションを表し、2つ目の3文字のセットはグループのパーミッションを表し、最後の3文字は全ユーザのパーミッションを表す。記号`r` は読み取りパーミッション、`w` は書き込みパーミッション、`x` は実行パーミッションを意味する。前の例では、ユーザー`root` はファイルの読み書きができるが、グループと他のすべてのユーザーはファイルを読むことしかできない。

### ファイルの所有者を変更する

ファイルやディレクトリのユーザとグループの代入を変更するには、`chown` コマンドを使用する。コマンドの構文は`chown owner:group filename` のパターンに従う。次のコマンドは、ファイルの所有者をユーザ`ben` に変更するが、グループの再割り当ては行わない。`chown` コマンドを実行するユーザには書き込み権限が必要である：

```
$ chown ben my-file
$ ls -l
total 0
-rw-r--r-- 1 ben  root 0 Sep 26 17:53 my-file
```

### ファイルのパーミッションを変更する

 `chmod` 、さまざまな表記でパーミッションの追加や削除ができる。例えば、ファイル所有者の書き込みパーミッションを削除するには、次のコマンドを使う：

```
$ chmod -w file1
$ ls -l
total 0
-r--r--r-- 1 ben  root 0 Sep 26 17:53 my-file
```

# ネットワークへの外部アクセスを最小限にする

クラスタノードへの外部からのアクセスは、Kubernetesの演算に必要なポートに対してのみ許可する必要がある。Kubernetesの標準ポートについては[「ノードのメタデータとエンドポイントの保護](#ch02.html#node-metadata-endpoints)」ですでに説明した。それ以外のポートへのアクセスはブロックする必要がある。

## 開いているポートを特定し、無効にする

FTPサーバ、ウェブサーバ、Sambaのようなファイルやプリントサービスのようなアプリケーショ ンは、クライアントに通信エンドポイントを公開する手段としてポートを開いている。ネットワーク通信をオープンにするアプリケーションを実行すると、セキュリティリスクにさらされる可能性がある。サービスを無効にし、アプリケーションをデインストールするだけで、リスクを排除できる。

例えば、以下のコマンドでコントロールプレーンノードに[Apache 2 HTTPウェブサーバーをインストール](https://oreil.ly/t-np3)したとしよう：

```
$ sudo apt update
$ sudo apt install apache2
```

# netstatコマンドについて更新

 `netstat` 」コマンドは、より高速で人間にとって読みやすい「 」コマンドに取って代わられ、廃止された。詳細については、使用しているオペレーティングシステムのドキュメントを参照のこと。`ss`

`netstat` と同様の機能を持つユーティリティである、コマンドラインツール`ss` を使って、すべてのオープンポートを検査することができる。 以下のコマンドは、プロセスも含めて、すべてのオープンポートを表示する。その中に、Apache 2が公開しているポート80がある：

```
$ sudo ss -ltpn
State    Recv-Q   Send-Q   Local Address:Port   Peer Address:Port   Process
...
LISTEN   0        511      *:80                 *:*                 users: \
(("apache2",pid=18435,fd=4),("apache2",pid=18434,fd=4),("apache2", ]\
pid=18432,fd=4))
```

一時的にウェブサーバが必要になっただけで、インストールするのを忘れてしまっただけかもしれない。このプロセスは現在、サーバによって管理されている。サービスの状態は、`systemctl status` コマンドで確認できる：

```
$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor \
     preset: enabled)
     Active: active (running) since Tue 2022-09-20 22:25:25 UTC; 39s ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 18432 (apache2)
      Tasks: 55 (limit: 2339)
     Memory: 5.6M
     CGroup: /system.slice/apache2.service
             ├─18432 /usr/sbin/apache2 -k start
             ├─18434 /usr/sbin/apache2 -k start
             └─18435 /usr/sbin/apache2 -k start
```

Apache 2はKubernetesには必要ない。サービスを停止し、パッケージをデインストールすることにした：

```
$ sudo systemctl stop apache2
$ sudo systemctl disable apache2
Synchronizing state of apache2.service with SysV service script with \
/lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install disable apache2
Removed /etc/systemd/system/multi-user.target.wants/apache2.service.
$ sudo apt purge --auto-remove apache2
```

そのポートがもう使われていないことを確認する。`ss` 、ポート80を公開しているアプリケーションが発見されない：

```
$ sudo ss -ltpn | grep :80
```

## ファイアウォールルールのセットアップ

ポートを制御するもう1つの方法は、演算子レベルのファイアウォールの助けを借りることだ。Linuxでは、[Uncomplicated Firewall（UFW](https://oreil.ly/iqiwv)）を使うことができる。このセクションでは、UFWを有効にする方法とファイアウォールの規則を設定する方法について、ごく簡単に紹介する。

最小権限の原則に従い、まずファイアウォールを有効にして*、*送受信されるネットワーク・トラフィックの拒否規則を設定するのがよいだろう。以下のコマンドは、そのための手順を示している：

```
$ sudo ufw allow ssh
Rules updated
Rules updated (v6)
$ sudo ufw default deny outgoing
Default outgoing policy changed to deny
(be sure to update your rules accordingly)
$ sudo ufw default deny incoming
Default incoming policy changed to deny
(be sure to update your rules accordingly)
$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```

`kubectl` のような外部ツールが、ポート 6443 で実行されている API サーバーに接続できるようにしたい。コントロールプレーンノードで以下のコマンドを実行し、APIサーバーポートへのアクセスを許可する：

```
$ sudo ufw allow 6443
Rule added
Rule added (v6)
```

コントロールプレーンとワーカーノードの他のポートを開放するために、同じプロセスを繰り返す必要がある。Kubernetesの演算子に必要のない他のポートがすべてブロックされていることを確認する。

# カーネルハードニングツールを使う

コンテナ内で実行されるアプリケーションやプロセスは、システム呼び出しを行うことができる。典型的な例は、HTTPリクエストを実行する`curl` コマンドである。システム呼び出しは、カーネルにサービスを要求するための、ユーザ空間で実行されるプログラム抽象化である。カーネルハードニングツールの助けを借りて、どのシステムコールの実行を許可するかを制限することができる。CKS試験では、この分野でAppArmorとseccompという2つのツールについて明確に言及している。この2つのツールと、それらをKubernetesと統合する仕組みについて説明する。

## AppArmorを使用する

[AppArmorは](https://apparmor.net)、Linuxシステム上で動作するプログラミングへのアクセス制御を提供する。このツールは、ユーザ空間で呼び出されるアプリケーションと、基盤となるシステム機能との間に、追加のセキュリティ・レイヤーを実装する。例えば、ネットワーク呼び出しやファイルシステムとのやり取りを制限することができる。多くのLinuxディストリビューション（Debian、Ubuntu、openSUSEなど）には、すでにAppArmorが同梱されている。そのため、AppArmorを手動でインストールする必要はない。AppArmorをサポートしていないLinuxディストリビューションは、代わりに[Security-EnhancedLinux（SELinux](https://oreil.ly/CKBr7)）を使用し、AppArmorと同様のアプローチを取る。SELinuxを理解することは、CKS試験の範囲外である。

### プロファイルを理解する

プログラムができること、またはできないことを定義する規則は、AppArmorプロファイルで定義される。すべてのプロファイルは、有効にする前にAppArmorにロードする必要がある。AppArmorには、ロードされたプロファイルを確認するためのコマンドラインツールが用意されている。コマンド`aa-status` を実行すると、ロードされたすべてのプロファイルの概要が表示される。AppArmorには、Linuxサービスを保護するためのデフォルト・アプリケーション・プロファイルのセットがすでに用意されていることがわかる：

```
$ sudo aa-status
apparmor module is loaded.
31 profiles are loaded.
31 profiles are in enforce mode.
   /snap/snapd/15177/usr/lib/snapd/snap-confine
   ...
0 profiles are in complain mode.
14 processes have profiles defined.
14 processes are in enforce mode.
   /pause (11934) docker-default
   ...
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
```

プロファイル・モードは、一致するイベントが発生した場合の、実行時の規則の処理を決定する。AppArmorは2種類のプロファイルモードを区別する：

強制する
: 

システムは規則を実行し、違反を報告し、それをsyslogに書き込む。このモードは、プログラムが特定の呼び出しを行えないようにするために使用する。

苦情を言う
: 

システムは規則を強制しないが、違反はログに書き込む。このモードは、プログラムが行う呼び出しを発見したい場合に役立つ。

[例4-1は](#ch04.html#apparmor-profile)、ファイルの書き込みアクセスを制限するためのカスタムプロファイルをファイル`k8s-deny-write` で定義している。このファイルは、ワークロードを実行するすべてのワーカーノードのディレクトリ`/etc/apparmor.d` に置かれるべきである。すべての規則を詳細に説明することは、本書の範囲外である。詳細については、[AppArmor wikiを](https://oreil.ly/mNuWB)参照のこと。

##### 例 4-1. ファイル書き込みアクセスを制限するためのAppArmorプロファイル

```
#include <tunables/global>

profile k8s-deny-write flags=(attach_disconnected) { 
  #include <abstractions/base>

  file, 

  deny /** w, 
}
```

[![1](assets/1.png)](#ch04.html#co_system_hardening_CO1-1)
: 

`profile` キーワードの後の識別子は、プロファイルの名前である。

[![2](assets/2.png)](#ch04.html#co_system_hardening_CO1-2)
: 

ファイル演算子に適用する。

[![3](assets/3.png)](#ch04.html#co_system_hardening_CO1-3)
: 

すべてのファイルの書き込みを拒否する。

### カスタムプロファイルをセットする

プロファイルをAppArmorにロードするには、ワーカー・ノードで以下のコマンドを実行する：

```
$ sudo apparmor_parser /etc/apparmor.d/k8s-deny-write
```

コマンドはデフォルトでenforceモードを使用する。不満モードでプロファイルをロードするには、`-C` オプションを使用する。`aa-status` コマンドは、デフォルトのプロファイルに加えてプロファイルをリストする。出力でわかるように、プロファイルはenforceモードでリストされる：

```
$ sudo aa-status
apparmor module is loaded.
32 profiles are loaded.
32 profiles are in enforce mode.
   k8s-deny-write
   ...
```

AppArmorは、ユーティリティ・パッケージの一部として、追加の便利コマンドをサポートしている。これらを使用したい場合は、以下のコマンドを使用してパッケージを手動でインストールすることができる：

```
$ sudo apt-get update
$ sudo apt-get install apparmor-utils
```

一度インストールすれば、`aa-enforce` というコマンドで強制モードでプロファイルをロードすることができ、`aa-complain` というコマンドで文句モードでプロファイルをロードすることができる。試験では、標準の`apparmor_parser` 。

### コンテナにプロファイルを適用する

Pod定義でAppArmor規則を使用する前に、いくつかの前提条件を確認する必要がある。まず、規則を有効にするには、コンテナ・ランタイムがAppArmorをサポートしている必要がある。さらに、Podを実行するワーカー・ノードにAppArmorがインストールされている必要がある。最後に、前のセクションで説明したように、プロファイルをロードしたことを確認する。

[例 4-2](#ch04.html#apparmor-pod)は、ファイル`pod.yaml` で定義された Pod の YAML マニフェストを示している。コンテナにプロファイルを適用するには、特定の注釈をセットする必要がある。注釈キーは`container.apparmor.security.beta.​kuber⁠netes.io/<container-name>` という形式のキーを使用する必要がある。この場合、コンテナ名は`hello` である。完全なキーは`container.apparmor.security.beta.kubernetes.io/hello` である。注釈の値は`localhost/<profile-name>` というパターンに従う。ここで使用したいカスタムプロファイルは`k8s-deny-write` である。設定オプションの詳細については、[Kubernetesのドキュメントを](https://oreil.ly/1o3zO)参照のこと。

##### 例 4-2. コンテナにAppArmorプロファイルを適用するPod

```
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: \ 
    localhost/k8s-deny-write 
spec:
  containers:
  - name: hello 
    image: busybox:1.28
    command: ["sh", "-c", "echo 'Hello AppArmor!' && sleep 1h"]
```

[![1](assets/1.png)](#ch04.html#co_system_hardening_CO2-1)
: 

ハードコードされたプレフィックスと、スラッシュ文字で区切られたコンテナ名からなる注釈キー。

[![2](assets/2.png)](#ch04.html#co_system_hardening_CO2-2)
: 

`localhost` で示される、現在のノードで利用可能なプロファイル名。

[![3](assets/3.png)](#ch04.html#co_system_hardening_CO2-3)
: 

コンテナ名。

Podを作成する準備ができた。`apply` コマンドを実行し、YAML マニフェストをポイントする。Podが「Running」ステータスに遷移するまで待つ：

```
$ kubectl apply -f pod.yaml
pod/hello-apparmor created
$ kubectl get pod hello-apparmor
NAME             READY   STATUS    RESTARTS   AGE
hello-apparmor   1/1     Running   0          4s
```

これでコンテナにシェルで入り、ファイル書き込み操作を実行できる：

```
$ kubectl exec -it hello-apparmor -- /bin/sh
/ # touch test.txt
touch: test.txt: Permission denied
```

AppArmorは、コンテナのファイルシステムへのファイルの書き込みを阻止する。この操作を実行しようとすると、"Permission denied "というメッセージが表示される。

## seccompを使う

Seccompは「セキュア・コンピューティング・モード」の略で、ユーザー空間からカーネルへの呼び出しを制限できるLinuxカーネルのもう1つの機能である。seccompプロファイルは、システムコールとその引数を制限する規則を定義するメカニズムである。seccompを使うことで、Linuxカーネルの脆弱性を悪用するリスクを減らすことができる。Kubernetesにおけるseccompの詳細については、[ドキュメントを](https://oreil.ly/B8I5L)参照のこと。

### デフォルトのコンテナランタイムプロファイルをコンテナに適用する

Docker Engineやcontainerdなどのコンテナランタイムには、デフォルトのseccompプロファイルが同梱されている。デフォルトのseccompプロファイルは、アプリケーションでよく使われるシステムコールを許可し、同時に危険とみなされるシステムコールの使用を禁止している。

KubernetesはPod作成時にデフォルトのコンテナランタイムプロファイルをコンテナに適用しないが、`SeccompDefault` [feature gateを](https://oreil.ly/m9g0G)使用して有効にすることができる。`seccompProfile`あるいは、セキュリティコンテキスト属性（seccomp profile type）を`RuntimeDefault` に設定することで、Podごとにこの機能を有効にすることもできる。[例4-3は](#ch04.html#seccomp-default-profile-pod)その使い方を示している。

##### 例 4-3. コンテナランタイムプロファイルが提供するデフォルトのseccompプロファイルを適用するPod

```
apiVersion: v1
kind: Pod
metadata:
  name: hello-seccomp
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault 
  containers:
  - name: hello
    image: busybox:1.28
    command: ["sh", "-c", "echo 'Hello seccomp!' && sleep 1h"]
```

[![1](assets/1.png)](#ch04.html#co_system_hardening_CO3-1)
: 

デフォルトのコンテナランタイムプロファイルを適用する。

 `apply` 、YAMLマニフェストをポイントしてPodを起動できる。Podは "Running "ステータスに移行するはずだ：

```
$ kubectl apply -f pod.yaml
pod/hello-seccomp created
$ kubectl get pod hello-seccomp
NAME            READY   STATUS    RESTARTS   AGE
hello-seccomp   1/1     Running   0          4s
```

コンテナ内で実行される`echo` コマンドは、デフォルトの seccomp プロファイルでは、セキュリティの観点から問題がないとみなされている。以下のコマンドは、コンテナのログを検査する：

```
$ kubectl logs hello-seccomp
Hello seccomp!
```

呼び出しは許可され、"Hello seccomp!"というメッセージが標準出力に書き出された。

### カスタムプロファイルをセットする

デフォルトのコンテナ・ランタイム・プロファイルに加えて、独自のカスタム・プロファイルを作成してセットすることができる。これらのファイルの標準ディレクトリは`/var/lib/kubelet/seccomp` である。カスタム・プロファイルは、サブディレクトリ`profiles` に整理する。ディレクトリがまだ存在しない場合は作成する：

```
$ sudo mkdir -p /var/lib/kubelet/seccomp/profiles
```

カスタム・プロファイルはprofileディレクトリのファイル`mkdir-violation.json` に作成することにする。[例4-](#ch04.html#seccomp-mkdir-violation-profile)4にプロファイル定義の詳細を示す。一言で言えば、この規則セットは`mkdir` システムコールの使用を許可しない。

##### 例4-4.  `mkdir` システムコールの実行を防ぐseccompプロファイル

```
{
    "defaultAction": "SCMP_ACT_ALLOW", 
    "architectures": [ 
        "SCMP_ARCH_X86_64",
        "SCMP_ARCH_X86",
        "SCMP_ARCH_X32"
    ],
    "syscalls": [
        {
            "names": [
                "mkdir"
            ],
            "action": "SCMP_ACT_ERRNO" 
        }
    ]
}
```

[![1](assets/1.png)](#ch04.html#co_system_hardening_CO4-1)
: 

デフォルトの動作は、すべてのシステム呼び出しに適用される。ここでは、`SCMP_ACT_ALLOW` を使ったすべてのシステムコールを許可することにする。

[![2](assets/2.png)](#ch04.html#co_system_hardening_CO4-2)
: 

デフォルトのアクションが適用されるべき特定のアーキテクチャをフィルタリングできる。フィールドの定義は任意である。

[![3](assets/3.png)](#ch04.html#co_system_hardening_CO4-3)
: 

デフォルトのアクションは、より細かい規則を宣言することで上書きできる。`SCMP_ACT_ERRNO` アクションは、`mkdir` システムコールの実行を阻止する。

カスタムプロファイルをディレクトリ`/var/lib/kubelet/seccomp` に配置しても、その規則が自動的にPodに適用されるわけではない。それを使用するには、まだPodを設定する必要がある。

### コンテナにカスタムプロファイルを適用する

カスタムプロファイルの適用は、わずかな違いはあるものの、デフォルトのコンテナ実行時プロファイルの適用と同じようなパターンに従う。[例 4-5](#ch04.html#seccomp-custom-profile-pod) を見るとわかるように、セキュリティ・プロファイルの`seccompProfile` 属性をファイル`mkdir-violation.json` に指定し、タイプを`Localhost` に設定している。

##### 例4-5. カスタムのseccompプロファイルを適用したPodが`mkdir` のシステムコールを防ぐ。

```
apiVersion: v1
kind: Pod
metadata:
  name: hello-seccomp
spec:
  securityContext:
    seccompProfile:
      type: Localhost 
      localhostProfile: profiles/mkdir-violation.json 
  containers:
  - name: hello
    image: busybox:1.28
    command: ["sh", "-c", "echo 'Hello seccomp!' && sleep 1h"]
    securityContext:
      allowPrivilegeEscalation: false
```

[![1](assets/1.png)](#ch04.html#co_system_hardening_CO5-1)
: 

現在のノードのプロファイルを指す。

[![2](assets/2.png)](#ch04.html#co_system_hardening_CO5-2)
: 

サブディレクトリ`profiles` にある`mkdir-violation.json` という名前のプロファイルを適用する。

宣言的な`apply` ・コマンドを使ってPodを作成する。Podが「Running」ステータスに移行するまで待つ：

```
$ kubectl apply -f pod.yaml
pod/hello-seccomp created
$ kubectl get pod hello-seccomp
NAME            READY   STATUS    RESTARTS   AGE
hello-seccomp   1/1     Running   0          4s
```

コンテナにシェルを入れ、seccompが適用された規則を正しく実行していることを確認する：

```
$ kubectl exec -it hello-seccomp -- /bin/sh
/ # mkdir test
mkdir: can't create directory test: Operation not permitted
```

出力でわかるように、`mkdir` コマンドを実行しようとすると、演算子はエラーメッセージを表示する。カスタムプロファイルの規則に違反している。

# 概要

セキュリティ面への対応は、Kubernetesクラスタのコンポーネントやワークロードに限ったことではない。ホストシステムレベルでもできることはたくさんある。我々は、潜在的なセキュリティの脆弱性を最小化するための様々なOSの機能とその使い方について議論した。

多くのオペレーティングシステムには、エンドユーザにより豊富な機能を体験してもらうために、豊富なパッケージやサービスが用意されている。Kubernetesクラスタの運用に不要な機能を見極めることが重要だ。不要なパッケージやサービスは厳格に削除し、必要のないポートは閉じる。また、特定のディレクトリ、ファイル、アプリケーションへのアクセスを許可するユーザを限定したい。Linuxのユーザ管理を使ってパーミッションを制限しよう。

コンテナ内で実行されるアプリケーションやプロセスがシステム呼び出しを行うのは、ごく一般的なことだ。AppArmorやseccompのようなLinuxカーネルハードニングツールを使って、これらの呼び出しを制限することができる。コンテナを実行するアプリケーションのニーズを満たすために不可欠なシステム呼び出しだけを許可する。

# 試験の要点

Linux OSのツールの基本を理解している。
: 

CKS試験は主にKubernetesのセキュリティ機能に関数している。この領域はLinux OSのセキュリティ機能との境界を越えている。この章で扱う内容とは別に、Linux特有のツールやセキュリティの側面を調べておいても損はないだろう。高いレベルでは、Linux上のサービス、パッケージ、ユーザ、ネットワーク管理に慣れ親しもう。

LinuxカーネルハードニングツールをKubernetesに統合する方法を知る。
: 

AppArmorとseccompは、コンテナからのシステム呼び出しを制限するためにKubernetesと統合できるカーネルハードニングツールにすぎない。プロファイルをロードしてコンテナに適用するプロセスを練習する。視野を広げるために、[SELinuxや](https://oreil.ly/DrGbB) [sysctlなど](https://oreil.ly/GyUoc)、Kubernetesと一緒に動作する他のカーネル機能を調べるのもよいだろう。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ) *app-a/ch04/close-ports*ディレクトリに移動する。`vagrant up` コマンドを使って、クラスタを実行している仮想マシンを立ち上げる。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使ってクラスタをシャットダウンする。
    
    仮想マシンのポート21でリッスンしているプロセスを特定する`kube-worker-1` 。攻撃者がこのポートを悪用するリスクを減らすために、このポートを公開しないことにした。対応するプロセスをシャットダウンして、ポートを閉じる。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    
2.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ)ディレクトリ*app-a/ch04/apparmorに*移動する。`vagrant up` コマンドを使用して、クラスタを実行している仮想マシンを起動する。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使ってクラスタをシャットダウンする。
    
    `network-deny` という名前のAppArmorプロファイルを作成する。このプロファイルは、ネットワークトラフィックの送受信を防止する必要がある。このプロファイルを、enforceモードのAppArmor規則のセットに追加する。`default` 名前付けで実行されている`network-call` という名前の Pod にプロファイルを適用する。Podのログをチェックして、ネットワーク呼び出しができなくなったことを確認する。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    
3.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ) *app-a/ch04/seccomp*ディレクトリに移動する。`vagrant up` コマンドを使用して、クラスタを実行している仮想マシンを起動する。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使ってクラスタをシャットダウンする。
    
    標準のseccompディレクトリのすべてのシステムコールをログに記録する、`audit.json` という名前のseccompプロファイルファイルを作成する。このプロファイルを、`default` 名前空間で実行されている`network-call` というPodに適用する。ログファイル`/var/log/syslog` のログエントリを確認する。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    
4.  イメージ`nginx:1.23.1` で、`sysctl-pod` という名前の新規Podを作成する。sysctlパラメータ`net.core.somaxconn` を1024に、`debug.iotrace` を`1` にセットする。Podのステータスをチェックする。
    

# 第5章 マイクロサービスの脆弱性を最小化する マイクロサービスの脆弱性を最小化する

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

Kubernetesクラスタで運用されるアプリケーションスタックは、マイクロサービスアーキテクチャに従うことが多い。マイクロサービスの脆弱性を最小化する」というドメインは、Podレベルでのセキュリティ設定のガバナンスと実施をカバーする。Kubernetesのコア機能と、セキュリティの脆弱性を最小化するのに役立つ外部ツールについて触れる。さらに、マイクロサービスを実行するPod間の暗号化されたネットワーク通信についても話す。

この章では、高いレベルで以下の概念をカバーする：

*   セキュリティコンテキスト、Pod Security Admission（PSA）、Open Policy Agent Gatekeeper を使用して、適切な OS レベルのセキュリティドメインを設定する
    
*   秘密を管理する
    
*   gVisorやKata Containersなどのコンテナランタイムサンドボックスを使用する。
    
*   相互TLS（Transport Layer Security）によるPod間通信暗号化の実装
    

# 適切なOSレベルのセキュリティドメインをセットする

コアKubernetesとKubernetesエコシステムの両方が、Podとコンテナレベルでセキュリティ設定を定義、実施、管理するためのソリューションを提供している。本セクションでは、セキュリティコンテキスト、Podセキュリティアドミッション、Open Policy Agent Gatekeeperについて説明する。各機能やツールの適用方法については、セキュリティにおける重要性を示す例を用いて学習する。まず、シナリオを設定することから始めよう。

## シナリオ攻撃者がルートユーザコンテナアクセスを悪用する

デフォルトでは、コンテナはルート権限で実行される。アプリケーションコンテナに脆弱性があると、攻撃者にコンテナへのアクセス権（`root` ）を与える可能性がある。コンテナのルートユーザは、ホストマシンのルートユーザと同じである。攻撃者は、アプリケーションを検査したり修正したりできるだけでなく、追加のツールをインストールすることで、コンテナから抜け出し、`root` 権限でホストのネームスペースに入ることができるようになる可能性がある。攻撃者はまた、機密データをホストのファイルシステムからコンテナにコピーする可能性もある。[図 5-1](#ch05.html#root-user-attacker)にシナリオを示す。

![](assets/ckss_0501.png)

そのため、デフォルトのルート・ユーザでコンテナを実行するのは良くない。次のセクションでは、コンテナのセキュリティ・コンテキストを宣言して、非 root ユーザ、または特定のユーザやグループ識別子の使用を強制する方法について説明する。また、コンテナからホスト・アクセスを保護するための、その他のセキュリティ・コンテキスト設定についても説明する。

## セキュリティ・コンテクストを理解する

Kubernetesはコンテナ・オーケストレーション・エンジンとして、コンテナ・セキュリティを強化するための追加設定を適用することができる。そのためには、セキュリティコンテキストを定義する。セキュリティコンテキストは、Podやコンテナに対する特権とアクセス制御のセットを定義する。以下のリストにいくつか例を示す：

*   Podおよび/またはコンテナの実行に使用するユーザID
    
*   ファイルシステムへのアクセスに使用するグループID
    
*   コンテナ内で実行中のプロセスに、rootユーザの権限の一部を付与するが、すべての権限を付与しない。
    

セキュリティコンテキストはKubernetesのプリミティブではない。Pod仕様内のディレクティブ`securityContext` 、属性のセットとしてモデル化されている。Podレベルで定義されたセキュリティセットは、Pod内で実行されているすべてのコンテナに適用されるが、コンテナレベルの設定が優先される。Podレベルのセキュリティ属性の詳細については、[PodSecurityContext APIを](https://oreil.ly/cJWXA)参照のこと。コンテナレベルのセキュリティ属性は[SecurityContext APIで](https://oreil.ly/XOy2a)発見できる。

## 非ルートユーザの使用を強制する

機能をより透明化するために、ユースケースを見てみよう。オープンソースのリバースプロキシサーバnginxのようなイメージの中には、`root` ユーザで実行しなければならないものがある。より賢明なセキュリティ戦略をサポートする手段として、コンテナを`root` ユーザで実行できないようにしたいとする。[例 5-1](#ch05.html#non-root-security-context-error)に示す YAML マニフェストファイル`container-non-root-user-error.yaml` は、コンテナ専用のセキュリティ構成を定義する。このセキュリティコンテキストはこのコンテナにのみ適用され、他のコンテナを定義することはできない。

##### 例 5-1. rootユーザで実行する必要があるイメージに非rootユーザを強制する

```
apiVersion: v1
kind: Pod
metadata:
  name: non-root-error
spec:
  containers:
  - image: nginx:1.23.1
    name: nginx
    securityContext:
      runAsNonRoot: true
```

コンテナは起動プロセス中に`CreateContainer​Confi⁠gError` というステータスで失敗する。Podのイベントログを見ると、イメージが`root` ユーザで実行しようとしていることがわかる。設定されたセキュリティ・コンテキストはこれを許可していない：

```
$ kubectl apply -f container-non-root-user-error.yaml
pod/non-root-error created
$ kubectl get pod non-root-error
NAME             READY   STATUS                       RESTARTS   AGE
non-root-error   0/1     CreateContainerConfigError   0          9s
$ kubectl describe pod non-root-error
...
Events:
  Type     Reason     Age               From               Message
  ----     ------     ----              ----               -------
  Normal   Scheduled  24s               default-scheduler  Successfully \
  assigned default/non-root to minikube
  Normal   Pulling    24s               kubelet            Pulling image \
  "nginx:1.23.1"
  Normal   Pulled     16s               kubelet            Successfully \
  pulled image "nginx:1.23.1" in 7.775950615s
  Warning  Failed     4s (x3 over 16s)  kubelet            Error: container \
  has runAsNonRoot and image will run as root (pod: "non-root-error_default \
  (6ed9ed71-1002-4dc2-8cb1-3423f86bd144)", container: secured-container)
  Normal   Pulled     4s (x2 over 16s)  kubelet            Container image \
  "nginx:1.23.1" already present on machine
```

`root` ユーザで実行する必要のない、代替のnginxコンテナイメージが利用できる。その一例が[bitnami/nginx](https://oreil.ly/EnvzT) である。[例5-](#ch05.html#non-root-security-context-success)2は、`container-non-root-user-success.yaml` ファイルの内容を示している。このファイルの主な変更点は、`spec.containers[].image` 属性に代入された値である。

##### 例 5-2. ユーザIDでの実行をサポートするイメージ上で非rootユーザを強制する

```
apiVersion: v1
kind: Pod
metadata:
  name: non-root-success
spec:
  containers:
  - image: bitnami/nginx:1.23.1
    name: nginx
    securityContext:
      runAsNonRoot: true
```

`runAsNonRoot` ディレクティブでコンテナを起動しても、問題なく動作する。コンテナは "Running "ステータスに遷移する：

```
$ kubectl apply -f container-non-root-user-success.yaml
pod/non-root-success created
$ kubectl get pod non-root-success
NAME               READY   STATUS    RESTARTS   AGE
non-root-success   1/1     Running   0          7s
```

コンテナがどのユーザIDで実行されているか、さっそくチェックしてみよう。コンテナにShellで入り、`id` コマンドを実行する。ユーザID、グループID、補足グループのIDが出力される。イメージ`bitnami/nginx` は、コンテナ・イメージのビルド時に、命令の助けを借りてユーザIDを1001にセットする：

```
$ kubectl exec non-root-success -it -- /bin/sh
$ id
uid=1001 gid=0(root) groups=0(root)
$ exit
```

## 特定のユーザとグループIDをセットする

多くのコンテナ・イメージは、明示的なユーザIDやグループIDを設定していない。`root` のデフォルトユーザで実行する代わりに、希望するユーザ ID とグループ ID を設定して、潜在的なセキュリティリスクを最小限に抑えることができる。ファイルに格納されている YAML マニフェストは、例 5-3 に示すようにユーザー ID を設定する。`container-user-id.yaml`[に](#ch05.html#user-group-id-security-context-success)格納されている YAML マニフェストは、ユーザ ID を 1000 に、グループ ID を 3000 にセットする。

##### 例 5-3. コンテナを特定のユーザとグループIDで実行する。

```
apiVersion: v1
kind: Pod
metadata:
  name: user-id
spec:
  containers:
  - image: busybox:1.35.0
    name: busybox
    command: ["sh", "-c", "sleep 1h"]
    securityContext:
      runAsUser: 1000
      runAsGroup: 3000
```

Podの作成は問題なく動作する。コンテナは「Running」ステータスに遷移する：

```
$ kubectl apply -f container-user-id.yaml
pod/user-id created
$ kubectl get pods user-id
NAME      READY   STATUS    RESTARTS   AGE
user-id   1/1     Running   0          6s
```

コンテナにシェリングした後、ユーザIDとグループIDを調べることができる。現在のユーザは、`/` ディレクトリにファイルを作成することはできない。`/tmp` ディレクトリにファイルを作成することは、ほとんどのユーザが書き込み権限を持っているので、うまくいくだろう：

```
$ kubectl exec user-id -it -- /bin/sh
/ $ id
uid=1000 gid=3000 groups=3000
/ $ touch test.txt
touch: test.txt: Permission denied
/ $ touch /tmp/test.txt
/ $ exit
```

## 特権コンテナを避ける

Kubernetesは、プロセス、ネットワーク、マウント、ユーザIDなどについて、コンテナのネームスペースとホストのネームスペースとの間に明確な分離を確立する。コンテナのセキュリティ・コンテキストを設定することで、ホスト・ネームスペースの特定の側面に対する特権を得ることができる。特権を持つコンテナを使用する場合は、次のような意味を想定する：

*   コンテナ内のプロセスは、ホスト上のプロセスとほぼ同じ権限を持つ。
    
*   コンテナはホスト上のすべてのデバイスにアクセスできる。
    
*   コンテナ内のrootユーザは、ホスト上の`root` ユーザと同様の権限を持つ。
    
*   ホストのファイルシステム上のすべてのディレクトリをコンテナにマウントできる。
    
*   カーネルのセットは、[`sysctl` コマンドなどで](https://oreil.ly/YEcOs)変更できる。
    

# 特権モードでコンテナを使う

特権モードを使用するようにコンテナを設定することは、めったにあることではない。コンテナ内で実行されるアプリケーションやプロセスのほとんどは、コンテナのネームスペース以上の特権を必要としない。特権モードを使用するように設定されたPodに遭遇した場合は、担当のチームまたは開発者に連絡して、そのことを明らかにすること。

特権モードで実行するように設定したコンテナと、非特権コンテナの振る舞いを比較してみよう。まず、[例5-](#ch05.html#non-privileged-container)4に示すように、通常のPodをセットアップする。Podレベルまたはコンテナ・レベルにセキュリティ・コンテキストは設定されていない。

##### 例 5-4. 非特権モードのコンテナを持つPod

```
apiVersion: v1
kind: Pod
metadata:
  name: non-privileged
spec:
  containers:
  - image: busybox:1.35.0
    name: busybox
    command: ["sh", "-c", "sleep 1h"]
```

Podを作成し、正しく表示されることを確認する：

```
$ kubectl apply -f non-privileged.yaml
pod/non-privileged created
$ kubectl get pods
NAME             READY   STATUS    RESTARTS   AGE
non-privileged   1/1     Running   0          6s
```

コンテナのネームスペースとホストのネームスペースが分離されていることを示すために、`sysctl` を使ってホスト名を変更してみる。コマンドの出力を見ればわかるように、コンテナは明らかに制限された権限を強制する：

```
$ kubectl exec non-privileged -it -- /bin/sh
/ # sysctl kernel.hostname=test
sysctl: error setting key 'kernel.hostname': Read-only file system
/ # exit
```

コンテナを特権化するには、単純に値`true` をセキュリティコンテキスト属性`privileged` に代入する。[例 5-5](#ch05.html#privileged-security-context)の YAML マニフェストはその例を示している。

##### 例5-5. 特権モードで実行するように構成されたコンテナを持つPod

```
apiVersion: v1
kind: Pod
metadata:
  name: privileged
spec:
  containers:
  - image: busybox:1.35.0
    name: busybox
    command: ["sh", "-c", "sleep 1h"]
    securityContext:
      privileged: true
```

通常通りPodを作成する。Podは "Running "ステータスに移行するはずだ：

```
$ kubectl apply -f privileged.yaml
pod/privileged created
$ kubectl get pod privileged
NAME         READY   STATUS    RESTARTS   AGE
privileged   1/1     Running   0          6s
```

同じ`sysctl` 、ホスト名を変更できることがわかる：

```
$ kubectl exec privileged -it -- /bin/sh
/ # sysctl kernel.hostname=test
kernel.hostname = test
/ # exit
```

特権モードに関連するコンテナ・セキュリティ・コンテキスト設定は、属性`allowPrivilegeEscalation` である。この属性は、コンテナを実行するプロセスが親プロセスよりも多くの特権を獲得できるようにする。この属性のデフォルト値は`false` であるが、この属性が`true` にセットされているのを見かけたら、その使用を疑ってみること。ほとんどの場合、この機能は必要ない。

## シナリオ開発者がPodセキュリティのベストプラクティスに従っていない

あらゆる職業やレベルの開発者が、Kubernetesの機能、特にセキュリティのベストプラクティスに適用される機能について幅広い知識を持っていると考えるのは不公平だ。前のセクションでは、セキュリティのコンテキストと、どの設定を避けるべきかについて学んだ。開発者はおそらく、継続的に教育を受けなければ、そうしたベストプラクティスを知らないため、問題のあるセキュリティ設定を使用するPodを作成したり、まったく使用しないPodを作成したりする可能性がある。[図 5-2](#ch05.html#privileged-mode-attacker)は、開発者がインターネットで見つけたマニフェストをコピーして特権モードで有効化した Pod を作成している様子を示しています。攻撃者はこの設定を喜んで自分の利点として利用するだろう。

![](assets/ckss_0502.png)

そこで、Kubernetesセキュリティのスペシャリストであるあなたの出番となる。Kubernetesエコシステムは、Podのセキュリティ標準を実施するためのコア機能と外部ツールを提供しており、適切な設定のないオブジェクトは拒否されるか、少なくとも監査されるようになっている。次のセクションでは、Pod Security Admissionと名付けられたKubernetesのコア機能について説明する。

## ポッド・セキュリティ・アドミッション（PSA）を理解する

Kubernetesの旧バージョンには、Pod Security Policies（PSP）と呼ばれる機能が同梱されていた。Podセキュリティポリシーは、Podオブジェクトのセキュリティ標準の実施を支援する概念だ。Kubernetes 1.21では、Pod Security Policiesは廃止され、代替機能としてPod Security Admissionが導入された。PSAは、どのPodセキュリティ標準（PSS）に従うかを決定する。PSSは、非常に制限的なものから非常に寛容なものまで、さまざまなセキュリティポリシーを定義する。

しかし、Kubernetesリリース1.25では、Pod Security Policiesは完全に削除された。旧バージョンのCKSカリキュラムにはまだこの機能がリストされているかもしれない。本書では、Pod Security Admissionのみに焦点を当てる。PSAはKubernetes 1.23でデフォルトで有効になっているが、どのPodがセキュリティ標準に準拠すべきかを宣言する必要がある。PSA機能をオプトインするために必要なことは、名前空間に特定の形式のラベルを追加することだけだ。その名前空間内のすべてのPodは、宣言された標準に従わなければならなくなる。

ラベルは、プレフィックス、モード、レベルの3つの部分で構成される。*プレフィックスには*、常にハードコードされた値`pod-security.kubernetes.io` を使用し、その後にスラッシュを付ける。*モードは*違反の処理を決定する。最後に、*レベルは*、遵守すべきセキュリティ標準の程度を指示する。このようなラベルの例は以下のようになる：

```
metadata:
  labels:
    pod-security.kubernetes.io/enforce: restricted
```

このモードでは、[表5-](#ch05.html#psa-modes)1に示す3つの異なるオプションをセットすることができる。

表5-1. Podセキュリティ入場モード

  
| モード || 振る舞い |
| --- | --- |
| `enforce` || 違反した場合、Podは却下される。 |
| --- | --- |
| `audit` || Podの作成が許可される。違反は監査ログに追加される。 |
| `warn` || Podの作成は許可される。違反はコンソールに表示される。 |

[表5-](#ch05.html#psa-levels)2は、PSAに設定されたレベルによって決定されるセキュリティポリシーを示している。

表5-2. Podセキュリティ入場許可レベル

  
| レベル || 振る舞い |
| --- | --- |
| `privileged` || 完全無制限の方針。 |
| --- | --- |
| `baseline` || 重要な基準をカバーする最小限の制限の方針。 |
| `restricted` || セキュリティの観点からPodを強化するためのベストプラクティスに従って、ポリシーを厳しく制限する。 |

使用例を含むPSAの詳細については、[Kubernetesのドキュメントを](https://oreil.ly/DYziy)参照のこと。

## 名前空間のPodセキュリティ標準を実施する

名前空間`psa` 内の Pod に PSA を適用してみよう。[例 5-6](#ch05.html#psa-namespace-label)に、ネームスペースの定義と関連するラベルの宣言を示す。このラベルは、最高レベルのセキュリティ標準に PSS を適用する。

##### 例5-6. 最高レベルのセキュリティ標準を強制する名前空間

```
apiVersion: v1
kind: Namespace
metadata:
  name: psa
  labels:
    pod-security.kubernetes.io/enforce: restricted
```

Pod が名前空間`psa` で作成されていることを確認する。[例5-7は](#ch05.html#psa-violating-pod)、`busybox` イメージを実行する単純な Pod の YAML マニフェストを示している。

##### 例5-7. PSAの制限に違反するPod

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
```

ネームスペースにPodを作成するコマンドを実行すると、違反がコンソールに表示される。以下のように、Podの作成は許可されていない：

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

非常に制限の厳しい標準に従うように、Podのセキュリティ・コンテキスト設定を構成する必要がある。[例5-8は](#ch05.html#psa-non-violating-pod)、Podセキュリティ標準に違反しない例示的なPod定義を示す。

##### 例5-8. PSSに続くPod

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

Podオブジェクトの作成は期待通りに機能するようになった：

```
$ kubectl apply -f psa-non-violating-pod.yaml
pod/busybox created
$ kubectl get pod busybox -n psa
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          10s
```

PSAは、Kubernetesバージョン1.23以降でデフォルトで有効になっているビルトイン機能だ。導入が簡単で、適切なポリシー標準を選択することができ、強制するようにも、違反のログだけを記録するようにも設定できる。

残念ながら、PSAはあらかじめポリシーのセットが記述されたPodにしか適用されない。独自のカスタム規則を書いたり、メッセージングを変更したり、PodオブジェクトがPSSに従わない場合に変異させたりすることはできない。次のセクションでは、PSAの機能を超えるツールについて見ていく。

## オープン・ポリシー・エージェント（OPA）とゲートキーパーを理解する

[Open Policy Agent（OPA](https://oreil.ly/oK9pI)）は、規則を適用するためのオープンソースの一般化ポリシーエンジンだ。OPAはKubernetesに特化したものではなく、他の技術スタックでも使用できる。その利点の一つは、非常に柔軟な方法でポリシーを定義できることだ。[Regoという](https://oreil.ly/0_mA8)クエリ言語の助けを借りて、独自の規則を書くことができる。Regoに書かれた検証ロジックが、オブジェクトを受け入れるか拒否するかを決定する。

[Gatekeeper](https://oreil.ly/AyVjP)は Kubernetes の拡張機能で、OPA を使用する。Gatekeeperは、あらゆる種類のKubernetes APIプリミティブに対してカスタムポリシーを定義し、実施することを可能にする。そのため、PSAよりもはるかに汎用性が高いが、それらの規則を作成する方法についてはより複雑な知識が必要となる。Gatekeeperは、[「リクエストの処理](#ch03.html#processing-api-request)」で説明した*アドミッション・コントロールの*段階に関与する[。](#ch03.html#processing-api-request)以下に、Gatekeeperで可能なポリシーをリストアップする：

*   すべてのサービスオブジェクトは、以下のキーを持つラベルを定義する必要がある。`team`
    
*   Podで定義されたコンテナイメージはすべて、社内レジストリから取得する必要がある。
    
*   デプロイは少なくとも3つのレプリカを管理する必要があることを保証する。
    

この記事を書いている時点では、Gatekeeper は、要件が満たされていない場合にオブジェクトの作成を拒否することで、ポリシーを強制することができる。将来のバージョンでは、オブジェクトの作成時にオブジェクトを変更するメカニズムも提供されるかもしれない。例えば、作成されたオブジェクトに特定のラベルのキーと値のペアを追加したい場合がある。そのような場合、変異によって自動的にラベルが追加される。

## ゲートキーパーのインストール

Gatekeeperのインストールは比較的簡単だ。Gatekeeperプロジェクトが提供するYAMLマニフェストからKubernetesオブジェクトを作成するだけだ。Gatekeeperを正しくインストールするには、クラスタ管理者権限が必要だ。以下のコマンドは、最新リリースの適用に使用する`kubectl` コマンドである。詳細は[インストールマニュアルを](https://oreil.ly/CyZ1c)参照のこと：

```
$ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
gatekeeper/master/deploy/gatekeeper.yaml
```

名前空間`gatekeeper-system` に Gatekeeper オブジェクトがインストールされた。Gatekeeper を使用する前に、名前空間内のすべての Pod が「Running」ステータスに移行していることを確認すること：

```
$ kubectl get namespaces
NAME                STATUS   AGE
default             Active   29h
gatekeeper-system   Active   4s
...
```

## OPA方針の実装

具体的なユースケースを例にして、カスタム OPA ポリシーの定義に必要な可動部分を示す。「[Network Policies to Restrict Pod-to-Pod Communication（ネットワーク・ポリシーを使ってポッド間通信を制限する）](#ch02.html#network-policies)」では、ネットワーク・ポリシーから選択できるようにネームスペースにラベルを代入する方法を説明した。中核となるカスタム OPA ポリシーは、ネームスペースがホストするアプリケーションを示すキー`app` を持つラベルを少なくとも 1 つ定義する必要があることを決定する。

Gatekeeperでは、カスタムポリシーのために、*制約テンプレートと* *制約という*2つのコンポーネントを実装する必要がある。簡単に言うと、制約テンプレートはレゴで規則を定義し、制約のスキーマを記述する。[例 5-9](#ch05.html#opa-constraint-template)は、ラベル代入を強制するための制約テンプレートの定義である。

##### 例 5-9. 少なくとも1つのラベルの定義を必要とするOPA制約テンプレート

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

制約が使用する種類を宣言する。

[![2](assets/2.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO1-2)
: 

制約の検証スキーマを指定する。この場合、必要なラベル・キーをキャプチャする`labels` という名前のプロパティを渡すことができる。

[![3](assets/3.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO1-3)
: 

Regoを使ってラベルの有無をチェックし、必要なキーのリストと比較する。

制約は、本質的に制約テンプレートの実装である。それは、制約テンプレートによって定義された種類を使用し、エンドユーザによって提供されたデータを入力する。[例5-10では](#ch05.html#opa-constraint)、種類は`K8sRequiredLabels` 、制約テンプレートで定義した。我々は、名前空間上でマッチングしており、それらがキー`app` を持つラベルを定義することを期待している。

##### 例 5-10. ポリシーの「データ」を定義するOPA制約

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

制約テンプレートによって定義された種類を使用する。

[![2](assets/2.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO2-2)
: 

制約テンプレートが適用されるべきAPIリソースを定義する。

[![3](assets/3.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO2-3)
: 

`labels` プロパティは、キー`app` が存在することを期待することを宣言する。

関連するYAMLマニフェストが配置されたので、制約テンプレートと制約のオブジェクトを作成しよう。制約テンプレートはファイル`constraint-template-labels.yaml` に、制約はファイル`constraint-ns-labels.yaml` に書き込まれたと仮定する：

```
$ kubectl apply -f constraint-template-labels.yaml
constrainttemplate.templates.gatekeeper.sh/k8srequiredlabels created
$ kubectl apply -f constraint-ns-labels.yaml
k8srequiredlabels.constraints.gatekeeper.sh/ns-must-have-app-label-key created
```

検証の振る舞いは、すぐに実行できる命令形コマンドで確認できる。以下のコマンドは、ラベル代入なしで新しい名前空間を作成しようとするものである。Gatekeeper はエラーメッセージを表示し、オブジェクトを作成できないようにする：

```
$ kubectl create ns governed-ns
Error from server (Forbidden): admission webhook "validation.gatekeeper.sh" \
denied the request: [ns-must-have-app-label-key] you must provide labels: {"app"}
```

期待されるラベル代入を持つ名前空間を実際に作成できることを確認しよう。[例 5-11](#ch05.html#namespace-app-label)はそのような名前空間の YAML マニフェストを示している。

##### 例 5-11. ラベル代入を持つ名前空間の YAML マニフェスト

```
apiVersion: v1
kind: Namespace
metadata:
  labels:
    app: orion
  name: governed-ns
```

次のコマンドは、`namespace-app-label.yaml` という名前の YAML マニフェストファイルからオブジェクトを作成する：

```
$ kubectl apply -f namespace-app-label.yaml
namespace/governed-ns created
```

この簡単な例では、OPA Gatekeeper の使用方法を示した。この他にも、[OPA Gatekeeper Libraryで](https://oreil.ly/1VV5e)多くの例を発見することができる。CKSのカリキュラムには明記されていないが、最近Kubernetesコミュニティで注目を集めた[Kyvernoという](https://kyverno.io)プロジェクトもチェックしてみるといいだろう。

# 秘密を管理する

Kubernetesのセキュリティ機能に関する議論は、Secretsのトピックを取り上げることなしには終わらないだろう。機密データを定義するためのAPIプリミティブSecretと、それをPodで消費するためのさまざまなオプションについては、すでによくご存じだと思う。このトピックはすでにCKA試験の一部なので、ここでは繰り返さない。詳細については、[*Certified Kubernetes Administrator (CKA) Study Guideの*](https://oreil.ly/cka-study-guide)該当セクション、または[Kubernetesのドキュメントを](https://oreil.ly/1afoc)参照してほしい。コンテナ内でConfigMapやSecretを消費する際のセキュリティ面については、[「ConfigMapやSecretを使ったコンテナの構成](#ch07.html#configuring-container-configmap-secret)」で話している。

CKS試験では、シークレット管理のより特殊化した側面に重点を置いている。その中の1つに、サービスアカウントに代入するシークレットの取り扱いがある。[サービスアカウントのシークレットを作成する」を](#ch03.html#creating-secret-serviceaccount)再確認し、このトピックに関する記憶をリフレッシュすること。ここでは、すべての組み込みSecretタイプについて説明するつもりはないので、[Kubernetesドキュメントの](https://oreil.ly/YU7Yy)関連セクションで、その目的と作成について読んでおくとよいだろう。

Secretsのキーバリューペアを保存する中心的な場所はetcdだ。攻撃者がクラスタデータのKubernetesバッキングストアにアクセスした場合に発生する可能性のある問題を見てみよう。

## シナリオ攻撃者がetcdを実行しているノードにアクセスする

etcdがどこで実行されるかは、[Kubernetesクラスタのトポロジーに](https://oreil.ly/bf5Gt)依存する。このシナリオでは、etcdはコントロールプレーンノード上で実行されると仮定する。etcdに保存されたデータは暗号化されていない形で存在するため、コントロールプレーンノードにアクセスすると、Secretsをプレーンテキストで読み取ることができる。[図 5-3](#ch05.html#etcd-attacker)は攻撃者がコントロールプレーンノードにアクセスし、etcd 内の暗号化されていない Secrets を読み取る様子を示している。

![](assets/ckss_0503.png)

この状況を緩和する一つの方法は、etcdにストアされているデータを暗号化することだ。`etcdctl` 、あるいはファイルシステムからetcdのデータを読み取り、etcdにアクセスすることで、人間が読み取り可能な機密情報を公開することはなくなる。

## etcdデータにアクセスする

まずは、攻撃者がコントロールプレーンノードにログインできた後、etcdのデータを読み取る方法を紹介する。まず、etcdに格納するSecretオブジェクトを作成する必要がある。以下の命令コマンドを使ってエントリを作成する：

```
$ kubectl create secret generic app-config --from-literal=password=passwd123
secret/app-config created
```

キーと値のペア`password=passwd123` でシークレットを作成した。SSHを使ってコントロールプレーンノードに入る。etcdクライアントツール`etcdctl` 、etcdからエントリを読み取る。

# etcdクライアントツールetcdctlを使う

まだコントロールプレーンノードに`etcdctl` がインストールされていない可能性が高い。[インストールマニュアルに従って](https://oreil.ly/wpCkO)、ツールを利用できるようにする。Debian Linuxでは、`sudo apt install etcd-client` でインストールできる。etcdに対して認証を行うには、必須コマンドラインオプション`--cacert` 、`--cert` 、`--key` を指定する必要がある。対応する値は、通常`/etc/kubernetes/manifests/kube-apiserver.yaml` にあるAPIサーバーの構成ファイルで発見できる。パラメータは`--etcd` という接頭辞で始まる必要がある。

以下のコマンドは、必須の CLI オプションを使用して、`app-config` という名前の Secret オブジェクトから内容を読み取り ます。以下の出力はファイルの内容を16進数形式で表示する。100%明らかではないが、出力からプレーンテキストのキーと値のペアを特定することができる：

```
$ sudo ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/\
etcd/server.key get /registry/secrets/default/app-config | hexdump -C
00000000  2f 72 65 67 69 73 74 72  79 2f 73 65 63 72 65 74  |/registry/secret|
00000010  73 2f 64 65 66 61 75 6c  74 2f 61 70 70 2d 63 6f  |s/default/app-co|
00000020  6e 66 69 67 0a 6b 38 73  00 0a 0c 0a 02 76 31 12  |nfig.k8s.....v1.|
00000030  06 53 65 63 72 65 74 12  d9 01 0a b7 01 0a 0a 61  |.Secret........a|
00000040  70 70 2d 63 6f 6e 66 69  67 12 00 1a 07 64 65 66  |pp-config....def|
00000050  61 75 6c 74 22 00 2a 24  36 38 64 65 65 34 34 38  |ault".*$68dee448|
00000060  2d 34 39 62 37 2d 34 34  32 66 2d 39 62 32 66 2d  |-49b7-442f-9b2f-|
00000070  33 66 39 62 39 62 32 61  66 66 36 64 32 00 38 00  |3f9b9b2aff6d2.8.|
00000080  42 08 08 97 f8 a4 9b 06  10 00 7a 00 8a 01 65 0a  |B.........z...e.|
00000090  0e 6b 75 62 65 63 74 6c  2d 63 72 65 61 74 65 12  |.kubectl-create.|
000000a0  06 55 70 64 61 74 65 1a  02 76 31 22 08 08 97 f8  |.Update..v1"....|
000000b0  a4 9b 06 10 00 32 08 46  69 65 6c 64 73 56 31 3a  |.....2.FieldsV1:|
000000c0  31 0a 2f 7b 22 66 3a 64  61 74 61 22 3a 7b 22 2e  |1./{"f:data":{".|
000000d0  22 3a 7b 7d 2c 22 66 3a  70 61 73 73 77 6f 72 64  |":{},"f:password|
000000e0  22 3a 7b 7d 7d 2c 22 66  3a 74 79 70 65 22 3a 7b  |":{}},"f:type":{|
000000f0  7d 7d 42 00 12 15 0a 08  70 61 73 73 77 6f 72 64  |}}B.....password|
00000100  12 09 70 61 73 73 77 64  31 32 33 1a 06 4f 70 61  |..passwd123..Opa|
00000110  71 75 65 1a 00 22 00 0a                           |que.."..|
```

次のステップでは、etcdに保存されているシークレットを暗号化し、同じコマンドで既存のエントリを検証する。

## etcdデータを暗号化する

etcdでAPIデータがどのように暗号化されるかは、APIサーバープロセスに提供されるコマン ドラインオプション`--encryption-provider-config` を使って制御できる。パラメータに代入される値は、`EncryptionConfiguration` オブジェクトを定義する構成ファイルを指す必要がある。まず構成ファイルを作成し、APIサーバー・プロセスがそれを使用するように設定する。

32バイトのランダムキーを生成し、それをbase64エンコードする。この値は、暗号化コンフィギュレーションでいわゆるプロバイダを設定するために必要である：

```
$ head -c 32 /dev/urandom | base64
W68xlPT/VXcOSEZJvWeIvkGJnGfQNFpvZYfT9e+ZYuY=
```

次に、[例5-12に](#ch05.html#encryption-configuration)示すように、base64エンコードされた鍵を使い、暗号化設定の プロバイダーに代入する。内容をファイル`/etc/kubernetes/enc/enc.yaml` に保存する。

##### 例5-12. 暗号化設定のYAMLマニフェスト

```
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets 
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: W68xlPT/VXcOSEZJvWeIvkGJnGfQNFpvZYfT9e+ZYuY= 
      - identity: {}
```

[![1](assets/1.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO3-1)
: 

etcdで暗号化するAPIリソースを定義する。ここではSecretsデータのみを暗号化する。

[![2](assets/2.png)](#ch05.html#co_minimizing_microservice_vulnerabilities_CO3-2)
: 

AES-CBC暗号化プロバイダに代入されるbase64エンコードされた鍵。

`/etc/kubernetes/manifests/kube-apiserver.yaml` のマニフェストを編集する。これは、APIサーバーをPodで実行する方法を定義するYAMLマニフェストである。`--encryption-provider-config` というパラメータを追加し、以下のように構成ファイルの Volume と mountpath を定義する：

```
$ sudo vim /etc/kubernetes/manifests/kube-apiserver.yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: \
    192.168.56.10:6443
  creationTimestamp: null
  labels:
    component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
    - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml
    volumeMounts:
    ...
    - name: enc
      mountPath: /etc/kubernetes/enc
      readonly: true
  volumes:
  ...
  - name: enc
    hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
...
```

APIサーバーを実行しているPodが自動的に再起動するはずである。このプロセスには数分かかるかもしれない。完全に再起動したら、クエリができるようになるはずだ：

```
$ kubectl get pods -n kube-system
NAME                           READY   STATUS    RESTARTS   AGE
...
kube-apiserver-control-plane   1/1     Running   0          69s
```

新しいシークレットは自動的に暗号化される。既存のシークレットは更新する必要がある。以下のコマンドを実行することで、全ネームスペースのシークレットを更新することができる。これには`default` 名前空間の`app-config` という名前のシークレットも含まれる：

```
$ kubectl get secrets --all-namespaces -o json | kubectl replace -f -
...
secret/app-config replaced
```

前に使った`etcdctl` コマンドを実行すると、`aescbc` プロバイダーがデータの暗号化に使われていることがわかる。パスワードの値をプレーンテキストで読み取ることはできなくなった：

```
$ sudo ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/\
etcd/server.key get /registry/secrets/default/app-config | hexdump -C
00000000  2f 72 65 67 69 73 74 72  79 2f 73 65 63 72 65 74  |/registry/secret|
00000010  73 2f 64 65 66 61 75 6c  74 2f 61 70 70 2d 63 6f  |s/default/app-co|
00000020  6e 66 69 67 0a 6b 38 73  3a 65 6e 63 3a 61 65 73  |nfig.k8s:enc:aes|
00000030  63 62 63 3a 76 31 3a 6b  65 79 31 3a ae 26 e9 c2  |cbc:v1:key1:.&..|
00000040  7b fd a2 74 30 24 85 61  3c 18 1e 56 00 a1 24 65  |{..t0$.a<..V..$e|
00000050  52 3c 3f f1 24 43 9f 6d  de 5f b0 84 32 18 84 47  |R<?.$C.m._..2..G|
00000060  d5 30 e9 64 84 22 f5 d0  0b 6f 02 af db 1d 51 34  |.0.d."...o....Q4|
00000070  db 57 c8 17 93 ed 9e 00  ea 9a 7b ec 0e 75 0c 49  |.W........{..u.I|
00000080  6a e9 97 cd 54 d4 ae 6b  b6 cb 65 8a 5d 4c 3c 9c  |j...T..k..e.]L<.|
00000090  db 9b ed bc ce bf 3c ef  f6 2e cb 6d a2 53 25 49  |......<....m.S%I|
000000a0  d4 26 c5 4c 18 f3 65 bb  a8 4c 0f 8d 6e be 7b d3  |.&.L..e..L..n.{.|
000000b0  24 9b a8 09 9c bb a3 f9  53 49 78 86 f5 24 e7 10  |$.......SIx..$..|
000000c0  ad 05 45 b8 cb 31 bd 38  b6 5c 00 02 b2 a4 62 13  |..E..1.8.\....b.|
000000d0  d5 82 6b 73 79 97 7e fa  2f 5d 3b 91 a0 21 50 9d  |..ksy.~./];..!P.|
000000e0  77 1a 32 44 e1 93 9b 9c  be bf 49 d2 f9 dc 56 23  |w.2D......I...V#|
000000f0  07 a8 ca a5 e3 e7 d1 ae  9c 22 1f 98 b1 63 b8 73  |........."...c.s|
00000100  66 3f 9f a5 6a 45 60 a7  81 eb 32 e5 42 4d 2b fd  |f?..jE`...2.BM+.|
00000110  65 6c c2 c7 74 9f 1d 6a  1c 24 32 0e 7a 94 a2 60  |el..t..j.$2.z..`|
00000120  22 77 58 c9 69 c3 55 72  e8 fb 0b 63 9d 7f 04 31  |"wX.i.Ur...c...1|
00000130  00 a2 07 76 af 95 4e 03  0a 92 10 b8 bb 1e 89 94  |...v..N.........|
00000140  45 60 01 45 bf d7 95 df  ff 2e 9e 31 0a           |E`.E.......1.|
0000014d
```

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

gVisorには、runscと呼ばれるOCI（Open Container Initiative）ランタイムが含まれている。runscランタイムは、DockerやKubernetesのようなツールと統合し、コンテナランタイム・サンドボックスを実行する。以下のコマンドは、リポジトリから実行ファイルをインストールする：

```
$ sudo apt-get update && sudo apt-get install -y runsc
```

コンテナランタイムとしてcontainerdを使っていると仮定しよう。containerdにrunscを認識させるための設定を追加する必要がある。他のコンテナ・ランタイムについても同様の説明がgVisorのドキュメントで発見できる：

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

最後にcontainerdを再起動して、変更を有効にする：

```
$ sudo systemctl restart containerd
```

gVisorのインストールに成功し、Podを設定できるようになった。

## ランタイム・クラスの作成と使用

コンテナ・ランタイム・サンドボックスをPodで使うには、2段階のアプローチがある。まず、ランタイムクラスを作成する必要がある。RuntimeClassは、コンテナランタイムの構成を定義するKubernetes APIリソースである。[例5-13は](#ch05.html#gvisor-runtimeclass)、コンテナランタイムのYAMLマニフェストを示しており、先ほどcontainerd構成ファイルで設定したハンドラとしてrunscを点示している。

##### 例 5-13. runsc ハンドラを使用してランタイムクラスを定義するための YAML マニフェスト

```
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

これで、Podの構成でランタイム・クラス名`gvisor` を参照できるようになった。[例5-14は](#ch05.html#nginx-pod-gvisor)、`spec.runtimeClassName` 属性を使用してランタイム・クラスを代入するPod定義を示している。

##### 例 5-14. ランタイムクラスを使用する Pod の YAML マニフェスト

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

コンテナランタイムのサンドボックスは、通常のコンテナランタイムよりも強力にプロセスやアプリケーションを分離するのに役立つ。Kata ContainersとgVisorというプロジェクトは、このようなコンテナランタイム・サンドボックスの実装であり、Kubernetesにインストールして動作するように設定することができる。gVisorを試してみた。gVisorをインストールして設定した後、runscを指すRuntimeClassオブジェクトを作成する必要がある。 Podの設定で、RuntimeClassオブジェクトを名前で指定する。

Pod間の通信は、デフォルトでは暗号化も認証もされていない。相互TLSはこのプロセスをより安全にする。互いに通信するPodは、身元を証明するために証明書を提供する必要がある。何百ものマイクロサービスでは、クラスタにmTLSを実装するのは面倒な作業だ。マイクロサービスを実行している各Podは、クライアント機関から承認された証明書を採用する必要がある。サービスメッシュは、Kubernetesクラスタに機能としてmTLSを追加するのに役立つ。

# 試験の要点

Kubernetesのコア機能と外部ツールを使用して、セキュリティ設定を管理する練習をする。
: 

この章では、OSレベルのセキュリティセットと、さまざまなコア機能と外部ツールでそれらを管理する方法について見てきた。さまざまなオプション、その利点と制限を理解し、コンテキスト要件を実装するためにそれらを適用できるようになる必要がある。セキュリティコンテキスト、Pod Security Admission、Open Policy Agent Gatekeeperの使い方を練習する。Kubernetesエコシステムは、この分野でより多くのツールを提供している。視野を広げるために、自分で自由に探索すること。

etcdがシークレットデータをどのように管理しているかを理解する。
: 

CKA試験では、機密設定データをPodに注入するためにSecretsを作成し使用するワークフローをすでにカバーしている。私は、あなたがすでにこの方法を知っていると仮定している。すべてのシークレットのキーバリューペアは etcd に保存される。etcdを実行しているホストにアクセスできる攻撃者がプレーンテキストで情報を読み取れないように、etcdを暗号化する方法を学ぶことで、Secret管理の知識を広げよう。

コンテナランタイムサンドボックスの使用設定方法を知る。
: 

コンテナ・ランタイム・サンドボックスは、コンテナにより厳格な分離を追加するのに役立つ。Kata ContainersやgVisorのようなコンテナランタイム・サンドボックスをインストールする必要はない。RuntimeClassオブジェクトの助けを借りてコンテナ・ランタイム・サンドボックスを構成するプロセスと、RuntimeClassを名前付けしてPodに代入する方法を理解する必要はある。

mTLSについての認識を深める。
: 

Podで実行されているすべてのマイクロサービスに対してmTLSをセットアップするのは、証明書の管理のために非常に面倒になる。試験のために、Pod間の通信にmTLSをセットアップしたいという一般的なユースケースを理解しておこう。実際に手動で実装することは想定されていないだろうが。プロダクションKubernetesクラスタはサービスメッシュを使ってmTLSを機能として提供している。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1.  コマンド`sh -c sleep 1h` を実行するコンテナイメージ`busybox:1.28` で`busybox-security-context` という名前の Pod を作成する。`emptydir` タイプのボリュームを追加し、パス`/data/test` にマウントする。以下の属性を持つセキュリティコンテキストを構成する：`runAsUser: 1000` `runAsGroup: 3000` および`fsGroup: 2000` を設定する。さらに、属性`allowPrivilegeEscalation` を`false` に設定する。
    
    コンテナに入り、`/data/test` というディレクトリに移動し、`hello.txt` という名前のファイルを作成する。このファイルに代入されているグループをチェックする。その値は？コンテナから出る。
    
2.  Pod Security Admission（PSA）規則を作成する。`audited` というネームスペースに、コンソールにレンダリングされるべきレベル`baseline` を持つ Pod Security Standard（PSS）を作成する。
    
    PSSに違反するネームスペースにPodを作成してみて、コンソールログにメッセージを出す。好きな名前、コンテナ・イメージ、セキュリティ設定を指定できる。Podは作成されるか？Podが作成されないようにするには、どのPSAレベルを設定する必要があるか？
    
3.  クラスタに Gatekeeper をインストールする。ReplicaSet が制御するレプリカの最小数と最大数を定義した Gatekeeper ConstraintTemplate オブジェクトを作成する。ConstraintTemplate を使用する Constraint オブジェクトをインスタンス化する。レプリカの最小数を 3、最大数を 10 にセットする。
    
    レプリカセット数を 15 に設定したデプロイオブジェクトを作成する。Gatekeeper は、デプロイ、ReplicaSet、および Pod の作成を許可してはならない。エラーメッセージが表示されるはずである。レプリカの数を 7 にして、デプロイオブジェクトを再度作成し、すべてのオブジェクトが正常に作成されたことを確認する。
    
4.  `aescbc` プロバイダーを使って etcd の暗号化を設定する。`Opaque` 型の新しい Secret オブジェクトを作成する。キーと値のペア`api-key=YZvkiWUkycvspyGHk3fQRAkt` を提供する。etcdctl を使ってシークレットの値をクエリする。暗号化された値は?
    
5.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ) *app-a/ch05/gvisor*ディレクトリに移動する。`vagrant up` コマンドを使って、クラスタを実行している仮想マシンを立ち上げる。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使ってクラスタをシャットダウンする。
    
    gVisorが仮想マシン`kube-worker-1` にインストールされている。仮想マシンにシェル・インし、runscをハンドラーとして`container-runtime-sandbox` という名前のRuntimeClassオブジェクトを作成する。次に、名前`nginx` 、コンテナイメージ`nginx:1.23.2` を持つPodを作成し、RuntimeClassを代入する。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    

# 第6章. サプライチェーンのセキュリティ

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

これまでの章では、主にKubernetesクラスタとそのコンポーネントのセキュリティ、クラスタノードの実行に使用されるOSインフラ、既存のコンテナイメージを使用してクラスタノード上でワークロードを実行するための演算子に焦点を当ててきた。本章では一歩引いて、コンテナイメージを設計、構築、最適化するためのプロセス、ベストプラクティス、ツールについて掘り下げていく。

独自のコンテナイメージを作成せず、別のチームや企業が作成した既存のコンテナイメージを利用することもある。コンテナイメージを使用してワークロードを実行する前に、既知の脆弱性を手動または自動化された方法でスキャンすることは、審査プロセスの一部であるべきだ。ここでは、構築済みコンテナイメージのセキュリティリスクを特定、分析、緩和するために使用される CKS 試験に関連するいくつかのオプションについて説明する。

この章では、高いレベルで以下の概念をカバーする：

*   ベース画像のフットプリントを最小限に抑える
    
*   サプライチェーンの確保
    
*   ユーザのワークロードを静的に分析する
    
*   既知の脆弱性について画像をスキャンする
    

# ベース画像のフットプリントを最小限に抑える

コンテナ・イメージの構築プロセスは、表面的には簡単そうに見えるが、悪魔はしばしば細部に宿る。コンテナ・イメージを構築する際に、不必要にサイズが大きすぎたり、脆弱性に満ちていたり、コンテナ層のキャッシュに最適化されていなかったりするのを避けることは、このトピックを始めたばかりの人にはわからないかもしれない。この章では、コンテナ・エンジンDockerの助けを借りて、これらの側面すべてに対処していく。

## シナリオコンテナの脆弱性を悪用する攻撃者

Dockerfileを定義する際に最初に決めなければならないことのひとつが、ベース・イメージの選択だ。ベースイメージはオペレーティングシステムと追加の依存子を提供し、シェルアクセスを公開することもある。

Docker Hubのようなパブリック・レジストリで選択できるベース・イメージの中には、サイズが大きく、アプリケーションをその中で実行するのに必ずしも必要でない機能が含まれている可能性が高いものもある。オペレーティングシステム自体や、ベースイメージで利用可能な依存関係は、脆弱性を露呈する可能性がある。

[図6-1では](#ch06.html#container-image-vulnerabilities-attacker)、攻撃者はコンテナにアクセスすることで、コンテナに関する詳細を把握することができた。これらの脆弱性は、より高度な攻撃のための発射台として利用することができる。

![](assets/ckss_0601.png)

最小限の機能と依存関係を持つベースイメージを使用することを推奨する。次の2、3のセクションでは、より最適化されたベースイメージを作成するメソッドについて説明する。このメソッドは、ビルドがより速く、コンテナレジストリからのダウンロードがより速く、そして最終的には、単に肥大化を抑えることで攻撃対象領域を小さくすることにつながる。次のセクションでは、最も重要なテクニックについて触れる。Dockerfileの書き方のベストプラクティスについては、[Dockerドキュメントで](https://oreil.ly/43Diy)より詳細なリストを発見できるだろう。

## 小さいサイズのベース画像を選ぶ

コンテナ・イメージの中には、1ギガバイト、あるいはそれ以上のサイズを持つものもある。そのようなコンテナ・イメージにバンドルされているすべての機能が本当に必要だろうか？そうではないだろう。ありがたいことに、多くのコンテナ・プロデューサが、同じリリースにさまざまなバリエーションのコンテナ・イメージをアップロードしている。そのバリエーションの1つに、`alpine` イメージがある。これは、小さくて軽量で、脆弱性の少ないLinuxディストリビューションだ。以下の出力でわかるように、`3.17.0` というタグでダウンロードされた`alpine` コンテナ・イメージのサイズは 7.05MB しかない：

```
$ docker pull alpine:3.17.0
...
$ docker image ls alpine
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
alpine       3.17.0    49176f190c7e   3 weeks ago   7.05MB
```

`alpine` コンテナ・イメージには、コンテナ内で実行中のプロセスのトラブルシューティングに使用できる`sh` シェルが付属している。以下のコマンドを使用して、新しいコンテナで対話型Shellを開くことができる：

```
$ docker run -it alpine:3.17.0 /bin/sh
/ # exit
```

ランタイムのトラブルシューティング機能は有用だが、コンテナ・イメージの一部としてシェルを提供することは、コンテナ・イメージのサイズを大きくし、潜在的に攻撃者に門戸を開くことになる。さらに、コンテナ・イメージ内のソフトウェアが多ければ多いほど、脆弱性も多くなる。

Googleが提供する[ディストロ・レス・イメージを](https://oreil.ly/J6Vra)使えば、コンテナ・イメージのサイズと攻撃対象領域をさらに縮小できる。以下のコマンドは、コンテナ・イメージ`gcr.io/distroless/static-debian11` の最新タグをダウンロードし、その詳細を表示する。コンテナ・イメージのサイズはわずか2.34MBである：

```
$ docker pull gcr.io/distroless/static-debian11
...
$ docker image ls gcr.io/distroless/static-debian11:latest
REPOSITORY                          TAG       IMAGE ID       CREATED      \
  SIZE
gcr.io/distroless/static-debian11   latest    901590160d4d   53 years ago \
  2.34MB
```

ディストロ・レスのコンテナ・イメージにはShellが同梱されておらず、以下のコマンドを実行することでそれを確認することができる：

```
$ docker run -it gcr.io/distroless/static-debian11:latest /bin/sh
docker: Error response from daemon: failed to create shim task: OCI runtime \
create failed: runc create failed: unable to start container process: exec: \
"/bin/sh": stat /bin/sh: no such file or directory: unknown.
```

Kubernetesは、ディストロのないコンテナのトラブルシューティングのために、エフェメラル・コンテナという概念を提供している。これらのコンテナは使い捨てを想定しており、通常はシェルを開くことができない最小限のコンテナのトラブルシューティング用にデプロイすることができる。エフェメラル・コンテナについては本書の範囲外だが、[Kubernetesのドキュメントで](https://oreil.ly/IRjP3)詳細な情報を発見できる。

## コンテナイメージの構築に多段階アプローチを用いる

開発者として、Dockerfileの指示の一部としてアプリケーションコードをビルドすることを決めることができる。このプロセスには、コードのコンパイルとコンテナイメージのエントリポイントとなるバイナリのビルドが含まれる。このプロセスを実装するために必要なツールや依存関係をすべて用意しておけば、コンテナ・イメージのサイズは自動的に大きくなる。

[Dockerにおけるマルチステージビルドの](https://oreil.ly/znQc3)考え方は、ビルドステージをランタイムステージから切り離すというものだ。その結果、ビルドステージで必要な依存関係はすべて、プロセス実行後に破棄されるため、最終的なコンテナイメージには残らない。このアプローチは、不要なゴミをすべて取り除くことで、コンテナ・イメージのサイズをはるかに小さくすることにつながる。

マルチステージDockerfileを作成し、完全に理解するための詳細には立ち入らないが、高いレベルでの違いを紹介したい。まず[例6-](#ch06.html#dockerfile-single-stage)1に示すように、プログラミング言語Goを使った簡単なプログラムをビルドしてテストするDockerfileを紹介する。要するに、Go 1.19.4を含む[ベースイメージを](https://oreil.ly/nwLtT)使用している。Goランタイムは`go` executesを提供する。これを起動してテストを実行し、アプリケーションのバイナリをビルドする。

##### 例6-1. Goベースイメージを使ったGoプログラムのビルドとテスト

```
FROM golang:1.19.4-alpine 
WORKDIR /app

COPY go.mod .
COPY go.sum .
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go test -v 
RUN go build -o /go-sample-app . 
CMD ["/go-sample-app"]
```

[![1](assets/1.png)](#ch06.html#co_supply_chain_security_CO1-1)
: 

Goのベースイメージを使用する

[![2](assets/2.png)](#ch06.html#co_supply_chain_security_CO1-2)
: 

アプリケーションコードに対してテストを実行する

[![3](assets/3.png)](#ch06.html#co_supply_chain_security_CO1-3)
: 

Goアプリケーションのバイナリをビルドする。

以下のように、`docker build` コマンドを使って画像を作成することができる：

```
$ docker build . -t go-sample-app:0.0.1
...
```

結果のコンテナ・イメージのサイズは348MBとかなり大きいが、これにはちゃんとした理由がある。コンテナ・イメージのサイズは348MBとかなり大きいが、これにはちゃんとした理由がある。コンテナを起動する際にGoランタイムはもう必要ないにもかかわらず、Goランタイムが含まれているのだ。`go build` コマンドは、コンテナのエントリポイントとして実行できるバイナリを生成した：

```
$ docker images
REPOSITORY      TAG     IMAGE ID       CREATED          SIZE
go-sample-app   0.0.1   88175f3ab0d3   44 seconds ago   358MB
```

次は、マルチステージ・アプローチを見てみよう。マルチステージDockerfileでは、少なくとも2つのステージを定義する。[例6-](#ch06.html#dockerfile-multi-stage)2では、`build` をエイリアス化したステージを指定し、テストを実行してバイナリをビルドしている。2番目のステージは、ステージ`build` でプロデューサを生成したバイナリを専用のディレクトリにコピーする。ただし、実行には`alpine` ベース・イメージを使用する。`docker build` コマンドを実行すると、ステージ`build` は最終的なコンテナ・イメージに含まれなくなる。

##### 例6-2. 多段階Dockerfileの一部としてGoプログラムをビルドし、テストする。

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

`build` という名前のステージで、プログラムのビルドとテストにGoのベースイメージを使用する。

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

`build` ステージでプロデューサが生成したアプリケーションバイナリをコピーし、コンテナ起動時に実行するコマンドとして使用する。

その結果、`alpine` ベース・イメージを使用した場合のコンテナ・イメージ・サイズは、わずか12MBと大幅に小さくなった。この結果は、`docker build` コマンドを再度実行し、コンテナ・イメージのサイズをリストアップすることで確認できる：

```
$ docker build . -t go-sample-app:0.0.1
...
$ docker images
REPOSITORY      TAG     IMAGE ID       CREATED          SIZE
go-sample-app   0.0.1   88175f3ab0d3   44 seconds ago   12MB
```

サイズを小さくしただけでなく、依存関係を少なくすることで攻撃対象も減らした。`alpine` ベース・イメージの代わりにディストロ・レス・ベース・イメージを組み込むことで、コンテナ・イメージのサイズをさらに小さくすることができる。

## レイヤー数を減らす

すべてのDockerfileは、順番に並んだ命令リストで構成されている。一部の命令だけが、作成されるコンテナイメージに読み取り専用レイヤーを作成する。これらの命令は、`FROM` 、`COPY` 、`RUN` 、`CMD` である。他のすべての命令は、一時的な中間イメージを作成するため、レイヤーを作成しない。コンテナ・イメージにレイヤーを追加すればするほど、ビルドの実行時間は遅くなり、また はコンテナ・イメージのサイズは大きくなる。したがって、コンテナイメージのフットプリントを最小化するために、Dockerfile で使用する命令には慎重を期す必要がある。

複数のコマンドを連続して実行することはよくある。[例6-](#ch06.html#dockerfile-multiple-run-instructions)3に示すように、`RUN` 命令のリストを個々の行に使って、それらのコマンドを定義することができる。

##### 例6-3. 複数の`RUN` 命令を指定するDockerfile

```
FROM ubuntu:22.10
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y curl
```

各`RUN` 命令はレイヤーを作成し、コンテナ・イメージのサイズを増やす可能性がある。`&&` で文字列化し、単一のレイヤーのみを確実に生成する方が効率的である。[例6-4は](#ch06.html#dockerfile-combined-run-instructions)、この最適化テクニックの例を示している。

##### 例6-4. 複数の`RUN` 命令を指定するDockerfile

```
FROM ubuntu:22.10
RUN apt-get update -y && apt-get upgrade -y && apt-get install -y curl
```

## コンテナイメージ最適化ツールを使う

先に述べた最適化のプラクティスを忘れてしまいがちだ。オープンソースコミュニティは、プロデューサのコンテナイメージを検査するのに役立つツールをいくつか開発した。それらの関数は、不要なレイヤー、ファイル、フォルダーを減らすための有用なヒントを提供してくれる：

ドッカースリム
: 

DockerSlimは、アプリケーションとその依存関係を分析することで、コンテナイメージを最適化し、セキュリティを確保する。詳細はツールの[GitHubリポジトリで](https://oreil.ly/ZbeZl)発見できる。

ダイブ
: 

Diveは、コンテナイメージにベイクされたレイヤーを探索するためのツールである。不要なレイヤーを簡単に特定し、さらに最適化することができる。Diveのコードとドキュメントは[GitHubリポジトリで](https://oreil.ly/UBqqj)公開されている。

これはコンテナイメージ最適化ツールのほんの短いリストに過ぎない。[ワークロードの静的解析](#ch06.html#static-analysis-workload)」では、DockerfileとKubernetesマニフェストの静的解析に焦点を当てた他のツールを見ていく。

# サプライチェーンの確保

サプライチェーンは、コンテナ・イメージのプロデューサと、それを実行環境（この場合はKubernetes）で演算子するプロセスを自動化する。コンテナイメージの最適化をサポートするためにサプライチェーンに統合できるツールについては、すでにいくつか触れた。このセクションでは、セキュリティの側面を対象としたプラクティスについて説明する。詳しくは、Liz Rice著『*Container Security*』（O'Reilly）を参照されたい。

## コンテナイメージに署名する

コンテナイメージをコンテナレジストリにプッシュする前に、署名することができる。署名には`docker trust sign` コマンドを使用する。 コマンドは、コンテナイメージに署名、いわゆるイメージダイジェストを追加する。イメージ・ダイジェストはコンテナ・イメージの内容から導き出され、一般的にSHA256の形式で表される。コンテナイメージを消費する際、Kubernetesはイメージダイジェストとイメージの内容を比較し、改ざんされていないことを確認できる。

## シナリオ攻撃者がコンテナイメージに悪意のあるコードを注入する

イメージダイジェストを検証するKubernetesコンポーネントはkubeletである。[イメージのプルポリシーを](https://oreil.ly/0wSjy) `Always` に設定した場合、kubeletは、コンテナイメージを以前にダウンロードして検証していても、コンテナレジストリからイメージダイジェストをクエリする。

攻撃者は、[図6-](#ch06.html#no-image-digest-validation-attacker)2に示すように、既存のコンテナ・イメージの内容を変更し、悪意あるコードを注入して、同じタグを持つコンテナ・レジストリにアップロードしようとすることができる。コンテナ内で実行されている悪意のある関係者は、攻撃者がアクセス可能なサードパーティ・サーバに機密情報を送信する可能性がある。

![](assets/ckss_0602.png)

# 画像のチェックサム検証は自動ではない

イメージダイジェストの検証は、Kubernetesのオプトイン機能である。Podを定義する際には、すべてのコンテナイメージに対してイメージダイジェストを明示するようにしよう。

## コンテナイメージを検証する

Kubernetesでは、コンテナの仕様にSHA256イメージダイジェストを追加することができる。例えば、これは`spec.containers[].image` という属性で実現できる。イメージダイジェストは一般にコンテナレジストリで利用できる。例えば、[図6-3は](#ch06.html#dockerhub-alpine-image-digest) [Docker Hub上の](https://oreil.ly/ZAV_H)コンテナイメージ[`alpine:3.17.0`](https://oreil.ly/ZAV_H) のイメージダイジェストを示している。この例では、イメージダイジェストは`sha256:c0d488a800e4127c334ad20d61d7bc21b4097540327217dfab52262adc02380c` である。

![](assets/ckss_0603.png)

画像ダイジェストの動きを見てみよう。[例6-5では](#ch06.html#container-valid-image-digest)、タグを使う代わりに、イメージ・ダイジェストを付加してコンテナ画像を指定している。

##### 例6-5. 有効なコンテナ・イメージ・ダイジェストを使用するPod

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

##### 例 6-6. 無効なコンテナ・イメージ・ダイジェストを使用するPod

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

イメージ名のプレフィックスは、レジストリのドメイン名を宣言する。例えば、`gcr.io/google-containers/debian-base:v1.0.1` は、[Google Cloud コンテナレジストリの](https://oreil.ly/QFxfY)コンテナイメージ`google-containers/debian-base:v1.0.1` を指し、`gcr.io` と表記する。コンテナイメージの宣言でドメイン名を省略すると、コンテナランタイムは[Docker Hubコンテナレジストリの](https://hub.docker.com) `docker.io` 。

## シナリオ攻撃者が悪意のあるコンテナイメージをアップロードする

公開コンテナ・レジストリからコンテナ・イメージを解決するのは便利だが、リスクがないわけではない。これらのコンテナ・レジストリにログインしていれば、誰でも新しいイメージをアップロードできる。コンテナ・イメージの消費には通常、アカウントすら必要ない。

[図 6-4](#ch06.html#public-registries-attacker) に示すように、攻撃者は盗んだ資格情報を使って、悪質なコードを含むコンテナ・イ メージを公開レジストリにアップロードすることができる。そのレジストリからコンテナイメージを参照するコンテナは、自動的に悪意のあるコードを実行することになる。

![](assets/ckss_0604.png)

企業レベルでは、どのコンテナレジストリを信頼するかを管理する必要がある。社内ネットワーク内に独自のコンテナレジストリをセットアップし、完全に制御・管理できるようにすることを推奨する。あるいは、クラウド・プロバイダー環境にプライベート・コンテナ・レジストリをセットアップし、他の誰もアクセスできないようにすることもできる。

あなたが選択できる著名なバイナリ・リポジトリ・マネージャーの1つが[JFrog Artifactory](https://oreil.ly/jdF_6)だ。この製品はコンテナイメージの保存、スキャン、提供を完全にサポートしている。コンテナイメージのコンシューマは、ホワイトリストに登録したコンテナレジストリからイメージを引き出すことだけを許可されるべきである。他のすべてのコンテナレジストリは拒否されるべきである。

## OPA GateKeeperで許可された画像レジストリをホワイトリスト化する

コンテナのレジストリ使用を管理する方法の 1 つに、OPA Gatekeeper がある。[オープン・ポリシー・エージェント（OPA）とゲートキーパーを理解する](#ch05.html#opa-gatekeeper)」で、OPA ゲートキーパーのインストー ル手順と関数について説明した。ここでは、信頼できるコンテナレジストリを許可するための制約テンプレートと制約について触れる。

[例6-7は](#ch06.html#opa-whitelisted-repositories-constraint-template)、OPA Gatekeeperライブラリから直接入手した[制約テンプレート](https://oreil.ly/hvgnr)である。入力プロパティとして、制約テンプレートはコンテナレジストリの接頭辞を表す文字列の配列を定義する。Rego 規則は、`spec.containers[]` 属性に代入されたコンテナイメージだけでなく、`spec.initContainers[]` と`spec.ephemeralContainers[]` も検証する。

##### 例 6-7. コンテナレジストリを強制するOPAゲートキーパー制約テンプレート

```
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8sallowedrepos
  annotations:
    metadata.gatekeeper.sh/title: "Allowed Repositories"
    metadata.gatekeeper.sh/version: 1.0.0
    description: >-
      Requires container images to begin with a string from the specified list.
spec:
  crd:
    spec:
      names:
        kind: K8sAllowedRepos
      validation:
        openAPIV3Schema:
          type: object
          properties:
            repos:
              description: The list of prefixes a container image is allowed to have.
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sallowedrepos

        violation[{"msg": msg}] {
          container := input.review.object.spec.containers[_]
          satisfied := [good | repo = input.parameters.repos[_] ; \
          good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("container <%v> has an invalid image repo <%v>, allowed \
          repos are %v", [container.name, container.image, input.parameters.repos])
        }

        violation[{"msg": msg}] {
          container := input.review.object.spec.initContainers[_]
          satisfied := [good | repo = input.parameters.repos[_] ; \
          good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("initContainer <%v> has an invalid image repo <%v>, \
          allowed repos are %v", [container.name, container.image, \
          input.parameters.repos])
        }

        violation[{"msg": msg}] {
          container := input.review.object.spec.ephemeralContainers[_]
          satisfied := [good | repo = input.parameters.repos[_] ; \
          good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("ephemeralContainer <%v> has an invalid image repo <%v>, \
          allowed repos are %v", [container.name, container.image, \
          input.parameters.repos])
        }
```

[例6-](#ch06.html#opa-whitelisted-repositories-constraint)8に示す制約は、どのコンテナレジストリを許可するかを定義する。通常、社内ネットワークでホストされているサーバーのドメイン名を使用する。ここでは、デモのために`gcr.io/` 。

##### 例 6-8. Google Cloud レジストリを信頼リポジトリとして代入する OPA ゲートキーパー制約

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAllowedRepos
metadata:
  name: repo-is-gcr
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    repos:
      - "gcr.io/"
```

 `apply` 、両方のオブジェクトを作成してみよう：

```
$ kubectl apply -f allowed-repos-constraint-template.yaml
constrainttemplate.templates.gatekeeper.sh/k8sallowedrepos created
$ kubectl apply -f gcr-allowed-repos-constraint.yaml
k8sallowedrepos.constraints.gatekeeper.sh/repo-is-gcr created
```

制約では、規則を適用するネームスペースを指定していない。したがって、クラスタ内のすべてのネームスペースに適用される。以下のコマンドで、ホワイトリストの規則が期待どおりに機能することを確認する。以下のコマンドは、Docker Hubから`nginx` コンテナイメージを使用してPodの作成を試みている。適切なエラーメッセージとともにPodの作成が拒否される：

```
$ kubectl run nginx --image=nginx:1.23.3
Error from server (Forbidden): admission webhook "validation.gatekeeper.sh" \
denied the request: [repo-is-gcr] container <nginx> has an invalid image \
repo <nginx:1.23.3>, allowed repos are ["gcr.io/"]
```

次のコマンドは、Google Cloudのコンテナ・レジストリからコンテナ・イメージを使ってPodを作成する。演算子は許可され、Podオブジェクトが作成される：

```
$ kubectl run busybox --image=gcr.io/google-containers/busybox:1.27.2
pod/busybox created
$ kubectl get pods
NAME      READY   STATUS      RESTARTS     AGE
busybox   0/1     Completed   1 (2s ago)   3s
```

## ImagePolicyWebhook Admission Controller プラグインで許可された画像レジストリをホワイトリストに登録する

許可された画像レジストリの使用を検証するもう一つの方法は、Podが作成されようとしているときにAPIサーバへの呼び出しをインターセプトすることである。この仕組みは、アドミッションコントローラプラグインを有効にすることで実現できる。いったん設定されると、アドミッションコントローラプラグインのロジックは、APIサーバがリクエストを受信したときに自動的に呼び出されるが、呼び出し元を認証認可した後に呼び出される。API呼び出しが通過しなければならないフェーズについては、[「リクエストの処理](#ch03.html#processing-api-request)」ですでに触れた。

アドミッションコントローラは、リクエストが有効になる前に、リクエストを承認、拒否、変更する方法を提供する。例えば、Podを作成するAPIリクエストで送信されたデータを検査し、代入されたコンテナイメージを繰り返しチェックし、コンテナイメージの表記を検証するカスタムロジックを実行することができる。コンテナ・イメージが期待される規約に従っていない場合は、Podの作成を拒否できる。[図 6-5](#ch06.html#imagepolicywebhook-admission-controller-plugin)に高レベルのワークフローを示す。

![](assets/ckss_0605.png)

[ImagePolicyWebhook](https://oreil.ly/JxmUu)アドミッションコントローラプラグインは、Kubernetes API呼び出しをインターセプトするために設定できるプラグインの1つだ。プラグインの名前から、このプラグインの機能を導き出すことができるだろう。このプラグインは、Pod内で定義されたすべてのコンテナイメージに対するポリシーを定義する。定義されたポリシーとコンテナイメージを比較するために、プラグインはHTTPS経由でKubernetesの外部のサービス、Webhookに呼び出す。そして、外部サービスがデータの検証方法を決定する。アドミッションコントローラプラグインのコンテキストでは、外部サービスは*バックエンドとも*呼ばれる。

## バックエンドアプリケーションの実装

バックエンドアプリケーションは、好きなプログラミング言語で実装できる。満たすべき要件は3つだけだ：

1.  HTTPSリクエストを処理できるウェブ・アプリケーションだ。
    
2.  JSONリクエストのペイロードを受け入れ、解析することができる。
    
3.  JSONレスポンスのペイロードを送信することができる。
    

バックエンド・アプリケーションの実装はCKS試験の一部ではないが、本書の[GitHubリポジトリで](https://oreil.ly/OF4fF)Goベースの実装サンプルを発見できる。しかし、本書のGitHubリポジトリにGoベースの実装サンプルがある。

以下のコマンドは、`https://localhost:8080/validate` でアクセスできるアプリケーションのランタイムの振る舞いを示している。リクエストとレスポンスのJSONボディの例は、[Kubernetesのドキュメントで](https://oreil.ly/8GaQe)発見できる。

次の`curl` コマンドは、コンテナイメージの検証ロジックを呼び出す`nginx:1.19.0` 。JSONレスポンスからわかるように、画像は拒否されている：

```
$ curl -X POST -H "Content-Type: application/json" -k -d \'{"apiVersion": \
"imagepolicy.k8s.io/v1alpha1", "kind": "ImageReview", "spec": \
{"containers": [{"image": "nginx:1.19.0"}]}}' https://localhost:8080/validate
{"apiVersion": "imagepolicy.k8s.io/v1alpha1", "kind": "ImageReview", \
"status": {"allowed": false, "reason": "Denied request: [container 1 \
has an invalid image repo nginx:1.19.0, allowed repos are [gcr.io/]]"}}
```

次の`curl` コマンドは、コンテナ画像`gcr.io/nginx:1.19.0` の検証ロジックを呼び出す。JSONレスポンスからわかるように、画像は許可されている：

```
$ curl -X POST -H "Content-Type: application/json"  -k -d '{"apiVersion": \
"imagepolicy.k8s.io/v1alpha1", "kind": "ImageReview", "spec": {"containers": \
[{"image": "gcr.io/nginx:1.19.0"}]}}' https://localhost:8080/validate
{"apiVersion": "imagepolicy.k8s.io/v1alpha1", "kind": "ImageReview", \
"status": {"allowed": true, "reason": ""}}
```

## ImagePolicyWebhookアドミッションコントローラプラグインを設定する

試験では、ImagePolicyWebhookアドミッションコントローラプラグインの「配線」方法を理解することが求められる。このセクションでは、その手順を説明する。最初に、アドミッションコントローラの構成ファイルを作成し、どのプラグインを使用し、実行時にどのように動作するかを知る必要がある。ファイル`/etc/kubernetes/admission-control/image-policy-webhook-admission-config.yaml` を作成し、[例6-9に](#ch06.html#admission-controller-configuration-file)示す内容を入力する。

##### 例6-9. アドミッションコントローラ構成ファイル

```
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: ImagePolicyWebhook 
    configuration:
      imagePolicy:
        kubeConfigFile: /etc/kubernetes/admission-control/ \
                        imagepolicywebhook.kubeconfig 
        allowTTL: 50
        denyTTL: 50
        retryBackoff: 500
        defaultAllow: false 
```

[![1](assets/1.png)](#ch06.html#co_supply_chain_security_CO3-1)
: 

ImagePolicyWebhookプラグインの設定を提供する。

[![2](assets/2.png)](#ch06.html#co_supply_chain_security_CO3-2)
: 

バックエンドを設定するための構成ファイルを指す。

[![3](assets/3.png)](#ch06.html#co_supply_chain_security_CO3-3)
: 

バックエンドに到達できない場合、APIリクエストを拒否する。デフォルトはtrueだが、falseにセットする方がはるかに賢明である。

次に、`plugins[].configuration.imagePolicy.kubeConfigFile` 属性で参照されるファイルを作成する。このファイルの点は、外部サービスの HTTPS URLを指す。また、クライアント証明書と鍵ファイル、およびディスク上のCAファイルも定義する。[例6-10に](#ch06.html#image-policy-configuration-file)構成ファイルの内容を示す。

##### 例6-10. イメージポリシー構成ファイル

```
apiVersion: v1
kind: Config
preferences: {}
clusters:
  - name: image-validation-webhook
    cluster:
      certificate-authority: /etc/kubernetes/admission-control/ca.crt
      server: https://image-validation-webhook:8080/validate 
contexts:
- context:
    cluster: image-validation-webhook
    user: api-server-client
  name: image-validation-webhook
current-context: image-validation-webhook
users:
  - name: api-server-client
    user:
      client-certificate: /etc/kubernetes/admission-control/api-server-client.crt
      client-key: /etc/kubernetes/admission-control/api-server-client.key
```

[![1](assets/1.png)](#ch06.html#co_supply_chain_security_CO4-1)
: 

バックエンド・サービスのURL。HTTPSプロトコルを使用しなければならない。

APIサーバーのImagePolicyWebhookアドミッションコントローラプラグインを有効にし、アドミッションコントローラを構成ファイルに点す。これを実現するには、APIサーバの構成ファイル（通常は`/etc/kubernetes/manifests/kube-apiserver.yaml` に発見される）を編集する。

コマンドラインオプション`--enable-admission-plugins` を発見し、値`ImagePolicyWebhook` を既存のプラグインリストにカンマ区切りで追加する。コマンドラインオプション`--admission-control-config-file` がまだ存在しない場合はそれを指定し、値を`/etc/kubernetes/admission-control/image-policy-webhook-admission-configuration.yaml` に設定する。 構成ファイルが新しいディレクトリに行 くことを考えると、それを Volume として定義し、コンテナにマウントする必要がある。[例 6-11](#ch06.html#api-server-configuration-file)に、API サーバーコンテナの関連オプションを示す。

##### 例6-11. APIサーバー構成ファイル

```
...
spec:
  containers:
  - command:
    - kube-apiserver
    - --enable-admission-plugins=NodeRestriction,ImagePolicyWebhook
    - --admission-control-config-file=/etc/kubernetes/admission-control/ \
      image-policy-webhook-admission-configuration.yaml
    ...
    volumeMounts:
    ...
    - name: admission-control
      mountPath: /etc/kubernetes/admission-control
      readonly: true
  volumes:
  ...
  - name: admission-control
    hostPath:
      path: /etc/kubernetes/admission-control
      type: DirectoryOrCreate
...
```

APIサーバーを実行しているPodが自動的に再起動するはずである。この処理には数分かかる場合がある。APIサーバが自力で立ち上がらない場合は、`sudo systemctl restart kubelet` コマンドでkubeletサービスを再起動する。完全に再起動したら、クエリができるはずだ：

```
$ kubectl get pods -n kube-system
NAME                           READY   STATUS    RESTARTS   AGE
...
kube-apiserver-control-plane   1/1     Running   0          69s
```

Podの作成を要求するAPI呼び出しはすべてバックエンドにルーティングされるようになった。検証結果に基づいて、オブジェクトの作成が許可または拒否される。

# ワークロードの静的解析

本書を通して、DockerfileとKubernetesマニフェストのベストプラクティスについて話す。これらのファイルを手作業で検査し、望ましくない構成ファイルを発見し、手作業で修正することができる。このアプローチは多くの複雑な知識を必要とし、非常に面倒でエラーになりやすい。適切なツールを使って自動化された方法でワークロードファイルを分析する方が、はるかに便利で効率的だ。静的解析のための商用およびオープンソースのツールのリストは長い。このセクションでは、Haskell Dockerfile LinterとKubesecという2つの選択肢の機能を紹介する。

何百、何千ものワークロードファイルを処理しなければならない企業レベルでは、[図6-6に](#ch06.html#continuous-delivery-kubernetes-static-analysis)示すように、継続的デリバリパイプラインの助けを借りて処理することになる。

![](assets/ckss_0606.png)

関連する静的解析ツールは、コンテナイメージがビルドされ、レジストリにプッシュされ、Kubernetesクラスタにデプロイされる前であっても、パイプラインの初期段階で品質ゲートとして呼び出すことができる。継続的デリバリの原則、プラクティスについては、Jez Humble氏とDavid Farley氏による優れた書籍『[*Continuous Delivery*](https://oreil.ly/8CR5M)』（Addison-Wesley Professional）を参照されたい。

## Dockerファイルの分析にHadolintを使う

[Haskell Dockerfile Linterは](https://oreil.ly/C9bvu)、略してhadolintとも呼ばれ、Dockerfile用のリンターである。このツールはDockerドキュメント・ページにリストされている[ベスト・プラクティスに基づいて](https://oreil.ly/Fwksr)Dockerfileを検査する。[例6-12は](#ch06.html#unoptimized-dockerfile)、Goベースのアプリケーションを実行するコンテナイメージを構築するための、最適化されていないDockerfileを示している。

##### 例6-12. 最適化されていないDockerファイル

```
FROM golang
COPY main.go .
RUN go build main.go
CMD ["./main"]
```

Haskell Dockerfile Linterは、`hadolint` の実行ファイルをディスク上のDockerfileに向けることができるモードをサポートしている。分析によって発見された警告メッセージを含む、この後のコマンド実行を見ることができる：

```
$ hadolint Dockerfile
Dockerfile:1 DL3006 warning: Always tag the version of an image explicitly
Dockerfile:2 DL3045 warning: `COPY` to a relative destination without \
`WORKDIR` set.
```

コマンドの出力は、ベース・イメージにタグを代入することを示唆している。既存のDockerfileは`latest` タグを引き、最新のGoコンテナ・イメージに解決する。このやり方は、Goランタイム・バージョンとアプリケーション・コードの間に非互換性をもたらす可能性がある。リソースをコピーする作業ディレクトリを定義することで、オペレーティングシステム固有のディレクトリやファイルと、アプリケーション固有のディレクトリやファイルを分けて管理することができる。[例6-13は](#ch06.html#optimized-dockerfile)、Haskell Dockerfile Linterが発見した警告メッセージを修正している。

##### 例6-13. 最適化されたDockerファイル

```
FROM golang:1.19.4-alpine
WORKDIR /app
COPY main.go .
RUN go build main.go
CMD ["./main"]
```

変更されたDockerfileに対する別の実行は成功した終了コードにつながり、追加の警告メッセージは表示されない：

```
$ hadolint Dockerfile
```

Dockerfileは現在、hadorintが認識しているベストプラクティスに従っている。

## Kubernetesマニフェストの分析にKubesecを使う

[Kubesecは](https://kubesec.io)Kubernetesマニフェストを分析するためのツールである。バイナリ、Dockerコンテナ、アドミッションコントローラプラグイン、さらには`kubectl` のプラグインとして実行できる。その使い方を実証するために、[例6-](#ch06.html#pod-yaml-manifest-initial-security-settings)14に示すYAMLマニフェストファイル`pod-initial-kubesec-test.yaml` を出発点としてセットアップする。

##### 例 6-14. 初期セキュリティ設定を使用したPod YAMLマニフェスト

```
apiVersion: v1
kind: Pod
metadata:
  name: kubesec-demo
spec:
  containers:
  - name: kubesec-demo
    image: gcr.io/google-samples/node-hello:1.0
    securityContext:
      readOnlyRootFilesystem: true
```

Podのコンフィギュレーションを確認すると、前の章の内容に基づいて改善すべき点がすでにいくつか提案されているかもしれない。Kubesecが何を推奨しようとしているのか見てみよう。

Kubesecを起動する最も簡単な方法は、Dockerの助けを借りてコンテナ内でロジックを実行することだ。以下のコマンドは、YAMLマニフェストの内容を標準入力ストリームにフィードする。JSONで形式化された結果は、スコアをレンダリングし、結果の人間が読めるメッセージを提供し、変更すべき点をアドバイスする：

```
$ docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin \
  < pod-initial-kubesec-test.yaml
[
  {
    "object": "Pod/kubesec-demo.default",
    "valid": true,
    "message": "Passed with a score of 1 points",
    "score": 1,
    "scoring": {
      "advise": [
        {
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access and \
                     should be configured with least privilege"
        },
        {
          "selector": ".metadata .annotations .\"container.apparmor.security. \
                       beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater \
                     protection from unknown threats. WARNING: NOT PRODUCTION \
                     READY"
        },
        {
          "selector": "containers[] .resources .requests .cpu",
          "reason": "Enforcing CPU requests aids a fair balancing of \
                     resources across the cluster"
        },
        {
          "selector": ".metadata .annotations .\"container.seccomp.security. \
                       alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure against \
                     unknown threats"
        },
        {
          "selector": "containers[] .resources .limits .memory",
          "reason": "Enforcing memory limits prevents DOS via resource \
                     exhaustion"
        },
        {
          "selector": "containers[] .resources .limits .cpu",
          "reason": "Enforcing CPU limits prevents DOS via resource exhaustion"
        },
        {
          "selector": "containers[] .securityContext .runAsNonRoot == true",
          "reason": "Force the running image to run as a non-root user to \
                     ensure least privilege"
        },
        {
          "selector": "containers[] .resources .requests .memory",
          "reason": "Enforcing memory requests aids a fair balancing of \
                     resources across the cluster"
        },
        {
          "selector": "containers[] .securityContext .capabilities .drop",
          "reason": "Reducing kernel capabilities available to a container \
                     limits its attack surface"
        },
        {
          "selector": "containers[] .securityContext .runAsUser -gt 10000",
          "reason": "Run as a high-UID user to avoid conflicts with the \
                     host's user table"
        },
        {
          "selector": "containers[] .securityContext .capabilities .drop | \
                       index(\"ALL\")",
          "reason": "Drop all capabilities and add only those required to \
                     reduce syscall attack surface"
        }
      ]
    }
  }
]
```

オリジナルのYAMLマニフェストの改良版は[例6-15で](#ch06.html#pod-yaml-manifest-improved-security-settings)発見できる。ここでは、Kubesecによって提案された推奨される変更のいくつかを組み込んだ。

##### 例6-15. 改善されたセキュリティ設定を使用したPod YAMLマニフェスト

```
apiVersion: v1
kind: Pod
metadata:
  name: kubesec-demo
spec:
  containers:
  - name: kubesec-demo
    image: gcr.io/google-samples/node-hello:1.0
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

変更されたPod YAMLマニフェストに対して同じDockerコマンドを実行すると、改善されたスコアが表示され、アドバイスされたメッセージの数が減る：

```
$ docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin \
  < pod-improved-kubesec-test.yaml
[
  {
    "object": "Pod/kubesec-demo.default",
    "valid": true,
    "message": "Passed with a score of 9 points",
    "score": 9,
    "scoring": {
      "advise": [
        {
          "selector": ".metadata .annotations .\"container.seccomp.security. \
                       alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure against \
                     unknown threats"
        },
        {
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access and should \
                     be configured with least privilege"
        },
        {
          "selector": ".metadata .annotations .\"container.apparmor.security. \
                       beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater \
                     protection from unknown threats. WARNING: NOT PRODUCTION \
                     READY"
        }
      ]
    }
  }
]
```

# 既知の脆弱性について画像をスキャンする

セキュリティの脆弱性を記録・発見するためのトップソースのひとつに、[CVE Detailsが](https://oreil.ly/DDlVa)ある。このページでは、既知の脆弱性（CVE）をリストアップし、スコアでランク付けしている。自動化されたツールは、コンテナイメージに埋め込まれたソフトウェアコンポーネントを特定し、それらを中央の脆弱性データベースと比較し、重大度によって問題にフラグを立てることができる。

この機能を持つオープンソースツールの1つが、CKSカリキュラムで明示的に言及されている[Trivy](https://oreil.ly/hqPHH)である。Trivyは、コマンドラインツールとして、コンテナ内で、継続的インテグレーションワークフローで設定可能なGitHubアクションとして、IDE VSCodeのプラグインとして、Kubernetes演算子として、さまざまなモードで実行できる。利用可能なインストール・オプションと手順の概要については、[Trivyのドキュメントを](https://oreil.ly/qB_c8)参照のこと。試験中、ツールをインストールする必要はない。それはすでにあなたのために事前設定されている。あなたが理解する必要があるのは、ツールの実行方法と、発見された脆弱性のインタプリタと修正方法だけだ。

Trivyのコマンドライン実装をインストールしたとする。Trivyのバージョンは以下のコマンドで確認できる：

```
$ trivy -v
Version: 0.36.1
Vulnerability DB:
  Version: 2
  UpdatedAt: 2022-12-13 12:07:14.884952254 +0000 UTC
  NextUpdate: 2022-12-13 18:07:14.884951854 +0000 UTC
  DownloadedAt: 2022-12-13 17:09:28.866739 +0000 UTC
```

[図6-7で](#ch06.html#trivy-execution)わかるように、Trivyは、既知の脆弱性のコピーが中央データベースからダウンロードされたタイムスタンプを示す。Trivyはさまざまな形式でコンテナ・イメージをスキャンできる。サブコマンド`image` は、単純にイメージの名前とタグのスペルを期待するもので、この場合は`python:3.4-alpine` 。

![](assets/ckss_0607.png)

出力の中で最も重要な情報は、特定の脆弱性を含むライブラリ、その深刻度、問題を修正するために使用すべき最小バージョンである。発見された脆弱性のうち、深刻度が高いもの、あるいはクリティカルなものは、修正を検討すべきである。コンテナ・イメージを自分でコントロールできない場合は、新しいバージョンへのアップグレードを試みることができる。

# 概要

Kubernetesの主な目的は、スケーラブルかつセキュアな方法でコンテナ内のアプリケーションを実行することである。この章では、サイズが小さく、既知のセキュリティ脆弱性ができるだけ少ないコンテナイメージを確実に出荷するためのプロセス、ベストプラクティス、ツールについて見てきた。

コンテナ・イメージのフットプリントを最小限に抑えるための、最も効率的なテクニックをいくつかレビューした。まずは小さなベースイメージを選ぶことから始めよう。極端な話、シェルをまったく提供しないことで、サイズをさらに小さくすることもできる。Dockerを使用している場合、目標達成に必要なコンパイラ、ランタイム、ビルドツールをバンドルすることなく、コンテナ内でアプリケーションをビルドできるマルチステージアプローチを活用できる。

Podでコンテナイメージを消費するときは、信頼できるレジストリからのみコンテナイメージを引き出すようにする。社内でコンテナレジストリをセットアップし、コンテナイメージを提供することが望ましい。そうすれば、パブリックでインターネットからアクセス可能なレジストリにアクセスする必要がなくなり、潜在的なセキュリティリスクを排除できる。OPA Gatekeeper の助けを借りて、信頼できるコンテナレジストリのリストの使用を強制することができる。コンテナイメージの SHA256 ハッシュを使用して期待される内容を検証することで、もう 1 つのセキュリティ対策を実施できる。

コンテナイメージのビルドとスキャンのプロセスは、CI/CDパイプラインに組み込むことができる。サードパーティのツールを使えば、デプロイ成果物をビルドする前でも、そのリソースファイルを解析・分析できる。我々は、Dockerfile用のHaskell Dockerfile LinterとKubernetesマニフェスト用のKubesecを調べた。セキュリティ面でカバーする必要があるもう1つのユースケースは、開発者であるあなたやあなたの会社の外部のエンティティによって構築された既存のコンテナイメージを消費することだ。Kubernetes Podでコンテナイメージを実行する前に、その中身に脆弱性がないか必ずスキャンすること。Trivyは、コンテナ・イメージの脆弱性を特定し、レポートすることができるツールの1つで、コンテナでそのイメージを演算子する場合に、どのようなセキュリティ・リスクにさらされているかを知ることができる。

# 試験の要点

コンテナ・イメージのフットプリントを削減するテクニックに精通する。
: 

このセクションでは、コンテナ・イメージのビルド時にサイズを小さくするテクニックをいくつか紹介した。DockerのWebページに記載されているベストプラクティスを読み取り、サンプルのコンテナイメージに適用してみることをお勧めする。テクニックを適用する前と後で、プロデューサーのコンテナ・イメージのサイズを比較する。コンテナ・イメージを可能な限り小さなサイズに縮小し、同時に重要な機能の損失を回避することに挑戦してみるといい。

コンテナイメージがどこから解決できるかを管理するプロセスを歩く。
: 

OPA Gatekeeper は、ユーザがコンテナイメージを解決することを許可されるレジストリを定義する方法を提供する。制約テンプレートと制約のオブジェクトをセットアップし、メインアプリケーションコンテナ、init コンテナ、およびエフェメラルコンテナを定義する Pod に対して、規則が適切に適用されるかどうかを確認する。さらに、Kubernetesスペースで同様の機能を提供する他の製品にも目を向けてみよう。それらの製品の1つが[Kyverno](https://kyverno.io)だ。

コンテナイメージに署名し、ハッシュを使って検証する。
: 

コンテナイメージを作成したら、必ずダイジェストも作成する。コンテナイメージとダイジェストをレジストリに公開する。Pod定義でダイジェストを使用する方法を練習し、コンテナイメージをプルしたときのKubernetesの振る舞いを検証する。

ImagePolicyWebhookアドミッションコントローラプラグインの設定方法を理解する。
: 

ImagePolicyWebhookのバックエンドを書くことは期待されていない。それは試験の範囲外であり、プログラミング言語の知識を必要とする。ただし、APIサーバの設定でプラグインを有効にする方法は理解しておく必要がある。実行中のバックエンドアプリケーションがなくても、ワークフローを練習することをお勧めする。

静的解析ツールが出す警告を修正する方法を知る。
: 

CKSのカリキュラムでは、DockerfileやKubernetesマニフェストを分析するための特定のツールを規定していない。試験中、エラーメッセージや警告メッセージのリストを生成する特定のコマンドを実行するよう求められることがある。メッセージをどのようにインタプリタし、関連リソースファイルでどのように修正するかを理解する。

セキュリティの脆弱性を特定し、修正するためにTrivyを使用する練習をする。
: 

CKSのFAQには、トリビーのドキュメントページがリストアップされている。従って、質問の中にTrivyが出てくる可能性があると考えるのが妥当だろう。コンテナ画像をスキャンするためにTrivyを呼び出すさまざまな方法を理解する必要がある。作成されたレポートは、修正すべき点と発見された脆弱性の深刻度を明確に示してくれる。コンテナ・イメージを簡単に修正できないことを考えると、既知の脆弱性を持つコンテナ・イメージを実行するPodにフラグを立てるよう求められる可能性が高い。

# サンプル練習

これらの練習問題の解答は[付録に](#app01.html#appendix-a)ある。

1.  以下のDockerfileを見てみよう。プロデューサーのコンテナ・イメージのフットプリントを減らす可能性を見つけられるか？
    
    ```
    FROM node:latest
    ENV NODE_ENV development
    WORKDIR /app
    COPY package.json .
    RUN npm install
    COPY . .
    EXPOSE 3001
    CMD ["node", "app.js"]
    ```
    
    Dockerfileのベストプラクティスを適用し、コンテナイメージのフットプリントを最適化する。最適化を行う前と後で、`docker build` コマンドを実行する。その結果、コンテナ・イメージのサイズは小さくなるが、アプリケーションは機能しているはずだ。
    
2.  KubernetesクラスタにKyvernoをインストールする。インストール方法は[ドキュメントで](https://oreil.ly/yxlLe)発見できる。
    
    [ドキュメントページに](https://oreil.ly/Kdj1k)記載されている「画像レジストリの制限」ポリシーを適用する。許可されるレジストリは`gcr.io` だけである。それ以外のレジストリの使用は拒否する。
    
    コンテナイメージを定義するPodを作成する`gcr.io/google-containers/busybox:1.27.2` 。Podの作成は失敗するはずである。コンテナイメージを使用して別のPodを作成する`busybox:1.27.2` 。KyvernoはPodの作成を許可する。
    
3.  YAMLマニフェスト`pod-validate-image.yaml` のコンテナイメージ`nginx:1.23.3-alpine` を使ってPodを定義する。Docker Hubからコンテナイメージのダイジェストを取得する。SHA256ハッシュを使用してコンテナイメージの内容を検証する。Podを作成する。Kubernetesはコンテナイメージを正常にプルできるはずである。
    
4.  Kubesecを使用して、YAMLマニフェストファイル`pod.yaml` の以下の内容を分析する：
    
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: hello-world
    spec:
      securityContext:
        runAsUser: 0
      containers:
      - name: linux
        image: hello-world:linux
    ```
    
    Kubesecが生成したサジェストを検査する。seccompとAppArmorのサジェストは無視する。すべてのメッセージの根本原因を修正し、ツールの別の実行が追加の提案を報告しないようにする。
    
5.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ)ディレクトリ*a/ch06/trivyに*移動する。コマンドを実行する`kubectl apply -f setup.yaml`.
    
    このコマンドは、名前空間`r61` に3つの異なるPodを作成する。コマンドラインから、Podで使用されているコンテナイメージに対してTrivyを実行する。CRITICAL」脆弱性を持つPodをすべて削除する。どのPodがまだ実行されているか？
    

# 第7章 監視、ログ、ランタイム・セキュリティ

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

カリキュラムの最後の領域では、主にKubernetesクラスタのホストとコンテナ・レベルでの不審なアクティビティの検出を扱う。まず、*振る舞い分析という*用語の定義と、それがKubernetesの領域にどのように適用されるかを説明する。理論が一段落したところで、侵入シナリオを検出できるFalcoというツールを導入する。

コンテナが起動した後も、その実行環境は変更できる。例えば、演算子として、手動で追加ツールをインストールしたり、コンテナの一時ファイルシステムにファイルを書き込んだりするために、コンテナにシェルを入れることを決定することができる。コンテナ起動後にコンテナを変更することは、セキュリティリスクへの扉を開く可能性がある。*不変性コンテナ*、つまり起動後に変更できないコンテナを作成することを目指したい。コンテナを不変にするための適切なセットでPodを構成する方法を学ぶ。

最後に、Kubernetesクラスタで発生したイベントのログをキャプチャすることについて説明する。これらのログは、クラスタレベルでのトラブルシューティングの目的で使用することができ、望ましくない、あるいは破壊的なランタイムの振る舞いにつながるようなクラスタ構成の変更がいつ、どのように行われたかを再構築することができる。また、ログエントリは、対策を実行する手段として、今まさに起きているかもしれない攻撃をトレースするために使用することもできる。

この章では、高いレベルで以下の概念をカバーする：

*   振る舞い分析を行い、悪意のある行為を検知する
    
*   深い分析調査と同定を行う
    
*   実行時にコンテナの不変性を確保する
    
*   監査ログを使ってアクセスを監視する
    

# 振る舞い分析を行う

Kubernetesクラスタの管理とアップグレードとは別に、管理者は潜在的に悪意のあるアクティビティを監視するのが仕事だ。クラスタノードにログインし、ホストレベルやコンテナレベルのプロセスを観察することで、このタスクを手動で実行することはできるが、それは恐ろしく非効率な作業だ。

*振る舞い分析とは*、クラスタノードを観察し、通常とは異なるアクティビティがないかどうかを調べるプロセスである。自動化されたプロセスは、特定の関心のあるイベントのフィルタリング、記録、アラートを支援する。

## シナリオKubernetes管理者は攻撃者の行動を観察できる

攻撃者がワーカーノード上で動作するシェルを開いてコンテナにアクセスし、Kubernetesクラスタ全体で追加の攻撃を開始した。管理者がコンテナを1つ1つ手動でチェックしても、このイベントを簡単に検出することはできない。[図 7-1](#ch07.html#behavioral-analytics-attacker)にシナリオを示す。

![](assets/ckss_0701.png)

管理者は、振る舞い分析ツールをインストールすることで、自らの手で問題を解決することにした。この監視ツールは特定のイベントを継続的に監視し、ほぼ瞬時に記録する。管理者は侵入を検知するための効率的なメカニズムを手に入れ、それに基づいて行動できるようになった。

試験に関連する振る舞い分析ツールには、[Falco](https://falco.org)、[Tracee](https://oreil.ly/ibXcO)、[Tetragonが](https://oreil.ly/q15oU)ある。本書では、試験中に利用可能なドキュメントページのリンクの中にリストされているFalcoのみに焦点を当てる。

## ファルコを理解する

Falcoは、ホストレベルおよびコンテナレベルのアクティビティを観測することで、脅威の検知を支援する。以下は、ファルコが監視できるイベントの例である：

*   ファイルシステムの特定の場所でファイルの読み取りや書き込みを行う。
    
*   コンテナ用のシェルバイナリを開く。たとえば、`/bin/bash` で bash シェルを開く。
    
*   望ましくないURLへのネットワーク呼び出しを試みた。
    

ファルコは、設定されたイベントと条件をリッスンするセンサーのセットをデプロイする。各センサーは、イベントをデータソースにマッピングする規則のセットで構成される。規則が特定のイベントにマッチするとアラートがプロデューサされる。アラートは、標準出力、ファイル、 HTTPS エンドポイントなど、イベントを記録する出力チャネルに送られる。Falco は同時に複数の出力チャネルを有効にすることができる。[図 7-2](#ch07.html#falco-high-level-architecture)に Falco のハイレベルアーキテクチャを示す。

![](assets/ckss_0702.png)

Falcoは多くの機能と設定オプションを持つツールである。本書ですべての機能を説明することはできないが、Falcoの上位概念を理解することに時間を割くことをお勧めする。

ファルコに関するもう一つの素晴らしい学習リソースは、シスディグのトレーニングポータルのウェブページで発見することができる。「[Falco 101 "](https://oreil.ly/PwGWx)は、ファルコのすべての機能を説明する無料のビデオコースである。あなたが始めるために必要なことは、アカウントにサインアップすることだ。さらに、Loris DegioanniとLeonardo Grassoによる書籍『[*Practical Cloud Native Security with Falco*](https://oreil.ly/0WHIR)』（オライリー）を一読することをお勧めする。内容は、Falcoを学ぶための初心者に優しいアプローチをとっている。

## ファルコのインストール

Falcoは、ホストシステム上のバイナリとして、またはKubernetesのDaemonSetオブジェクトとしてインストールすることができる。Falco は試験環境にプリインストールされていると考えて差し支えない。インストールプロセスの詳細については、Falco ドキュメントの[該当箇所を](https://oreil.ly/MB6wU)参照されたい。以下のステップでは、Ubuntu マシンへのバイナリのインストールを簡単に説明する。FalcoはKubernetesクラスタのすべてのワーカーノードにインストールする必要がある。これらの手順は、将来のFalcoのバージョンによって変更される可能性があることに注意されたい。

まず、FalcoのGPGキーを信頼し、Falco専用のaptリポジトリを設定し、パッケージリストを更新する必要がある：

```
$ curl -s https://falco.org/repo/falcosecurity-packages.asc | apt-key add -
$ echo "deb https://download.falco.org/packages/deb stable main" | tee -a \
  /etc/apt/sources.list.d/falcosecurity.list
$ apt-get update -y
```

その後、以下のコマンドでカーネル・ヘッダをインストールする：

```
$ apt-get -y install linux-headers-$(uname -r)
```

最後に、バージョン0.33.1のFalco aptパッケージをインストールする必要がある：

```
$ apt-get install -y falco=0.33.1
```

Falcoは正常にインストールされ、バックグラウンドでsystemdサービスとして稼働している。以下のコマンドを実行して、Falcoサービスのステータスを確認する：

```
$ sudo systemctl status falco
● falco.service - Falco: Container Native Runtime Security
     Loaded: loaded (/lib/systemd/system/falco.service; enabled; vendor preset: \
             enabled)
     Active: active (running) since Tue 2023-01-24 15:42:31 UTC; 43min ago
       Docs: https://falco.org/docs/
   Main PID: 8718 (falco)
      Tasks: 12 (limit: 1131)
     Memory: 30.2M
     CGroup: /system.slice/falco.service
             └─8718 /usr/bin/falco --pidfile=/var/run/falco.pid
```

Falco サービスは "active "ステータスにあるはずである。Falco サービスはすでにシステム内のイベントを監視している。

## ファルコを設定する

Falco サービスは、システムを監視するための演算子環境を、一連のデフォルト規則で提供する。これらの規則は`/etc/falco` ディレクトリの YAML ファイルに存在する。`/etc/falco` 内のファイルとサブディレクトリのリストは以下の通りである：

```
$ tree /etc/falco
/etc/falco
├── aws_cloudtrail_rules.yaml
├── falco.yaml
├── falco_rules.local.yaml
├── falco_rules.yaml
├── k8s_audit_rules.yaml
├── rules.available
│   └── application_rules.yaml
└── rules.d
```

これらのファイルのうち、最も重要なものの大まかな目的を説明したい。

### ファルコ構成ファイル

 `falco.yaml` という名前のファイルは、Falcoプロセスの構成ファイルである。アラートが発生した場合に通知されるチャンネルを制御する。チャネルは標準出力またはファイルである。さらに、このファイルは、ログに含めるアラートの最小ログレベル、および Falco プロセスの正常性確認を実装するために使用される組み込みウェブサーバの構成方法を定義する。すべての利用可能なオプションの完全なリファレンスについては、[「ファルコの設定オプ ション](https://oreil.ly/sfHW9)」を参照のこと。

### デフォルト規則

 `falco_rules.yaml` という名前のファイルは、プリインストールされた規則のセットを定義している。Falco はこれらの規則がデフォルトで適用されると考えている。その中には、コンテナへのシェルがオープンされたとき、またはシステムディレク トリへの書き込みが実行されたときにアラートを作成するためのチェックがある。[ルール例」のページで](https://oreil.ly/fxHQm)、他の例とそれに対応するルール定義を見つけることができる。

### カスタム規則

 `falco_rules.local.yaml` という名前のファイルにより、カスタム規則を定義し、デフォルト規則を上書きすることができる。Falco の新規インストールでは、このファイルにはコメントアウトされた規則のみが含ま れており、独自の規則を追加するための出発点となる。[カスタムルールの記述に関するガイダンスは](https://oreil.ly/mJnPo)Falco ドキュメンテーションで発見できる。

### Kubernetes固有の規則

ファイル`k8s_audit_rules.yaml` 、システムコールイベントのロギングに加えて、[Kubernetes固有の規則を](https://oreil.ly/d5FGD)定義する。典型的な例としては、"ネームスペースが削除されたときにイベントをログする"、"RoleまたはClusterRoleオブジェクトが作成されたときにイベントをログする "などがある。

### 設定変更を適用する

構成ファイルの内容を変更しても、Falcoプロセスには自動的に反映されない。以下のコマンドで示されるように、Falcoサービスを再起動する必要がある：

```
$ sudo systemctl restart falco
```

次は、Falcoのデフォルト規則でカバーされているいくつかのイベントをトリガーする。それぞれのイベントは、設定されたチャンネルに書き込まれるアラートを作成する。Falcoの初期設定では、メッセージは標準出力にルーティングされる。

## イベントの生成と Falco ログの検査

Falcoのアラートを実際に見てみよう。Falcoによって追加されたデフォルト規則の1つは、ユーザがコンテナに対してシェルを開こうとするたびにアラートを作成する。ログエントリを見るには、このアクティビティを実行する必要がある。そのためには、`nginx` という名前の新しいPodを作成し、コンテナに対してbashシェルを開き、コンテナから抜ける：

```
$ kubectl run nginx --image=nginx:1.23.3
pod/nginx created
$ kubectl exec -it nginx -- bash
root@nginx:/# exit
```

Podのランタイムの詳細を調べることで、Podが実行されているクラスタ・ノードを特定する：

```
$ kubectl get pod nginx -o jsonpath='{.spec.nodeName}'
kube-worker-1
```

この Pod は`kube-worker-1` という名前のクラスタノード上で実行されている。関連するログエントリを発見するには、そのマシンの Falco ログを検査する必要がある。`kube-worker-1` クラスタノード上で直接`journalctl` コマンドを使用することで、ログに記録されたイベントを検査することができる。次のコマンドは`falco` というキーワードを含むエントリを検索する：

```
$ sudo journalctl -fu falco
...
Jan 24 18:03:37 kube-worker-1 falco[8718]: 18:03:14.632368639: Notice A shell \
was spawned in a container with an attached terminal (user=root user_loginuid=0 \
nginx (id=18b247adb3ca) shell=bash parent=runc cmdline=bash pid=47773 \
terminal=34816 container_id=18b247adb3ca image=docker.io/library/nginx)
```

コンテナの状態を変更しようとすると、追加の規則が適用されることを発見するだろう。`nginx` コンテナに`apt` を使って Git パッケージをインストールしたとしよう：

```
root@nginx:/# apt update
root@nginx:/# apt install git
```

ファルコはこれらのシステムレベルの演算子のログエントリを追加した。以下の出力はアラートを表示している：

```
$ sudo journalctl -fu falco
Jan 24 18:55:48 ubuntu-focal falco[8718]: 18:55:05.173895727: Error Package \
management process launched in container (user=root user_loginuid=0 \
command=apt update pid=60538 container_id=18b247adb3ca container_name=nginx \
image=docker.io/library/nginx:1.23.3)
Jan 24 18:55:48 ubuntu-focal falco[8718]: 18:55:11.050925982: Error Package \
management process launched in container (user=root user_loginuid=0 \
command=apt install git-all pid=60823 container_id=18b247adb3ca \
container_name=nginx image=docker.io/library/nginx:1.23.3)
...
```

次のセクションでは、アラート作成のトリガーとなるファルコの規則とその構文を検証する。

## ファルコルールファイルの基本を理解する

Falco の規則ファイルは通常、YAML で定義された 3 つの要素、すなわち規則、マクロ、 リストから構成される。それらの要素を高いレベルで理解し、ある目標を達成するためにそれらをどのように修正するかを知っておく必要があるだろう。

# ファルコの規則を作る

試験中、ファルコの規則を自分で作る必要はないだろう。あなたには既存の規則セットが提供される。もしFalcoの規則についてもっと深く知りたければ、関連[文書のページを見て](https://oreil.ly/PD1ro)ほしい。

### 規則

*規則とは*、アラートが生成されるべき条件付きである。また、アラートの出力メッセージも定義する。出力メッセージは、ハードコードされたメッセージで構成され、組み込み変数を組み込むことができる。アラートは`WARNING` のログレベルに記録される。[例7-](#ch07.html#falco-rule-camera)1は、SkypeやWebexなど、従来のビデオ会議ソフトウェア以外のプロセスからマ シンのカメラにアクセスしようとするイベントをリッスンする規則のYAMLを示している。

##### 例7-1. カメラアクセスを監視するファルコ規則

```
- rule: access_camera
  desc: a process other than skype/webex tries to access the camera
  condition: evt.type = open and fd.name = /dev/video0 and not proc.name in \
             (skype, webex)
  output: Unexpected process opening camera video device (command=%proc.cmdline)
  priority: WARNING
```

### マクロ

*マクロは*再利用可能な規則条件付きであり、複数の規則にわたって同じようなロジックをコピーペーストすることを避けるのに役立つ。マクロは、規則ファイルが長くなり、保守性を向上させたい場合に有用である。

規則ファイルを単純化している最中だとする。あなたは、複数の規則がカメラアクセスをリッスンする同じ条件付き を使用していることに気づいた。[例7-](#ch07.html#falco-macro-camera)2は、ロジックをマクロに分割する方法を示している。

##### 例7-2. 再利用可能な条件を定義するファルコマクロ

```
- macro: camera_process_access
  condition: evt.type = open and fd.name = /dev/video0 and not proc.name in \
             (skype, webex)
```

[例7-](#ch07.html#falco-rule-with-macro)3に示すように、規則定義の中でマクロを名前付けして参照できるようになった。

##### 例7-3. マクロを組み込んだファルコ規則

```
- rule: access_camera
  desc: a process other than skype/webex tries to access the camera
  condition: camera_process_access
  output: Unexpected process opening camera video device (command=%proc.cmdline)
  priority: WARNING
```

### リスト

*リストとは*、規則やマクロ、その他のリストに含めることができる項目の集まりである。リストは、従来のプログラミング言語における配列のようなものと考えてほしい。[例7-4は](#ch07.html#falco-list-video-software)、ビデオ会議ソフトウェアに関連するプロセス名のリストを作成する。

##### 例7-4. ファルコのリスト

```
- list: video_conferencing_software
  items: [skype, webex]
```

[例7-](#ch07.html#falco-macro-with-list)5は、マクロでの名前によるリストの使い方を示している。

##### 例7-5. リストを使うファルコ・マクロ

```
- macro: camera_process_access
  condition: evt.type = open and fd.name = /dev/video0 and not proc.name in \
             (video_conferencing_software)
```

### 既存の規則を分解する

Falcoがデフォルト規則とともに出荷される理由は、本番クラスタの運用を開始するまでの期間を短縮するためである。あなた自身の規則と正しい構文を考え出さなければならない代わりに、あなたは単にFalcoをインストールし、最初からベストプラクティスの利点を享受することができる。

[イベントの生成とFalcoログの検査](#ch07.html#generating-events-inspect-falco-logs)」でトリガーしたイベントの1つに戻ってみよう。この記事を書いている時点では、私はFalcoのバージョン0.33.1を使用している。同梱されているルールファイル`/etc/falco/falco_rules.yaml` には "Terminal shell in container" という規則が含まれている。これは、コンテナにシェルをオープンするイベントを監視する。例えば、"A shell was spawned in a container "といったログメッセージの一部を検索することで、この規則を簡単に見つけることができる。[例 7-6](#ch07.html#default-open-shell-falco-rule-file)に、規則定義の構文と YAML の注釈部分を示す。

##### 例7-6. コンテナへのシェルアクティビティを監視するFalco規則

```
- macro: spawned_process 
  condition: (evt.type in (execve, execveat) and evt.dir=<)

- macro: container 
  condition: (container.id != host)

- macro: container_entrypoint 
  condition: (not proc.pname exists or proc.pname in (runc:[0:PARENT], \
              runc:[1:CHILD], runc, docker-runc, exe, docker-runc-cur))

- macro: user_expected_terminal_shell_in_container_conditions 
  condition: (never_true)

- rule: Terminal shell in container 
  desc: A shell was used as the entrypoint/exec point into a container with an \
        attached terminal.
  condition: > 
    spawned_process and container
    and shell_procs and proc.tty != 0
    and container_entrypoint
    and not user_expected_terminal_shell_in_container_conditions
  output: > 
    A shell was spawned in a container with an attached terminal (user=%user.name \
    user_loginuid=%user.loginuid %container.info
    shell=%proc.name parent=%proc.pname cmdline=%proc.cmdline pid=%proc.pid \
    terminal=%proc.tty container_id=%container.id image=%container.image.repository)
  priority: NOTICE 
  tags: [container, shell, mitre_execution] 
```

[![1](assets/1.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-1)
: 

マクロ（名前付けされた複数の規則で再利用可能な条件）を定義する。

[![2](assets/2.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-5)
: 

規則の名前を指定する。

[![3](assets/3.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-6)
: 

複数のマクロで構成される集約条件。

[![4](assets/4.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-7)
: 

イベントが発生した場合のアラートメッセージ。メッセージは、実行時の値を参照するために組み込みフィールドを使用することができる。

[![5](assets/5.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-8)
: 

規則違反の重大性を示す。

[![6](assets/6.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO1-9)
: 

管理を容易にするために、規則セットを関連する規則のグループに分類する。

既存の規則を変更したい場合もあるだろう。次のセクションでは、デフォルトの規則を上書きする最善の方法を説明する。

## 既存の規則を覆す

 `/etc/falco/falco_rules.yaml` のルール定義を直接修正するのではなく、 でルールを再定義することをお勧めする。そうすることで、修正したルールを削除したくなったときや、途中でミスをしたときに、元のルール定義に戻すことができる。クラスタの全ワーカーノードで規則定義を変更する必要がある。`/etc/falco/falco_rules.local.yaml`

[例7-7に](#ch07.html#modified-open-shell-falco-rule-file)示す規則定義は、優先順位を`ALERT` に変更し、[組み込みフィールドを](https://oreil.ly/z5oAk)組み込んで出力を新しい形式に変更することで、「コンテナ 内のターミナルシェル」という名前の規則をオーバーライドする。

##### 例7-7. コンテナに対するシェルの活動を監視する変更規則

```
- rule: Terminal shell in container
  desc: A shell was used as the entrypoint/exec point into a container with an \
        attached terminal.
  condition: >
    spawned_process and container
    and shell_procs and proc.tty != 0
    and container_entrypoint
    and not user_expected_terminal_shell_in_container_conditions
  output: >
    Opened shell: %evt.time,%user.name,%container.name 
  priority: ALERT 
  tags: [container, shell, mitre_execution]
```

[![1](assets/1.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO2-1)
: 

違反時に表示されるログ出力を簡素化する。

[![2](assets/2.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO2-2)
: 

規則違反を`ALERT` の優先順位で扱う。

`falco_rules.local.yaml` に規則を追加した後、Falcoに変更を取り込ませる必要がある。以下のコマンドでFalcoサービスを再起動する：

```
$ sudo systemctl restart falco
```

その結果、コンテナへのシェルの試みは、以下に示すように、異なる出力形式と優先順位でログに記録される：

```
$ sudo journalctl -fu falco
...
Jan 24 21:19:13 kube-worker-1 falco[100017]: 21:19:13.961970887: Alert Opened \
shell: 21:19:13.961970887,<NA>,nginx
```

既存のFalco規則をオーバーライドすることに加えて、`/etc/falco/falco_rules.local.yaml` で独自のカスタム規則を定義することもできる。カスタムルールの作成は本書の範囲外であるが、このトピックに関する情報はFalcoのドキュメンテーションにたくさん見つかるはずである。

# コンテナの不変性を確保する

コンテナはデフォルトで可変性である。コンテナを起動した後、そのコンテナに対してシェルを開いたり、既存のソフトウェアのパッチをインストールしたり、ファイルを修正したり、構成を変更したりすることができる。可変性コンテナでは、攻撃者が悪意のあるソフトウェアをダウンロードまたはインストールすることで、コンテナにアクセスできるようになる。

この状況に対抗するには、コンテナを不変性状態で運用するようにする。つまり、コンテナのファイルシステムへの書き込み演算子を禁止するか、あるいはコンテナへのシェル・アクセスを禁止する。依存関係の更新やアプリケーションの新機能の組み込みなど、コンテナに大幅な変更を加える必要がある場合は、コンテナ自体を手動で変更するのではなく、コンテナイメージの新しいタグをリリースする。そうすれば、コンテナ・イメージの新しいタグをPodに代入することで、内部を直接修正する必要がなくなる。

## シナリオ攻撃者が悪意のあるソフトウェアをインストールする

[図 7-3](#ch07.html#installs-malicious-software-attacker)は、攻撃者が盗んだ資格情報を使用してコンテナにアクセスするシナリオを示している。攻撃者は、コンテナがルートファイルシステムへの書き込み操作を許可していることを前提に、悪意のあるソフトウ ェアをダウンロードしてインストールする。悪意のあるソフトウエアはコンテナ内の活動を監視し続ける。例えば、アプリケーションログを解析して機密情報を探し出す。そして、その情報をKubernetesクラスタの外部にあるサーバに送信する可能性がある。その結果、データはシステムの他の部分にログインする手段として使用される。

![](assets/ckss_0703.png)

次のセクションでは、ディストロのないコンテナ・イメージを使い、ConfigMapやSecretの助けを借りて設定データを注入し、読み取り専用のコンテナ・ファイルシステムを設定することで、このような状況を防ぐ方法を学ぶ。これらのセット全体が、コンテナを真の不変性とする。

## ディストロレス・コンテナ・イメージを使う

コンテナの世界では、ディストロレス・コンテナ・イメージの人気が高まっている。これは、アプリケーションとその実行時の依存関係のみをバンドルし、同時にシェル、パッケージ・マネージャー、ライブラリなどのオペレーティング・システムを可能な限り削除する。Distrolessコンテナ・イメージはサイズが小さいだけでなく、より安全だ。攻撃者はコンテナにシェルを入れることができないため、ファイルシステムを悪用して悪意のあるソフトウェアをインストールすることはできない。ディストリビューションレス・コンテナー・イメージを使用することは、不変性コンテナを作成する際の防御の第一線となる。ディストリビューションレス・コンテナ・イメージについては、[「サイズの小さいベース・イメージを選ぶ](#ch06.html#small-sized-base-image)」ですでに説明した。詳細はそのセクションを参照のこと。

## ConfigMapまたはシークレットでコンテナを設定する

異なるデプロイ環境では、ランタイム構成が異なっていても、同じコンテナイメー ジを使用するのがベストプラクティスである。資格情報やシステムの他の部分への接続URLなど、環境固有の設定はすべて外部化する必要がある。Kubernetesでは、ConfigMapまたはSecretを使用して、環境変数またはVolumesを介してマウントされたファイルとして設定データをコンテナに注入できる。[図7-4は](#ch07.html#configmap-runtime-configuration)、同じコンテナイメージを再利用して、開発クラスタと本番クラスタでPodを構成している様子を示している。環境に固有の設定データは ConfigMap によって提供される。

![](assets/ckss_0704.png)

環境固有のコンテナ・イメージの作成を避けることで、作成プロセスが簡素化され、偶発的なセキュ リティ・リスクが発生するリスクが減り、機能のテストが容易になる。ランタイム値の注入は、コンテナ起動後にコンテナを変更する必要がないため、コンテナを不変性にするための鍵となる。

コンテナ内でSecretsを環境変数として使用する場合は、ログメッセージを書く際に、誤って値をプレーンテキスト値として標準出力に記録しないように注意すること。コンテナのログにアクセスできる人なら誰でも、Secretsの値を解析できてしまう。抜き取り検査として、アプリケーションコードの中でシークレットを使用している場所を特定し、暴露のリスクを評価する。

ConfigMapsとSecretsの作成、設定、コンシューマについてブラッシュアップするには、[Kubernetesのドキュメントを](https://oreil.ly/RjxjE)再確認してほしい。

## 読み取り専用コンテナのルートファイルシステムを設定する

コンテナ不変性のもう1つの側面は、コンテナのファイルシステムへの書き込みアクセ スを防止することである。この実行時の振る舞いは、属性`spec.containers[].securityContext.readOnlyRootFilesystem` に値`true` を代入することで設定できる。

それでもなお、機能要件を満たすために書き込みアクセスが必要な関数もある。たとえば、[nginx は](https://www.nginx.com)`/var/run`、`/var/cache/nginx` 、`/usr/local/nginx` ディレクトリに書き込む必要がある。`readOnlyRootFilesystem` から`true` のセットと組み合わせて、これらのディレクトリを書き込み可能にするボリュームを宣言できる。[例7-8は](#ch07.html#nginx-immutable-container)、nginxを実行する不変性コンテナのYAMLマニフェストを示す。

##### 例7-8. ルートファイルシステムへの書き込みアクセスを禁止するコンテナ

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.21.6
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: nginx-run
      mountPath: /var/run
    - name: nginx-cache
      mountPath: /var/cache/nginx
    - name: nginx-data
      mountPath: /usr/local/nginx
  volumes:
  - name: nginx-run
    emptyDir: {}
  - name: nginx-data
    emptyDir: {}
  - name: nginx-cache
    emptyDir: {}
```

Podを作成する前に、アプリケーションのファイルシステム読み取り/書き込み要件を特定する。Volumesの助けを借りて書き込みマウントパスを設定する。それ以外のファイルシステムのパスは読み取り専用にする。

# 監査ログを使ってアクセスを監視する

Kubernetes管理者にとって、クラスタで発生したイベントの記録を残すことは必須だ。これらの記録はリアルタイムで侵入を検知するのに役立つし、トラブルシューティングのために設定変更を追跡するのに使うこともできる。*監査ログは*、APIサーバが受け取ったイベントの時系列ビューを提供する。

## シナリオ管理者は悪意のあるイベントをリアルタイムで監視できる

[図7-](#ch07.html#observing-audit-log-attacker)5はKubernetes APIイベントを監視する利点を示している。このシナリオでは、攻撃者はAPIサーバへの呼び出しを試みている。関心のあるイベントは監査ログメカニズムによってキャプチャされている。管理者はこれらのイベントをいつでも閲覧し、侵入の試みを特定し、対策を講じることができる。

![](assets/ckss_0705.png)

ここでは、ユースケースの1つ、セキュリティ上の懸念に当てはまるものだけをレビューした。企業内部のAPIリクエストをトレースする機能を過小評価すべきではない。監査ログをレビューすることで、管理者はKubernetesオブジェクトを作成しようとしているアプリケーション開発者にガイダンスを提供したり、クラスタの振る舞いに不具合が生じた可能性のある設定変更を再構築したりすることができる。

## 監査ログを理解する

Kubernetesは、エンドユーザがAPIサーバに行ったリクエストや、コントロールプレーン自身が発したイベントに対してトリガーしたイベントのレコードを保存することができる。監査ログのエントリは[JSON行](https://jsonlines.org)形式で存在し、以下の情報で構成されるが、これに限定されるものではない：

*   どのような出来事が起きたのか？
    
*   誰が引き金となったのか？
    
*   そのきっかけはいつだったのか？
    
*   どのKubernetesコンポーネントがリクエストを処理したか？
    

記録されるイベントのタイプと対応するリクエストデータは、*監査ポリシーによって*定義される。監査ポリシーはこれらの規則を指定するYAMLマニフェストであり、APIサーバプロセスに提供されなければならない。

*監査バックエンドは*、監査ポリシーによって定義されるように、記録された監査イベントを保存する責任を負う。バックエンドには2つの設定可能なオプションがある：

*   イベントをファイルに書き出すログバックエンド。
    
*   ウェブフック・バックエンドは、HTTP(S)を介して外部サービスにイベントを送信する-例えば、集中型ロギング・モニタリング・システムを統合する目的で。このようなバックエンドは、クラッシュしたアプリケーションのようなランタイムの問題をデバッグするのに役立つ。
    

[図 7-6 は](#ch07.html#audit-log-architecture)、監査ログを構成するために必要なすべての部品をまとめたものである。以下のセクションでは、それらを構成する詳細を説明する。

![](assets/ckss_0706.png)

監査ポリシーファイルとその構成オプションについて詳しく見てみよう。

## 監査ポリシーファイルの作成

監査ポリシーファイルは事実上、`Policy` リソースの YAML マニフェストである。APIサーバによって受信されたイベントはすべて、定義順にポリシーファイルで定義された規則と照合される。一致する規則が発見された場合、イベントは宣言された監査レベルでログに記録される。[表 7-1](#ch07.html#audit-levels)に、利用可能なすべての監査レベルをリストする。

表7-1. 監査レベル

  
| レベル || 効果 |
| --- | --- |
| `None` || この規則に一致するイベントをログに記録しない。 |
| --- | --- |
| `Metadata` || イベントのリクエストメタデータのみをログに記録する。 |
| `Request` || イベントのメタデータとリクエストボディを記録する。 |
| `RequestResponse` || イベントのメタデータ、リクエスト、レスポンスボディを記録する。 |

[例 7-9](#ch07.html#audit-policy)は例示的な監査ポリシーである。規則は、`rules` という属性を持つ項目の配列として指定される。各規則は、レベル、適用するリソースタイプと API グループ、およびオプションのネームスペースを宣言する。

##### 例7-9. 監査ポリシーファイルの内容

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
      resources: ["pods/log", "pods/status"]
```

[![1](assets/1.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO3-1)
: 

`RequestReceived` 、すべてのリクエストに対してログが生成されないようにする。

[![2](assets/2.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO3-2)
: 

`RequestResponse` レベルで Pod の変更を記録する。

[![3](assets/3.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO3-3)
: 

`Metadata` レベルで、特殊化された Pod イベント（ログやステータス要求など）を記録する。

先ほどの監査ポリシーはそれほど広くはないが、その形式の印象を与えるはずだ。その他の例や詳細については、[Kubernetesのドキュメントを](https://oreil.ly/Zozkf)参照してほしい。

監査ポリシー・ファイルが作成されると、APIサーバー・プロセスによって消費される。ファイル`/etc/kubernetes/manifests/kube-apiserver.yaml` のフラグ`--audit-policy-file` をAPIサーバー・プロセスに追加する。パラメータに代入される値は、監査ポリシー・ファイルへの完全修飾パスである。

次は、ファイルベースのログバックエンドとウェブフックバックエンドについて、APIサーバの監査ロギングの構成に必要なセットについて説明する。

## ログバックエンドを設定する

ファイルベースのログバックエンドをセットアップするには、3つの構成ファイルをファイル`/etc/kubernetes/manifests/kube-apiserver.yaml` に追加する必要がある。以下のリストに設定をまとめる：

1.  フラグ`--audit-policy-file` は監査ポリシーファイルを指し、フラグ`--audit-log-path` はログ出力ファイルを指す。
    
2.  監査ログポリシーファイルとログ出力ディレクトリのボリュームマウントパスを追加する。
    
3.  監査ログポリシーファイルとログ出力ディレクトリのホストパスにボリューム定義を追加する。
    

[例7-10は](#ch07.html#log-backend-api-server)、APIサーバー構成ファイルの変更内容を示す。

##### 例7-10. 監査ポリシーファイルと監査ログファイルを構成する

```
...
spec:
  containers:
  - command:
    - kube-apiserver
    - --audit-policy-file=/etc/kubernetes/audit-policy.yaml 
    - --audit-log-path=/var/log/kubernetes/audit/audit.log 
    ...
    volumeMounts:
    - mountPath: /etc/kubernetes/audit-policy.yaml 
      name: audit
      readOnly: true
    - mountPath: /var/log/kubernetes/audit/ 
      name: audit-log
      readOnly: false
  ...
  volumes:
  - name: audit 
    hostPath:
      path: /etc/kubernetes/audit-policy.yaml
      type: File
  - name: audit-log 
    hostPath:
      path: /var/log/kubernetes/audit/
      type: DirectoryOrCreate
```

[![1](assets/1.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO4-1)
: 

ポリシーファイルとログファイルの場所をAPIサーバプロセスに提供する。

[![2](assets/2.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO4-3)
: 

ポリシーファイルと監査ログディレクトリを与えられたパスにマウントする。

[![3](assets/3.png)](#ch07.html#co_monitoring__logging__and_runtime_security_CO4-5)
: 

ポリシーファイルと監査ログディレクトリのボリュームを定義する。

ログバックエンドの実行時の振る舞いは、APIサーバープロセスに追加のフラグを渡すことで、さらにカスタマイズすることができる。例えば、`--audit-log-maxage` フラグを指定することで、古い監査ログファイルを保持する最大日数を指定できる。フラグの完全なリストを見るには、[Kubernetesのドキュメントを](https://oreil.ly/L-63d)参照すること。

そろそろログエントリーを作成しよう。次の`kubectl` コマンドは、`nginx` という名前のPodを作成するリクエストをAPIサーバに送信する：

```
$ kubectl run nginx --image=nginx:1.21.6
pod/nginx created
```

前のステップでは、`/var/log/kubernetes/audit/audit.log` に監査ログファイルを構成した。監査ポリシーの規則によっては、エ ントリの数が膨大になり、特定のイベントを発見するのが難しくなるかもしれない。設定されたイベントをフィルタリングする簡単な方法は、`apiVersion` 属性に代入された値`audit.k8s.io/v1` を検索することである。以下のコマンドは、関連するログエントリを発見する。1つは`RequestResponse` レベル用、もう1つは`Metadata` レベル用である：

```
$ sudo grep 'audit.k8s.io/v1' /var/log/kubernetes/audit/audit.log
...
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"RequestResponse", \
"auditID":"285f4b99-951e-405b-b5de-6b66295074f4","stage":"ResponseComplete", \
"requestURI":"/api/v1/namespaces/default/pods/nginx","verb":"get", \
"user":{"username":"system:node:node01","groups":["system:nodes", \
"system:authenticated"]},"sourceIPs":["172.28.116.6"],"userAgent": \
"kubelet/v1.26.0 (linux/amd64) kubernetes/b46a3f8","objectRef": \
{"resource":"pods","namespace":"default","name":"nginx","apiVersion":"v1"}, \
"responseStatus":{"metadata":{},"code":200},"responseObject":{"kind":"Pod", \
"apiVersion":"v1","metadata":{"name":"nginx","namespace":"default", \
...
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID": \
"5c8e5ecc-0ce0-49e0-8ab2-368284f2f785","stage":"ResponseComplete", \
"requestURI":"/api/v1/namespaces/default/pods/nginx/status","verb":"patch", \
"user":{"username":"system:node:node01","groups":["system:nodes", \
"system:authenticated"]},"sourceIPs":["172.28.116.6"],"userAgent": \
"kubelet/v1.26.0 (linux/amd64) kubernetes/b46a3f8","objectRef": \
{"resource":"pods","namespace":"default","name":"nginx","apiVersion":"v1", \
"subresource":"status"},"responseStatus":{"metadata":{},"code":200}, \
...
```

## Webhookバックエンドを設定する

Webhookバックエンドの設定は、ログバックエンドの設定とは異なっている。ファイルの代わりに外部サービスにHTTP(S)リクエストを送信するようにAPIサーバに指示する必要がある。外部サービス、ウェブフック、認証に必要な資格情報への構成は、[「ImagePolicyWebhook Admission Controllerプラグインの構成](#ch06.html#configuring-imagepolicywebhook-admission-controller-plugin)」で行ったのと同様に、[kubeconfig](https://oreil.ly/bUnmO)ファイルで定義する。

フラグ`--audit-webhook-config-file` をファイル`/etc/kubernetes/manifests/kube-apiserver.yaml` のAPIサーバープロセスに追加し、kubeconfigファイルの場所をポイントする。フラグ`--audit-webhook-initial-backoff` は、外部サービスへの初期リクエスト後、再試行するまでの待機時間を定義する。監査ポリシーファイルを指すように、フラグ`--audit-policy-file` を代入する必要がある。

# 概要

Kubernetesクラスタにおけるイベントの監視とロギングは、すべての管理者の重要な義務である。我々は、セキュリティ関連のイベントを特定し、フィルタリングするために Falco を使用した。さまざまな構成ファイルの目的と構文、そしてログの中から関連するアラートを発見する方法について学んだ。

振る舞い分析ツールの採用に加え、APIサーバに到達するリクエストの監査ロギングをセットアップしたい。監査ロギングは、コントロールプレーンノード上のファイルか、HTTP(S)呼び出しによる外部サービスのどちらかのバックエンドに、構成されたイベントを記録する。ここでは、API サーバープロセスに対して監査ロギングを有効にするプロセスについて説明する。

より安全なコンテナへの賢明な一歩は、コンテナを不変性にすることだ。不変性コンテナは読み取り専用のファイルシステムのみをサポートし、潜在的な攻撃者が悪意のあるソフトウェアをインストールできないようにする。コンテナ内で実行するアプリケーションがデータを書き込む必要がある場合は、ボリュームをマウントする。ディストロなしのコンテナ・イメージを使用して、攻撃者がコンテナにシェルを入れないようにする。

# 試験の要点

ファルコの設定と演算子を練習する。
: 

ファルコは間違いなく試験中にトピックとして出てくるだろう。構成ファイルの規則をどのように読み取り、変更するかを理解する必要がある。自分で書く必要がある場合に備えて、シンタックスとオプションをより詳細に閲覧することをお勧めする。Falco を実行するための主なエントリポイントはコマンドラインツールである。このツールは試験環境にプリインストールされていると考えるのが妥当であろう。

不変性コンテナを識別する方法を知る。
: 

不変性コンテナは、この試験領域の中心的なトピックである。Podに`spec.containers[].securityContext.readOnlyRootFilesystem` 属性を設定する方法と、コンテナ・プロセスで書き込み演算子が必要な場合に備えて特定のパスにVolumeをマウントする方法を理解する。

監査ログの設定オプションを深く理解する。
: 

監査ロギングのセットアップは、2つのステップからなる。一つは、監査ポリシーファイルの構文と構造を理解する必要がある。もう1つは、APIサーバーをどのように構成して、監査ポリシーファイルを消費し、バックエンドへの参照を提供し、関連するファイルシステムのVolumeをマウントするかである。これらの面をすべて実践的に練習するようにしよう。

# サンプル練習

1.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ)ディレクトリ*a/ch07/falcoに*移動する。`vagrant up` コマンドを使って、クラスタを実行している仮想マシンを立ち上げる。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使用してクラスタをシャットダウンする。Falcoはすでにsystemdサービスとして稼働している。
    
    `malicious` という名前の既存の Pod で実行されているプロセスを検査する。Falco のログを見て、規則がそのプロセスのログを作成したかどうかを確認する。
    
    出力を`<timestamp>,<username>,<container-id>` に変更して、イベントのログを作成する既存の規則を再構成する。ファルコのログから変更されたログエントリを発見する。
    
    `/var/logs/falco.log` のファイルにログを書き込むように Falco を再設定する。標準出力チャンネルを無効にする。Falco が新しいメッセージをログファイルに追加することを確認する。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    
2.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ) *app-a/ch07/immutable-container*ディレクトリに移動する。`kubectl apply -f setup.yaml` コマンドを実行する。
    
    `default` 名前空間の YAML マニフェストによって作成された Pod を検査する。コンテナが不変性とみなされるように、Pod に関連する変更を加える。
    
3.  チェックアウトしたGitHubリポジトリ[*bmuschko/cks-study-guideの*](https://oreil.ly/sImXZ) *app-a/ch07/audit-log*ディレクトリに移動する。`vagrant up` コマンドを使って、クラスタを実行している仮想マシンを立ち上げる。このクラスタは、`kube-control-plane` という単一コントロールプレーンノードと、`kube-worker-1` というワーカーノードから構成される。終了したら、`vagrant destroy -f` を使ってクラスタをシャットダウンする。
    
    `/etc/kubernetes/audit/rules/audit-policy.yaml` にある既存の監査ポリシーファイルを編集する。`Metadata` 、ConfigMapsとSecretsのイベントを記録する規則を追加する。`Request` レベルでサービスのイベントをログ記録する別の規則を追加する。
    
    APIサーバーが監査ポリシーファイルを使用するように構成する。ログはファイル`/var/log/kubernetes/audit/logs/apiserver.log` に書き込む。監査ログファイルを保持する最大日数を5日と定義する。
    
    ログファイルが作成され、構成されたイベントに一致するエントリが少なくとも1つ含まれていることを確認する。
    
    *前提条件*この演習には、[Vagrantと](https://oreil.ly/FiyeH) [VirtualBoxの](https://oreil.ly/WW8IK)インストールが必要である。
    

# 付録 復習問題の解答

# 章、"クラスタのセットアップ"

1.  ネットワークポリシーを定義するために、`deny-egress-external.yaml` という名前のファイルを作成する。ネットワークポリシーはPodセレクタを`app=backend` に設定し、`Egress` ポリシータイプを定義する必要がある。プロトコルUDPとTCPのポート53を許可することを確認する。egressポリシーの名前空間セレクタは、`{}` を使用してすべての名前空間を選択する必要がある：
    
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
    
    `apply` コマンドを実行して、YAML ファイルからネットワーク・ポリシー・オブジェクトをインスタンス化する：
    
    ```
    $ kubectl apply -f deny-egress-external.yaml
    ```
    
2.  ネットワークポリシーのラベル選択と一致しないPodは、クラスタ外のURLへの呼び出しを行うことができる。この場合、ラベル代入は`app=frontend` ：
    
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
    
3.  ネットワークポリシーのラベル選択に一致するPodは、クラスタ外のURLへの呼び出しを行うことができない。この場合、ラベル代入は`app=backend` ：
    
    ```
    $ kubectl run web --image=busybox:1.36.0 -l app=backend --port=80 -it \
      --rm --restart=Never -- wget http://google.com --timeout=5 --tries=1
    wget: download timed out
    pod "web" deleted
    pod default/web terminated (Error)
    ```
    
4.  まず、Dashboardがすでにインストールされているかどうかを確認する。Dashboardが通常作成するネームスペースを確認することができる：
    
    ```
    $ kubectl get ns kubernetes-dashboard
    NAME                   STATUS   AGE
    kubernetes-dashboard   Active   109s
    ```
    
    名前空間が存在しない場合は、Dashboard がまだインストールされていないと考えられる。以下のコマンドでインストールする：
    
    ```
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/\
    dashboard/v2.6.0/aio/deploy/recommended.yaml
    ```
    
    ServiceAccount、ClusterRole、ClusterRoleBindingを作成する。ClusterRoleがデプロイオブジェクトのリストのみを許可することを確認する。以下のYAMLマニフェストがファイル`dashboard-observer-user.yaml` に保存されている：
    
    ```
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
    
    以下のコマンドでオブジェクトを作成する：
    
    ```
    $ kubectl apply -f dashboard-observer-user.yaml
    ```
    
5.  以下のコマンドを実行して、ServiceAccount用のトークンを作成する。オプション`--duration 0s` 、トークンの有効期限が切れることはない。コマンドのコンソール出力に表示されたトークンをコピーする：
    
    ```
    $ kubectl create token observer-user -n kubernetes-dashboard \
      --duration 0s
    eyJhbGciOiJSUzI1NiIsImtpZCI6Ik5lNFMxZ1...
    ```
    
    Proxyコマンドを実行し、[*http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy のリンクをブラウザで*](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy)開く：
    
    ```
    $ kubectl proxy
    ```
    
    トークン」認証メソッドを選択し、先ほどコピーしたトークンを貼り付ける。ダッシュボードにサインインする。デプロイ・オブジェクトだけがリスト可能であることがわかるはずである（[図 A-1](#app01.html#dashboard-deployments) 参照）。
    
    それ以外のオブジェクトは "There is nothing to display here." と表示される。[図A-](#app01.html#dashboard-pods)2はPodのリストをレンダリングしたものである。
    
    ![](assets/ckss_aa01.png)![](assets/ckss_aa02.png)
6.  以下のコマンドでAPIサーバーのバイナリをダウンロードする：
    
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
    

# 章、"クラスタ・ハードニング"

1.   `openssl` 実行ファイルを使用して秘密鍵を作成する。 のように、表現力豊かなファイル名を指定する。 オプションは、ユーザー名（CN）とグループ（O）を指定する。以下のコマンドでは、ユーザー名を 、グループ名を とする：`jill.key` `-subj` `jill` `observer`
    
    ```
    $ openssl genrsa -out jill.key 2048
    $ openssl req -new -key jill.key -out jill.csr -subj \
      "/CN=jill/O=observer"
    ```
    
    CSRファイル・コンテンツのbase64エンコード値を以下のコマンドで取得する。次のステップでCertificateSigningRequestオブジェクトを作成する際に必要になる：
    
    ```
    $ cat jill.csr | base64 | tr -d "\n"
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tL...
    ```
    
    以下のスクリプトは、CertificateSigningRequestオブジェクトを作成する：
    
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
    
    kubeconfigファイルにユーザを追加し、ユーザのコンテキストを追加する。ここで使用しているクラスタ名は`minikube` 。 Kubernetes環境では異なるかもしれない：
    
    ```
    $ kubectl config set-credentials jill --client-key=jill.key \
      --client-certificate=jill.crt --embed-certs=true
    $ kubectl config set-context jill --cluster=minikube --user=jill
    ```
    
2.  RoleとRoleBindingを作成する。以下の命令形代入コマンドは、Pod、ConfigMaps、および Secrets に関する動詞`get` 、`list` 、および`watch` を、`group` 型の`observer` という名前のサブジェクトに代入する。ユーザ`jill` はグループの一部である：
    
    ```
    $ kubectl create role observer --verb=create --verb=get --verb=list \
      --verb=watch --resource=pods --resource=configmaps --resource=secrets
    $ kubectl create rolebinding observer-binding --role=observer \
      --group=observer
    ```
    
3.  ユーザのコンテキストに切り替える：
    
    ```
    $ kubectl config use-context jill
    ```
    
    ここでは、ConfigMapオブジェクトをリストする、許可された操作を1つ選ぶ。ユーザは呼び出しのマッピングを許可されている：
    
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
    
4.  名前空間`t23` を作成する：
    
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
    
    Podのログを確認する。以下のログ出力に示されているように、API呼び出しは許可されていない：
    
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
    
5.  YAML マニフェストをファイル`clusterrole.yaml` に作成する：
    
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
    
    ファイル`rolebinding.yaml` で定義されたRoleBindingでClusterRoleを参照する。サブジェクトは、名前空間`t23` のサービスアカウント`api-call` をリストしなければならない：
    
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
    
    これで、コンテナ内で実行されているAPI呼び出しが認証され、`default` 名前空間内のサービスオブジェクトのリストが許可されるはずである。以下の出力に示すように、ネームスペースは現在、少なくとも1つのサービスオブジェクト（`kubernetes.default` Service）をホストしている：
    
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
    
6.  以下のコマンドを使用して、サービス・アカウントのトークンを作成する：
    
    ```
    $ kubectl create token api-call -n t23
    eyJhbGciOiJSUzI1NiIsImtpZCI6IjBtQkJzVWlsQjl...
    ```
    
    ライブ・オブジェクトを削除して再作成し、既存のPod定義を変更する。以下のように、トークンの自動マウントを無効にする属性を追加する：
    
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
    
7.  このサンプル練習の解答は、多くの手動ステップを必要とする。以下のコマンドは出力を表示しない。
    
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
    

# 第4章、"システム・ハードニング"

1.  以下のコマンドでワーカー・ノードにシェル・インする：
    
    ```
    $ vagrant ssh kube-worker-1
    ```
    
    ポート21を公開しているプロセスを特定する。これを行う一つの方法は、`lsof` コマンドを使うことである。ポートを公開するコマンドは`vsftpd` である：
    
    ```
    $ sudo lsof -i :21
    COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    vsftpd  10178 root    3u  IPv6  56850      0t0  TCP *:ftp (LISTEN)
    ```
    
    あるいは、次のように`ss` ：
    
    ```
    $ sudo ss -at -pn '( dport = :21 or sport = :21 )'
    State   Recv-Q   Send-Q   Local Address:Port \
       Peer Address:Port   Process
    LISTEN  0   32   *:21 \
       *:*   users:(("vsftpd",pid=10178,fd=3))
    ```
    
    プロセス`vsftpd` はサービスとして開始された：
    
    ```
    $ sudo systemctl status vsftpd
    ● vsftpd.service - vsftpd FTP server
         Loaded: loaded (/lib/systemd/system/vsftpd.service; enabled; \
                 vendor preset: enabled)
         Active: active (running) since Thu 2022-10-06 14:39:12 UTC; \
                 11min ago
       Main PID: 10178 (vsftpd)
          Tasks: 1 (limit: 1131)
         Memory: 604.0K
         CGroup: /system.slice/vsftpd.service
                 └─10178 /usr/sbin/vsftpd /etc/vsftpd.conf
    
    Oct 06 14:39:12 kube-worker-1 systemd[1]: Starting vsftpd FTP server...
    Oct 06 14:39:12 kube-worker-1 systemd[1]: Started vsftpd FTP server.
    ```
    
    サービスを停止し、パッケージをデインストールする：
    
    ```
    $ sudo systemctl stop vsftpd
    $ sudo systemctl disable vsftpd
    $ sudo apt purge --auto-remove -y vsftpd
    ```
    
    ポートを確認すると、もうリストには載っていない：
    
    ```
    $ sudo lsof -i :21
    ```
    
    ノードの外に出る：
    
    ```
    $ exit
    ```
    
2.  以下のコマンドでワーカー・ノードにシェル・インする：
    
    ```
    $ vagrant ssh kube-worker-1
    ```
    
    コマンド`sudo vim /etc/apparmor.d/network-deny` を使用して、AppArmorプロファイルを`/etc/apparmor.d/network-deny` に作成する。ファイルの内容は以下のようになる：
    
    ```
    #include <tunables/global>
    
    profile network-deny flags=(attach_disconnected) {
      #include <abstractions/base>
    
      network,
    }
    ```
    
    以下のコマンドを実行して、AppArmorプロファイルを強制する：
    
    ```
    $ sudo apparmor_parser /etc/apparmor.d/network-deny
    ```
    
    AppArmor用のアノテーションを追加するために、既存のPodオブジェクトを変更することはできない。まずオブジェクトを削除する必要がある。Podの定義をファイルに書き出す：
    
    ```
    $ kubectl get pod -o yaml > pod.yaml
    $ kubectl delete pod network-call
    ```
    
    `pod.yaml` ファイルを編集し、AppArmorアノテーションを追加する。関連する注釈では、コンテナの名前`network-call` をキー・サフィックスの一部として使用し、`localhost/network-deny` を値として使用する。接尾辞`network-deny` は、AppArmorプロファイルの名前を指す。最終的なコンテンツは、少し整理すると以下のようになる：
    
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: network-call
      annotations:
        container.apparmor.security.beta.kubernetes.io/network-call: \
        localhost/network-deny
    spec:
      containers:
      - name: network-call
        image: alpine/curl:3.14
        command: ["sh", "-c", "while true; do ping -c 1 google.com; \
                  sleep 5; done"]
    ```
    
    マニフェストからPodを作成する。数秒後、Podは「Running」ステータスに移行するはずだ：
    
    ```
    $ kubectl create -f pod.yaml
    $ kubectl get pod network-call
    NAME           READY   STATUS    RESTARTS   AGE
    network-call   1/1     Running   0          27s
    ```
    
    AppArmorがPodのネットワーク呼び出しを防止している。ログをチェックして確認することができる：
    
    ```
    $ kubectl logs network-call
    ...
    sh: ping: Permission denied
    sh: sleep: Permission denied
    ```
    
    ノードの外に出る：
    
    ```
    $ exit
    ```
    
3.  以下のコマンドでワーカー・ノードにシェル・インする：
    
    ```
    $ vagrant ssh kube-worker-1
    ```
    
    seccompプロファイルのターゲットディレクトリを作成する：
    
    ```
    $ sudo mkdir -p /var/lib/kubelet/seccomp/profiles
    ```
    
    ディレクトリ`/var/lib/kubelet/seccomp/profiles` に、以下の内容のファイル`audit.json` を追加する：
    
    ```
    {
        "defaultAction": "SCMP_ACT_LOG"
    }
    ```
    
    セキュリティコンテキスト経由でseccomp設定を追加するために、既存のPodオブジェクトを修正することはできない。まずオブジェクトを削除する必要がある。Podの定義をファイルに書き出す：
    
    ```
    $ kubectl get pod -o yaml > pod.yaml
    $ kubectl delete pod network-call
    ```
    
    `pod.yaml` ファイルを編集する。seccompのプロファイルを定義に点す。最終的には以下のようになる：
    
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: network-call
    spec:
      securityContext:
        seccompProfile:
          type: Localhost
          localhostProfile: profiles/audit.json
      containers:
      - name: network-call
        image: alpine/curl:3.14
        command: ["sh", "-c", "while true; do ping -c 1 google.com; \
                  sleep 5; done"]
        securityContext:
          allowPrivilegeEscalation: false
    ```
    
    マニフェストからPodを作成する。数秒後、Podは「Running」ステータスに移行するはずだ：
    
    ```
    $ kubectl create -f pod.yaml
    $ kubectl get pod network-call
    NAME           READY   STATUS    RESTARTS   AGE
    network-call   1/1     Running   0          27s
    ```
    
    例えば、`sleep` コマンドのようなシステムコールのログエントリを発見できるはずだ：
    
    ```
    $ sudo cat /var/log/syslog
    Oct  6 16:25:06 ubuntu-focal kernel: [ 2114.894122] audit: type=1326 \
    audit(1665073506.099:23761): auid=4294967295 uid=0 gid=0 \
    ses=4294967295 pid=19226 comm="sleep" exe="/bin/busybox" \
    sig=0 arch=c000003e syscall=231 compat=0 ip=0x7fc026adbf0b \
    code=0x7ffc0000
    ```
    
    ノードの外に出る：
    
    ```
    $ exit
    ```
    
    ファイル`pod.yaml` にPod定義を作成する：
    
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: sysctl-pod
    spec:
      securityContext:
        sysctls:
        - name: net.core.somaxconn
          value: "1024"
        - name: debug.iotrace
          value: "1"
      containers:
      - name: nginx
        image: nginx:1.23.1
    ```
    
    Podを作成し、ステータスを確認する。ステータスが "SysctlForbidden "であることがわかるだろう：
    
    ```
    $ kubectl create -f pod.yaml
    $ kubectl get pods
    NAME         READY   STATUS            RESTARTS   AGE
    sysctl-pod   0/1     SysctlForbidden   0          4s
    ```
    
    イベントログを見れば、その理由がわかるだろう：
    
    ```
    $ kubectl describe pod sysctl-pod
    ...
    Events:
      Type     Reason           Age    From    \
                 Message
      ----     ------           ----   ----    \
                 -------
      Warning  SysctlForbidden  2m48s  kubelet \
                 forbidden sysctl: "net.core.somaxconn" \
                 not allowlisted
    ```
    

# 第5章 "マイクロサービスの脆弱性を最小限に抑える"

1.  ファイル`busybox-security-context.yaml` でセキュリティ設定を持つPodを定義する。以下のYAMLマニフェストの内容が発見できる：
    
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
    
    以下のコマンドでPodを作成する：
    
    ```
    $ kubectl apply -f busybox-security-context.yaml
    $ kubectl get pod busybox-security-context
    NAME                       READY   STATUS    RESTARTS   AGE
    busybox-security-context   1/1     Running   0          54s
    ```
    
    コンテナに入り、ファイルを作成する。セキュリティ・コンテキストで定義されているように、ファイル・グループが2000であることが発見できるだろう：
    
    ```
    $ kubectl exec busybox-security-context -it -- /bin/sh
    / $ cd /data/test
    /data/test $ touch hello.txt
    /data/test $ ls -l
    total 0
    -rw-r--r--    1 1000     2000             0 Nov 21 18:29 hello.txt
    /data/test $ exit
    ```
    
2.  ファイル`psa-namespace.yaml` で`audited` という名前空間を指定する。PSAラベルに`baseline` レベルと`warn` モードをセットする：
    
    ```
    apiVersion: v1
    kind: Namespace
    metadata:
      name: audited
      labels:
        pod-security.kubernetes.io/warn: baseline
    ```
    
    YAML マニフェストからネームスペースを作成する：
    
    ```
    $ kubectl apply -f psa-namespace.yaml
    ```
    
    ファイル`psa-pod.yaml` で次の Pod 構成を使用すると、エラーを生成できる。YAML マニフェストは`hostNetwork: true` という属性をセットしているが、これは[`baseline` レベルでは](https://oreil.ly/c8JEW)許可されていない：
    
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
    
    Podを作成すると警告メッセージが表示される。それでもPodは作成される。`restricted` レベルでPodを設定することで、Podの作成を防ぐことができる：
    
    ```
    $ kubectl apply -f psa-pod.yaml
    Warning: would violate PodSecurity "baseline:latest": host namespaces \
    (hostNetwork=true)
    pod/busybox created
    $ kubectl get pod busybox -n audited
    NAME      READY   STATUS    RESTARTS   AGE
    busybox   1/1     Running   0          2m21s
    ```
    
3.  Gatekeeperは以下のコマンドでインストールできる：
    
    ```
    $ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
    gatekeeper/master/deploy/gatekeeper.yaml
    ```
    
    Gatekeeper ライブラリには、[レプリカの制限を](https://oreil.ly/gyD1-)定義するための ConstraintTemplate が記述されている。このページで説明されている YAML マニフェストを確認する。次のコマンドでマニフェストを適用する：
    
    ```
    $ kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/\
    gatekeeper-library/master/library/general/replicalimits/template.yaml
    ```
    
    ここで、`replica-limits-constraint.yaml` という名前のファイルでYAMLマニフェストを使って制約を定義する：
    
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
    
    以下のコマンドでコンストレイントを作成する：
    
    ```
    $ kubectl apply -f replica-limits-constraint.yaml
    ```
    
    デプロイは、指定されたレプリカ数が制約の範囲内にある場合にのみ作成できることがわかる：
    
    ```
    $ kubectl create deployment nginx --image=nginx:1.23.2 --replicas=15
    error: failed to create deployment: admission webhook \
    "validation.gatekeeper.sh" denied the request: [replica-limits] \
    The provided number of replicas is not allowed for deployment: nginx. \
    Allowed ranges: {"ranges": [{"max_replicas": 10, "min_replicas": 3}]}
    $ kubectl create deployment nginx --image=nginx:1.23.2 --replicas=7
    deployment.apps/nginx created
    ```
    
4.  [etcdデータの暗号化](#ch05.html#encrypting-etcd-data)」で説明するように、etcdの暗号化を設定する。次に、以下の必須コマンドで新しいシークレットを作成する：
    
    ```
    $ kubectl create secret generic db-credentials \
      --from-literal=api-key=YZvkiWUkycvspyGHk3fQRAkt
    ```
    
    etcdに保存されているシークレットの暗号化された値は、以下のコマンドで確認できる：
    
    ```
    $ sudo ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/\
    etcd/server.key get /registry/secrets/default/db-credentials | hexdump -C
    ```
    
5.  Vagrantを使ってワーカーノードへの対話型シェルを開く：
    
    ```
    $ vagrant ssh kube-worker-1
    ```
    
    以下の YAML マニフェストで RuntimeClass を定義する。内容はファイル`runtime-class.yaml` に格納されている：
    
    ```
    apiVersion: node.k8s.io/v1
    kind: RuntimeClass
    metadata:
      name: container-runtime-sandbox
    handler: runsc
    ```
    
    RuntimeClassオブジェクトを作成する：
    
    ```
    $ kubectl apply -f runtime-class.yaml
    ```
    
    `spec.runtimeClassName` 属性を使用して、RuntimeClass の名前を Pod に代入する。nginx Podはファイル`pod.yaml` で定義されている：
    
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
    
    Podオブジェクトを作成する。Podはステータス「Running」に遷移する：
    
    ```
    $ kubectl apply -f pod.yaml
    $ kubectl get pod nginx
    NAME    READY   STATUS    RESTARTS   AGE
    nginx   1/1     Running   0          2m21s
    ```
    
    ノードの外に出る：
    
    ```
    $ exit
    ```
    

# 第6章、"サプライチェーンの安全保障"

1.  提供されたDockerfileでビルドされた初期コンテナ・イメージのサイズは998MBである。以下のコマンドでコンテナ・イメージをプロデュースし、実行することができる。アプリケーションによって公開されているエンドポイントに到達できるかどうかを確認するために、`curl` コマンドを素早く実行する：
    
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
    
    変更点のひとつは、大きなベースイメージを使わないことだ。`alpine` バージョンのノード・ベース画像に置き換えることができる。また、`latest` 。実際にアプリケーションを実行させたいNode.jsのバージョンを選ぶ。以下のコマンドは、ベース・イメージ`node:19-alpine` 、コンテナ・イメージ・サイズを176MBに縮小したDockerfileを使用している：
    
    ```
    $ docker build . -t node-app:0.0.1
    ...
    $ docker images
    REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
    node-app     0.0.1     ef2fbec41a75   2 seconds ago    176MB
    ```
    
2.  Kyvernoのインストールは、Helmを使うか、プロジェクトのGitHubリポジトリにあるYAMLマニフェストをポイントして行う。ここではYAMLマニフェストを使う：
    
    ```
    $ kubectl create -f https://raw.githubusercontent.com/kyverno/\
    kyverno/main/config/install.yaml
    ```
    
    `restrict-image-registries.yaml` という名前の YAML マニフェストファイルをセットアップする。このファイルに以下の内容を追加する。このマニフェストは、`gcr.io/` で始まるコンテナイメージの使用のみを許可する ClusterPolicy を表す。属性`spec.validationFailureAction` に値`Enforce` を代入する：
    
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
    
    以下のコマンドを実行して、ポリシーがアクティブになったことを確認する。接頭辞`gcr.io/` を使用しないコンテナイメージ定義は拒否される：
    
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
    
3.  Docker Hubの検索機能を使って、イメージ`nginx:1.23.3-alpine` のSHA256ハッシュを発見する。[検索](https://oreil.ly/a4o8E)結果はイメージのタグにつながる。ページの一番上に、ダイジェスト`sha256:c1b9fe3c0c015486cf1e4a0ecabe78d05864475e279638e9713eb55f013f907f` が見つかるはずだ。Pod定義でタグの代わりにダイジェストを使用する。結果は以下のYAMLマニフェストになる：
    
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx@sha256:c1b9fe3c0c015486cf1e4a0ecabe78d05864475e279638 \
               e9713eb55f013f907f
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
    
4.  DockerコンテナでKubesecを実行すると、以下の出力に示すように、たくさんのサジェストが表示される：
    
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
    
5.  既存の`setup.yaml` マニフェストに対して`kubectl apply` コマンドを実行すると、名前付け`r61` に`backend` 、`loop` 、`logstash` という名前の Pod が作成される：
    
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
    
    `kubectl describe` コマンドを使用して、名前空間`r61` 内の各 Pod の画像を確認する。使用されるイメージは`bmuschko/nodejs-hello-world:1.0.0` 、`alpine:3.13.4` 、`elastic/logstash:7.13.3` である：
    
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
    
    Trivy実行ファイルを使って、すべての画像の脆弱性をチェックする：
    
    ```
    $ trivy image bmuschko/nodejs-hello-world:1.0.0
    $ trivy image alpine:3.13.4
    $ trivy image elastic/logstash:7.13.3
    ```
    
    脆弱性のリストをよく見ると、すべてのイメージに深刻度「CRITICAL」の問題が含まれていることが発見できる。結果として、すべてのPodを削除する：
    
    ```
    $ kubectl delete pod backend -n r61
    $ kubectl delete pod logstash -n r61
    $ kubectl delete pod loop -n r61
    ```
    

# 第7章 "モニタリング、ログ、ランタイムセキュリティ"

1.  以下のコマンドでワーカー・ノードにシェル・インする：
    
    ```
    $ vagrant ssh kube-worker-1
    ```
    
    実行中の`malicious` というPodのコマンドと引数を調べる。`/etc/threat` というファイルにメッセージを追加しようとしているのがわかるだろう：
    
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
    
    ファルコのデフォルトルールの一つは、`/etc` ディレクトリへの書き込みを試みるファイル演算子を監視する。標準出力には、すべての書き込み試行に対するメッセージが発見できる：
    
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
    
    "etc opened for writing "という文字列を検索して、`/etc/falco/falco_rules.yaml` のメッセージを生成する規則を見つける。規則は以下のようになる：
    
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
    
    この規則をファイル`/etc/falco/falco_rules.local.yaml` にコピーし、出力定義を以下のように修正する：
    
    ```
    - rule: Write below etc
      desc: an attempt to write to any file below /etc
      condition: write_etc_common
      output: "%evt.time,%user.name,%container.id"
      priority: ERROR
      tags: [filesystem, mitre_persistence]
    ```
    
    ファルコのサービスを再起動し、ファルコのログから変更された出力を発見する：
    
    ```
    $ sudo systemctl restart falco
    $ sudo journalctl -fu falco
    Jan 24 23:48:18 kube-worker-1 falco[17488]: 23:48:18.516903001: \
    Error 23:48:18.516903001,<NA>,e72a6dbb63b8
    ...
    ```
    
    ファイル`/etc/falco/falco.yaml` を編集し、出力チャンネルを変更する。標準出力を無効にし、ファイル出力を有効にし、`file_output` 属性をファイル`/var/log/falco.log` に点す。その結果、コンフィギュレーションは以下のようになる：
    
    ```
    file_output:
      enabled: true
      keep_alive: false
      filename: /var/log/falco.log
    
    stdout_output:
      enabled: false
    ```
    
    ログファイルにはファルコのログが追加される：
    
    ```
    $ sudo tail -f /var/log/falco.log
    00:10:30.425084165: Error 00:10:30.425084165,<NA>,e72a6dbb63b8
    ...
    ```
    
    仮想マシンから出る：
    
    ```
    $ exit
    ```
    
2.  `setup.yaml` ファイルから`hash` という名前のPodを作成する。そのコンテナで実行されているコマンドは、無限ループで`/var/config/hash.txt` 、ファイルにハッシュを追加する：
    
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
    
    コンテナを不変性にするには、既存のPod定義に設定を追加する必要がある。ルートファイルシステムを読み取り専用に設定し、パス`/var/config` にボリュームをマウントして`hash.txt` という名前のファイルへの書き込みを許可する必要がある。結果として、YAMLマニフェストは以下のようになる：
    
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
    
3.  以下のコマンドでコントロールプレーンノードにShellコマンドで入る：
    
    ```
    $ vagrant ssh kube-control-plane
    ```
    
    `/etc/kubernetes/audit/rules/audit-policy.yaml` にある既存の監査ポリシーファイルを編集する。説明で尋ねられた規則を追加する。最終的な監査ポリシーファイルの内容は以下のようになる：
    
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
    
    ファイル`/etc/kubernetes/manifests/kube-apiserver.yaml` を編集して、APIサーバが監査ポリシーファイルを使用するように構成する。要求に応じて、追加のオプションを指定する。必要な構成は以下のとおりである：
    
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
    
    記録されたリソースの一つは、`Metadata` レベルのConfigMapである。次のコマンドはConfigMapオブジェクトの一例を作成する：
    
    ```
    $ kubectl create configmap db-user --from-literal=username=tom
    configmap/db-user created
    ```
    
    監査ログファイルは、このイベントのエントリを含むようになる：
    
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
    
    仮想マシンから出る：
    
    ```
    $ exit
    ```
    

# インデックス

### A

*   aa-complainコマンド、[カスタムプロファイルのセット](#ch04.html#idm44958046119024)
*   aa-enforceコマンド、[カスタムプロファイルのセット](#ch04.html#idm44958046120976)
*   aa-statusコマンド、[プロファイルを理解する](#ch04.html#idm44958046157936)
*   アクセス 
    *   監査ログを使ったモニタリング,[監査ログを使ってアクセスを監視する-](#ch07.html#ac_mon)[ウェブフック・バックエンドを](#ch07.html#idm44958035061424) [設定する](#ch07.html#ac_mon)
    *   APIサーバへのアクセスを制限する、[APIサーバへのアクセスを制限する-](#ch03.html#ac_res)[サービスアカウントのシークレットを](#ch03.html#idm44958046484224) [作成する](#ch03.html#ac_res)
*   adduserコマンド、[ユーザの追加](#ch04.html#idm44958046301648)
*   管理者権限, [管理者](#ch02.html#idm44958051070576)権限を持つユーザの作成,[管理者権限を持つユーザの作成](#ch02.html#idm44958051070576)
*   アドミッションコントロールは、リクエスト処理の段階として、[リクエストを処理する。](#ch03.html#idm44958051351552)
*   AKSアプリケーションゲートウェイイングレスコントローラ、[TLS終端によるイングレスの作成](#ch02.html#idm44958051758624)
*   匿名アクセス,[匿名アクセス](#ch03.html#idm44958051320656)
*   復習問題の解答 
    *   振る舞い分析、[第7章、"モニタリング、ログ、ランタイムセキュリティ"](#app01.html#a_ba)
    *   クラスター・ハードニング、[第3章 "クラスター・ハードニング"](#app01.html#a_ch)\-Chapter[3, "Cluster Hardening"](#app01.html#idm44958034150560)
    *   クラスターセットアップ、[第2章 "クラスターセットアップ](#app01.html#a_cs)["](#app01.html#idm44958034708224)\-Chapter[2, "Cluster Setup"](#app01.html#idm44958034708224)
    *   マイクロサービスの脆弱性、[第5章 "マイクロサービスの脆弱性を最小限に抑える](#app01.html#a_mv)["](#app01.html#idm44958033064048)\-Chapter[5, "Minimize Microservice Vulnerabilities"](#app01.html#idm44958033064048)
    *   サプライチェーンセキュリティ、[第6章「サプライチェーンセキュリティ](#app01.html#a_scs)」-Chapter[6, "Supply Chain Security"](#app01.html#idm44958032639504)
    *   システム・ハードニング、[第4章 "システム・ハードニング](#app01.html#a_sh)["](#app01.html#idm44958033672192)\-Chapter[4, "System Hardening"](#app01.html#idm44958033672192)
*   APIサーバ 
    *   接続先、[APIサーバへの接続-](#ch03.html#api_con)[クライアント証明書による](#ch03.html#idm44958047303520) [アクセス](#ch03.html#api_con)
    *   [APIサーバーへのアクセスを制限する-](#ch03.html#api_res)[サービスアカウントのシークレットを作成する](#ch03.html#idm44958046483344)
*   AppArmor、[関係する外部ツール](#ch01.html#idm44958052109984)、[AppArmorの使用-](#ch04.html#aa_ab)[コンテナへのプロファイルの](#ch04.html#idm44958045962544) [適用](#ch04.html#aa_ab)
*   applyコマンド、[RoleBindingの作成](#ch03.html#idm44958046783072)、[コンテナへのデフォルトコンテナランタイムプロファイルの適用](#ch04.html#idm44958045871280)、[OPA GateKeeperによる許可イメージレジストリのホワイトリスト化](#ch06.html#idm44958037522128)
*   apt purge コマンド、[不要なパッケージを削除する](#ch04.html#idm44958046335872)
*   アタックサーフェス, リダクション,[コンテナイメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038297872)
*   監査バックエンド,[監査ログを理解する](#ch07.html#idm44958035470960)
*   監査ログ,[監視、ログ、およびランタイム・セキュリティ](#ch01.html#idm44958054182528),[監査ログを使用してアクセスを監視する-](#ch07.html#al_ab)[Webhookバックエンドを](#ch07.html#idm44958035060224) [設定する](#ch07.html#al_ab)
*   監査モード、[PSA（Pod Security Admission）を理解する](#ch05.html#idm44958039808608)
*   監査ポリシー、[監査ログを理解する](#ch07.html#idm44958035473968)
*   認証は、リクエスト処理の段階として、[リクエストを処理する](#ch03.html#idm44958051354912)。
*   リクエスト処理の段階として、「[リクエストの処理」を参照のこと。](#ch03.html#idm44958051353312)
*   サービスアカウントトークンの自動マウント、無効化 [サービスアカウントトークンの自動マウントを無効にする。](#ch03.html#idm44958046679488)

### B

*   ベース画像 
    *   フットプリントの最小化、[ベースイメージのフットプリントの最小化-](#ch06.html#bi_min)[コンテナイメージ最適化ツールの](#ch06.html#idm44958038169440) [使用](#ch06.html#bi_min)
    *   [サイズの小さいベース画像を選ぶ](#ch06.html#idm44958038621088)
*   ベースラインレベル、[ポッドセキュリティアドミッション（PSA）を理解する](#ch05.html#idm44958039808000)
*   振る舞い分析 
    *   [第7章 "モニタリング、ログ、ランタイムセキュリティ "](#app01.html#ba_a)の復習問題への解答
    *   定義、[モニタリング、ロギング、ランタイム・セキュリティ](#ch07.html#idm44958036545872)
    *   コンテナの不変性の確保,[コンテナの不変性の確保](#ch07.html#idm44958035700896)
    *   [振る舞い分析を行う-](#ch07.html#ba_per)[既存の規則を](#ch07.html#idm44958035752864) [覆す](#ch07.html#ba_per)
    *   練習問題,[試験要点](#ch07.html#idm44958035047104)
    *   監査ログを使ってアクセスを監視する（[Using Audit Logs to Monitor Access - Configuring](#ch07.html#ba_al)[Webhook Backend）。](#ch07.html#idm44958035059280)
*   バイナリ, ハッシュに対する検証,[ハッシュに対するバイナリの検証](#ch02.html#idm44958051440288)
*   ブログ（Kubernetes）,[ドキュメンテーション](#ch01.html#idm44958054193280)
*   Burns, Brendan, Kubernetesの管理,[Kubernetes APIとの対話](#ch03.html#idm44958051365680)

### C

*   CA（認証局）、[KubernetesでmTLSを採用する](#ch05.html#idm44958038695184)
*   Calico、[KubernetesにmTLSを採用する](#ch05.html#idm44958038690656)
*   イングレス呼び出し,[イングレス呼び出し](#ch02.html#idm44958053777120)
*   候補者スキル,[候補者スキル](#ch01.html#idm44958053952272)
*   証明書承認コマンド、[CertificateSigningRequest の作成と承認](#ch03.html#idm44958047249440)
*   CertificateSigningRequest、作成と承認、[CertificateSigningRequestの作成と承認](#ch03.html#idm44958047256736)
*   [Kubernetes](#ch01.html#idm44958053979424)認定資格、ラーニング・パス、[Kubernetes認定資格ラーニング・パス](#ch01.html#idm44958053979424)
*   Certified Kubernetes Administrator (CKA) Study Guide (Muschko),[Who This Book Is For](#preface01.html#idm44958054158304),[Candidate Skills](#ch01.html#idm44958053954064),[Using Network Policies to Restrict Pod-to-Pod Communication](#ch02.html#idm44958052869440),[Creating an Ingress with TLS Termination](#ch02.html#idm44958051764224),[Performing Upgrade Process](#ch03.html#idm44958046425888),[Managing Secrets](#ch05.html#idm44958039076720).
*   chmodコマンド、[ファイルのパーミッションを変更する](#ch04.html#idm44958046221776)
*   chownコマンド、[ファイルの所有者を変更する](#ch04.html#idm44958046228496)
*   キリウム、[KubernetesにmTLSを採用する](#ch05.html#idm44958038689728)
*   CIS（インターネットセキュリティセンター）、[Kubernetesコンポーネントのセキュリティベストプラクティスを適用する](#ch02.html#idm44958052043184)
*   CISベンチマーク、Ubuntu Linux用、[ホストOSのフットプリントを最小限に抑える](#ch04.html#idm44958046365456)
*   CKA（認定Kubernetesアドミニストレーター）、[序文](#preface01.html#idm44958054089440)、[認定Kubernetesアドミニストレーター（CKA）](#ch01.html#idm44958054305760)
*   CKAD（認定Kubernetesアプリケーション開発者）、[序文](#preface01.html#idm44958054090352)、[認定Kubernetesアプリケーション開発者（CKAD）](#ch01.html#idm44958054307952)
*   CKS（認定Kubernetesセキュリティスペシャリスト）、[序文](#preface01.html#idm44958054130832)、[認定Kubernetesセキュリティスペシャリスト（CKS）](#ch01.html#idm44958054303568)
*   クライアント証明書、クライアント証明書によるアクセス、[クライアント証明書によるアクセス](#ch03.html#idm44958051314512)
*   クラスター・ハードニング,[クラスター・ハードニング-サンプル](#ch03.html#ch_ch)[演習](#ch03.html#idm44958046404880) 
    *   について,[クラスタハードニング](#ch01.html#idm44958052152416),[クラスタハードニング](#ch03.html#idm44958051378208)
    *   復習問題の答え,[Chapter 3, "Cluster Hardening"](#app01.html#ch_a)\-Chapter[3, "Cluster Hardening"](#app01.html#idm44958034102896)
    *   Kubernetes APIとの対話,[Kubernetes APIとの対話-](#ch03.html#ch_api)[クライアント証明書による](#ch03.html#idm44958047305440) [アクセス](#ch03.html#ch_api)
    *   APIサーバへのアクセスを制限する,[APIサーバへのアクセスを制限する-](#ch03.html#ch_res)[サービスアカウントのシークレットを](#ch03.html#idm44958046482400) [作成する](#ch03.html#ch_res)
    *   サンプル演習,[サンプル演習](#ch03.html#idm44958046406800)
    *   Kubernetesを更新する,[Kubernetesを](#ch03.html#ch_up)[頻繁に](#ch03.html#idm44958046419248) [更新する-まとめ](#ch03.html#ch_up)
*   クラスタ設定,[クラスタ設定-サンプル](#ch02.html#cs_ch)[演習](#ch02.html#idm44958051396448) 
    *   について,[クラスタ設定](#ch01.html#idm44958053134512),[クラスタ設定](#ch02.html#idm44958054013584)
    *   復習問題の答え,[第2章 "Cluster Setup"](#app01.html#cs_a)\-Chapter[2, "Cluster Setup"](#app01.html#idm44958034707248)
    *   Kubernetesコンポーネントのベストセキュリティプラクティスの適用,[Kubernetesコンポーネントのセキュリティベストプラクティスの適用-](#ch02.html#cs_app)[検出されたセキュリティ問題の](#ch02.html#idm44958051778416) [修正](#ch02.html#cs_app)
    *   TLSターミネーションによるイングレスの作成,[TLSターミネーションによるイングレスの作成-](#ch02.html#cs_ing)[イングレスの](#ch02.html#idm44958053773248) [呼び出し](#ch02.html#cs_ing)
    *   GUI要素の保護、[GUI要素の保護-](#ch02.html#cs_pro)[安全でない設定引数の回避](#ch02.html#idm44958051480224)
    *   ノードのメタデータとエンドポイントの保護 [\- ノードのメタデータとエンドポイントの保護 -](#ch02.html#cs_pn)[ネットワークポリシーによるメタデータサーバアクセスの](#ch02.html#idm44958053654224) [保護](#ch02.html#cs_pn)
    *   ネットワークポリシーを使ってPod間通信を制限する\-[ネットワークポリシーを使ってPod間通信を制限する -](#ch02.html#cs_pp)[細かい受信トラフィックを](#ch02.html#idm44958052054080) [許可する](#ch02.html#cs_pp)
    *   サンプル演習,[サンプル演習](#ch02.html#idm44958051398368)
    *   Kubernetesプラットフォームバイナリの検証,[Kubernetesプラットフォームバイナリの検証](#ch02.html#idm44958051474752)
*   ClusterIPサービスタイプ、[シナリオ：攻撃者がダッシュボード機能にアクセスする](#ch02.html#idm44958051110400)
*   ClusterRole、[制限された権限を持つユーザの作成](#ch02.html#idm44958053042096)、[ClusterRoleの作成](#ch03.html#idm44958047012848)
*   ClusterRoleBindingオブジェクト、[管理権限を持つユーザの作成](#ch02.html#idm44958051071968)
*   CMDコマンド、[レイヤー数を減らす](#ch06.html#idm44958038293632)
*   CNCF (Cloud Native Computing Foundation)、[試験オブジェクト](#ch01.html#idm44958054085680)
*   CNI（コンテナ・ネットワーク・インターフェイス）プラグイン、[ネットワーク・ポリシーを使ってポッド間通信を制限する](#ch02.html#idm44958052871536)
*   コマンド 
    *   aa-complain,[カスタムプロファイルのセット](#ch04.html#idm44958046118352)
    *   aa-enforce、[カスタムプロファイルをセットする](#ch04.html#idm44958046119968)
    *   aa-status、[プロファイルを理解する](#ch04.html#idm44958046157264)
    *   adduser,[ユーザの追加](#ch04.html#idm44958046300976)
    *   適用、[RoleBinding の作成](#ch03.html#idm44958046714176)、[コンテナへのデフォルトコンテナランタイムプロファイルの適用](#ch04.html#idm44958045870576)、[OPA GateKeeper による許可イメージレジストリのホワイトリスト化](#ch06.html#idm44958037566144)
    *   apt purge,[不要なパッケージを削除する](#ch04.html#idm44958046335200)
    *   証明書の承認,[CertificateSigningRequestの作成と承認](#ch03.html#idm44958047248672)
    *   chmod,[ファイルのパーミッションを変更する](#ch04.html#idm44958046221072)
    *   chown,[ファイルの所有者を変更する](#ch04.html#idm44958046227568)
    *   CMD、[レイヤー数を減らす](#ch06.html#idm44958038292960)
    *   コピー、[レイヤー数を減らす](#ch06.html#idm44958038290400)
    *   イングレスの作成,[イングレスの作成](#ch02.html#idm44958051207600)
    *   ロールバインディングの作成,[ロールとロールバインディングの作成](#ch03.html#idm44958047241904)
    *   トークンの作成,[サービスアカウントトークンの作成](#ch03.html#idm44958046550480)
    *   curl,[匿名アクセス](#ch03.html#idm44958051321600),[与えられたパーミッションの検証](#ch03.html#idm44958046707792),[バックエンドアプリケーションの実装](#ch06.html#idm44958037477120)
    *   [サービスを](#ch04.html#idm44958046342160)無効にする
    *   docker build,[コンテナイメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038501776)
    *   docker trust sign,[コンテナイメージへの署名](#ch06.html#idm44958038158960)
    *   echo,[デフォルトのコンテナランタイムプロファイルをコンテナに適用する。](#ch04.html#idm44958045866432)
    *   etcdctl、[シナリオ：攻撃者がetcdを実行しているノードにアクセスする](#ch05.html#idm44958039032400),[etcdのデータを暗号化する](#ch05.html#idm44958038900144)
    *   FROM、[レイヤー数を減らす](#ch06.html#idm44958038292016)
    *   [Ingressバックエンドをセットアップする](#ch02.html#idm44958052259840)
    *   Go build,[コンテナイメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038460160)
    *   groupadd,[グループの追加](#ch04.html#idm44958046268272)
    *   groupdel,[グループを削除する](#ch04.html#idm44958046254448)
    *   journalctl,[イベントの生成と Falco ログの検査](#ch07.html#idm44958036416880)
    *   kubectl,[ゲートキーパーのインストール](#ch05.html#idm44958039421072),[ログバックエンドの設定](#ch07.html#idm44958035141104)
    *   kubectl apply、[デフォルトの振る舞いを観察する](#ch02.html#idm44958052778928)
    *   kubectlログ、[kube-benchを使う](#ch02.html#idm44958052025728)
    *   ls,[ファイルのパーミッションと所有者の表示](#ch04.html#idm44958046236240)
    *   mkdir,[コンテナにカスタムプロファイルを適用する](#ch04.html#idm44958040472336)
    *   netstat、[オープンポートの特定と無効化](#ch04.html#idm44958046199344)
    *   OpenSSL、[TLS証明書と鍵の作成](#ch02.html#idm44958051254848)
    *   RUN、[レイヤー数を減らす](#ch06.html#idm44958038288784)
    *   ss、[オープンポートの特定と無効化](#ch04.html#idm44958046197728)
    *   ステータス、[サービスの無効化](#ch04.html#idm44958046348400)
    *   su,[ユーザに切り替える](#ch04.html#idm44958046292336)
    *   sudo,[ユーザに切り替える](#ch04.html#idm44958046285728)
    *   sudo systemctl restart kubelet,[ImagePolicyWebhook Admission Controller Pluginの設定](#ch06.html#idm44958037204912)
    *   sysctl、[特権コンテナを回避する](#ch05.html#idm44958039957808)
    *   systemctl,[サービスを無効にする](#ch04.html#idm44958046353216)
    *   systemctl status,[開いているポートの特定と無効化](#ch04.html#idm44958046191536)
    *   タッチ、[ファイルのパーミッションと所有権を表示する](#ch04.html#idm44958046240048)
    *   usermod,[グループの削除](#ch04.html#idm44958046250144)
    *   wget、[指向性ネットワークトラフィックを拒否する](#ch02.html#idm44958051675344)
*   [プロファイルを理解しよう](#ch04.html#idm44958046150896)
*   コンポーネント・セキュリティのベストプラクティス, 適用,[Kubernetesコンポーネント・セキュリティのベストプラクティスの適用-](#ch02.html#csb_app)[検出されたセキュリティ問題の](#ch02.html#idm44958051777440) [修正](#ch02.html#csb_app)
*   ConfigMap、[シナリオ：攻撃者が悪意のあるソフトウェアをインストールする](#ch07.html#idm44958035692688)、[ConfigMapやシークレットでコンテナを設定する](#ch07.html#idm44958035684960)
*   構成ファイル（ファルコ）、[ファルコ構成ファイル](#ch07.html#idm44958036447280)
*   設定 
    *   コンテナをConfigMapで構成する、[ConfigMapまたはシークレットでコンテナを構成する](#ch07.html#idm44958035683296)
    *   シークレットを持つコンテナ、[ConfigMapやシークレットを持つコンテナを構成する](#ch07.html#idm44958035682336)
    *   ファルコ、[ファルコの設定](#ch07.html#idm44958036452960)
    *   gVisor、[gVisorのインストールと設定](#ch05.html#idm44958038870736)
    *   ImagePolicyWebhook Admission Controller プラグイン、[ImagePolicyWebhook Admission Controller プラグインの設定-](#ch06.html#con_pl)[ImagePolicyWebhook Admission Controller プラグインの設定](#ch06.html#idm44958037152448)
    *   ログバックエンド,[ログバックエンドの設定](#ch07.html#idm44958035281888)
    *   APIサーバーのポート、[APIサーバーへの接続](#ch03.html#idm44958051338944)
    *   読み取り専用コンテナ・ルート・ファイルシステムの設定,[読み取り専用コンテナ・ルート・ファイルシステムの設定](#ch07.html#idm44958035673008)
    *   ウェブフック・バックエンド,[ウェブフック・バックエンドを設定する](#ch07.html#idm44958035065792)
*   APIサーバへの接続、[APIサーバへの接続-](#ch03.html#con_api)[クライアント証明書による](#ch03.html#idm44958047302576) [アクセス](#ch03.html#con_api)
*   制約テンプレート、[関与するKubernetesプリミティブ](#ch01.html#idm44958054176320)、[OPAポリシーの実装](#ch05.html#idm44958039410784)、[OPA GateKeeperによる許可イメージレジストリのホワイトリスト化](#ch06.html#idm44958037879696)
*   コンテナイメージ 
    *   [コンテナイメージ](#ch06.html#idm44958038178304)最適化ツールの使用,[コンテナイメージ最適化](#ch06.html#idm44958038178304)ツールの [使用](#ch06.html#idm44958038178304)
    *   [コンテナイメージに署名する](#ch06.html#idm44958038157376)
    *   [コンテナ・イメージの構築のための](#ch06.html#idm44958038600336)多段階アプローチを使用する
    *   検証、[コンテナイメージの検証](#ch06.html#idm44958038144864)
*   コンテナ・ランタイム・サンドボックス,[コンテナ・ランタイム・サンドボックスを理解する-](#ch05.html#crs_ab)[ランタイム・クラスの作成と使用](#ch05.html#idm44958038720912)
*   コンテナ・セキュリティ（米）、[サプライチェーンの安全確保](#ch06.html#idm44958038163392)
*   containerd,[デフォルトのコンテナランタイムプロファイルをコンテナに適用する](#ch04.html#idm44958045955232)
*   コンテナ 
    *   カスタムプロファイルを適用する,[カスタムプロファイルをコンテナに適用する](#ch04.html#idm44958042490032)
    *   プロファイルを適用する,[プロファイルをコンテナに適用する](#ch04.html#idm44958046114320)
    *   ConfigMapやシークレットを使ったコンテナの設定、[ConfigMapやシークレットを使ったコンテナの設定](#ch07.html#idm44958035681392)
    *   不変性の確保,[コンテナの不変性の確保](#ch07.html#idm44958035700016)
    *   特権モードでの使用、[特権コンテナの回避](#ch05.html#idm44958040068592)
*   継続的デリバリ（ハンブルとファーレイ）、[ワークロードの静的分析](#ch06.html#idm44958037141552)
*   COPYコマンド、[レイヤー数を減らす](#ch06.html#idm44958038289456)
*   CRD（カスタムリソース定義）、[関与するKubernetesプリミティブ](#ch01.html#idm44958054175616)
*   create ingressコマンド、[イングレスの作成](#ch02.html#idm44958051206400)
*   createロールバインディングコマンド,[役割とロールバインディングの作成](#ch03.html#idm44958047240928)
*   create tokenコマンド、[サービス・アカウント・トークンの作成](#ch03.html#idm44958046549504)
*   CSR（証明書署名要求）、[秘密鍵の作成](#ch03.html#idm44958047262864)、[KubernetesでのmTLSの採用](#ch05.html#idm44958038694496)
*   curlコマンド、[匿名アクセス](#ch03.html#idm44958051322304)、[付与されたパーミッションの検証](#ch03.html#idm44958046708464)、[バックエンドアプリケーションの実装](#ch06.html#idm44958037477824)
*   カリキュラム,[教育課程](#ch01.html#idm44958054078000)
*   カスタムプロファイル 
    *   コンテナに適用する,[カスタムプロファイルをコンテナに適用する](#ch04.html#idm44958043099856)
    *   [カスタムプロファイルの](#ch04.html#idm44958046127264)設定,[カスタムプロファイルの設定](#ch04.html#idm44958046127264),[カスタムプロファイルの設定](#ch04.html#idm44958045818352)
*   カスタム規則, ファルコ用,[カスタム規則](#ch07.html#idm44958036437104)
*   CVE（Common Vulnerabilities and Exposures）データベース、[Kubernetesの頻繁なアップデート](#ch03.html#idm44958046442416)
*   CVEの詳細、[既知の脆弱性について画像をスキャンする](#ch06.html#idm44958036721744)

### D

*   デフォルトの振る舞い, 可観測性,[デフォルトの振る舞いを観察する](#ch02.html#idm44958053966256)
*   デフォルトコンテナランタイムプロファイル、コンテナへの適用、[デフォルトコンテナランタイムプロファイルをコンテナに適用する](#ch04.html#idm44958045953856)
*   デフォルトの名前空間、[デフォルトの振る舞いの観察](#ch02.html#idm44958053897216)、[kube-benchの使用](#ch02.html#idm44958052031344)、[パーミッションの検証](#ch03.html#idm44958047219568)
*   デフォルトのパーミッション, 検証,[デフォルトのパーミッションを検証する](#ch03.html#idm44958047023168)
*   デフォルト規則, ファルコ用,[デフォルト規則](#ch07.html#idm44958036442176)
*   デジオアンニ、ロリス、ファルコによる実践的クラウド・ネイティブ・セキュリティ、[ファルコを理解する](#ch07.html#idm44958036471808)
*   deny-allネットワークポリシー、[指向性ネットワークトラフィックを拒否する](#ch02.html#idm44958051682032)
*   非推奨ポリシー（Kubernetes）、[バージョニングスキーム](#ch03.html#idm44958046435216)
*   方向性ネットワーク・トラフィック、[方向性ネットワーク・トラフィックの拒否](#ch02.html#idm44958053886992)
*   disable コマンド、[サービスを無効にする](#ch04.html#idm44958046342864)
*   無効化 
    *   サービスアカウントトークンの自動マウント、[サービスアカウントトークンの自動マウントを無効にする](#ch03.html#idm44958046678880)
    *   オープンポート、[オープンポートの特定と無効化](#ch04.html#idm44958046207200)
*   ディストロレスイメージ,[小さなサイズのベースイメージを選ぶ](#ch06.html#idm44958038611840),[ディストロレスコンテナイメージを使う](#ch07.html#idm44958035689520)
*   ダイブ、[コンテナイメージ最適化ツールを使う](#ch06.html#idm44958038172240)
*   docker buildコマンド、[コンテナイメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038500800)
*   Docker Engine、[デフォルトのコンテナランタイムプロファイルをコンテナに適用する](#ch04.html#idm44958045954528)
*   Docker Hub、[シナリオ：攻撃者がコンテナの脆弱性を悪用](#ch06.html#idm44958038630160)、[パブリック・イメージ・レジストリを利用する](#ch06.html#idm44958037958560)
*   docker trust signコマンド、[コンテナイメージに署名する](#ch06.html#idm44958038158016)
*   Docker、マルチステージビルド、[コンテナイメージのビルドにマルチステージアプローチを使う](#ch06.html#idm44958038599296)
*   Dockerfile、[シナリオ：攻撃者がコンテナの脆弱性を悪用する](#ch06.html#idm44958038632288),[コンテナイメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038601952),[Dockerfileの解析にHadolintを使用する](#ch06.html#idm44958037055184)
*   DockerSlim、[コンテナイメージ最適化ツールを使う](#ch06.html#idm44958038175360)
*   ドキュメンテーション 
    *   AWS、[ネットワークポリシーでメタデータサーバへのアクセスを保護する](#ch02.html#idm44958053709072)
    *   Docker、[シナリオ：攻撃者がコンテナの脆弱性を悪用する](#ch06.html#idm44958038625792)
    *   EKS、[APIサーバへのアクセスを制限する](#ch03.html#idm44958047292608)
    *   ファルコ、[ファルコのインストール](#ch07.html#idm44958036463424)、[ファルコ規則ファイルの基本を理解する](#ch07.html#idm44958036403856)
    *   GKE、[API サーバへのアクセスを制限する](#ch03.html#idm44958047291664)
    *   Kubernetes,[学習内容](#preface01.html#idm44958048016528),[ドキュメンテーション](#ch01.html#idm44958054196640),[ネットワークポリシーを使ってPod間通信を制限する](#ch02.html#idm44958052870416),[ネットワークトラフィックの方向性を拒否する](#ch02.html#idm44958053885888),[TLSターミネーションによるイングレスの作成](#ch02.html#idm44958051762640),[ハッシュに対するバイナリの検証](#ch02.html#idm44958051424128),[リクエストの処理](#ch03.html#idm44958051360272),[ユーザパーミッションを制限する](#ch03.html#idm44958047275968)、[デフォルトパーミッションの検証](#ch03.html#idm44958047019728),[seccompの使用](#ch04.html#idm44958045960000),[シークレットの管理](#ch05.html#idm44958039075168),[etcdデータの暗号化](#ch05.html#idm44958038894320),[KubernetesでmTLSを採用する](#ch05.html#idm44958038693424),[サイズの小さいベースイメージを選ぶ](#ch06.html#idm44958038605984),[バックエンドアプリケーションの実装](#ch06.html#idm44958037480368),[ConfigMapまたはシークレットを使用したコンテナの構成](#ch07.html#idm44958035676416),[監査ポリシーファイルの作成](#ch07.html#idm44958035335904),[ログの実装](#ch07.html#idm44958035143568)
    *   OpenSSL、[TLS証明書と鍵の作成](#ch02.html#idm44958051253872)
    *   Trivy、[既知の脆弱性について画像をスキャンする](#ch06.html#idm44958036719968)

### E

*   echo コマンド、[デフォルトのコンテナランタイムプロファイルをコンテナに適用する。](#ch04.html#idm44958045867136)
*   EKS（Amazon Elastic Kubernetes Service）、[Kubernetesコンポーネントのセキュリティのベストプラクティスを適用する](#ch02.html#idm44958052039184)。
*   暗号化 
    *   etcdデータ、[etcdデータの暗号化](#ch05.html#idm44958039013728)
    *   Pod-to-Pod、[mTLSによるPod-to-Pod暗号化を理解する](#ch05.html#idm44958038714944)
*   エンドポイント, 保護,[ノードメタデータとエンドポイントの保護-](#ch02.html#end_pn)[ネットワークポリシーによるメタデータサーバアクセスの](#ch02.html#idm44958053653376) [保護](#ch02.html#end_pn)
*   エンフォースメントモード、[PSA（Pod Security Admission）を理解する](#ch05.html#idm44958039807392)
*   プロファイルモードの強制、[プロファイルを理解する](#ch04.html#idm44958046152880)
*   etcdデータ 
    *   [etcdデータに](#ch05.html#idm44958039029120)アクセスする
    *   暗号化,[etcdデータの暗号化](#ch05.html#idm44958039012752)
*   etcdctlコマンド、[シナリオ：攻撃者がetcdを実行しているノードにアクセスする](#ch05.html#idm44958039033104)、[etcdデータを暗号化する](#ch05.html#idm44958038900848)
*   イベント、Falcoでの生成、[イベントの生成とFalcoログの検査](#ch07.html#idm44958036423984)
*   試験の目的,[試験の目的](#ch01.html#idm44958054085008)
*   実行引数、[安全でないコンフィギュレーション引数の回避](#ch02.html#idm44958051482832)
*   外部アクセス, ネットワークへの最小化,[ネットワークへの外部アクセスの最小化](#ch04.html#idm44958046212128)
*   外部ツール,[関与する外部ツール](#ch01.html#idm44958052104464)

### F

*   F5 NGINX Ingress Controller、[TLSターミネーションによるIngressの作成](#ch02.html#idm44958051757664)
*   ファルコ 
    *   について、[監視、ログ、およびランタイムセキュリティ](#ch01.html#idm44958054183856)、[関係する外部ツール](#ch01.html#idm44958052106352)、[監視、ログ、およびランタイムセキュリティ](#ch07.html#idm44958036546848)、[ファルコを理解する](#ch07.html#idm44958036480496)。
    *   構成ファイル、[ファルコ構成ファイル](#ch07.html#idm44958036446512)
    *   コンフィギュレーション,[ファルコのコンフィギュレーション](#ch07.html#idm44958036451984)
    *   カスタム規則,[カスタム規則](#ch07.html#idm44958036436400)
    *   デフォルト規則,[デフォルト規則](#ch07.html#idm44958036441408)
    *   イベントの生成,[イベントの生成とFalcoログの検査](#ch07.html#idm44958036423264)
    *   ログの検査,[イベントの生成と Falco ログの検査](#ch07.html#idm44958036422320)
    *   インストール,[ファルコのインストール](#ch07.html#idm44958036465344)
    *   既存の規則を上書きする,[既存の規則を上書きする](#ch07.html#idm44958035904704)
    *   ルールファイルの基本,[ファルコのルールファイルの基本を理解する](#ch07.html#idm44958036406784)
*   ファルコ101ビデオコース,[ファルコを理解する](#ch07.html#idm44958036469696)
*   falco.yamlファイル,[Falco構成ファイル](#ch07.html#idm44958036445568)
*   falco\_rules.local.yamlファイル,[カスタム規則](#ch07.html#idm44958036435456)
*   falco\_rules.yamlファイル,[デフォルト規則](#ch07.html#idm44958036440464)
*   ファーリー、デビッド、継続的デリバリ、[ワークロードの静的分析](#ch06.html#idm44958037059152)
*   フィーチャーゲート、[デフォルトのコンテナランタイムプロファイルをコンテナに適用する](#ch04.html#idm44958045952832)
*   ファイルの所有権,[ファイルのパーミッションと所有権を理解する](#ch04.html#idm44958046243760)
*   ファイルパーミッション,[ファイルパーミッションと所有権の理解](#ch04.html#idm44958046244464),[ファイルパーミッションの変更](#ch04.html#idm44958046220128)
*   きめ細かな着信トラフィック、許可する、[きめ細かな着信トラフィックを許可する](#ch02.html#idm44958051641488)
*   ファイアウォール、ルールのセットアップ、[ファイアウォールルールのセットアップ](#ch04.html#idm44958046183872)
*   FROMコマンド、[レイヤー数を減らす](#ch06.html#idm44958038291072)
*   FTPサーバ、[オープンポートの特定と無効化](#ch04.html#idm44958046207904)

### G

*   全コマンドの取得,[Ingressバックエンドのセットアップ](#ch02.html#idm44958052260672)
*   GitHub,[ドキュメント](#ch01.html#idm44958054193952),[コンテナイメージ最適化ツールの使用](#ch06.html#idm44958038172944),[バックエンドアプリケーションの実装](#ch06.html#idm44958037482144)
*   GKE（Google Kubernetes Engine）、[Kubernetesコンポーネントのセキュリティベストプラクティスを適用する](#ch02.html#idm44958052038416)
*   go buildコマンド、[コンテナ・イメージの構築に多段階アプローチを使用する](#ch06.html#idm44958038459184)
*   Goランタイム、[コンテナ・イメージの構築に多段階アプローチを用いる](#ch06.html#idm44958038597168)
*   Google Cloudコンテナレジストリ、[パブリックイメージレジストリの利用](#ch06.html#idm44958037959392)
*   Googleのディストロ・レス・イメージ、[サイズの小さいベース・イメージを選ぶ](#ch06.html#idm44958038611168)
*   付与されたパーミッション, 検証,[付与されたパーミッションを検証する](#ch03.html#idm44958046710304)
*   グラッソ、レオナルド、ファルコによる実践的クラウドネイティブ・セキュリティ、[ファルコを理解する](#ch07.html#idm44958036471136)
*   グループID, 設定,[特定のユーザとグループIDの設定](#ch05.html#idm44958040203232)
*   グループ経営,[グループ経営を理解する](#ch04.html#idm44958046277664)
*   groupaddコマンド、[グループの追加](#ch04.html#idm44958046268976)
*   groupdelコマンド、[グループの削除](#ch04.html#idm44958046255152)
*   グループ、[リストグループ](#ch04.html#idm44958046274832)
*   GUI（グラフィカル・ユーザ・インタフェース）、[クラスタ・セットアップ](#ch01.html#idm44958052155552)、[GUI要素の保護](#ch02.html#gui_ab)
*   バイザー 
    *   [マイクロサービスの脆弱性の最小化](#ch01.html#idm44958052395632)、[関係する外部ツール](#ch01.html#idm44958052108640)、[利用可能なコンテナランタイムサンドボックスの実装について](#ch05.html#idm44958038876576)
    *   設定、[gVisorのインストールと設定](#ch05.html#idm44958038869760)
    *   インストール、[gVisorのインストールと設定](#ch05.html#idm44958038868816)

### H

*   hadolint(Haskell Dockerfile Linterを参照)
*   ハッシュに対するバイナリの検証,[ハッシュに対するバイナリの検証](#ch02.html#idm44958051439568)
*   HaskellのDockerfileリンター、[Dockerfileの解析にHadolintを使う](#ch06.html#idm44958037054480)
*   Helmパッケージマネージャ、[シナリオ攻撃者はサービスアカウントからAPIサーバを呼び出すことができる](#ch03.html#idm44958047213376)
*   helpオプション、[Ingressの作成](#ch02.html#idm44958051205728)
*   ホストOSのフットプリント, 最小化,[ホストOSのフットプリントの最小化-](#ch04.html#hos_min)[不要なパッケージの](#ch04.html#idm44958046331984) [削除](#ch04.html#hos_min)
*   ハンブル、ジェズ、継続的デリバリ、[ワークロードの静的分析](#ch06.html#idm44958037058544)

### I

*   IDおよびアクセス管理（IAM）ロール、最小化、[IAMロールの最小化-](#ch04.html#iam_min)[ファイルパーミッションの](#ch04.html#idm44958046219456) [変更](#ch04.html#iam_min)
*   イメージダイジェストの検証、[コンテナイメージの検証](#ch06.html#idm44958038141776)
*   イメージプルポリシー、[シナリオ攻撃者がコンテナイメージに悪意のあるコードを注入する](#ch06.html#idm44958038153248)、[パブリックイメージレジストリを使用する](#ch06.html#idm44958037961168)
*   イメージレジストリ 
    *   ImagePolicyWebhook Admission Controllerプラグインを使用したホワイトリスト、[ImagePolicyWebhook Admission Controllerプラグインを使用したホワイトリスト許可画像レジストリ](#ch06.html#idm44958037495632)
    *   OPAゲートキーパーによるホワイトリスト登録、[OPAゲートキーパーによる許可画像レジストリのホワイトリスト登録-](#ch06.html#ir_wh)[OPAゲートキーパーによる許可画像レジストリの](#ch06.html#idm44958037546912) [ホワイトリスト登録](#ch06.html#ir_wh)
*   ImagePolicyWebhook Admission Controller プラグイン 
    *   設定する、[ImagePolicyWebhook Admission Controller プラグインの設定-](#ch06.html#pl_con)[ImagePolicyWebhook Admission Controller プラグインの](#ch06.html#idm44958037151504) [設定](#ch06.html#pl_con)
    *   [ImagePolicyWebhook Admission Controllerプラグインで許可](#ch06.html#idm44958037494784)された画像レジストリをホワイトリストに登録する。
*   画像 
    *   ベース、[小さいサイズのベース画像を選ぶ](#ch06.html#idm44958038620144)
    *   コンテナ、[コンテナ・イメージ構築のための多段階アプローチの使用](#ch06.html#idm44958038601280)、[コンテナ・イメージ最適化ツールの使用](#ch06.html#idm44958038177456)、[コンテナ・イメージの検証](#ch06.html#idm44958038142720)
    *   ディストロレス,[小さなサイズのベースイメージを選ぶ](#ch06.html#idm44958038612816),[ディストロレス・コンテナ・イメージを使う](#ch07.html#idm44958035688816)
    *   既知の脆弱性に対するスキャン、[既知の脆弱性に対する画像のスキャン](#ch06.html#idm44958036718096)
*   不変性, コンテナのための不変性,[コンテナの不変性の確保](#ch07.html#idm44958035699072)
*   必須メソッド、[イングレスの作成](#ch02.html#idm44958051204384)
*   インバウンド制御ノードポート、[ノードメタデータとエンドポイントの保護](#ch02.html#idm44958053732112)
*   イングレス、TLS終端による作成、[TLS終端によるイングレスの作成-](#ch02.html#ing_cre)[イングレスの](#ch02.html#idm44958053772272) [呼び出し](#ch02.html#ing_cre)
*   安全でないコンフィギュレーションの引数, 回避,[安全でないコンフィギュレーションの引数を回避する](#ch02.html#idm44958051483568)
*   インストールする 
    *   ファルコ、[ファルコを設置する](#ch07.html#idm44958036464368)
    *   ゲートキーパー、[ゲートキーパーの設置](#ch05.html#idm44958039419376)
    *   gVisor、[gVisorのインストールと設定](#ch05.html#idm44958038867872)
    *   Kubernetes Dashboard,[Kubernetes Dashboardのインストール](#ch02.html#idm44958051086976)
    *   の選択肢、[練習と模擬試験](#ch01.html#idm44958053945600)

### J

*   JFrog Artifactory、[シナリオ：攻撃者が悪意のあるコンテナイメージをアップロードする](#ch06.html#idm44958037887648)
*   journalctl コマンド、[イベントの生成と Falco ログの検査](#ch07.html#idm44958036415680)
*   JSON行形式、[監査ログを理解する](#ch07.html#idm44958035479712)

### K

*   k8s\_audit\_rules.yamlファイル、[Kubernetes固有の規則](#ch07.html#idm44958036432000)
*   Kataコンテナ、[マイクロサービスの脆弱性の最小化](#ch01.html#idm44958052396304)、[関係する外部ツール](#ch01.html#idm44958052107696)、[利用可能なコンテナランタイムサンドボックスの実装](#ch05.html#idm44958038877280)
*   KCNA (Kubernetes and Cloud Native Associate),[Kubernetes and Cloud Native Associate (KCNA)](#ch01.html#idm44958054040400)
*   KCSA (Kubernetes and Cloud Native Security Associate),[Kubernetes and Cloud Native Security Associate (KCSA)](#ch01.html#idm44958054037792)
*   カーネルハードニングツールを使用 [する-](#ch04.html#kht_ab)[カスタムプロファイルをコンテナに](#ch04.html#idm44958040420688) [適用する](#ch04.html#kht_ab)。
*   キラー・シェル、[練習と模擬試験](#ch01.html#idm44958054025200)
*   既知の脆弱性、画像のスキャン、[既知の脆弱性のための画像のスキャン](#ch06.html#idm44958036717184)
*   kube-bench,[関係する外部ツール](#ch01.html#idm44958052110688),[kube-benchを使う](#ch02.html#idm44958052035824),[kube-benchを使う](#ch02.html#idm44958052030640)
*   kubeadm,[Kubernetesプラットフォームのバイナリを検証する](#ch02.html#idm44958051458720)
*   kubeconfigファイル,[kubeconfigファイルへのユーザの追加](#ch03.html#idm44958047231824),[Webhookバックエンドの構成](#ch07.html#idm44958035064144)
*   kubectl applyコマンド、[デフォルトの振る舞いを観察する](#ch02.html#idm44958052779536)
*   kubectlコマンド、[Gatekeeperのインストール](#ch05.html#idm44958039421680)、[ログバックエンドの設定](#ch07.html#idm44958035140128)
*   kubectl logsコマンド、[kube-benchを使用する](#ch02.html#idm44958052024752)
*   kubectlツール,[GUI要素の保護](#ch02.html#idm44958051117136),[Kubernetesプラットフォームバイナリの検証](#ch02.html#idm44958051458048),[パーミッションの検証](#ch03.html#idm44958047222784),[Kubernetesマニフェストの分析にKubesecを使用する](#ch06.html#idm44958036971072)
*   Kubernetes 
    *   リリースノート,[試験オブジェクト](#ch01.html#idm44958054083104)
    *   更新,[Kubernetesを](#ch03.html#kub_up)[頻繁に](#ch03.html#idm44958046417328) [更新する-まとめ](#ch03.html#kub_up)
    *   [Kubernetesで](#ch05.html#idm44958038696160)mTLSを使用する,[KubernetesでmTLSを採用する](#ch05.html#idm44958038696160)
*   Kubernetes APIとの対話, [Kubernetes APIとの対話-](#ch03.html#api_int)[クライアント証明書によるアクセス](#ch03.html#idm44958047304464)
*   Kubernetes CIS Benchmark、[Kubernetesコンポーネントのセキュリティ・ベスト・プラクティスを適用する](#ch02.html#idm44958052042512)
*   Kubernetesダッシュボード 
    *   [GUI要素の保護について](#ch02.html#idm44958051116464)
    *   [Kubernetesダッシュボードに](#ch02.html#idm44958051079424)アクセスする
    *   インストール,[Kubernetes Dashboardのインストール](#ch02.html#idm44958051087952)
*   Kubernetesマニフェスト、Kubesecを使った分析、[Kubernetesマニフェストの分析にKubesecを使う-](#ch06.html#km_an)[Kubernetesマニフェストの分析にKubesecを使う](#ch06.html#idm44958036726496)
*   Kubernetesプリミティブ（プリミティブ参照）
*   Kubernetesサービス、[kubernetesサービスを使う](#ch03.html#idm44958051333376)
*   Kubernetes固有の規則、[Kubernetes固有の規則](#ch07.html#idm44958036431232)
*   Kubesec、[ワークロードの静的解析](#ch06.html#idm44958037145760)、[Kubernetesマニフェストの解析にKubesecを使う-](#ch06.html#ku_ab)[Kubernetesマニフェストの解析にKubesecを使う](#ch06.html#idm44958036725552)
*   カイバーノ、[OPAポリシーの実装](#ch05.html#idm44958039087888)

### L

*   レイヤーの数を減らす,[レイヤーの数を減らす](#ch06.html#idm44958038295296)
*   リスト・グループ,[リスト・グループ](#ch04.html#idm44958046275536)
*   リスト・ユーザー,[リスト・ユーザー](#ch04.html#idm44958046312208)
*   リスト（ファルコ）、[リスト](#ch07.html#idm44958036222400)
*   ログバックエンド,[監査ログを理解する](#ch07.html#idm44958035472336),[ログバックエンドを設定する](#ch07.html#idm44958035280912)
*   ロギング、[モニタリング、ロギング、ランタイムセキュリティ](#ch01.html#idm44958054185200)、[モニタリング、ロギング、ランタイムセキュリティ-試験](#ch07.html#log_ch)[要点](#ch07.html#idm44958035045184) 
    *   (振る舞い分析も参照）。
*   ログ、Falcoでの検査、[イベントの生成とFalcoログの検査](#ch07.html#idm44958036421376)
*   lsコマンド、[ファイルのパーミッションと所有者の表示](#ch04.html#idm44958046236944)
*   LTS（長期サポート）、[Kubernetesの頻繁なアップデート](#ch03.html#idm44958046443440)

### M

*   マクロ (Falco),[マクロ](#ch07.html#idm44958036370304)
*   Kubernetesの管理（BurnsとTracey）、[Kubernetes APIとの対話](#ch03.html#idm44958051366400)
*   MD5、[ハッシュに対するバイナリの検証](#ch02.html#idm44958051437936)
*   メタデータの監査レベル、[監査ポリシーファイルの作成](#ch07.html#idm44958035441056)
*   メタデータ・サーバーへのアクセス, ネットワークポリシーによる保護,[ネットワークポリシーによるメタデータ・サーバーへのアクセスの保護](#ch02.html#idm44958053708096)
*   マイクロサービスの脆弱性,[マイクロサービスの脆弱性を最小限に抑える-Exam](#ch05.html#mv_ch)[Essentials](#ch05.html#idm44958038675232) 
    *   について,[マイクロサービスの脆弱性を最小限に抑える](#ch01.html#idm44958052398448),[マイクロサービスの脆弱性を最小限に抑える](#ch05.html#idm44958040377888)
    *   復習問題の解答、[第5章「マイクロサービスの脆弱性を最小限に抑える」](#app01.html#mv_a)\-Chapter[5, "Minimize Microservice Vulnerabilities"](#app01.html#idm44958033063168)
    *   コンテナランタイムサンドボックス,[Understanding Container Runtime Sandboxes-Creating](#ch05.html#mv_crs)[and Using a Runtime Class](#ch05.html#idm44958038719936).
    *   シークレットの管理」（Managing [Secrets）-「](#ch05.html#mv_ms)[etcdデータの](#ch05.html#idm44958038893344) [暗号化」（Managing Secrets-Encrypting](#ch05.html#mv_ms)[etcd Data](#ch05.html#idm44958038893344)
    *   mTLSによるPod間暗号化,[mTLSによるPod間暗号化を理解する](#ch05.html#idm44958038713968)
    *   サンプル演習,[サンプル演習](#ch05.html#idm44958038671600)
    *   OSレベル・セキュリティ・ドメインのセット,[OSレベル・セキュリティ・ドメインの適切なセット-](#ch05.html#mv_set)[OPAポリシーの](#ch05.html#idm44958039084224) [実装](#ch05.html#mv_set)
*   ミニキューブ、[練習と模擬試験](#ch01.html#idm44958053948320)
*   mkdirコマンド、[コンテナにカスタムプロファイルを適用する](#ch04.html#idm44958040421296)
*   モード、[PSA（Pod Security Admission）を理解する](#ch05.html#idm44958039816608)
*   モニタリング、[モニタリング、ログ、およびランタイムセキュリティ](#ch01.html#idm44958052387712)、[モニタリング、ログ、およびランタイムセキュリティ-試験の](#ch07.html#mon_ch)[要点](#ch07.html#idm44958035044240)、[監査ログを使用してアクセスを監視する-](#ch07.html#mon_ab)[Webhookバックエンドを](#ch07.html#idm44958035058336) [設定する](#ch07.html#mon_ab) 
    *   (振る舞い分析も参照すること。）
*   mTLS Pod間暗号化、[マイクロサービスでは脆弱性を最小限に抑える](#ch01.html#idm44958052397024)、[mTLSによるPod間暗号化を理解する](#ch05.html#idm44958038713088)
*   コンテナイメージ構築のための多段階アプローチ, [コンテナイメージ構築のための多段階アプローチの](#ch06.html#idm44958038602704)使用,[コンテナイメージ構築のための多段階アプローチの使用](#ch06.html#idm44958038602704)
*   ムシュコ、ベンジャミン 
    *   Certified Kubernetes Administrator (CKA) Study Guide,[Who This Book Is For](#preface01.html#idm44958054145424),[Candidate Skills](#ch01.html#idm44958053953312),[Creating an Ingress with TLS Termination](#ch02.html#idm44958051763552),[Performing](#ch03.html#idm44958046425216) [an](#ch02.html#idm44958051763552) [Upgrade Process](#ch03.html#idm44958046425216),[Managing Secrets](#ch05.html#idm44958039076080).
    *   認定Kubernetesアプリケーション開発者(CKAD)勉強ガイド,[ネットワークポリシーを使ってPod間の通信を制限する](#ch02.html#idm44958052868720)

### N

*   ネームスペース、ポッドセキュリティ基準の適用、[ネームスペースのポッドセキュリティ基準の適用](#ch05.html#idm44958039750672)
*   NAT（ネットワークアドレス変換）、[ネットワークポリシーを使ってPod間通信を制限する](#ch02.html#idm44958052873136)
*   netstatコマンド、[オープンポートの特定と無効化](#ch04.html#idm44958046200048)
*   ネットワークポリシー 
    *   メタデータサーバーへのアクセスを [ネットワークポリシーで](#ch02.html#idm44958053707456)保護する,[メタデータサーバーへのアクセスをネットワークポリシーで保護する](#ch02.html#idm44958053707456)
    *   ポッド間通信を制限するには、[ネットワークポリシーを使ってポッド間通信を制限する。](#ch02.html#no_pp)
*   networkpolicy.io「[ネットワークポリシーを使ってPod間通信を制限する](#ch02.html#idm44958053975952)
*   ネットワーク、外部アクセスの最小化、[ネットワークへの外部アクセスの最小化](#ch04.html#idm44958046211488)
*   nginx、[読み取り専用コンテナのルートファイルシステムを設定する](#ch07.html#idm44958035669680)
*   ノードメタデータ, 保護,[ノードメタデータとエンドポイントの保護](#ch02.html#n_pn)
*   NodePort サービスタイプ、[シナリオ：攻撃者がダッシュボード機能にアクセスする](#ch02.html#idm44958051109728)
*   非ルートユーザー、利用の強制、[非ルートユーザーの利用の強制](#ch05.html#idm44958040350160)
*   監査レベルなし、[監査ポリシーファイルの作成](#ch07.html#idm44958035441760)

### O

*   OCI（Open Container Initiative）ランタイム、[gVisorのインストールと設定](#ch05.html#idm44958038858672)
*   OPA（オープン・ポリシー・エージェント）、[オープン・ポリシー・エージェント（OPA）とゲートキーパーを理解する](#ch05.html#idm44958039458816)
*   OPA（オープン・ポリシー・エージェント）ゲートキーパー 
    *   [Kubernetesプリミティブ](#ch01.html#idm44958054174880)、[適切なOSレベルのセキュリティドメインのセット](#ch05.html#idm44958040370304)、[Open Policy Agent (OPA)とGatekeeperについて理解する](#ch05.html#idm44958039458112)。
    *   インストール,[ゲートキーパーのインストール](#ch05.html#idm44958039420224)
    *   図書館、[OPAポリシーの実装](#ch05.html#idm44958039085136)
    *   [OPAゲートキーパーで許可](#ch06.html#opa_wh)された画像レジストリをホワイトリストに登録する,[OPA GateKeeperで許可された画像レジストリをホワイトリストに登録する-](#ch06.html#opa_wh)[OPA GateKeeperで許可された画像レジストリを](#ch06.html#idm44958037545936) [ホワイトリストに登録する。](#ch06.html#opa_wh)
*   OpenSSLコマンド、[TLS証明書と鍵の作成](#ch02.html#idm44958051252928)
*   最適化ツール, コンテナイメージ用,[コンテナイメージ最適化ツールを使う](#ch06.html#idm44958038176608)
*   オライリーの学習プラットフォーム、[プラクティスと模擬試験](#ch01.html#idm44958053944656)

### P

*   パッケージ, 不要なパッケージの削除,[不要なパッケージの削除](#ch04.html#idm44958046336592)
*   許可 
    *   サービスアカウントの最小化,[サービスアカウントの最小化権限-](#ch03.html#per_sa)[サービスアカウントのシークレットの作成](#ch03.html#idm44958046480512)
    *   [権限を](#ch03.html#idm44958047226640)確認する,[権限を確認する](#ch03.html#idm44958047226640)
*   プラットフォームバイナリ, 検証,[Kubernetesプラットフォームバイナリの検証](#ch02.html#idm44958051455520)
*   Podセキュリティの承認、[適切なOSレベルのセキュリティドメインのセット](#ch05.html#idm44958040369456)
*   ポッド間通信、ネットワークポリシーによる制限、[ネットワークポリシーによるポッド間通信の制限-](#ch02.html#pp_re)[きめ細かな受信トラフィックの](#ch02.html#idm44958052052160) [許可](#ch02.html#pp_re)
*   mTLSによるPod間暗号化、 [mTLSによるPod間暗号化を理解する](#ch05.html#idm44958038712448)
*   ポッド 
    *   サービスアカウントを [Podに](#ch03.html#idm44958047198128)バインドする。
    *   名前空間のセキュリティ標準の適用,[名前空間のPodセキュリティ標準の適用](#ch05.html#idm44958039749936)
*   PodSecurityContext API,[セキュリティコンテキストを理解する](#ch05.html#idm44958040355200)
*   政策（OPA）、[OPA政策の実装](#ch05.html#idm44958039410080)
*   ポーテナー、[GUI要素を保護する](#ch02.html#idm44958051115520)
*   ポート 
    *   APIサーバーの設定,[APIサーバーへの接続](#ch03.html#idm44958051337968)
    *   オープンポートの無効化,[オープンポートの特定と無効化](#ch04.html#idm44958046206256)
    *   開いているポートの特定 [、開いているポートの特定と無効化](#ch04.html#idm44958046205312)
    *   開いている [ポートを特定し、無効にする](#ch04.html#idm44958046204368)
    *   ポート10250、[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053720016)
    *   ポート10257、[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053734256)
    *   ポート10259、[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053733056)
    *   ポート2379-2380、[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053731472)
    *   ポート30000-32767「[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053719040)
    *   ポート6443「[ノードのメタデータとエンドポイントの保護](#ch02.html#idm44958053730528)
*   ファルコによる実践的クラウドネイティブ・セキュリティ（デジオアンニとグラッソ）、[ファルコを理解する](#ch07.html#idm44958036470416)
*   模擬試験,[練習と模擬試験](#ch01.html#idm44958053947616)
*   prefix、[ポッドセキュリティ入場（PSA）を理解する](#ch05.html#idm44958039816000)
*   プリミティブ,[関与するKubernetesプリミティブ](#ch01.html#idm44958054180176)
*   秘密鍵, 作成,[秘密鍵の作成](#ch03.html#idm44958047266352)
*   特権コンテナ, 避ける,[特権コンテナを避ける](#ch05.html#idm44958040067648)
*   特権レベル、[ポッドセキュリティアドミッション（PSA）を理解する](#ch05.html#idm44958039789792)
*   プロフィール 
    *   AppArmor、[プロファイルを理解する](#ch04.html#idm44958046158912)
    *   コンテナへの適用、[コンテナへのプロファイルの適用](#ch04.html#idm44958046113344)
    *   custom,[カスタムプロファイルの設定](#ch04.html#idm44958046128240),[カスタムプロファイルの設定](#ch04.html#idm44958045819328),[コンテナへのカスタムプロファイルの適用](#ch04.html#idm44958043525344)
*   PSA（Pod Security Admission）、[PSA（Pod Security Admission）を理解する](#ch05.html#idm44958039818336)
*   PSP（ポッドセキュリティポリシー）、[ポッドセキュリティアドミッション（PSA）を理解する](#ch05.html#idm44958039819552)
*   PSP（PodSecurityPolicy）アドミッションコントローラー、[バージョニングスキーム](#ch03.html#idm44958046435888)
*   PSS（ポッド・セキュリティ・スタンダード）、[PSA（ポッド・セキュリティ・アドミッション）を理解する](#ch05.html#idm44958039818944)
*   パブリックイメージレジストリ,[パブリックイメージレジストリの利用](#ch06.html#idm44958037962512)

### R

*   RBAC（役割ベースのアクセス制御）、[クラスタ・ハードニング](#ch01.html#idm44958052151056)
*   読み取り専用コンテナ・ルート・ファイルシステム, 設定,[読み取り専用コンテナ・ルート・ファイルシステムの設定](#ch07.html#idm44958035672016)
*   攻撃面／ペイパーの数を減らす、[レイヤーの数を減らす](#ch06.html#idm44958038294400)
*   参考マニュアル、試験用、[ドキュメンテーション](#ch01.html#idm44958054194624)
*   レジストリ、パブリックイメージ、[パブリックイメージレジストリの活用](#ch06.html#idm44958037961808)
*   Rego,[Open Policy Agent (OPA) と Gatekeeper を理解する](#ch05.html#idm44958039457200),[OPA GateKeeper で許可されたイメージレジストリをホワイトリストに登録する](#ch06.html#idm44958037878768)。
*   リリース・ケイデンス,[リリース・ケイデンス](#ch03.html#idm44958046431344)
*   リリースノート,[試験オブジェクト](#ch01.html#idm44958054082128)
*   監査レベルの要求、[監査ポリシーファイルの作成](#ch07.html#idm44958035440384)
*   RequestResponse監査レベル、[監査ポリシーファイルの作成](#ch07.html#idm44958035439712)
*   リクエスト, 処理,[リクエストを処理する](#ch03.html#idm44958051359328)
*   制限レベル、[ポッドセキュリティアドミッション（PSA）を理解する](#ch05.html#idm44958039789120)
*   制限付き特権, [制限](#ch02.html#idm44958053089552)付きユーザの作成,[制限付きユーザの作成](#ch02.html#idm44958053089552)
*   セーブ 
    *   APIサーバーへのアクセスを制限する、[APIサーバーへのアクセスを制限する-](#ch03.html#res_api)[サービスアカウントのシークレットを](#ch03.html#idm44958046481456) [作成する。](#ch03.html#res_api)
    *   ネットワークポリシーを使用したポッド間通信、[ネットワークポリシーを使用してポッド間通信を制限する-](#ch02.html#re_pp)[きめ細かな受信トラフィックを](#ch02.html#idm44958052051216) [許可する](#ch02.html#re_pp)
    *   ユーザパーミッション、[ユーザパーミッションの制限-シナリオ](#ch03.html#re_up)[：攻撃者はサービスアカウントからAPIサーバを呼び出すことができる](#ch03.html#idm44958047209808)
*   ライス、リズ、コンテナ・セキュリティ、[サプライチェーンの安全確保](#ch06.html#idm44958038162752)
*   ロールバインディング, 作成,[ロールとロールバインディングの作成](#ch03.html#idm44958047240192),[ロールバインディングの作成](#ch03.html#idm44958046846784)
*   役割 
    *   作成,[役割とRoleBindingの作成](#ch03.html#idm44958047239520)
    *   アイデンティティとアクセス管理（IAM）、[IAMロールの最小化-](#ch04.html#rol_min)[ファイルパーミッションの](#ch04.html#idm44958046218512) [変更](#ch04.html#rol_min)
*   規則引数、[イングレスの作成](#ch02.html#idm44958051205056)
*   規則 
    *   ファルコ [規則](#ch07.html#idm44958036400512)
    *   ファイアウォールの場合は、[ファイアウォールの規則を設定する](#ch04.html#idm44958046183152)。
    *   既存の規則を上書きする,[既存の規則を上書きする](#ch07.html#idm44958035903760)
*   ルールファイル（Falco）,[Falcoルールファイルの基本を理解する](#ch07.html#idm44958036405808)
*   RUNコマンド、[レイヤー数を減らす](#ch06.html#idm44958038287840)
*   [gVisorの](#ch05.html#idm44958038857904)実行、[インストール、設定](#ch05.html#idm44958038857904)
*   ランタイム・クラス, 作成,[ランタイム・クラスの作成と使用](#ch05.html#idm44958038851024)
*   ランタイムセキュリティ、[モニタリング、ロギング、ランタイムセキュリティ](#ch01.html#idm44958054184528)、[モニタリング、ロギング、ランタイムセキュリティ-試験](#ch07.html#rs_ch)[必修科目](#ch07.html#idm44958035043296) 
    *   (振る舞い分析も参照）。

### S

*   サンプル練習 
    *   振る舞い分析,[試験の要点](#ch07.html#idm44958035046128)
    *   クラスターハードニング,[サンプル練習](#ch03.html#idm44958046405824)
    *   クラスタ設定,[サンプル演習](#ch02.html#idm44958051397392)
    *   マイクロサービスの脆弱性、[サンプル演習](#ch05.html#idm44958038672544)
    *   サプライチェーン・セキュリティ,[サンプル演習](#ch06.html#idm44958036671088)
    *   システム・ハードニング,[サンプル演習](#ch04.html#idm44958040407456)
*   サンドボックス, コンテナランタイム,[コンテナランタイム・サンドボックスを理解する-](#ch05.html#sa_cr)[ランタイム・クラスの作成と使用](#ch05.html#idm44958038718992)
*   既知の脆弱性について画像をスキャンする,[既知の脆弱性について画像をスキャンする](#ch06.html#idm44958036716544)
*   シナリオ 
    *   管理者は悪意のあるイベントをリアルタイムで監視できる [シナリオ管理者は悪意のあるイベントをリアルタイムで監視できる](#ch07.html#idm44958035485408)
    *   攻撃者はインターネットからAPIサーバを呼び出すことができる [：攻撃者はインターネットからAPIサーバを呼び出すことができる](#ch03.html#idm44958047286896)
    *   攻撃者はサービスアカウントからAPIサーバを呼び出すことができる [：攻撃者はサービスアカウントからAPIサーバを呼び出すことができる](#ch03.html#idm44958047212672)
    *   コンテナの脆弱性を悪用する攻撃者 [シナリオコンテナの脆弱性を悪用する攻撃者](#ch06.html#idm44958038631584)
    *   攻撃者がパッケージの脆弱性を悪用する、[シナリオ攻撃者がパッケージの脆弱性を悪用する](#ch04.html#idm44958046359264)
    *   攻撃者が別のコンテナにアクセスする [シナリオ攻撃者が別のコンテナにアクセスする](#ch05.html#idm44958038883280)
    *   攻撃者がダッシュボード機能にアクセスする[シナリオ攻撃者がダッシュボード機能にアクセスする](#ch02.html#idm44958051111344)
    *   攻撃者がetcdを実行しているノードにアクセスする [シナリオ攻撃者がetcdを実行しているノードにアクセスする](#ch05.html#idm44958039068048)
    *   攻撃者がポッドにアクセスする [シナリオ攻撃者がPodにアクセスする](#ch02.html#idm44958053972688)
    *   攻撃者がバイナリに悪意のあるコードを注入する[シナリオ攻撃者がバイナリに悪意あるコードを注入する](#ch02.html#idm44958051450592)
    *   攻撃者がコンテナイメージに悪意あるコードを注入する [シナリオ攻撃者がコンテナイメージに悪意あるコードを注入する](#ch06.html#idm44958038154192)
    *   攻撃者が悪意のあるソフトウェアをインストールする [シナリオ攻撃者が悪意のあるソフトウェアをインストールする](#ch07.html#idm44958035696592)
    *   攻撃者が2つのPod間の通信を盗聴する [シナリオ攻撃者が2つのPod間の通信を盗聴する](#ch05.html#idm44958038701920)
    *   攻撃者がルートユーザーコンテナアクセスを悪用する[シナリオ攻撃者がルートユーザコンテナアクセスを悪用する](#ch05.html#idm44958040364992)
    *   攻撃者が悪意のあるコンテナイメージをアップロードする[シナリオ攻撃者が悪意のあるコンテナイメージをアップロードする](#ch06.html#idm44958037953152)
    *   攻撃者が資格情報を使ってファイルにアクセスする [シナリオ攻撃者が資格情報を使ってファイルにアクセスする](#ch04.html#idm44958046321632)
    *   危殆化したPodがメタデータサーバにアクセスする、[シナリオ：侵害されたPodがメタデータサーバにアクセスできる](#ch02.html#idm44958053715216)
    *   開発者がPodセキュリティのベストプラクティスに従わない [シナリオ開発者がPodセキュリティのベストプラクティスに従っていない](#ch05.html#idm44958039847296)
    *   Kubernetes管理者は攻撃者の行動を観察できる [シナリオKubernetes管理者は攻撃者の行動を観察できる](#ch07.html#idm44958036490176)
*   seccomp,[関係する外部ツール](#ch01.html#idm44958052109312),[seccompを使用する-](#ch04.html#se_ab)[カスタムプロファイルをコンテナに](#ch04.html#idm44958040418144) [適用する](#ch04.html#se_ab)。
*   秘密 
    *   [シナリオ攻撃者が悪意のあるソフトウェアをインストールする](#ch07.html#idm44958035691984)
    *   でコンテナを構成する,[ConfigMapまたはSecretでコンテナを構成する](#ch07.html#idm44958035684256)
    *   サービスアカウント用の [シークレット](#ch03.html#idm44958046543008)作成,[サービスアカウント用のシークレット作成](#ch03.html#idm44958046543008)
    *   [秘密の管理](#ch05.html#sec_man),[etcdデータの](#ch05.html#idm44958038892320) [暗号化](#ch05.html#sec_man)
*   セキュリティ 
    *   ネームスペースの標準の適用,[ネームスペースのポッドセキュリティ標準の適用](#ch05.html#idm44958039748960)
    *   [検出されたセキュリティ問題を修正する](#ch02.html#idm44958051828736)
    *   サプライチェーンの [セキュリティ](#ch01.html#idm44958052391488) 
        *   (サプライチェーンのセキュリティも参照のこと）
    *   ランタイム、[モニタリング、ロギング、ランタイムセキュリティ](#ch01.html#idm44958052388688)
    *   OS レベルドメインの設定、「[適切な OS レベルセキュリティドメインのセット -](#ch05.html#sec_set)[OPA ポリシーの](#ch05.html#idm44958039083280) [実装](#ch05.html#sec_set)」 を[参照](#ch05.html#idm44958039083280)。
*   セキュリティ・コンテキスト,[セキュリティ・コンテキストを理解する](#ch05.html#idm44958040358992)
*   SecurityContext API,[セキュリティコンテキストを理解する](#ch05.html#idm44958040354592)
*   SELinux（セキュリティ強化Linux）、[AppArmorを使う](#ch04.html#idm44958046165776)
*   セマンティック・ヴァージョニング・スキーム,[ヴァージョニング・スキーム](#ch03.html#idm44958046438928)
*   サービスアカウント 
    *   [サービスアカウントをPodにバインドする](#ch03.html#idm44958047197088)
    *   シークレット作成,[サービスアカウントのシークレット作成](#ch03.html#idm44958046531840)
    *   トークンの自動マウントを無効にする,[サービスアカウントトークンの自動マウントを無効にする](#ch03.html#idm44958046678032)
    *   トークンの失効、[管理者権限を持つユーザの作成](#ch02.html#idm44958053101216)
    *   トークンの生成,[サービスアカウントトークンの生成](#ch03.html#idm44958046567888)
    *   [サービスアカウントの](#ch03.html#sa_per)権限を最小化する [\-](#ch03.html#sa_per)[サービスアカウントのシークレットを](#ch03.html#idm44958046479568) [作成する](#ch03.html#sa_per)
*   ServiceAccount、[管理者権限を持つユーザの作成](#ch02.html#idm44958051071248)
*   サービス、無効化、[サービスの無効化](#ch04.html#idm44958046352272)
*   SHA、[ハッシュに対するバイナリの検証](#ch02.html#idm44958051437264)
*   SHA256、[ハッシュに対するバイナリの検証](#ch02.html#idm44958051417920)、[コンテナイメージの検証](#ch06.html#idm44958038141104)
*   コンテナ・イメージに署名する,[コンテナ・イメージに署名する](#ch06.html#idm44958038159632)
*   ベース画像のサイズ,[サイズの小さいベース画像を選ぶ](#ch06.html#idm44958038621792)
*   snapd パッケージマネージャー、[シナリオ攻撃者がパッケージの脆弱性を悪用する](#ch04.html#idm44958046359968)
*   ssコマンド、[オープンポートの特定と無効化](#ch04.html#idm44958046198400)
*   静的解析, ワークロードの,[ワークロードの静的解析-](#ch06.html#sa_wo)[Kubernetesマニフェストの解析にKubesecを使う](#ch06.html#idm44958036908000)
*   status コマンド、[サービスを無効にする](#ch04.html#idm44958046349104)
*   Study4exam、[練習と模擬試験](#ch01.html#idm44958054024496)
*   suコマンド、[ユーザへの切り替え](#ch04.html#idm44958046293008)
*   sudoコマンド、[ユーザへの切り替え](#ch04.html#idm44958046286432)
*   sudo systemctl restart kubeletコマンド、[ImagePolicyWebhook Admission Controllerプラグインの設定](#ch06.html#idm44958037204064)
*   サプライチェーンセキュリティ,[サプライチェーンセキュリティ-サンプル](#ch06.html#scs_ch)[演習](#ch06.html#idm44958036669168) 
    *   [サプライチェーンセキュリティ](#ch01.html#idm44958052392432)、[サプライチェーンセキュリティについて](#ch06.html#idm44958038645216)
    *   復習問題の解答,[第6章「サプライチェーンセキュリティ」](#app01.html#scs_a)\-[第6](#app01.html#idm44958032638528)章[「サプライチェーンセキュリティ」](#app01.html#idm44958032638528).
    *   ベースイメージフットプリントの最小化,[ベースイメージフットプリントの最小化-](#ch06.html#scs_min)[コンテナイメージ最適化ツールを](#ch06.html#idm44958038168464) [使用する](#ch06.html#scs_min)
    *   サンプル演習,[サンプル演習](#ch06.html#idm44958036670112)
    *   既知の脆弱性に対するイメージのスキャン,[既知の脆弱性に対するイメージのスキャン](#ch06.html#idm44958036715904)
    *   サプライチェーンの安全性確保、[サプライチェーンの安全性確保-](#ch06.html#scs_sec)[ImagePolicyWebhook Admission Controllerプラグインの](#ch06.html#idm44958037153424) [設定](#ch06.html#scs_sec)
    *   ワークロードの静的解析,[ワークロードの静的解析-](#ch06.html#scs_sa)[Kubernetesマニフェストの解析にKubesecを使う](#ch06.html#idm44958036907152)
*   sysctlコマンド、[特権コンテナを回避する](#ch05.html#idm44958039958448)
*   システム・ハードニング,[システム・ハードニング-サンプル](#ch04.html#sh_ch)[演習](#ch04.html#idm44958040408304) 
    *   について、[システム・ハードニング](#ch01.html#idm44958052148784)、[システム・ハードニング](#ch04.html#idm44958046378832)
    *   復習問題の解答、[第4章「システムのハード化」](#app01.html#sh_a)\-[第4章「システムのハード化](#app01.html#idm44958033671280)
    *   ネットワークへの外部アクセスの最小化,[ネットワークへの外部アクセスの最小化](#ch04.html#idm44958046213072)
    *   ホストOSのフットプリントの最小化,[ホストOSのフットプリントの最小化-](#ch04.html#sh_min)[不要なパッケージの](#ch04.html#idm44958046331008) [削除](#ch04.html#sh_min)
    *   IAMロールの最小化,[IAMロールの最小化-](#ch04.html#sh_rol)[ファイルパーミッションの](#ch04.html#idm44958046217568) [変更](#ch04.html#sh_rol)
    *   サンプル演習,[サンプル演習](#ch04.html#idm44958040406608)
    *   カーネルハードニングツールの使用,[カーネルハードニングツールの使用-](#ch04.html#sh_kht)[コンテナへのカスタムプロファイルの](#ch04.html#idm44958040419840) [適用](#ch04.html#sh_kht)
*   systemctlコマンド、[サービスを無効にする](#ch04.html#idm44958046353920)
*   systemctl statusコマンド、[オープンポートの特定と無効化](#ch04.html#idm44958046192304)

### T

*   Tetragon、[シナリオ：Kubernetes管理者は攻撃者の行動を観察できる](#ch07.html#idm44958036485728)
*   TLS（トランスポート・レイヤー・セキュリティ） 
    *   終端を伴うイングレスの作成、[TLS終端を伴うイングレスの作成-](#ch02.html#tls_cre)[イングレスの](#ch02.html#idm44958053771328) [呼び出し](#ch02.html#tls_cre)。
    *   TLS証明書と鍵の作成,[TLS証明書と鍵の作成](#ch02.html#idm44958051252256)
    *   [クラスタ設定](#ch01.html#idm44958052157312)終了
    *   TLSシークレット、[TLS型シークレットの作成](#ch02.html#idm44958051245216)
*   TLS型シークレット, 作成,[TLS型シークレットの作成](#ch02.html#idm44958051244176)
*   トークン, サービスアカウント,[サービスアカウントトークンの自動マウントを無効にする](#ch03.html#idm44958046677184)
*   用具 
    *   外部、[関与する外部ツール](#ch01.html#idm44958052105408)
    *   カーネルハードニング、[カーネルハードニングツールの使用-](#ch04.html#t_kht)[カスタムプロファイルをコンテナに](#ch04.html#idm44958040418992) [適用する](#ch04.html#t_kht)
    *   最適化、[コンテナイメージ最適化ツールの使用](#ch06.html#idm44958038193360)
*   touchコマンド、[ファイルのパーミッションと所有者の表示](#ch04.html#idm44958046240752)
*   トレーシー、[シナリオKubernetes管理者は攻撃者の行動を観察できる](#ch07.html#idm44958036485024)
*   Tracey, Craig, Kubernetesの管理,[Kubernetes APIとの対話](#ch03.html#idm44958051364992)
*   トリヴィ、[サプライチェーンセキュリティ](#ch01.html#idm44958052393136)、[外部ツール](#ch01.html#idm44958052107024)、[既知の脆弱性に関するイメージのスキャン](#ch06.html#idm44958036718768)
*   TTL（有効期間）、[管理者権限を持つユーザの作成](#ch02.html#idm44958053100240)

### U

*   Ubuntu Linux、 [ホストOSのフットプリントを最小化](#ch04.html#idm44958046366224)するCISベンチマーク
*   UFW (Uncomplicated Firewall)、[ファイアウォールルールのセットアップ](#ch04.html#idm44958046182208)
*   Kubernetesをアップデートする,[Kubernetesを](#ch03.html#up_kub)[頻繁に](#ch03.html#idm44958046418272) [アップデートする-まとめ](#ch03.html#up_kub)
*   ユーザID, 設定,[特定のユーザとグループIDの設定](#ch05.html#idm44958040202528)
*   ユーザ管理,[ユーザ管理を理解する](#ch04.html#idm44958046315488)
*   ユーザパーミッション、制限、[ユーザパーミッションの制限-シナリオ](#ch03.html#up_re)[：攻撃者はサービスアカウントからAPIサーバを呼び出すことができる](#ch03.html#idm44958047208832)
*   usermodコマンド、[グループの削除](#ch04.html#idm44958046251072)
*   ユーザ 
    *   追加,[ユーザの追加](#ch04.html#idm44958046302624)
    *   グループに追加する,[ユーザをグループに代入する](#ch04.html#idm44958046261984)
    *   kubeconfigファイルへの追加,[kubeconfigファイルへのユーザの追加](#ch03.html#idm44958047232864)
    *   管理者権限を持つユーザの作成,[管理者権限を持つユーザの作成](#ch02.html#idm44958051069936)
    *   制限付き権限の作成,[制限付き権限を持つユーザの作成](#ch02.html#idm44958053088752)
    *   削除,[ユーザの削除](#ch04.html#idm44958046281280)
    *   リスト,[リスト・ユーザ](#ch04.html#idm44958046313184)
    *   ユーザへの切り替え,[ユーザへの切り替え](#ch04.html#idm44958046293984)

### V

*   Vagrant、[練習と模擬試験](#ch01.html#idm44958053946944)
*   検証中 
    *   リクエスト処理の段階として、[リクエストを処理する](#ch03.html#idm44958051350016)
    *   コンテナ・イメージ,[コンテナ・イメージの検証](#ch06.html#idm44958038143664)
*   検証 
    *   ハッシュに対するバイナリの検証,[ハッシュに対するバイナリの検証](#ch02.html#idm44958051438880)
    *   デフォルトのパーミッション、[デフォルトのパーミッションを確認する](#ch03.html#idm44958047022464)
    *   付与されたパーミッション、[付与されたパーミッションの検証](#ch03.html#idm44958046709440)
    *   Kubernetesプラットフォームバイナリ,[Kubernetesプラットフォームバイナリの検証](#ch02.html#idm44958051457376)
    *   パーミッション、[パーミッションの確認](#ch03.html#idm44958047225664)
    *   プラットフォームバイナリ,[Kubernetesプラットフォームバイナリの検証](#ch02.html#idm44958051456464)
*   バージョン・スキーム,[バージョン・スキーム](#ch03.html#idm44958046439632)
*   VirtualBox、[練習と模擬試験](#ch01.html#idm44958053946272)
*   VPN（仮想プライベートネットワーク）、[KubernetesでmTLSを採用する](#ch05.html#idm44958038688384)
*   脆弱性、既知の画像のスキャン、[既知の脆弱性の画像のスキャン](#ch06.html#idm44958036714992)

### W

*   ワーンモード、[PSA（ポッドセキュリティアドミッション）を理解する](#ch05.html#idm44958039806720)
*   ウェブフック・バックエンド,[監査ログを理解する](#ch07.html#idm44958035471632),[ウェブフック・バックエンドを設定する](#ch07.html#idm44958035064816)
*   wgetコマンド、[指向性ネットワークトラフィックを拒否する](#ch02.html#idm44958051676272)
*   ホワイトリスティング 
    *   ImagePolicyWebhook Admission Controllerプラグインで許可された画像レジストリ,[ImagePolicyWebhook Admission Controllerプラグインで許可された画像レジストリのホワイトリスト化](#ch06.html#idm44958037493744)
    *   OPAゲートキーパーで許可された画像レジストリ、[OPAゲートキーパーで許可された画像レジストリのホワイトリスト化-](#ch06.html#wh_opa)[OPAゲートキーパーで許可された画像レジストリの](#ch06.html#idm44958037544992) [ホワイトリスト化](#ch06.html#wh_opa)
*   ワイヤーガード、[KubernetesにmTLSを採用する](#ch05.html#idm44958038689056)
*   ワークロード、 [ワークロードの](#ch06.html#wo_sa)静的解析、[ワークロードの静的解析-](#ch06.html#wo_sa)[Kubernetesマニフェストの解析にKubesecを使う](#ch06.html#idm44958036727440)

### Y

*   YAMLマニフェスト、[デフォルトの振る舞いを観察する](#ch02.html#idm44958053965232)

# 著者について

**ベンジャミン・ムシュコは**、ソフトウェア・エンジニア、コンサルタント、トレーナーとして20年以上の経験を持つ。プロジェクトの自動化、テスト、継続的デリバリに情熱を注ぐ。著述家であり、カンファレンスでの講演も多く、熱心なオープンソース擁護者でもある。CKAD、CKA、CKSの資格を持ち、2023年CNCFアンバサダーを務める。

ソフトウェア・プロジェクトは時に山登りのように感じる。自由時間には[コロラドの14ers](https://www.14ers.com)ハイキングが好きで、長距離トレイルの制覇を楽しんでいる。

# 奥付

*Certified Kubernetes Security Specialist (CKS) Study Guideの*表紙に描かれている動物は、家ガチョウである。これらの鳥は、野生のハイイロガン*（Anser anse*）とハクチョウガン*（Anser cygnoides domesticus*）から選択的に繁殖された。南極大陸を除くすべての大陸に導入されている。考古学的証拠によれば、ガチョウは少なくとも4,000年前から家畜化されていた。

野生のガチョウの大きさは7～9ポンドであるのに対し、家ガチョウは大きさを求めて繁殖され、体重は22ポンドにもなる。脂肪の分布により、野生のガチョウの祖先の水平姿勢に比べ、家畜のガチョウはより直立した姿勢をしている。また、体が大きいため、飛ぶことは少ないが、ある程度の飛行は可能である。

歴史的には、ガチョウはその肉、卵、羽毛を利用するために家畜化されてきた。最近では、ガチョウは裏庭のペットとして、あるいは雑草や葉っぱを食べるので庭の手入れのために飼われている。ガチョウのうるさく攻撃的な性格のため、脅威や侵入者を察知すると大声で騒ぐので、財産を守るためにも使われてきた。

家畜はIUCNによる評価を受けていない。オライリーの表紙を飾る動物の多くは絶滅の危機に瀕しているが、そのすべてが世界にとって重要な動物である。

表紙のイラストはカレン・モンゴメリー。表紙のフォントはGilroy SemiboldとGuardian Sansである。テキストフォントはAdobe Minion Pro、見出しフォントはAdobe Myriad Condensed、コードフォントはDalton MaagのUbuntu Monoである。