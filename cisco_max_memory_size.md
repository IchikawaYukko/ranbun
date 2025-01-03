# Cisco router's max flash memory size information

## Cisco 1600
16MB PCMCIA Linear Flash

## Cisco 2800 (2811)
2GB CF (FAT16)

If insert 4GB, negative capacity will be appear on show flash: command.
```
CallManagerExpress#sh fla
No files on device

-185270272 bytes available (0 bytes used)

2GB is OK!!
CallManagerExpress#sh fla
-#- --length-- -----date/time------ path
1     36805116 Dec 27 2024 11:24:12 +09:00 c2800nm-advipservicesk9-mz.124-10a.bin

2042068992 bytes available (36831232 bytes used)
```

## Cisco 2900 (2901)
8GB CF (FAT16)

16GB CF can be formatted by `format` command. but after the format, cannot mount.
```
Cisco2901K9>sh flash0:
-#- --length-- -----date/time------ path
1     74503236 Dec 30 2024 00:01:42 +09:00 c2900-universalk9-mz.SPA.151-4.M4.bin

7926255616 bytes available (74444800 bytes used)
```

## Cisco 3600 (3620 / 3640 / 3660)
20??MB Linear Flash

# Extra info
## Sun Fire V120
Main Memory: 4GB ECC SDRAM (1GB x 4)

## Catalyst 3750 Series
archive コマンド でIOSのバックアップを取るときに以下のようなエラーが出る場合
```
spine-1#archive upload-sw tftp://192.168.16.6/c3750-ipservices-mz.122-25.SEE.tar
!
Warning: Unable to determine image running on system 1
Error: No systems in the current stack have been selected for upload!
```
`cd flash:/` してから `archive` コマンドを叩けばおっけい！
```
spine-1#cd flash:/
spine-1#dir
Directory of flash:/

    2  drwx         192   Jan 2 2025 17:30:41 +00:00  c3750-ipservices-mz.122-25.SEE
    4  -rwx         108   Mar 1 1993 00:04:17 +00:00  info
  407  -rwx        1904  Dec 27 2024 22:00:54 +00:00  config.text
  408  -rwx         736   Mar 1 1993 00:01:04 +00:00  vlan.dat
  410  -rwx           5  Dec 27 2024 22:00:54 +00:00  private-config.text

15998976 bytes total (6656000 bytes free)
spine-1#archive upload-sw tftp://192.168.16.6/c3750-ipservices-mz.122-25.SEE.tar
!
System software to be uploaded:
System Type:             0x00000000!
archiving c3750-ipservices-mz.122-25.SEE (directory)
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!archiving c3750-ipservices-mz.122-25.SEE/html (directory)
```

[InfraExpert Catalyst - archive command](https://www.infraexpert.com/study/ciscoios15.html) には IOSがあるディレクトリに`cd`してから`archive`を実行せよと書かれているが、ことCatalyst 3750シリーズにおいては`cd`してはいけないので、ちうい！！
