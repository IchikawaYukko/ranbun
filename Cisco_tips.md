百合子設計局  
報告書番号 001 （ぉ

# Cisco Tips
## るうたあで telnet 設定を入れても telnet 接続できない時～
`show run` で line vty に transport input none が入っていないか確認して `transport input telnet` しましょう☆

## IPv6ルート設定を入れても全くルーティングできない時～
IPv6スタティックルートを設定して、OSPFルーティングも設定したのに全くルーター越えできないので困っていたら、`ipv6 unicast-routing` コマンドの入れ忘れでした・・・（ぉ


## コマンドラインで ? を文字として入力する方法
URL入力時に?を入力するとヘルプが出てしまう時～～

[Ctrl] + [V] → [?]でOK
