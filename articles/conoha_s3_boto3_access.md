---
title: "ConoHa 3.0のオブジェクトストレージを boto3 で操作する"
type: "tech"
emoji: "🗑"
date: "2025-12-09"
tags: ["boto3", "ConoHa", "Python", "objectstorage", "S3"]
author: "IchikawaYukko"
published: true
---
百合子設計局  
報告書番号 020

# ConoHa 3.0のオブジェクトストレージを boto3 で操作する
ConoHa オブジェクトストレージを使い続けること10年、よーやく [S3API に正式たいおーした](https://vps.conoha.jp/conoha/news/?ap=2015054049)！ものの、boto3 でアクセスする方法を書いた記事がまだないよーなので書いてみるっっっ(中の人は初代 ConoHa の時代から使っている、オブジェクトストレージヘビーユーザーです笑)

なぉ、S3API にたいおーしてるのは、ConoHa 3.0 の方のみなので、ちういっっっ！(ConoHa 2.0でやろうとしてハマった人)

# 事前準備
`ACCESS_KEY`と`SECRET_KEY`が必要。ConoHa VPS コントロールパネルでは作れないので、以下の公式ドキュメントを読みつつ`curl`を叩いて作ろう。

1. [API Token発行](https://doc.conoha.jp/reference/api-vps3/api-identity-vps3/identity-post_tokens-v3/)
2. [Credential作成](https://doc.conoha.jp/reference/api-vps3/api-identity-vps3/identity-create_credential-v3/)

# AWS Lambda から ConoHa オブジェクトストレージにアクセスしてみる
事前準備で作成した Credential をLambdaの環境変数`ACCESS_KEY`、`SECRET_KEY`にセットすれば、以下のコードでサクっとイケる。

リージョンは conoha 扱いだけど、指定しなくても可

```
import json
import boto3
import os

def lambda_handler(event, context):
    service_name = 's3'
    endpoint_url = 'https://s3.c3j1.conoha.io'
    access_key = os.environ['ACCESS_KEY']
    secret_key = os.environ['SECRET_KEY']
    region_name = "conoha" # なくても可

    s3 = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key, aws_secret_access_key=secret_key, region_name=region_name)

    response = s3.list_buckets()
    statusCode = response['ResponseMetadata']['HTTPStatusCode']
    buckets = response['Buckets']

    return {
        'statusCode': statusCode,
        'body': str(buckets)
    }
```
実行すれば、以下のようにHTTPレスポンスコードと、コンテナ(AWSで言うバケット)の一覧が返る
```
Status: Succeeded
Test Event Name: run

Response:
{
  "statusCode": 200,
  "body": "[{'Name': 'test', 'CreationDate': datetime.datetime(2025, 12, 9, 13, 38, 37, 654000, tzinfo=tzlocal())}]"
}
```

# AWS Lambda から ConoHa オブジェクトストレージに書き込んでみる
これでOK
```
import hashlib

body = 'Hello World!'
bucket_name = 'test'
hash_sha256 = hashlib.sha256(body.encode())
s3.put_object(Bucket=bucket_name, Key='test.txt', Body=body, ChecksumSHA256=hash_sha256.hexdigest())
```
`ChecksumSHA256`がないと怒られるので、ちうい！
