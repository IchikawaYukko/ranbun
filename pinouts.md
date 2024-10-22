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
```

+5Vのみでブート自体は可能

It can be booted on only +5V

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

1-- GND
2-- RxD
3--
4--
5--
6--
7-- TxD
8-- Vcc 5V??
```

### Cisco UCS M240M3 GPU_PWR Port (Motherboard)
```
+12V  +12V  +12V  GND
 GND   GND   GND  GND
          KEY
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
