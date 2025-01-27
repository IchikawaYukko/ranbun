# IPv6 Tips
## w コマンド / w command
```
[yuriko@cisco-ucs ~]$ w
 06:31:08 up 32 days, 14:56,  1 user,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
yuriko   pts/0    2001:470:fd50:16 06:00    4.00s  0.09s  0.06s w
```
w コマンドで ログイン元IPv6 アドレスがはみ出る時は `PROCPS_FROMLEN` 環境変数をオーバーライドすればおっけい！
```
[yuriko@cisco-ucs ~]$ PROCPS_FROMLEN=37 w
 06:32:03 up 32 days, 14:57,  1 user,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM                                  LOGIN@   IDLE   JCPU   PCPU WHAT
yuriko   pts/0    2001:470:fd50:16:85ab:824f:9e1e:ebf2  06:00    3.00s  0.06s  0.03s w
```
~/.bash_profile にこう書くのもオススメ☆
```
PROCPS_FROMLEN=37
export PROCPS_FROMLEN
```

## ::/0 or 2000::/3
```
OE2 ::/0 [110/1]
     via FE80::66A0:E7FF:FE52:91C9, Vlan111
```
宅内ネットワークでWAN宛てルートをデフォルトルートとして、OSPFv3で配布してるんだけど、なんか気が付いたら`::/0`で撒いてしまっていたので
```
# no ipv6 route default gateway tunnel 1
# ipv6 route 2000::/3 gateway tunnel 1

※YAMAHA RTX 1200
```
して`2000::/3`を撒くように変更したっ

YAMAHA ルーターで`ipv6 route default gateway tunnel 1`すると、2000::/3 ではなく ::/0 として扱うもよお・・・

WAN/LAN境界ルータでは、グローバルユニキャストアドレス以外はWAN側へ流す必要がない（流してもルーティングされない）ので、`2000::/3`で明示しておく方が良い（カッコいい!!）気がする・・・（ぉ

※`::/0`　デフォルトルートを示す

※`2000::/3`　グローバルユニキャストアドレス（インターネット上でルーティングされるアドレス）全てを示す（2または3で始まるIPv6アドレス）