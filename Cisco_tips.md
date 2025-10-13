百合子設計局  
報告書番号 001 （ぉ

# Cisco Tips
## るうたあで telnet 設定を入れても telnet 接続できない時～
`show run` で line vty に transport input none が入っていないか確認して `transport input telnet` しましょう☆

## IPv6ルート設定を入れても全くルーティングできない時～
IPv6スタティックルートを設定して、OSPFルーティングも設定したのに全くルーター越えできないので困っていたら、`ipv6 unicast-routing` コマンドの入れ忘れでした・・・（ぉ


## コマンドラインで ? を文字として入力する方法
URL入力時に?を入力するとヘルプが出てしまう時～～

[Ctrl] + [V] → [?]でOK

## exec-timeout 0 0 はやめよう
ゾンビセッションでVTY lineが埋まるとtelnetできなくなるので `exec-timeout 0 0` はやめましょう

せめて `exec-timeout 35791 0` でｗ（忘れたころにタイムアウトするので、事実上 0 0 と等価）
```
Cisco867VAE#show line
   Tty Typ     Tx/Rx    A Modem  Roty AccO AccI   Uses   Noise  Overruns   Int
      0 CTY              -    -      -    -    -      0       0     0/0       -
      1 AUX      0/0     -    -      -    -    -      0       0     0/0       -
*     2 TTY   9600/9600  -    -      -    -    -   2706       0     0/0       -
*     3 VTY              -    -      -    -    -    140       0     0/0       -
*     4 VTY              -    -      -    -    - 151182       0     0/0       -
*     5 VTY              -    -      -    -    -  35334       0     0/0       -
      6 VTY              -    -      -    -    -  10941       0     0/0       -
      7 VTY              -    -      -    -    -   5124       0     0/0       -
```
