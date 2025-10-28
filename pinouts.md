百合子設計局  
報告書番号 011

# ピンアウト Pinouts
各種機器のピン配列情報

Pinouts informations

☜[cables.md](cables.md) も見よ

See also:[cables.md](cables.md)

```
※RJ-11
  凸
123456

※RJ-45
   凸
12345678
```
## Cisco
### Console / AUX Port
```
RJ-45

1-- RTS
2-- DTR
3-- TxD
4-- GND
5-- GND
6-- RxD
7-- DSR
8-- CTS
```

### Cisco 811 Power connector
```
           LATCH
-24V  -71V  RTN  +5V
-12V  +12V  RTN  ROF

RTN is GND
ROF is Remote On/ofF (PS_ON as ATX)

Plug: Molex 5557-08
Jack: Molex 5559-08
```

+5Vのみでブート自体は可能

It can be booted on only +5V

AC Adaptor: ADP-29DB (29W)

### Cisco 891F Power connector
Molex 5558 Connector
```
   KEY
-54V  +12V
GND   FG

G: GND
FG: Frame Ground（ぉ
※FG: GNDとは導通のない+12V系統の接地。その名の通り筐体金属と導通がある
```
#### ACアダプタ型番
* `PWR-MF4-125W-AC=`
* `341-0502-01`
* `PA-2121-1-LF`

### IP Phone Handset
```
RJ-9 Plug

1-- MIC 800ohm
2-- SPEAKER 40ohm
3-- SPEAKER
4-- MIC
```

### IP Phone AUX
```
RJ-11 Plug

1-- ??
2-- RxD
3-- GND
4-- TxD
5-- ??
6-- ??
```

### UC Phone 7921G Battery
```
<-LEFT       RIGHT->
1 2 3 4 5 (+)<-Screw

1-- ??
2-- R1
3-- GND
4-- ??
5-- BAT+ 3.7V

GND - R1: 10k
```

### UC Phone 7921G Charging Cradle
```
left L₁ L₂ K 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 K R₁R₂ right
K = Key

1 GND
9 USB V+
10 ???
11 USB D+
12 USB D-
13 GND
14 ???
15 +5V from AC Adaptor
```

### TelePresence EX90 Power
```
Power DIN 4pin
12V 12.5A
+   +
 - -
```
AC Adaptor: Model 12125

You can get Power DIN 4pin in Akihabara, Sengoku Densho shop.
[ロック式パワーDIN4Pプラグ](https://www.sengoku.co.jp/mod/sgk_cart/detail.php?code=6AFK-RGE2)
### TelePresence EX90 Console
```
RJ-45 TTL Level

38400bps
8N1 Flow:none
1-- Vcc 3.3V
2-- TxD (EX90 -> PC)
3--
4--
5--
6--
7-- RxD (EX90 <- PC)
8-- GND
```

### Cisco UCS M240M3 GPU_PWR Port (Motherboard)
```
+12V  +12V  +12V  GND
 GND   GND   GND  GND
          KEY

Plug: Molex 5557-08
Jack: Molex 5559-08
```

## NTT
### ISDN Digital Phone S-1000
```
RJ-45 (RJ-48??)

1-- +12V
2-- GND
3--
4--
5--
6--
7--
8--
```

## 富士通フロンテック - Fujitsu Frontech
### キャッシュドロワ KD30904-1602 Cash drawer
```
RJ-45

1-- OPEN DETECT+
2-- OPEN DETECT-
3-- NC
4-- Solenoid+ (DC 24V)
5-- Solenoid+
6-- Solenoid-
7-- Solenoid-
8-- Solenoid-
```

ドロワクローズ時1-2間短絡

Between Pin 1-2 is shorted when drawer is closed.

12Vでもソレノイド駆動可

It can be open by 12V drive.

## TERAOKA
### Cash Drawer Unkown Model Number
```
RJ-11

1-- Frame GND
2-- Solenoid+
3-- OPEN DETECT-
4-- Solenoid-
5-- Frame GND
6-- OPEN DETECT+
```

Between Pin 1-4 is shorted when drawer is closed.

Drawer can be opened manually by bottom lever.

## NEC
### Express5800 デバイス増設ユニット N8141-48 - Device extension unit N8141-48
CN1 Power connector
```
1 IN  PWR_OK
2 OUT PS_ON#
3 IN  +5V
4 IN  +12V
5     GND
6     GND

# -> Active Low
```

ATX 電源で純正電源ユニットを置き換える際の参考に・・・（純正電源壊れた人）

Please refer this when you replace original power unit by ATX power unit.

## Tokyo HY-POWER
# HT-100 シリーズ　HT-100 Series (HT-115 etc...)
ACC Connector (5pin DIN)

Left to Right PIN No.: 3 5 2 4 1
```
3 OUT  13.8V (500mA MAX)
5 IN  ALC
2     GND
4 OUT  HIGH LEVEL (+8V) when Receive
1 OUT  HIGH LEVEL (+8V) when Send

# -> Active Low
```

## KENWOOD
### 8pin Microphone (MC-80, MC-43 etc...)
```
1 MIC+
2 PTT
3 DOWN
4 UP
5 +5V (25mA MAX)
6 NC (or Detector OUT ???)
7 MIC-
8 GND
```

## HP
### PA-RISC rp3440 25pin Console Port
```
DB-25 Female Port

13 12 11 10  9  8  7  6  5  4  3  2  1
  25 24 23 22 21 20 19 18 17 16 15 14
```
| Signal | DB-25 |
|--------|-------|
| RXD    |    2  |
| TXD    |    3  |
| CTS    |    4  |
| RTS    |    5  |
| GND    |    7  |
| DSR    |    8  |
| DTR    |    20 |

### inmarsat
#### IsatPhonePro
```
Li-ion Battery Connector Left-to-Right

+3.7  ?  GND

It is okay ? pin open to boot the phone.
(or pull up?)
```

## Server PWM Fans
### DELTA
#### DELTA PFR0912XHE
12V 4.50A 8pin fan
```
  KEY
R R Y O   2.54mm pitch
? B G G

R: +12V (RED)
G: GND  (BLACK)
B: PWM CONTROL INPUT (BLUE)
Y: (YELLOW)
O: (ORANGE)
```

#### DELTA BFB1212VH
12V 1.88A
```
KEY  G B  KEY
     - +

G LED + (Green)
B LED - (Black)
+ 12V+
- GND

Plug: Molex 5557-04
Jack: Molex 5559-04

DO NOT Directory Connect 5V or 12V to LED. It may burn! So, current limiting diode is required.
LED に5V, 12Vを直接繋がないこと。要電流制限ダイオード。

5557プラグのノッチは要カット。（カットしないと入らない）
You must cut locking notch on 5557 plug to insert.
```

## PCIe CARD ##
### HP P/N 640269-001 3 Port +12V Powered USB ###
+5V from J3 Connector, +12V from PCIe slot.

F1 to F3 is fuse for +12V

C2 C9 C12 is 470uF/16V for 12V Vcc-GND

C1003 C1004 is 470uF/6.3V for 5V Vcc-GND

R282 = 1kΩ?
```
J3 PINOUT

  KEY
 5G678V
12GG34V

1 CN3 DP
2 CN3 DM
G GND
G GND
3 CN1 DP
4 CN1 DM
V Vcc +5V

5 1kΩ to GND??? via R282??? 5V OUT???
G GND
6 CN2 DP
7 CN2 DM
8 5V OUT???
V Vcc +5V
```

## PSTN
### FXS
```
   凸
  + - 　←ノーマル極性時
 L1 L2
```