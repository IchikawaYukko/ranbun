百合子設計局  
報告書番号 022

# ConoHa 3.0 のオブジェクトストレージを python-swiftclient で操作する
`swift`コマンド(python-swiftclient)で長年ConoHa 2.0のオブジェクトストレージを叩いていたので、3.0も`swift`で叩きたかったけど、叩き方がどこにも書かれていない・・・ しかもConoHa 2.0と同じ設定にしても叩けなかったけど、ムカ💢ついて調べまくったらイケたので、ここに書き残しておくこととするっっっ

[ConoHaの公式ドキュメント](https://doc.conoha.jp/reference/api-vps3/api-objectstorage-vps3/object-get_objects_list-v3/)には
> ConoHaにて提供しておりますAPIにつきましては、クラウド基盤として採用しておりますOpenStackの機能にて実装しておりますので、詳細な情報や使い方はOpenStackのドキュメントにてご確認ください。

と書いてお茶を濁されてるんだよな・・・💢 ConoHa 2.0向けには[懇切丁寧](https://support.conoha.jp/v/objectstorageswift/)に書かれてるのに・・・💦

要点：
* ConoHa 3.0からは認証がKeystone 3.0になりましたっっっ。

## やり方

### 事前準備（インストール）
`pip install python-swiftclient python-keystoneclient`

で`swift`コマンドと認証クライアントをインストールしませう。`python-swiftclient`は4.6.0以上でｵﾅｼｬｽ

### 事前準備（環境変数）
* OS_AUTH_VERSION=3    ←ここ重要
* OS_AUTH_URL=https://identity.c3j1.conoha.io/v3
* OS_USERNAME=APIユーザー名
* OS_PASSWORD=APIパスワード  ※コントロールパネル・ログインパスワードは異なる
* OS_PROJECT_NAME=テナント名

が必要です。セットできたらちゃんと

`export OS_AUTH_VERSION OS_AUTH_URL OS_USERNAME OS_PASSWORD OS_PROJECT_NAME`

しましょう

### 接続確認
`swift stat` や `swift list` してエラーにならなければOK。エラーの場合は環境変数の内容や`export`を忘れてないか確認しませう

## 番外
実は環境変数が2.0仕様の`OS_TENANT_NAME`, `OS_TENANT_ID`のままでもイケるｗ

```
/ # env | grep OS
OS_PASSWORD=**********
OS_AUTH_URL=https://identity.c3j1.conoha.io/v3
OS_TENANT_NAME=gnct********
OS_USERNAME=gncu********
OS_TENANT_ID=********************************

/ # swift list --auth-version 3
planetosm
```
