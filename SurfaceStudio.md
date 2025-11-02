百合子設計局  
報告書番号 003

# 初代さーひすスタジオ Tips
2022年に初代 Surface Studio を入手してしばらく使っているが、日本語の技術情報があまり落ちていないので、気が向いた時にちょこちょこ書くとする。Surface Studio 2, 2+ もほぼ同様のマシンなので、2の場合でも参考になるはず。

なぉ新品定価は60万円（初代、2）、72万円（2+）と異様に高価ですが、ちうこ品は20万円前後で出回っています。（百合子さんも状態のいい品を20万円で入手）

# スペックについて
意外とゲームができる！まぁ最新ゲームは無理なものの、2015年までに発売されたゲームは、ほぼ動くとみていい。

第六世代 intel CPU なため Windows 11 は正規の手順では動かない。

初代は環境光センサーが付いていないよおで、部屋の明るさに応じてディスプレイ輝度が変化しない。要手動調整。

大柄な筐体ながら重心低めに作ってあるようで、震度5+の地震でもまったく倒れなかった。（倒れないことを保証するものではありません）
## スペックは数種類あるようだが、手元にあるゃっはこんな感じ
|  |  | 備考 |
|---|---|---|
| OS  | プリインストールはWindows 10 __Pro__ | |
| CPU | Core i7 6820HQ | FCBGAなので交換不可 |
| HDD | SATA 2TB + NVMe キャッシュ 128GB | 2TB SSHDとして動作。交換可能。 |
| RAM | DDR4-2133 32GB | BGAなので増設不可 |
| ディスプレイ | 4500x3000 4K+ 28インチタッチパネル | Surface Pen, Surface Dial対応　ペンは横にマグネットでくっつく |
| GPU | NVIDIA GeForce GTX 980M, GDDR5 4GB | M付きのモバイル用ながら、最新ゲームでなければけっこう動く。交換不可なBGA品。 |
| WiFi | 802.11ac/bgn WiFi 5??? | |
| Bluetooth | BT 4.0 | |
| 有線LAN | GbE 1000BASE-T | |
| USB | USB 3.0 A x 4 | 背面にあるので地味に刺しにくい |
| SDカードスロット   | フルサイズSDXCスロットx1 | 同じく背面にあるので刺しにくい |
| 外部ディスプレイ   | mini Display Port x 1  | 背面にあるので(ry |
| ヘッドホンジャック | ステレオミニジャックx1 | 背面にあるので(ry |
| スピーカー | ステレオスピーカーx1 | 実はディスプレイではなく土台部分に入っている | 
| カメラ | Windows Hello 対応カメラx1 | |
| 付属周辺機器 | Surface Mouse (Model 1741), Surface Keyboard (Model 1742), Surface Pen (Model 1710) | Surface Dialは別売り |

## プレイできたゲーム
注釈のない限り Steam 版

なぉGPU排気はPC土台部分の右側から出てくるので、ゲーム中はここにモノを置かないことを推奨。（🙍🏻‍♀️←チョコレート置いてたら溶けた人ｗ）
* Grand Theft Auto V　　(2250x1500でfps20くらい出る　1920x1080なら大変快適にプレイ可)
* ｱｻｼﾝｸﾘｰﾖ ﾕﾘﾋﾟｨ（ぉ　　※ Assassin's Creed Unity　　（要1:1スケーリング化　Windowsの設定→システム→ディスプレイ→拡大縮小とレイアウト→100%）
* Fallout 4
* Metro 2033　　ロードがやや重い
* Chernobylite　ロード時にカクつくけどプレイは快適
* S.T.A.L.K.E.R.（SoC, CoP）　古いゲームということもあり、最高画質でもょゅーょゅー
* ArmA 3
* Euro Truck Simulator 2
* American Truck Simulator
* Call of Duty 4　　最高画質でプレイ可。まぁ2007年のゲームだしね。
* Metal Gear Solid V Ground Zeros
* Kerbal Space Program
* The Long Dark

🤔なんか、百合子さんの好きなゲームリストになってないか？コレ・・・（ぉ

## たぶん動かないゲーム
* Escape from Tarkov

( T T) プレイしたい～🤦🏻‍♀️

# 分解について
* すっげークセのある分解手順については [iFixit](https://jp.ifixit.com/Device/Microsoft_Surface_Studio_Gen_1) 参照
* SATA ドライブはヒートシンクを外さないと外せない💢
* CMOS バッテリーはヒートシンクを外し、更にSATA HDDを外したその裏にある💢
* 経年でホコリが溜まって GPU のサーマルスロットリングが発生しがちなので、年イチで分解掃除を推奨（ゲームのプレイにかなり支障がある程度にはスロットリングする）

# タッチパネルがイカレた時の切り離し方
経年劣化でタッチパネルや Surface Pen 用のデジタイザがイカレやすいが、そのままにしておくとマウスカーソルが飛び回る原因になるので、イカレたらソフト的に切り離した方が良い。

## 手順
1. スタートボタンを右クリックし`デバイスマネージャー`を開く
2. ヒューマンインタフェースデバイス > HID準拠タッチスクリーンと辿る
3. HID準拠タッチスクリーンを右クリックし`デバイスを無効にする(D)`をクリック

# 内蔵ディスプレイはCentOSで認識できない
どおやらCentOS Stream 10では、内蔵ディスプレイが認識できないもよお。  
たまたま外付けディスプレイを繋いでいたので気づいたが、外付けが第1ディスプレイとして認識されて、そちらにのみ表示が出る・・・  
ディストリビューションによるかもしれないけど、Linuxを入れようと思っている人は、ちうい！

[参考ツイート](https://x.com/IchikawaYukko/status/1925474905891737668)

# 内蔵NVMe SSDはCentOSで認識できない
Intel Rapid Storage Technology でSSDとHDDを分離してもLinuxで見えるのはSATA HDD側のみなので、ちうい！`dd`でSSDのクローンなどをするには、別マシンへの差し替えが必要。

# NVMe SSDにヒートシンクは装着不可
本体の隙間がわずかしかないため、市販のNVMe SSD用*ﾋｰﾖﾁﾝｸ*は装着不可。3mm厚の物ですら厚くて入らなかったし、これ以上薄いと*ﾋｰﾖﾁﾝｸ*としての役目を果たせなくなるので、装着不可と見てよいだろう。

ただし、WD BLACK SN850X (2TB) を装着して使った感じでは、いちおお*ﾋｰﾖﾁﾝｸ*なしで260MB/s(2080Gbps)出ているので、付けなくても問題ないだろう。

2TB SATA HDD + 2TB NVMe SSD の構成にして、っょっょマシンを作ると快適でいいぞっっっ！

# UEFI
UEFI(BIOS) 画面には

[音量上ボタン] を押しながら電源ONで入れる。UEFI画面が出るまで、音量上ボタンは押したままで。（さーひすシリーズ共通）

# 気が向いたら書くネタ
## RAID 0 キャッシュの切り離し方
デフォルトだとSATA HDDにキャッシュのNVMe SSDがくっついて SSHD 動作をしている、大変変わったストレージ構成になっている。

SATA HDDをSATA SSDに交換してからNVMeキャッシュを切り離すと、SATA, NVMe双方がWindowsから見えるようになるので、大容量SSDが安価になった令和時代はこのほうが便利。

SATA, NVMe にそれぞれ同容量SSDを詰んでRAID 1化しても良いし、RAIDを組まずに内蔵ストレージ2台構成としても便利っ。

なぉ、RAID 0 キャッシュを解除すれば、SATA側、NVMe側どちらからもブートできるよぉになるので、高速なNVMe側にWindowsをクローンして、システムディスクとしてしまうのもオススメ☆（百合子さんは、そーしました☆）

英語が読める人は[Surface Studio 1: How to disconnect the Rapid Hybrid Drive and install Windows 10 on full SSD](https://medium.com/@stephan.romhart/surface-studio-1-how-to-disconnect-the-rapid-hybrid-drive-and-install-windows-10-on-full-ssd-743a6375fab1)を参照

