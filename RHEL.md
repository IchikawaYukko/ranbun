# RHEL tips

## CentOS 7 Grub でシリアルコンソールを有効にする

`/etc/default/grub`　に以下を追加
```
GRUB_CMDLINE_LINUX_DEFAULT="console=tty0 console=ttyS0,115200n8"
GRUB_TERMINAL=serial
```
その後 `grub2-mkconfig` を叩く


## CentOS 7 に MinEd を入れる
CJKV対応IME内蔵テキストエディタの[MinEd](https://mined.github.io/)をepelでCentOS 7に入れようとすると「ねーよ💢」と怒られます・・・（ぉ

```CONSOLE
[yuriko☢cisco-ucs 16:08:43 ~]$ sudo -r sysadm_r yum install --enablerepo=epel mined
読み込んだプラグイン:fastestmirror, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile
 * epel: repo.jing.rocks
パッケージ mined は利用できません。
エラー: 何もしません
```

これは EPEL 7 のリポジトリから削除されているためなのですが、EPEL 6 の方には残っていて、なんとこれがそのままCentOS 7にインストールして使えますっっっ(glibcのバージョンとかも互換な模様)

### インストール
`yum install https://archives.fedoraproject.org/pub/archive/epel/6/x86_64/Packages/m/mined-2012.22-2.el6.x86_64.rpm` コマンドでEPEL 6 のアーカイブから引っ張って来ましょう♫

```CONSOLE
[yuriko☢cisco-ucs 16:08:45 ~]$sudo -r sysadm_r yum install https://archives.fedoraproject.org/pub/archive/epel/6/x86_64
/Packages/m/mined-2012.22-2.el6.x86_64.rpm
読み込んだプラグイン:fastestmirror, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

mined-2012.22-2.el6.x86_64.rpm                                                                   | 2.9 MB  00:00:01
/var/tmp/yum-root-5vV3Ae/mined-2012.22-2.el6.x86_64.rpm を調べています: mined-2012.22-2.el6.x86_64
/var/tmp/yum-root-5vV3Ae/mined-2012.22-2.el6.x86_64.rpm をインストール済みとして設定しています
依存性の解決をしています
--> トランザクションの確認を実行しています。
---> パッケージ mined.x86_64 0:2012.22-2.el6 を インストール
--> 依存性解決を終了しました。

依存性を解決しました

========================================================================================================================
 Package             アーキテクチャー     バージョン                    リポジトリー                               容量
========================================================================================================================
インストール中:
 mined               x86_64               2012.22-2.el6                 /mined-2012.22-2.el6.x86_64               9.2 M

トランザクションの要約
========================================================================================================================
インストール  1 パッケージ

合計容量: 9.2 M
インストール容量: 9.2 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  インストール中          : mined-2012.22-2.el6.x86_64                                                              1/1
  検証中                  : mined-2012.22-2.el6.x86_64                                                              1/1
keybase                                                                                          | 3.5 kB  00:00:00

インストール:
  mined.x86_64 0:2012.22-2.el6

完了しました!

[yuriko☢cisco-ucs 16:13:46 ~]$ whereis mined
mined: /usr/bin/mined /usr/share/mined /usr/share/man/man1/mined.1.gz

[yuriko☢cisco-ucs 16:14:14 ~]$ file /usr/bin/mined
/usr/bin/mined: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.18, BuildID[sha1]=f036c74e83a0699aff1d58161e1b3aa3b5c51409, stripped
```

🙋🏻‍♀️インストールでぎだっっっっっ!!!

### libc バージョン

`ls`とかと同じglibc使ってるので、CentOS 7でそのまま使えます☆
```
[yuriko☢cisco-ucs 16:35:23 ~]$ ldd -v /usr/bin/mined
        linux-vdso.so.1 =>  (0x00007ffc749b8000)
        libtinfo.so.5 => /lib64/libtinfo.so.5 (0x00007f58bcd2e000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f58bc960000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f58bcf58000)

        Version information:
        /usr/bin/mined:
                libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.3.4) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6


[yuriko☢cisco-ucs 16:33:26 ~]$ ldd -v /bin/ls
        linux-vdso.so.1 =>  (0x00007ffe7bbf0000)
        libselinux.so.1 => /lib64/libselinux.so.1 (0x00007fdcd8700000)
        libcap.so.2 => /lib64/libcap.so.2 (0x00007fdcd84fb000)
        libacl.so.1 => /lib64/libacl.so.1 (0x00007fdcd82f2000)
        libc.so.6 => /lib64/libc.so.6 (0x00007fdcd7f24000)
        libpcre.so.1 => /lib64/libpcre.so.1 (0x00007fdcd7cc2000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007fdcd7abe000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fdcd8927000)
        libattr.so.1 => /lib64/libattr.so.1 (0x00007fdcd78b9000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fdcd769d000)

        Version information:
        /bin/ls:
                libacl.so.1 (ACL_1.0) => /lib64/libacl.so.1
                libc.so.6 (GLIBC_2.14) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.17) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.3.4) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
                libc.so.6 (GLIBC_2.3) => /lib64/libc.so.6
```
