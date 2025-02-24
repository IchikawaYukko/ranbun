# Nginx で暗号スイート CHACHA20-POLY1305 を第一優先にする
`CHACHA20-POLY1305`って、暗号スイートに設定できる割には実際に使ってる場面をあんま見ませんよね？ ってな訳で、SSL接続時に第一優先で使うよーに設定してしまおお！という訳～（ぉ

各設定ファイルに以下の記述を足せばOK！

Dockerコンテナ`nginx:1.21.6-alpine`で[動作確認](https://x.com/IchikawaYukko/status/1893987888343241123)済み

## nginx.conf
http セクション内に以下の設定を追加（バージョンによって異なるかも）
```
http {
  ssl_protocols TLSv1.3 TLSv1.2;
  ssl_ciphers 'TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-256-GCM-SHA384:TLS13-AES-128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256'; #:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_ecdh_curve secp521r1:secp384r1;
  ssl_prefer_server_ciphers on;
}
```
## /etc/ssl/openssl.cnf
冒頭に以下の行を追加
```
openssl_conf = default_conf

[default_conf]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
Ciphersuites = TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384
Options = ServerPreference
```

* 参考：[#jnsp - Prefer ChaCha20-Poly1305 in TLS 1.3 with nginx](https://blog.pinterjann.is/chacha20-tls13-openssl.html)
