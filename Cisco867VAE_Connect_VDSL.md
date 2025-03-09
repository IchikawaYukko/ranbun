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

### デバッグ
`debug vdsl 0 daemon interface` で接続の進行状況が見える

* VDSL 0: vdsl line state : discovery
VDSL信号を検出中・・・

### 各種確認コマンド
`show pppoe session`

`show pppoe session all`

`show controllers VDSL 0`

### Cisco るうたあで telnet 設定を入れても telnet 接続できない時～
`show run` で line vty に transport input none が入っていないか確認して `transport input telnet` しましょう☆
