百合子設計局  
報告書番号 004

# Cisco 867VAE るうたあで VDSL2 接続っっっっ
Cisco 867VAEるうたあで VDSL2 接続に成功したので、とりま情報をまとめておくっ！意外と日本語の情報がない・・・👩🏻‍💻💦　ADSLモデム機能も付いているので `operating mode adsl2` すればADSL接続もできそお！（今時ADSL使ってるゃっ居るのか・・・？🤔）

もともとNTT謹製の公式VDSLモデム + YAMAHA RTX1200でVDSL接続していたものの、1台にまとめたいな～～とずっと思っていたので、チャレンジしてみたっ！

## コンフィグ
いまだにVDSL環境な ﾕﾘｺ🏡ﾊｳｽ✨ でインターネット接続に成功したコンフィグ例っ！

ﾕﾘｺ🏡ﾊｳｽ✨ ←FTTB(Fiber To The Building)環境なので建物までは光ファイバーが来ているものの、各戸への配線は電話線を転用したVDSL2環境・・・（100Mbps MAX）　プロバイダはぉーゃ👨🏻‍🌾さん名義のInfoSphere♫

一旦 WAN 側コンフィグのみ掲載。LAN側は調整が済んだら更新予定～～
```
version 15.6
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Router
!
boot-start-marker
boot-end-marker
!
!
enable password 💢
!
no aaa new-model
wan mode dsl
!
!
!
!
ip dhcp pool TEST
 network 172.16.0.0 255.255.255.252
!
!
!
ip cef
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
controller VDSL 0
 operating mode vdsl2
!
!
!
!
!
!
!
!
!
!
!
!
!
interface ATM0
 no ip address
 shutdown
 no atm ilmi-keepalive
!
interface Ethernet0
 description xDSL modem L2 virtual interface
 no ip address
 no cdp enable
 pppoe-client dial-pool-number 1
 no lldp transmit
!
interface FastEthernet0
 no ip address
!
interface FastEthernet1
 no ip address
!
interface FastEthernet2
 no ip address
!
interface GigabitEthernet0
 no ip address
!
interface GigabitEthernet1
 no ip address
!
interface GigabitEthernet2
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface Vlan1
 ip address 172.16.0.1 255.255.255.252
 ipv6 address 4000::/64 eui-64
 ipv6 address autoconfig
!
interface Dialer1
 description xDSL modem L3 virtual interface
 ip address negotiated
 ip mtu 1454
 ip nat outside
 ip virtual-reassembly in
 encapsulation ppp
 ip tcp adjust-mss 1414
 dialer pool 1
 dialer-group 1
 no cdp enable
 ppp authentication pap chap ms-chap ms-chap-v2 callin
 ppp chap hostname 👩🏻‍✈️@ma00.bbisp.net
 ppp chap password 0 🛂
 ppp pap sent-username 👩🏻‍✈️@ma00.bbisp.net password 0 🛂
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
ip route 0.0.0.0 0.0.0.0 Dialer1
!
dialer-list 1 protocol ip permit
ipv6 route ::/0 Vlan1
!
!
!
line con 0
 exec-timeout 0 0
 logging synchronous
 no modem enable
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport input all
 stopbits 1
line vty 0 4
 password 💢
 login
 transport input none
!
scheduler allocate 60000 1000
!
end
```

### コンフィグ解説
* `wan mode dsl`　VDSL/ADSL (OVER POTS) 物理インターフェースをWANインターフェースとして使う（内蔵xDSLモデムを使う）

* `controller VDSL 0`

xDSLモデムのレイヤ1設定
```
controller VDSL 0
 operating mode vdsl2
 description xDSL L1 interface
```
`operating mode vdsl2`　xDSLモデムの動作モードを VDSL 2 にする

ちなみに色々選べる
```
Router(config-controller)#operating mode ?
  adsl1   ITU G.992.1
  adsl2   ITU G.992.3
  adsl2+  ITU G.992.5
  ansi    ANSI T1.413
  auto    auto detect mode
  vdsl2   ITU G.993.2
```

* `interface Ethernet0`

xDSLモデムの外向き仮想インターフェース。
```
interface Ethernet0
 description xDSL modem L2 virtual interface
 no ip address
 no cdp enable
 pppoe-client dial-pool-number 1
 no lldp transmit
```

`no ip address`　WAN IP アドレスは Dialer1 インターフェースに割り当てるので、Ethernet0には振らない。

`pppoe-client dial-pool-number 1`　`dialer pool 1`とEthernet0 インターフェースを紐付け（Dialer1と紐付け）

`no cdp enable` `no lldp transmit` WAN宛にCDPとLLDPの広告をしない

* `interface Dialer1`

PPPoE認証を行う仮想インターフェース
```
interface Dialer1
 description xDSL modem L3 virtual interface
 ip address negotiated
 ip mtu 1454
 ip nat outside
 ip virtual-reassembly in
 encapsulation ppp
 ip tcp adjust-mss 1414
 dialer pool 1
 dialer-group 1
 no cdp enable
 ppp authentication pap chap ms-chap ms-chap-v2 callin
 ppp chap hostname 👩🏻‍✈️@ma00.bbisp.net
 ppp chap password 0 🛂
 ppp pap sent-username 👩🏻‍✈️@ma00.bbisp.net password 0 🛂
!
```
`ip address negotiated`　PPPoE 認証でWAN側IPアドレスをもらう

`ip nat outside`　NATの外向きインターフェースにする

`ip virtual-reassembly in`　フラグメント化されたパケットを再組み立てする(NATの設定を入れると自動で入るので、気にせず)

`encapsulation ppp`　PPPでカプセル化（他にSLIP, Frame-relay か選べる）

`ip mtu 1454`　要調整 （フレッツ光の場合は1454）

`ip tcp adjust-mss 1414`　要調整（1454 - 40 = 1414）

`dialer pool 1`　ダイアラープール1番とする。Ethernet0インターフェースからPPPo __E__ 認証パケットを投げる。（int Ethernet0 の `dialer pool 1`と対応）

`dialer-group 1`　ダイアラーリスト1番と紐付け（認証開始トリガを指定）

`no cdp enable` WAN宛にCDPの広告をしない

`ppp authentication pap chap ms-chap ms-chap-v2 callin`　PPP認証方式を選択。とりま PAP, CHAP, MS-CHAP, MS-CHAPv2, 着信認証全て有効。（プロバイダの指定に合わせよう）

`ppp chap hostname 👩🏻‍✈️@ma00.bbisp.net`　CHAP認証のユーザー名（ホスト名）を指定

`ppp chap password 0 🛂`　CHAP, MS-CHAP, MS-CHAPv2　の認証パスワードを指定

`ppp pap sent-username 👩🏻‍✈️@ma00.bbisp.net password 0 🛂` PAP認証のユーザー名、パスワードを指定

* `ip route 0.0.0.0 0.0.0.0 Dialer 1`　デフォルトルートを `Dialer 1` に向ける

* `dialer-list 1 protocol ip permit`　ダイアラーリスト1番を定義。IPパケットが飛んで来たら Dialer1 インターフェースでPPP認証を開始する（オンデマンドダイアル）

## トラシュー
### changed state to up, changed state to down が繰り返し出る時
```
%DIALER-6-BIND: Interface Vi1 bound to profile Di1
%LINK-3-UPDOWN: Interface Virtual-Access1, changed state to up
%DIALER-6-UNBIND: Interface Vi1 unbound from profile Di1
%LINK-3-UPDOWN: Interface Virtual-Access1, changed state to down
```
認証方式の不一致、PPPoEユーザー名、パスワードの誤りのいずれか

### 各種確認コマンド
`show pppoe session`

`show pppoe session all`

`show controllers VDSL 0`

### デバッグ
`debug vdsl 0 daemon interface` で接続の進行状況が見える

* VDSL 0: vdsl line state : discovery
VDSL信号を検出中・・・
```
Feb 12 09:56:08.151: VDSL 0: vdsl line state : discovery
Feb 12 09:56:08.151: VDSL 0: SM_LINE_TRAIN boolean event
Feb 12 09:56:08.151:     vdsl_daemon_sm VDSL 0: during state training, got event 17(line_training)
Feb 12 09:56:08.151: @@@ vdsl_daemon_sm VDSL 0: training -> training
Cisco867VAE#
Feb 12 09:56:13.151: VDSL 0: vdsl line state : fullinit
Cisco867VAE#
Feb 12 09:56:25.118: VDSL 0: SM_LINE_SHOWTIME boolean event
Feb 12 09:56:25.118: VDSL 0: line state : showtime !
Feb 12 09:56:26.118:     vdsl_daemon_sm VDSL 0: during state training, got event 18(showtime)
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: training -> ready
Feb 12 09:56:26.118: VDSL 0: selected tc = 0, sysif = 1
Feb 12 09:56:26.118: %CONTROLLER-5-UPDOWN: Controller VDSL 0, changed state to up
Feb 12 09:56:26.118: VDSL 0: api (sys if get) ret = 0
Feb 12 09:56:26.118:     vdsl_daemon_sm VDSL 0: during state ready, got event 20(conn_mode_chk)
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: ready -> mode_pending
Feb 12 09:56:26.118:     vdsl_daemon_sm VDSL 0: idle during state mode_pending
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: mode_pending -> ready
Feb 12 09:56:26.118: VDSL 0: tc mode selected = 0
Feb 12 09:56:26.118:     vdsl_daemon_sm VDSL 0: during state ready, got event 22(if_state_chk)
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: ready -> ready
Feb 12 09:56:26.118
Cisco867VAE#:     vdsl_daemon_sm VDSL 0: during state ready, got event 21(if_wakeup)
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: ready -> running
Feb 12 09:56:26.118: VDSL 0: VDSL PTM mode is activated
Feb 12 09:56:26.118:     vdsl_daemon_sm VDSL 0: during state running, got event 23(linkup)
Feb 12 09:56:26.118: @@@ vdsl_daemon_sm VDSL 0: running -> running
Feb 12 09:56:26.118: VDSL 0: api (sys if get) ret = 0
Cisco867VAE#
Feb 12 09:56:31.149: %LINK-3-UPDOWN: Interface Ethernet0, changed state to up
Feb 12 09:56:32.149: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0, changed state to up
Feb 12 09:57:26.488: %DIALER-6-BIND: Interface Vi1 bound to profile Di1
Feb 12 09:57:26.492: %LINK-3-UPDOWN: Interface Virtual-Access1, changed state to up
Feb 12 09:57:26.676: %LINEPROTO-5-UPDOWN: Line protocol on Interface Virtual-Access1, changed state to up
```

### PAPかCHAPかを調べるには
`debug ppp negotiation` すればOK！ (telnet/ssh なら `terminal monitor` も忘れずに！)
```
May 28 22:10:22.333: Vi1 LCP: I CONFREQ [REQsent] id 1 len 19
May 28 22:10:22.333: Vi1 LCP:    MRU 1454 (0x010405AE)
May 28 22:10:22.333: Vi1 LCP:    AuthProto CHAP (0x0305C22305)
May 28 22:10:22.333: Vi1 LCP:    MagicNumber 0x0E11E47F (0x05060E11E47F)

May 28 22:10:22.349: Vi1 PPP: Queue CHAP code[1] id[1]
May 28 22:10:22.353: Vi1 PPP: Phase is AUTHENTICATING, by the peer
May 28 22:10:22.353: Vi1 CHAP: Redirect packet to Vi1
May 28 22:10:22.353: Vi1 CHAP: I CHALLENGE id 1 len 24 from "BAS"
May 28 22:10:22.353: Vi1 LCP: State is Open
May 28 22:10:22.353: Vi1 CHAP: Using hostname from interface CHAP
May 28 22:10:22.353: Vi1 CHAP: Using password from interface CHAP
May 28 22:10:22.353: Vi1 CHAP: O RESPONSE id 1 len 49 from "👩🏻‍✈️@ma05.bbisp.net"
May 28 22:10:22.549: Vi1 CHAP: I SUCCESS id 1 len 4
```

`AuthProto CHAP (0x0305C22305)` でCHAPを使っているとわかる

*参考: [VDSL のトラブルシューティング](https://www.cisco.com/c/ja_jp/support/docs/long-reach-ethernet-lre-digital-subscriber-line-xdsl/lre-vdsl-long-reach-ethernet-very-high-data-rate-dsl/119009-technote-vdsl-00.html)

## VDSL 接続状況を見る
たのしぃたのしぃ `show controller VDSL 0` タイム～～！（ぉ

NTT製VDSLモデムには真似できない芸当っっっ VDSL接続パラメータとか、ネゴシエーション結果、トレーニング結果が見れるっっっ！

### ﾕﾘｺ🏡ﾊｳｽ✨ の show controller VDSL 0 例
```
Cisco867VAE#sh controllers VDSL 0
Controller VDSL 0 is UP

Daemon Status:           Up

                        XTU-R (DS)              XTU-C (US)
Chip Vendor ID:         'BDCM'                   'IKNS'
Chip Vendor Specific:   0x0000                   0x0001
Chip Vendor Country:    0xB500                   0xB500
Modem Vendor ID:        'CSCO'                   '    '
Modem Vendor Specific:  0x4602                   0x0000
Modem Vendor Country:   0xB500                   0x0000
Serial Number Near:    GMK2230008V C867VAE 15.6(3)M
Serial Number Far:
Modem Version Near:    15.6(3)M
Modem Version Far:     0x0001

Modem Status:            TC Sync (Showtime!)

DSL Config Mode:         VDSL2
Trained Mode:   G.993.2 (VDSL2) Profile 17a
TC Mode:                 PTM
Selftest Result:         0x00
DELT configuration:      disabled
DELT state:              not running

Full inits:             17
Failed full inits:      0
Short inits:            0
Failed short inits:     1

Firmware        Source          File Name
--------        ------          ----------
VDSL            embedded        N/A

Modem FW  Version:      4.12L.08
Modem PHY Version:      A2pv6F039t.d24m
Trellis:                 ON                       ON
SRA:                     disabled                disabled
 SRA count:              0                       0
Bit swap:                enabled                 enabled
 Bit swap count:         108                     859
Line Attenuation:         3.3 dB                  0.0 dB
Signal Attenuation:       0.0 dB                  0.0 dB
Noise Margin:            12.6 dB                  5.5 dB
Attainable Rate:        136801 kbits/s           47608 kbits/s
Actual Power:            10.3 dBm                - 8.5 dBm
Per Band Status:        D1      D2      D3      U0      U1      U2      U3
Line Attenuation(dB):   1.3     3.4     4.7     N/A     1.6     3.2     N/A
Signal Attenuation(dB): 1.3     3.5     4.7     N/A     1.4     3.1     N/A
Noise Margin(dB):       13.0    12.5    12.5    N/A     5.4     5.5     N/A
Total FECC:             2200541                  405028726
Total ES:               509                      8801
Total SES:              174                      230
Total LOSS:             13                       0
Total UAS:              990                      1022
Total LPRS:             0                        0
Total LOFS:             90                       0
Total LOLS:             0                        0


                  DS Channel1     DS Channel0   US Channel1       US Channel0
Speed (kbps):             0           103998             0             48095
SRA Previous Speed:       0                0             0                 0
Previous Speed:           0           103998             0             48065
Reed-Solomon EC:          0            27910             0          37544718
CRC Errors:               0           147137             0             85852
Header Errors:            0             5301             0                 0
Interleave (ms):       0.00             1.00          0.00              1.00
Actual INP:            0.00             0.00          0.00              0.00

Training Log :  Stopped
Training Log Filename : flash:vdsllog.bin
```
### Trained Mode
```
Trained Mode:   G.993.2 (VDSL2) Profile 17a
```

VDSL動作モード。この例では VDSL2のプロファイル17a(17.664MHz帯域、下り150Mbps/上り50Mbps)で動作している。

プロファイルの一覧は[VDSL - Wikipedia](https://en.wikipedia.org/wiki/VDSL)を参照っ
### TC

👩🏻‍✈️ほ～ん！ TCってのはTransmission Coverage（伝送カバレッジ）で、TC: PTMってのは伝送カバレッジがPacket Transfer Mode(パケット転送モード）になっていることを示しているのか～ へ～へ～（ぉ

### Vendor ID
Chip Vendor ID:  
Modem Vendor ID  
の略号は以下を参照っ

|Code|Vendor   |
|----|---------|
|ALCB|Alcatel  |
|ANDV|Analog Devices|
|BDCM|Broadcom |
|GSPN|Globespan|
|IKNS|Ikanos   |
|IFTN|Infineon |
|META|Metanoia |
|STMI|STMicroelectronics|
|TSTS|Texas Instruments|
|CSCO|Cisco    |

ソース： https://forum.kitz.co.uk/index.php?topic=14005.0
