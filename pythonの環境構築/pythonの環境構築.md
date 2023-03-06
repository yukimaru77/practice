# python環境構築

## 対象者

~~windowsかつVScodeでWSL2でUbuntuを扱い、そこでdockerを起動しそこでpythonをいじりたい人。~~

 __3/5追記__ すみません。WSL上でdocker動かすとバグ寄りの仕様(RAMが一瞬で食われる) のがストレス過ぎてデュアルブートにしました。デュアルブート編も下に書いてます。

## OSとソフトウェア

ソフトウェアはOSの上で動作する。そしてdockerはソフトウェアの一種である。ソフトウェアはカーネルを直接は扱えずシステムコールという形でOSに依頼して実行する。

## dockerの仕組み

Linuxには様々なディストリビューション(OS)がある。例えばUbuntu、Debian、CentOSなどである。これらの共通点はLinuxカネールを使っているという点である。LinuxカネールにはOSに必要な最低限のプログラムが書かれている。それに肉付けしたのが先ほどのディストリビューションたちだ。よって逆転の発想で例えば今現在、Ubuntuを使っているとしても __核が同じ__ なのだからLinuxカネールの力をUbuntu経由で借りれば、足りない部分を少し補強してやればDebianとして動くんじゃね？。そーゆう思想で作られたのがdockerである。なのでUbuntu目線(ホスト目線)はdockerなるソフトウェアが動いてるようにしか見えない。つまりただのプロセスとなる。しかし今dockerはDebianとして構成されている上で動いてるプログラムはDebianにしか見えない。これがコンテナの仕組み。数式で表してみよう。

$$
K:=\lbrace Linuxカーネル\rbrace,U:=\lbrace Ubuntu\rbrace,D:=\lbrace Debian\rbrace,
$$
とするとディストリビューションの性質より
$$
K\subset U,K\subset D
$$
ここでdockerが肉付けする集合$d$を
$$
d:=D-U\quad(=(D-K))
$$
とおく。ここで今、ホストOSはディストリビューションなので$K$の力を借りれる。これにdockerの肉付けを足すと
$$
K\cup d=D
$$
よってDebianの集合が構成できた。つまり __dockerはカーネルの力を借りれるという前提__ のもとで動いている。この肉付けする集合$d$がDockerイメージに他ならない。

## ----------windows for linux編------------

## ~~windows for docker~~

実はwondowsもdockerが使える。なぜだろうか。dockerはカーネルの力を借りれるという前提が崩れている。ここで役に立つのがWSLである。WSLは(ちょっと誤解を恐れず言うなら)windoswでLinuxカーネルを動かせる仕組みである。よってWSLを使ってディストリビューションを何か動かせばソフトウェアであるdockerを動かせる。これがwindows for dockerの仕組みである。例えば、WSLでUbuntuを立ち上げてそのうえでdockerを動かせば __dockerはカーネルの力を借りれるという前提__ が復活するのでDebianを動かせる。

## ~~WSL2の導入~~

ubuntuでdockerを動かすために、WSL2を入れます。やり方はググればたくさん出てくるため割愛。

導入後、一応power shellを開いて
```
wsl --update
```
でアップグレードしておこう。

## ~~Ubuntuのインストール~~

マイクロソフトストアでUbuntuと調べていれる。自分は"Ubuntu 22.04.2 LTS"を入れた。

## ~~dockerhubでアカウントを作る~~

[dockerhub](https://www.bing.com/ck/a?!&&p=64b8dd486b9b2086JmltdHM9MTY3NzU0MjQwMCZpZ3VpZD0xOTYxZDllMC1iZjAxLTZiMzYtMDhkOC1jYjg2YmVlYjZhZjQmaW5zaWQ9NTE4NQ&ptn=3&hsh=3&fclid=1961d9e0-bf01-6b36-08d8-cb86beeb6af4&psq=dockerhub&u=a1aHR0cHM6Ly9odWIuZG9ja2VyLmNvbS8&ntb=1)でsign inしてアカウントを作っておく。

## ~~systmectlを動くようにする~~

[こちら](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/)をコピペ。ググると、ひと昔前の情報(genieを使うやつ)であふれているが必ず先ほどののやつをやる事。一応英語なのでやる事だけ書いておくと"/etc/wsl.conf"を作り
```
[boot]
systemd=true
```
を記載し保存。後は再起動するだけ。

## ~~GUI化~~

したければ[こちら](https://qiita.com/sakai00kou/items/e2523b6c195693e8f184)

## ~~GPUを動かせるようにする~~

[こちら](https://zenn.dev/holliy/articles/e1ac7f2f806c35)

## ----------デュアルブート編------------

## デュアルブートとは？

デュアルブートとは1台のパソコンで複数のOSを起動時に選択して選べるような仕組みである。

OS起動の仕組みは、正直深い内容は忘れたが先頭セクタにはマスターブートレコードがあって起動パーティションを探してパーティションテーブル持ってきて$\cdots$みたいな話があるのだが長くなるしうろ覚えなので割愛する。

## メリット、デメリット

メリットはやっぱりWSLとは違って無駄がない。

デメリットは導入コストが高めなと。そもそもwindowsはデュアルブート非推奨らしく、ミスをするとWinsdowsが使えなくなってしまう可能性がある。

## デメリットを出来る限り小さくする

要はデメリットはOSが起動できなる事である。OSが起動できなくなるということはつまり、ハードディスク(SSDやHDD)のOSに関するファイルが手違いで書き換えられてしまう可能性に付随する。よってこの可能性を極力避けるために、2つSSDを用意し、1つはwindows用で一つはUbuntu用といった感じでリスクヘッジする。つまり物理的にパーティションを分ける。よって少しお金がかかる。自分は500GBのSSDを追加で買ったので5000円程かかった。

## 

## ~~Ubuntuからdockerを動かしてみよう~~

まずは(しなくてもいいが)wsl2でubuntuを起動するとCUIなのでGUIにしよう。
```
sudo /etc/init.d/xrdp start
```
その後、デフォルトで入っているアプリ"リモートデスクトップ接続"を起動。"コンピュータ"の欄にlocalhost:3390と入力して、Ubuntuにログインするユーザー名とパスワードを入力。これでGUIで操作出来る。

ターミナルを開いたら
```
sudo service docker start
```
で起動して
```
docker login -u [dockerhubに登録したユーザー名]
```
でログインする。試しにdockerイメージをdockerhubからpullしてみよう。
'''
docker pull hello-world
'''
ちなみに[ここ](https://hub.docker.com/u/library)がdockerイメージの置き場。一つのリポジトリに一つのイメージと複数のタグ(バージョンを表すもの)が存在している。

その後、
```
docker imgaes
```
でpullされているのを確認出来る。さらに
```
docker run hello-world
```
でイメージからコンテナを作成する。まだ、アクティブではないので
```
docker ps
```
としても何も表示されない。
```
docker ps -a
```