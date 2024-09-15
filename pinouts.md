# ピンアウト Pinouts
各種機器のピン配列情報

Pinouts informations

☜[Cables.md](Cables.md) も見よ

See also:[Cables.md](Cables.md)

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
