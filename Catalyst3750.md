百合子設計局  
報告書番号 015

# Cisco Catalyst 3750 Series
家庭用L3スイッチとして非常にオススメな Catalyst 3750, 3750G, 3750Eシリーズの技術情報

## 概要
* 低発熱量
* 低騒音（ファン搭載ながら、寝室における静かさ）
* スタッカブル（Gi2/0/1とかGi3/0/1とか生やせる！）
* GbE対応（3750G）
* 10GbE対応（3750E）（要X2モジュール）
* PoE対応（3750-48PS）
* IPv6対応
* 安価（ちうこ相場5,000～10,000円）
* 家庭用としては十分なスイッチング容量
* Cisco IOS 15.x対応（3750E）

あくまでL3スイッチなので、NATはできません。ルーターは別にょーぃしよお☆

IOS 15.x 搭載機は付属ライセンスによってはOSPF非対応・・・( T T) *IPBASE* ライセンスだと**RIP EIGRP**のみ対応・・・

## ラインナップ
|型番|SFPポート数|X2(10GbE)ポート数|100BASE-TXポート数|1000BASE-Tポート数|
|----------|---|---|--------|---|
|3750-48PS |4  |0  |48 (PoE)|0  |
|3750G-12S |12 |0  |0       |0  |
|3750G-24T |0  |0  |0       |24 |
|3750E-24TD|0 or 2 or 4|2|0 |24 |
|3560-8PC ※参考|1|0|8 (PoE)|0  |

* T: 1000BASE-**T**
* S: GbE **S**FP
* P: **P**oE

3750G-12SはSFPとっかえひっかえして自分好みのポート構成を作れるので便利✨ あまり出回ってないので見かけたら速攻入手しよお！（4台持ってる人ｗ）

3750E-24TDは0 SFPポートながら**CVR-X2-SFP TwinGig コンバータ**を1, 2本刺すことでSFPポートを2っ、4っに増やすことができる（10GbE X2と排他）

## archive コマンド でIOSのバックアップを取るときに以下のようなエラーが出る場合
```
spine-1#archive upload-sw tftp://192.168.16.6/c3750-ipservices-mz.122-25.SEE.tar
!
Warning: Unable to determine image running on system 1
Error: No systems in the current stack have been selected for upload!
```
`cd flash:/` してから `archive` コマンドを叩けばおっけい！
```
spine-1#cd flash:/
spine-1#dir
Directory of flash:/

    2  drwx         192   Jan 2 2025 17:30:41 +00:00  c3750-ipservices-mz.122-25.SEE
    4  -rwx         108   Mar 1 1993 00:04:17 +00:00  info
  407  -rwx        1904  Dec 27 2024 22:00:54 +00:00  config.text
  408  -rwx         736   Mar 1 1993 00:01:04 +00:00  vlan.dat
  410  -rwx           5  Dec 27 2024 22:00:54 +00:00  private-config.text

15998976 bytes total (6656000 bytes free)
spine-1#archive upload-sw tftp://192.168.16.6/c3750-ipservices-mz.122-25.SEE.tar
!
System software to be uploaded:
System Type:             0x00000000!
archiving c3750-ipservices-mz.122-25.SEE (directory)
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!archiving c3750-ipservices-mz.122-25.SEE/html (directory)
```

[InfraExpert Catalyst - archive command](https://www.infraexpert.com/study/ciscoios15.html) には IOSがあるディレクトリに`cd`してから`archive`を実行せよと書かれているが、ことCatalyst 3750シリーズにおいては`cd`してはいけないので、ちうい！！
