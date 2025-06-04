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

