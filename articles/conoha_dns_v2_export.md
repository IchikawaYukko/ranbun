---
title: "ConoHa DNS (v2.0)のレコードエクスポート方法"
type: "tech"
emoji: "🌏"
date: "2025-12-22"
tags: ["jq", "ConoHa", "DNS", "migration", "restapi"]
author: "IchikawaYukko"
published: true
---
百合子設計局  
報告書番号 021

# TL;DR
* ConoHa DNS 2.0はゾーンにあるレコードをまるっとエクスポートするAPIがあるけど、3.0 にはのるっとインポートするAPIがない
* 3.0にインポートするには1件ずつ手かAPIで登録する必要がある💢

# ConoHa DNS (v2.0)のレコードエクスポート方法・DNS 3.0 への移行方法
ConoHa DNS 2.0から3.0にDNSを移行しようと思って作業してみたものの、コントロールパネルからだとレコードのコピペが出来ない・・・ ぃゃ、1項目ずつならコピーできるけど、全部まとめてコピーして一旦エディターに貼っておくということが、ページの仕様上できない・・・ レコード数40近くあってちょっと困ったので、APIを叩いて全レコード取得する方法に転換っっ

## ょーぃするもの
* `curl`
* `jq`

以下の説明では curl + jq の組み合わせでAPIを叩いていくので、これらが使える環境をょーぃしませう☆

## ConoHa DNS v2.0 API
[DNS API ゾーンファイルエクスポート](https://doc.conoha.jp/reference/api-vps2/api-dns-vps2/paas-dns-export-zone-v2/?btn_id=reference-account-billing-invoices-list-v2--sidebar_reference-paas-dns-export-zone-v2)という、そのものズバリなAPIがあったので、これを叩いて中身をまるっと取ってみる。

### 手順
1. アクセストークン取得
2. ドメインID取得
3. ゾーンファイル取得

### アクセストークン取得
API ユーザー名、テナントIDはコントロールパネルのAPI画面を参照
> curl -X POST -H "Accept: application/json" -d '{"auth":{"passwordCredentials":{"username":"APIユーザー名","password":"コントロールパネル・パスワード"},"tenantId":"テナントID"}}' https://identity.tyo1.conoha.io/v2.0/tokens|jq .access.token.id

これを叩くと以下のようにアクセストークンが取れる

> "2cc57c69b5914d8fb3bb3056ceb66465"

### ドメインID取得
> curl -X GET -H "Accept: application/json" -H "Content-Type: application/json" -H "X-Auth-Token: 取得したアクセストークン" https://dns-service.tyo1.conoha.io/v1/domains|jq

以下のようにドメイン(ゾーン)一覧が取れる。VPSの逆引きレコードもゾーン扱いの模様。

name: がゾーン名、id: がドメインID(GUID)

```
{
  "domains": [
    {
      "created_at": "2018-10-14T22:26:25.000000",
      "description": null,
      "email": "postmaster@example.org",
      "gslb": 0,
      "id": "82b4a6f0-55e5-47f2-9aab-3e9f15a86255",
      "name": "yuriko.co.nz.",
      "serial": 1765936946,
      "ttl": 3600,
      "updated_at": "2025-12-17T02:02:26.000000"
    },
    {
      "created_at": "2020-04-21T10:38:43.000000",
      "description": null,
      "email": "hostmaster@conoha.io",
      "gslb": 0,
      "id": "3e1aad3e-16ae-4346-afd9-465ba279b18f",
      "name": "120.138.95.150.in-addr.arpa.",
      "serial": 1587465524,
      "ttl": 3600,
      "updated_at": "2020-04-21T10:38:44.000000"
    }
  ]
}
```

### ゾーンファイル取得
> curl -X GET -H "Accept: text/dns" -H "X-Auth-Token: 取得したアクセストークン" https://dns-service.tyo1.conoha.io/v2/zones/ほしいゾーンのドメインID

ゾーンにあるレコードが、ゾーンファイルとして、まるっと取れる。ゎーぃ！やったねっ☆ コピペするか、リダイレクトでファイルに保存しよお！

```
$ORIGIN yuriko.co.nz.
$TTL 3600

yuriko.co.nz.  IN NS  ns-a3.conoha.io.
yuriko.co.nz.  IN NS  ns-a2.conoha.io.
yuriko.co.nz.  IN NS  ns-a1.conoha.io.
yuriko.co.nz.  IN SOA  ns-a1.conoha.io. postmaster.example.org. 1765936946 3600 600 86400 3600
yuriko.co.nz. 86400 IN A  150.95.138.120
yuriko.co.nz. 86400 IN MX 10 trijn.tyo.yuriko.co.nz.
yuriko.co.nz. 86400 IN AAAA  2400:8500:1302:821:150:95:138:120
yuriko.co.nz. 86400 IN TXT  v=spf1 a mx a:trijn.tyo.yuriko.co.nz -all
yuriko.co.nz. 86400 IN TXT  keybase-site-verification=AMI3TjY1rDx-EJvxqwpbzUXFapP6-1OOvwBAQowp79o
www.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
_dmarc.yuriko.co.nz. 86400 IN TXT  v=DMARC1; p=quarantine; rua=mailto:dmarc@yuriko.co.nz; ruf=mailto:dmarc@yuriko.co.nz; adkim=s; aspf=s
mail.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
ipv6.yuriko.co.nz. 86400 IN AAAA  2400:8500:1302:821:150:95:138:120
search.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
passportcontrol.net._report._dmarc.yuriko.co.nz. 86400 IN TXT  v=DMARC1
oldconoha.yuriko.co.nz. 86400 IN CNAME  aagje.tyo.yuriko.co.nz.
ipsec.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
openvpn.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
bfv.yuriko.co.nz. 86400 IN CNAME  trijn.tyo.yuriko.co.nz.
dkim._domainkey.yuriko.co.nz. 600 IN TXT  v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC7HOkHR5SFmi2omSnW/XhsBK1TEkuAIOF5QYOPzDYd2Oq4O8bsg9SzPubdsFGSAG60kT/2FK88VPOCRn6bbvfAMperhba2yfFHQE6Qc+o8Em+9ZS8fvoRgzaAXRemJwTTIZ/wL2PmwT5PqUOx/SxeFkQSmlMXxA9SLb/D+gH9y6QIDAQAB
yps.yuriko.co.nz. 86400 IN A  52.193.85.235
akibahome.yuriko.co.nz. 86400 IN AAAA  2001:470:24:327::1
xn--44q544fq7sknc.yuriko.co.nz. 86400 IN AAAA  2001:470:24:327::114:514
akiba.yuriko.co.nz. 86400 IN NS  a.conoha-dns.com.
akiba.yuriko.co.nz. 86400 IN NS  b.conoha-dns.org.
_discord.yuriko.co.nz. 86400 IN TXT  dh=ba0f85d66c062734535ba6bcc979b68d50273ddb
pbx.yuriko.co.nz.  IN CNAME  nijntje.tyo.yuriko.co.nz.
xn--3t8h.yuriko.co.nz.  IN CNAME  pbx.yuriko.co.nz.
voip.yuriko.co.nz. 86400 IN NS  a.conoha-dns.com.
voip.yuriko.co.nz. 86400 IN NS  b.conoha-dns.org.
tyo.yuriko.co.nz. 86400 IN NS  a.conoha-dns.com.
tyo.yuriko.co.nz. 86400 IN NS  b.conoha-dns.org.
status.yuriko.co.nz. 86400 IN NS  ns-319.awsdns-39.com.
status.yuriko.co.nz. 86400 IN NS  ns-1923.awsdns-48.co.uk.
status.yuriko.co.nz. 86400 IN NS  ns-1256.awsdns-29.org.
status.yuriko.co.nz. 86400 IN NS  ns-901.awsdns-48.net.
```

## おまけ1: ConoHa DNS v3.0 API
なお[ConoHa v3.0 API](https://doc.conoha.jp/reference/api-vps3/?btn_id=reference--terms_reference-api-vps3)には、v2.0にはあった[DNS API ゾーンファイルインポート](https://doc.conoha.jp/reference/api-vps2/api-dns-vps2/paas-dns-import-zone-v2/?btn_id=reference-api-vps2--sidebar_reference-paas-dns-import-zone-v2)がないっっっっ。なのでエクスポートで取得したゾーンファイルを3.0にまるっとインポートすることはできない・・・💢

コントロールパネルから頑張って1つずつ登録するか、v3.0のAPIを叩くスクリプトを書こお!!（ぉ

## おまけ2: ConoHa DNS の残念な仕様
ConoHa DNS 2.0と3.0は何かを共有しているようで、同名のゾーンを両方に持つことができない。

DNS 2.0にあるゾーンをDNS 3.0に移行するには、まず2.0のゾーンを削除してからでないと、3.0にゾーンを作成できないっっっ💢
