# 2進接頭辞 or 10進接頭辞？？？
各ソフトウェア・ハードウェアでどちらを採用しているかの分類表

Table showing whether each software and hardware uses binary or decimal prefix.
# 2進接頭辞 Binary Prefix
## pv コマンド
pv -s 447G  <-こー見えてGiB

dd | pv -s 931G | pv

などとしてddでHDDをまるっとコピーしつつ pv で進捗を見るときは、GiB単位で指定しましょう☆

## du -h コマンド

## cgdisk コマンド

## Windows Explorer
1TB HDD を刺す -> 931GB!? として見える -> 実は931GiB!!

## ConoHa ObjStorage
Object Storage 100GB 借りる -> 実は 100GiB -> つまり10進だと 107GB!! 使える。やったね！

# 10進接頭辞 Decimal Prefix
## cfdisk コマンド

## LTO Ultrium Tape
LTO1 100GB にデータを入れると？ -> 実は93.1GiB (107GB / 100GiB ではない)ので、100GiBは入らないっ！

## Hard disks / SSDs
1TB 1000GBとして売っているHDDは実は？ -> 931GiBなので？ -> 1000GiB (1073GB) 入れようとすると溢れるっ。たいへんっっっ！（ぉ

# 2 / 10 進切替可能
## parted
デフォルトでは10進。unit GiB コマンドで2進に切替可能
