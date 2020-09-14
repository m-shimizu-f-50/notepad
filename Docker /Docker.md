# Dockerとは？


## はじめに

よく聞く単語だけどDockerって何？ 何ができるの？ という声を目にする機会が多かったので, 今回は Docker について紹介します。

いきなりDockerの仕組みとメリットを書いてもわかりづらいのでウェブサイトの作成工程の中で、Dockerが大活躍するサーバーのデプロイに絡めて解説していきたいと思います。

そもそもサーバーとはなにか、フロントエンドとバックエンドとは、デプロイとは
と言った関連ワードに関しても解説してます

## サーバーとは
皆さんなんとなくリンクをクリックしたりブラウザ(ChromeやIE)の検索バーにurlを入力してウェブサイトを見ていますが、その裏では一体何が起きているのでしょうか?

urlとはUniform Resource Locatorの略で、文字通りリソースの場所を表すものです。

ここで言う`リソース`とは, htmlやcss, javascript, webページ中で表示したい画像など、`webページを表示するのに必要な材料`を表します。

皆さんがブラウザでurlを入力すると, ブラウザはその "urlが表す場所にあるリソースをください" というリクエストをインターネットに送りだします。

この`リクエストを受け取って必要なリソースを返送してあげるのがサーバー`です。

返送されてきたリソース(`レスポンス`と呼ばれます)はブラウザによって解釈され、みなさんが今見ているような画面として出力されます。

ちなみにサーバーの仕事は、リクエストに応じてただ`事前に用意してあったリソースを返す`だけではありません。

例えばユーザーに似ている芸能人を機械学習で算出して教えてあげるサイトを作ったとすると、ユーザーが画像をアップロードするまで似ている芸能人を計算することはできません。

なので、リクエストが来てからその場で計算したり、機械学習用の別のサーバーに計算を依頼してユーザーに返す画像を決定します。

またデータベースと呼ばれるデータを管理するためのサーバーに、ユーザーの情報を登録してもらったり、レスポンスに必要なユーザー情報を引き出すのもサーバーが行う操作です。

このようにサーバーの役割は非常に広範囲に渡り、また役割ごとに異なる名称で呼ばれることが多いです。（データベースや機械学習サーバーなど)

### まとめ
ウェブサイトとは

`ページを表示するために必要なリソース群と、ユーザーからのリクエストに応じて適切なリソースを返してくれるサーバーのセット`という感じ



## フロントエンドとバックエンド
webサイトを作るときは、各ページを表示するのに必要なリソース群とリクエストに応じて適切なリソースを返すサーバーの両方を用意する必要があることがわかったと思います。

これらは分離して開発可能で、一般的にhtmlやjavascriptなどのwebページの表示に必要なリソースを開発する仕事(スマホアプリのUI開発なども)を`フロントエンド`開発、サーバープログラムを開発する仕事を`バックエンド開発`と呼びます。

ユーザーから見た時にユーザーにより近いところ(ユーザーの端末上)で動くので、htmlやjavascriptなどはフロントエンド、その対比でサーバーはバックエンド、と覚えるといいでしょう。

主にユーザーが目にするUIを扱いたい方はフロントエンドを、それ以外の裏側の仕組みを扱いたい方はバックエンドを勉強するといいでしょう。

## デプロイ
PCの中でサーバープログラムとユーザーに渡すためのリソースが用意できました、サイトの完成です!

が、せっかく作ったサイトもこのままでは他の人に見てもらえません。

他の人がアクセスできる場所(主にインターネット上)に公開する必要があるのです。
ユーザーがサイトやアプリなどを利用可能な状態にする作業のことを、デプロイと呼びます。

Dockerはこのデプロイ作業時に発生しうる様々な問題点を解消してくれるのです。
まずはデプロイ出で起こる様々な問題を見ていきましょう



### 1.管理の複雑化・リソースの無駄遣い
まずサーバーを構成するPCが増えると管理と運用が大変なのは、想像に難くないですよね。
たとえばもし100台の小型PCを組み合わせる運用を考えた場合、当然100台分の環境構築や設定が必要になります。

一度設定してしまえば終わりというものでも有りません。プログラムのアップデートが入るとまた100台分プログラムをインストールし直す必要があります。(そして数が増えるほどミスが発生し、アップデートし忘れたサーバーなどが出てくる)
これは大変な手間ですね。

また、この複数のPCが常に100%稼働しているわけではなく、時間によっては遊んでいる(リクエストが全然来ない)PCが沢山出てきます。

「暇なら一台のPCでたくさんのサーバープログラムを動かせばいいじゃん」とお思いになる方もいらっしゃると思います。

もちろんひとつのPCで複数のプログラムを同時に実行することは可能なのですが、いくつか懸念点が出てきます。

まずリソース(CPUやメモリなど)を共有するため、`単純に各プログラムの処理速度が遅く`なります。

沢山の人が同じ場所に詰めかけると渋滞が起こりますよね？
あれと同じでメモリなどに複数のプログラムが同時にアクセスしようとすると、待ち時間が発生するようになります。

またセキュリティや可用性(サーバーが安定して稼働し続ける能力のこと)に関しても気になる点があります。

例えばある1台のPCが10個のサーバープログラムを動かしていたとします。
仮にこのPCが悪意のあるユーザーから攻撃を受け、PCの管理者権限を乗っ取られてしまった場合、この10個のサーバープログラムでやり取りされたすべての情報が攻撃者に渡ってしまうことになります。

1つのサーバープログラムで100人分のユーザーのリクエストをさばいていた場合、100×10=1000人分の個人情報が流出することになります。

もしこれを10台のPCで分散して実行していたなら100*1=100人分の個人情報の流出ですむわけです。(十分アウトですが)

また攻撃だけではなく、プログラムのバグや大量のリクエストによる処理落ちでPCがフリーズした際、同じPCで10個のプログラムを実行しているとその全てが停止してしまうことになります。


### 2. 実行環境の多様化
またPCの数が増えるにつれ、システムを構成する環境が複雑かつ多様化しました。
これにより各PCの環境の違いにも気を使う必要が出てきます。

開発時に用いたPC上では正しく動くプログラムでも、実行環境(OSやライブラリのバージョン)が異なるPC上では動かないことが多々あります。

オペレーティング・システム(Operation System、OS) ・・・ アプリやデバイスを動作させるための基本となるソフトウェアのこと。
ソフトウェア(みなさんが開発してるプログラムなど)とハードウェア(CPUやメモリなどの器械)の連携を司る中枢的な役割を果たす。
プログラムを実行する際、プログラムの実行に必要なCPUやRAMメモリなどがOSによって確保、割り当てられる。
OSの助けがないとプログラムはほぼ何もできない。(OSの機能を使わないとプログラムは標準出力機構にアクセスできないので、Hello Worldを画面に出力することさえできません!)
有名どころを上げるとwindowsやmacOS、Linuxなどが該当する。

実行環境 ・・・ 作成したプログラム、アプリケーションを実行するための環境のこと。
OSの種類や、ライブラリのversion、環境変数などが該当する。
上述したとおり実行環境が異なると、同じプログラムでも動かなくなったりする。

プログラムはOSの機能に依存しているのにそのOSの種類が開発時と異なったら、動かなくなるのはなんとなく想像つきますよね。

異なる環境でプログラムを動かそうとすると動かないのでプログラムの開発時に使用するPCの環境はサーバーとしての使用するPC環境に合わせる必要がありました


## 仮想化とは？
これらの悩みを解決してくれるのが、仮想化という技術です。

仮想化とは、ソフトウェアによって複数のハードウェアを統合し、自由なスペックでハードウェアを再現する技術で、限られた数量の物理リソース（CPU、メモリ、ストレージ、ネットワーク等）を、実際の数量以上のリソース（論理リソース）が稼働しているかのように見せかけることです。


* CPU(Central Processing Unit) ・・・ コンピュータの制御や演算や情報転送をつかさどる中央演算処理装置。簡単に言うとCPUの性能によってどのくらい早く計算できるか(プログラムを動かすことができるか)を表す。CPUの性能指標について詳しくはこちら

* メモリ(RAM、Random Access Memory) ・・・ データやプログラムを一時的に記憶する部品で、高速に読み書きできます。 プログラムやアプリケーションはこのメモリの上に読み込まれ、実行されます。簡単に言うとメモリが大きいほど同時にたくさんのアプリケーションを動かせます。(動画編集しながら、powerpointいじったり、ネットサーフィンしたり...etc)
PCの電源を切るとメモリ(RAM)上に保持されていたデータは全て消えます。

* ストレージ ・・・ データやプログラムを永続的に記憶する部品で、ストレージが大きいほどたくさんのデータを記憶することができます。(写真を100万枚保存できる、音楽を1万曲保存できる、など)使用されていないデータやアプリケーションは普段ストレージに保存されており、使用するとなった時にメモリ(RAM)にコピーされアプリケーションが実行されます。ストレージの種類としてはHDDやSSDなどがあります。
PCの電源を切ってもストレージに保持されているデータは消えません。

実際には１つしかないのに複数あるように見せかける ということで、仮想化技術と呼ばれているのですね。(蛇足ですがメモリやストレージは一つのPCに複数搭載することもできます。複数搭載されているメモリやストレージを仮想化技術によって１つのメモリやストレージとして扱うこともできます。)

## 仮想マシン
最初に流行となった仮想化技術は仮想マシンというものでした。
これはPC上に仮想的なPCを立ち上げるという技術です。

例えば仮想マシンを用いると、1台しかPCが存在しないにも関わらずあたかも4つのPCが存在しているように振る舞うことができます。

また、この際に`仮想マシンが使用するOSは自由に選択することができます。`
macOSの上でwindowsを動かす といったことができるのです。

これにより開発用のPCとサーバー用のPCが別の環境であったとしても、仮想マシンを利用することで環境を揃えるということが可能になりました。

また仮想化によって生成された仮想マシンは安全に分離されているので、1つの仮想マシンに異常が発生しても他の仮想マシンに影響が出るリスクを軽減できます。(実態のPCに異常が発生すると全ての仮想マシンに影響が出てしまうので、リスクを軽減できる という表現にとどめています。)

これにより、１台のPC上で安全に複数のプログラムを動かすことができるようになったのでした。

# Docker
仮想マシンの登場によってデプロイ時の悩みの多くは解消されました。
しかし仮想マシンにも弱点は有りました。

それは目的のプログラムとは関係のないサービスなども多数動作することになるので、オーバーヘッド(立ち上げにかかる時間など)が大きく、リソースも無駄に多く必要になりがちだという点です。(OSに関するプログラムの分多くのメモリを消費する)

PCを起動すると、デスクトップが表示されるまである程度時間がかかるのはみなさんもよくご存知だと思います。

そこで誕生したのがコンテナ仮想化という技術でした。

コンテナ仮想化という技術をはプログラムを開発・実行環境から隔離することで、安全に素早く複数のプログラムを実行することができます。

コンテナ仮想化技術はOSを新たにインストールすることはしません。
もともとPCに備わっているOSをそのまま利用します。
OSのもつ複数の機能を組み合わせて作り出した「コンテナ」という隔離空間上でアプリケーションを実行することで、同じOS上でも互いに隔離されたプロセスを作ることができるのです。

新たにOSを立ち上げる必要がないため、仮想マシンに比べてアプリケーション(プログラム)の立ち上げが早く、またメモリも節約することができます。

Dockerはこのコンテナ仮想化技術を簡単に扱えるようにしたソフトウェアです。
本来コンテナを作成するのには複数のOSコマンドを実行しなければならないところ、Dockerコマンドを一発叩くだけでコンテナが作成できるようになっているのです。
ちなみにコンテナ仮想化技術はDocker固有のものでなく、他にもコンテナ仮想化が簡単に行えるようにと開発されたソフトウェアは多数存在します。
それぞれのソフトウェアはコンテナ仮想化技術に加えて各々特徴をもっています。
Dockerならではの特徴としてはDockerfileやDockerhubといったものが挙げられるでしょう。

Docker != コンテナ仮想化技術

## Dockerfile
DockerではDockerfileというファイルを元にコンテナを生成(アプリを実行)します。
Dockerfileは環境構築のレシピのようなもので、Dockerはレシピ通りに上から順番にコマンドを実行し、コンテナ内の環境を整えていきます。

Dockerfileの例
image.png

Dockerfileで環境を管理することで以下のようなメリットがえられます。

* 簡単に何度でもコンテナを生成できる
一度Dockerfileを作成してしまえば、コマンド一発で何度でもコンテナを生成できます。
なので例えばサーバーが急に100台必要になっても、リソース(CPUやメモリなど)さえあれば簡単に用意することができるのです。

* コンテナを持ち運びできる
またDockerfileはただのソースコードなので、簡単に別のPCに移植したり第三者と共有することができます。
Dockerfileを共有するためのDockerhubといったサービスも有り、手軽に他の方が生成したコンテナを利用することができます。
例えば会社に新入社員が入ってきた際の環境構築も、Dockerfileを共有するだけで済むのです。

* アプリケーションのバージョン管理が楽になる
Dockerfileはただのソースコードなので、git等で差分管理ができます。
なので例えば新しくリリースしたバージョンが何かしらの理由でバグっていた場合にも、確実に動いていた前のバージョンに戻すことができます。(プログラムのみをgitで管理していると、ライブラリのバージョンと言った環境依存のバグを解消できません。)

このような利便性からDockerは現在人気を博しているわけですね。




# まとめ
Dockerとは
他に実行しているプログラムから影響を受けない隔離された環境上で新たなプログラムを実行することを可能にする、コンテナ型の仮想環境を作成、配布、実行するためのソフトウェアです。

Dockerを使うことで次のようなメリットがえられる。
- １台のPC上で簡単かつ安全に独立したサーバーを複数動かすことができる。
- アプリケーションのバージョン管理が楽になる(アップデートによってアプリケーションが壊れても、すぐに動いてたバージョンに戻せる)
- 相手がDockerを使える環境であれば、確実に動かせるソフトウェアを作ることができる(環境の違いを吸収できる)


DockerはGitと並んでエンジニアにとっては習得必須の技術になりつつあります。


おまけ1
環境の違いと言うと、フロントエンドエンジニアの方ならIEとChrome などと言ったブラウザの違いに苦しめらている方も多いと思います。
Dockerでは残念ながらこれらの違いを吸収することはできません。

DockerがカバーしてくれるのはOSや環境変数といったものの違いであるのに対して、IEとChromeの違いはブラウザの持つレンダリングエンジンやJavascriptエンジンの違いであるからです。

これらの違いを吸収してくれるソフトウェアが出たら嬉しいですね。

おまけ2
Dockerはもともと持っているOSを使い回すと説明した時に、
鋭い方は「あれ、これじゃOSの違いを吸収できなくね？」と思ったかもしれません。


実はコンテナという機能はLinuxOSの機能であり、以前はDockerはLinuxでしか使えませんでした。
その後macOSやwindowsでもDockerが使えるようにと「Docker Desktop for Mac」や「Docker for Windows」といったソフトウェアが登場します。
これらは環境がLinuxであるコンテナを実行する際、裏側でハイパーバイザー型の仮想マシン技術を使ってLinux環境の仮想マシンを立ち上げ、その上でアプリケーションを実行しています。