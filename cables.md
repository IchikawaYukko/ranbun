# ケーブル・ハーネス情報 - Cables and Harness info

```
※RJ-11
  凸
123456

※RJ-45
   凸
12345678
```

# Network Fibre Cable 光ファイバーケーブル
Right hand traffic 右側通行（上から見て）
```
SFP         SFP
RX <------- TX
TX -------> RX
```
100BASE-FX, 1000BASE-SX, LX etc... (Both of SC, LC connector)
# Cisco
## Cisco IP Phone Console Cable
```
RJ-45        RJ-11
Cisco        IP Phone
Console      AUX
Cable        Port

1--- RTS
2--- DTR
3--- TxD ---2
5--- GND ---3
4--- GND
6--- RxD ---4
7--- DSR
8--- CTS
```
115200bps 8N1

See also: https://www.cisco.com/c/ja_jp/support/docs/collaboration-endpoints/unified-ip-phone-7900-series/212061-How-To-Make-A-Cisco-IP-Phone-Console-Cab.html

## Reverse Rollover Cable
ストレートLANケーブルは ___リバース___ ロールオーバーケーブルとして使える!!!（ぉ

Straight LAN cable can be used as ___Reverse___ roll over cable!!!

# Cisco / HP
## Cisco Console Cable to PA-RISC rp3440 Console Port Converter
| Cisco Rollover(RJ-45 Jack) | Signal | DB-25 |
|----------------------------|--------|-------|
| 8                          | RTS    |    5  |
| 7 Blue                     | DSR    |    8  |
| 6 yellow                   | TXD    |    3  |
| 5 green                    | GND    |    7  |
| 4 red                      | GND    |    7  |
| 3 black                    | RXD    |    2  |
| 2 white                    | DTR    |    20 |
| 1                          | CTS    |    4  |

# SAS
カオスなSASコネクタまとめ（速度は1レーン当たり）

Chaotic SAS Connecters Outline

## SATA PHY1レーン
SFF-8448（一般的なSATAコネクタ）

## SAS 3～12Gb/s PHY1レーン
SFF-8482（ドライブ用、電源ピン付き）

## SAS 3Gb/s PHY4レーン
SFF-8470（外部用、機器間用、InfiniBand 4Xコネクタと同じ）
SFF-8484（内部用、HBA用）
ちうこ品でも見なくなりつつある規格

## miniSAS 6Gb/s PHY4レーン
SFF-8088（外部用、機器間用）
SFF-8087（内部用、HBA用）

## miniSAS HD 12Gb/s PHY4レーン
SFF-8644（外部用、機器間用）
SFF-8643（内部用、HBA用）

## QSFP+ 10Gb/s PHY4レーン
SFF-8436（外部用、機器間用、InfiniBand QDRコネクタと同じ）
Fujitsu ETERNUS S2のエンクロージャ間コネクタがコレ。S3からは8644になった。
