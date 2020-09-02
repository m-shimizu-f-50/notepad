# Vagrant コマンド一覧

## リファレンス 
* コマンド一覧表示
```
$ vagrant list-commands
```
* ヘルプ
```
vagrant -h
```

## 仮想マシンの操作

* `vagrant up`・・・仮想マシンの起動。vagrantfileのあるディレクトリ内で実行
* `vagrant halt`・・・仮想マシンの終了(シャットダウン)
* `vagrant suspend`・・・仮想マシンの一時停止
* `vagrant resume`・・・仮想マシン一時停止から復帰
* `vagrant reload`・・・仮想マシンの再起動
* `vagrant provision`・・・仮想マシンは起動したままプロビジョニングのみ再度実行
* `vagrant destroy`・・・仮想マシンの削除(boxは消えない)
* `vagrant status`・・・仮想マシーンのステータス
* `vagrant global-status`・・・全仮想マシンの一覧で削除済みのものが出る場合
* `vagrant ssh`・・・バージョン確認＋最新バージョンの表示(`vagrant -v`はバージョン確認のみ)


## boxの一覧、追加、アップデート、削除

* `vagrant box list`・・・ boxの一覧を表示
*`vagrant box add **/** `・・・boxの追加。Vagrant Cloud、ローカルのbox、URLから取得から取得

* `vagrant box add`・・・ 付けたい名前 box名.box
* `vagrant box update --box **/** `・・・(vagrant box updateでまとめて)
* `vagrant init BOX_NAME 初期化`・・・(Vagrantfileの作成)
* ` vagrant box remove **/**` ・・・boxの削除
※ → ユーザーディレクトリ/.vagrant.d/boxes/box名 を手動で削除
* `vagrant box remove **/** --box-version 0.0.1` ・・・boxの削除(バージョン指定)
* `vagrant package box名`・・・ 現在の仮想マシンをboxに。box名省略なら元のbox名
→ これを元に vagrant box add box名 だけで同じ環境を構築できる
* `vagrant box repackage mackerel/centos66 virtualbox 0.0.2 `・・・??再パッケージ化

## スナップショット (仮想マシンの状態を保存/復元)

スナップショットファイルの保存場所は、
"ユーザーディレクトリ/VirtualBox VMs/kingoma.localhost/Snapshots/{XXXX}.vmdk"
スナップショット名と save で付けたファイル名は異なる

* `vagrant snapshot list`・・・ 名前付きスナップショットの一覧を表示
* `vagrant snapshot save <スナップショット名>`・・・ スナップショット作成
--provision 付けると強制的にプロビジョニング、--no-provision 付けると強制的にプロビジョニングさせない
`vagrant snapshot push `・・・名前付けずにスナップショット作成(名前は自動で付く 例：push_1591925823_8279)。使わない
* `vagrant snapshot restore <スナップショット名>`・・・スナップショット復元。--[no-]provisionは上の save のと同じ
* `vagrant snapshot pop `・・・これも復元だが、元スナップショットファイルは消える
* `vagrant snapshot delete <スナップショット名> `・・・スナップショットの削除

## プラグイン
* `vagrant plugin list `・・・プラグイン一覧
* `vagrant plugin install <プラグイン名1> <プラグイン名2> `・・・インストール
* `vagrant plugin uninstall <プラグイン名1> <プラグイン名2> `・・・アンインストール
* `vagrant plugin update [names...]`・・・ アップデート（名前省略で全てのプラグイン）
* `vagrant plugin repair`・・・ 初期化に失敗したプラグインの修復を試みる
* `vagrant plugin expunge --reinstall`・・・ アンインストールして再インストール


## Vagrant Cloud
* `vaglant login` ・・・ログイン
* `vagrant share `・・・ローカル環境を公開。表示されるURLで外部からアクセス可能に。Ctrl + C で終了
* `vagrant connect xxxxx `・・・SSHなどでの接続も可能。セキュリティ的リスクあるが便利な場面も
"hashicorp/precise32"のような名前のboxが作られる。終了しても残るので、要らなくなったらこのboxは手動で削除する


















