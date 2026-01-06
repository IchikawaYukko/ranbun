百合子設計局  
報告書番号 023

# いまさらVoIP網 Asterisk＆IP電話機編
***※この記事は[いまさらVoIP網](https://zenn.dev/kusaremkn/articles/abd760f9f2f450)のオマージュ、パロディ、二番煎じですっっっ。なので意図的に似た文章、似た章立てにしてあります（ぉ***

みなさんの自宅にはIP電話機が置いてありますか？ おひーすではよく見かけるアレを自宅で使ってみたいと思いませんかっ！？

この記事では、オープンソースの代表的なIP-PBXである Asterisk をインストールして、そこにIP電話機を接続し通話ができるようにするまでを解説していきますっっ
## ょーぃするもの
* 💴カネ（ぉ
  * クラウドサーバ代を払う支払い原資っ
* 🏡IPv6インターネットが来ている家
  * IPv4環境しかない人は回れ右するか、トンネルを掘る🚧か、IPv6オプションを契約しませう
* 📞IP電話機1台以上
  * 2台あると、よりたのしい♫

## 動作確認環境
### ConoHa VPS (仮想ホスト)
```
[root@trijn planetosm_updater]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

[root@trijn planetosm_updater]# cat /etc/centos-release
CentOS Linux release 7.9.2009 (Core)
```
古すぎるだろ、ぉぃ
### Asterisk on Alpine Linux (Container)
```
/ # cat /etc/alpine-release
3.21.3
```
# フリーのIP-PBXシステム
IP電話機同士で通話をするにはIP電話交換機、いわゆるIP-PBXが必要です。フリーのIP-PBXとして大変有名かつ事実上デファクトスタンダードとなっているものに Asterisk があります。ここでは Asterisk の派生PBXには目もくれずに Asterisk をそのまま（いわゆる生Asterisk）使っていきたいと思います。Asterisk は複雑な設定ファイルにより柔軟な制御が行えます。

Asterisk をインストールする方法は多数存在しますが、今回はConoHa VPS上のDockerにAlpine Linuxのコンテナを動かし、その中で apk add してインストールする方法を紹介します。この方法はAlpineの特徴である「小さなフットプリント」を活かすことができ、少ストレージ、少メモリなVPSで動くため、毎月のサーバ代をケチるのに真価を発揮します！（ぉ

※ConoHa最小構成の 512MB RAM, 30GB SSDで、ょゅーで動くｗ

# 土台の作成
## サーバの用意＆Dockerのインストール
とりま以下のスタートアップスクリプトをConoHa VPSのサーバ追加画面に流し込んで、どっかーが動く環境をサクっと作っちゃってくだせぇ！（投げやり）
```
TODO ここにスクリブトを書く
```

# Asteriskのインストール
手前味噌ですが、百合子さん謹製のAlpine Linuxを使ったAsteriskコンテナが作成済みなので、それを pull って動かしてくだせぇ！ pbx.yuriko.co.nz で使っているものと同一だよっっ

```
TODO ここに説明の続きを書く
```

# PBXの相互接続
## IPv6
PBX同士を相互接続するために IPv6 を用います。IP-PBXはSIPサーバを含んでいて、SIPサーバをそのままインターネットに晒すのはちょっと気が引けますが、そこは気合で何とかします（ぉ

Asteriskを収容しているホスト（要はConoHa VPS）はデフォルトで16コのグローバルIPv6アドレスを持っているので、そのままIPv6インターネットの海に漕ぎ出すことができ、IP-PBX同士の接続を行うことができますっ。またIPv4でのVoIP接続時によく問題となるNAT越えの問題も、IPv6では問題とならないので気軽に相互接続できるほか、IPv6インターネットの海に浮かぶIP-PBXに自宅のIP電話機をそのまま直結っっっという曲芸も簡単にできちゃいますっっっ（ただし、家にIPv6インターネットが来ている場合）

※IPv6ではNATが非推奨。また、アドレス空間が大きいので必要がないともいえる。なおNAT64については、ここでは扱いません。

IPv6網経由でIP-PBXを共有する場合は Asterisk を収容しているホスト（VPS）のグローバルIPv6アドレスを共有しておく必要があります。（それはそう）

### 気合
SIPサーバの5060/udpをインターネットの海に晒したままにしておくと、死ぬほどスキャン食らうし、死ぬほどピンポンダッシュされます（ぉ　SIP/TLSなどで認証アリにしていても、ピンポンダッシュの度にログが埋もれていくのは気持ちのいいものではないし、設定ミスによる悪用（踏み台としての利用）なども怖いものです。また公衆網に発信できる設定になっていると、悪用されたときに青天井の通話料金が請求されるリスクなども伴います。

で、そこを気合で何とかします。とりま手っ取り早いのは
* ポート番号の変更
* 接続元IPの制限
* 接続時の認証

です。ちなみに pbx.yuriko.co.nz では 60/udp, 61/tcp を使っています（ぇ　何故か接続元の制限は行っていませんが、今のところピンポンダッシュを全く食らっていないのと、認証もかけているので悪用された形跡は皆無となっています。まさか 60/udp でSIPサーバを動かしているバカ👩🏻‍✈️💢が居るとは思わないんだろうな・・・ ちなみに ssh サーバは 7/tcp で動いています（ぉ

（何故7番なのか・・・）

# 実運用上の留意点
## 実際のIP電話機を接続したいっっ
Asterisk によるIP電話網に実際のIP電話機（Cisco機も！）を接続できます。このためにPoEスイッチを利用しますが、ぶっちゃけなくてもイケます（ぉ ※ACアダプタでも給電できる電話機の場合

### IP電話機の接続 Grandstream GXP-1620 の場合
### IP電話機の接続 Snom D315 の場合
### IP電話機の接続 Cisco IP Phone の場合

# 編集後記
気が向いたら AWS EC2 で動かす記事も書きますね。`t4g.nano` とかの、すぽっとインスタンスで動かせば、停止リスクを抱えつつも毎月の維持費が$2～3に収まっちゃうっっっ（ぉ　※データ転送料金は別途
