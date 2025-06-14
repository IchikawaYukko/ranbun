百合子設計局  
報告書番号 007

# ZFS Tips

## ZFS ディスクプールがデグレってたら、ピーピー鳴るよーに設定する（超うるせぇｗ）
crontab に以下のように書く
```
[root@cisco-ucs ~]#crontab -l
\# Alert when zpool is degrated
* * * * * /usr/sbin/zpool status -x|grep -q "all pools are healthy" || beep -f 2000 -l 2000
```

`beep` コマンドがない時は `yum install beep` などでインストールする（要EPEL）、`beep`コマンドを叩いてもビープ音が鳴らない時は`modprobe pcspkr`するっ

🙍🏻‍♀️ちょっとチェックサムエラーが発生した程度でピーピー鳴るし、その度に

`zpool clear tank`

叩くのめんどいので、DEGRADED を検出した時だけピーピー鳴るよーにした方がベター


## ZFS のペア系コマンド
```
zpool import/export
zpool online/offline
zpool attach/detach
zpool create/destroy
zfs send/recv
```

ｶﾀ👩🏻‍💻ｶﾀ どれとどれのサブコマンドがペアか忘れるのでメモ

## リードオンリーマウント
```
zpool import -o readonly=on <pool>
```
👩🏻‍💻RDXカートリッジなどのHDDメディアをバックアップ先とした時、そのHDDからリストアする時は読取専用マウントし、バックアップ元データに変更がかからないようにして作業すると安全


## レイテンシー・ヒストグラムを見る
ｶﾀ👩🏻‍💻ｶﾀ　`zpool iostat -w` で見れる。たのすぃ♫

## ARC キャッシュサマリーを見る
ｶﾀ👩🏻‍💻ｶﾀ　`arc_sumamry -g` で見れる。たのすぃ♫


### ログイン時に ARC キャッシュのサマリーを表示するようにすると、カッコいい！（rootでなくても可）
```
[yuriko☢cisco-ucs 16:52:05 ~]$ su -
パスワード:
最終ログイン: 2025/06/04 (水) 16:49:11 JST日時 pts/1

    ARC: 52.2 GiB (43.5 %)  MFU: 17.7 GiB  MRU: 33.5 GiB  META: 5.1 GiB DNODE 235.1 MiB (12.0 GiB)
    +----------------------------------------------------------+
    |FFFFFFFFRRRRRRRRRRRRRRRR                                  |
    +----------------------------------------------------------+

[root@cisco-ucs ~]#
```

`/root/.bash_profile` や `~/.bash_profile` に

```
arc_summary -g
```
を書けばおっけい☆

## 百合子さん謹製ZFS便利ツール
よく使うzfsコマンドワンライナーをシェルスクリプト化した [centos-admin-script/zfs/](https://github.com/IchikawaYukko/centos-admin-script/tree/master/zfs) も絶対見てくれよなっっっっ！

# SELinux RBAC 環境で zpool/zfsコマンドを使う
普通に su - しただけでは root 取っても実行できず
```
[root@cisco-ucs ~]# zpool list
Permission denied the ZFS utilities must be run as root.
```
と怒られてしまうので以下のように `sudo -r unconfined_r -t unconfined_t` 付けて実行すればおっけいっっっっっ!!! (SELinuxガチ勢→👩🏻‍💻🎶)
```
[yuriko☢cisco-ucs 22:53:29 ~]$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

[yuriko☢cisco-ucs 22:53:45 ~]$ sudo -r unconfined_r -t unconfined_t zpool list
NAME   SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
dvd   3.81T  3.04T   793G        -         -     4%    79%  1.00x    ONLINE  -
tank  3.32T  2.31T  1.02T        -         -    15%    69%  1.00x    ONLINE  -

[yuriko☢cisco-ucs 22:53:47 ~]$ sudo -r unconfined_r -t unconfined_t id
uid=0(root) gid=0(root) groups=0(root) context=staff_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```