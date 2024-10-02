# 沸騰水型原子炉シミュレーター　日本語プレイマニュアル（ぉ
英語で作られた沸騰水型原子炉シミュレーターが結構面白かったので、気が向いた時に日本語プレイマニュアルをチマチマ作ることとする・・・

アメリカ製ということもあってか単位がヤードポンド法なので、ちうい！（ヤードポンド法を滅ぼそう！！）

# ダウンロード先
[Getting The Program - BWR](https://acmenuclearservices.com/home/getting-the-program/)

動作環境：Windows 7～10

# 操作画面解説
## メニューバー
* Save As New IC　新しい初期条件としてセーブ
* Help　ヘルプ

## メインパネル
* INITIALIZATION CONDITION　初期条件（ロードしたセーブデータ名）

※RPV: Reactor Pressure Vessel　原子炉圧力容器

* RPV PRESS　原子炉圧力容器圧（psi）
* RPV TEMP　原子炉圧力容器温度（℃　℉！？）
* RPV LEVEL　原子炉圧力容器水位（インチ）
* SRM　中性子源領域モニタ（cps　カウント毎秒）
* PERIOD　原子炉時定数（秒）
* IRM　中間領域モニタ
* RANGE　中間領域モニタ計測レンジ
* UP　大レンジに切替
* DOWN　小レンジに切替
* APRM　平均出力領域モニタ（熱出力%）
* CORE FLOW　炉心流量（%）
* RPV LEVEL　原子炉圧力容器水位計（インチ）
* LEVEL CONTROL　水位制御
* STEAM FLOW　蒸気流量（MLB/h　ミリポンド/時）
* FEED FLOW　冷却水供給量（MLB/h　ミリポンド/時）
* LEVEL SETPOINT　水位設定（インチ）
* RAISE　水位上昇
* LOWER　水位下降
* FEED PUMPS　冷却水ポンプ稼働数
* START　稼働ポンプを1台追加
* STOP　稼働ポンプを1台停止
* EXIT SIMULATOR　シミュレーター画面を終了
* ALARM MESSAGE DISPLAY　警告表示
* ROD CONTROL　制御棒操作
* SELECTED ROD　選択中制御棒番号
* ROD GROUP　選択中制御棒グループ
* POSITION　制御棒位置（引抜量　00～48）
* IN　制御棒挿入
* OUT　制御棒引抜
* ROD SELECT　制御棒選択
* SRM DETECTOR　中性子源検出器（%引抜）
* OUT　検出器引抜
* IN　検出器挿入
* RECIRCULATION CONTROL　再循環ポンプ制御
* PUMP SPEED　ポンプスピード（%）
* RAISE　再循環ポンプ出力増加
* LOWER　再循環ポンプ出力減少
* TURBINE GENERATOR　タービン発電機
* SPEED/LOAD　タービン回転速度/電気出力（rpm/MW）
* BYPASS POS　バイパス弁開量（%）
* RUNUP RATE　発電機始動モード
* FAST　高速
* SLOW　低速
* TERM SPEED　回転数設定
* 1800 RPM
* 900 RPM
* GOVERNOR/LOAD　ガバナ、電気出力設定
* RAISE　ガバナ設定値+
* LOWER　ガバナ設定値-
* GEN BKR　発電機開閉器　※GENERATOR BRAKER
* OPEN　開列（送電網から切り離す）
* CLOSE　並列（送電網に接続）
* REACTOR MODE　原子炉モード
* RUN　運転モード
* STARTUP　始動モード
* SHUTDOWN　停止モード
* MANUAL SCRAM　手動スクラム
* MSIV　主蒸気隔離弁（Main Steam Isolation Valve）
* OPEN　弁開
* CLOSE　弁閉
* GEN SYNCH　発電機同期検定器（位相角°）
* GEN VOLTS　発電機出力電圧（kV）
* GEN FREQ　発電機周波数（Hz）

[解説ツイート](https://x.com/IchikawaYukko/status/1654880852181123072)も参照

## 制御棒選択画面
メインパネルのROD SELECTを押すと選択画面表示
* NEXT ROD WITHDRAW　次の引抜制御棒ヒントを表示（長押し）
* NEXT ROD INSERT　次の挿入制御棒ヒントを表示（長押し）
* CANCEL SELECT　選択をキャンセルして戻る
* ROD PATTERN　制御棒パターン表示

# アラーム一覧
* IRM UPSCALE ALARM　中間領域モニタ高
* CONTROL ROD WITHDRAWL BLOCK　制御棒引抜ブロック（インターロック）

