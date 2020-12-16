---
description: AmazonのElasticsearch関連
---

# Amazon ES

## 異なるクラスター間でのスナップショットリストア

### 全体的な流れ

クラスターAからクラスターBに移行する場合

1. S3バケットの用意
2. IAMの設定
3. リポジトリの登録（クラスターA）
4. リポジトリの登録（クラスターB）
5. スナップショットの作成（クラスターA）
6. インデックスの削除（クラスターB）
7. スナップショットの復元（クラスターB）

### 参考リンク

* [Amazon Elasticsearch Service インデックススナップショットの使用](https://docs.aws.amazon.com/ja_jp/elasticsearch-service/latest/developerguide/es-managedomains-snapshots.html)
* [Snapshot and restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshot-restore.html)

### 用語

#### 自動スナップショット

> クラスター復元専用のスナップショット。事前設定されたS3バケットに自動で保存され、ドメインの復元に利用できる。

#### 手動スナップショット

> クラスター復元及びクラスター間でのデータ移行に利用できるスナップショット。保存先にS3バケットを設定して手動でのスナップショット開始が必要。

#### リポジトリ/スナップショットリポジトリ

> スナップショットを保存する場所。マウントするイメージ。  
> クラスター間で移行を行う場合はS3を利用する。

#### スナップショット

> 文字通りスナップショット。  
> インデックス毎にスナップショットが作成されるので、特定のスナップショットのみを作成したり復元したりすることができる。

### 準備

#### S3

手動スナップショットの保存先となるバケットの作成する。

{% hint style="info" %}
手動スナップショットは、S3 Glacierをサポートしていないため、それ以外のライフサイクルを利用すること。
{% endhint %}

#### IAM

Amazon Elasticsearch Serviceにロールを付与

```javascript
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "",
    "Effect": "Allow",
    "Principal": {
      "Service": "es.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}
```

上記のロールに下記ポリシーを付与

```javascript
{
  "Version": "2012-10-17",
  "Statement": [{
      "Action": [
        "s3:ListBucket"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::s3-bucket-name"
      ]
    },
    {
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::s3-bucket-name/*"
      ]
    }
  ]
}
```

スナップショットリポジトリの登録を行うために、上記のロールを受け付けるパーミッションを付与

```javascript
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::123456789012:role/TheSnapshotRole"
    },
    {
      "Effect": "Allow",
      "Action": "es:ESHttpPut",
      "Resource": "arn:aws:es:region:123456789012:domain/my-domain/*"
    }
  ]
}
```

#### リポジトリの登録

アクセスが許可された認証情報を使用してリクエストする必要がある。  
リクエスト内容は下記の通り。

```text
PUT your_ess_domain/_snapshot/your_snapshot_repo_name
{
  "type": "s3",
  "settings": {
    "bucket": "your_s3_bucket",
    "region": "your_region",
    "role_arn": "arn:aws:iam::123456789012:role/TheSnapshotRole"
  }
}
```

なお、クラスター間で移行を行う場合は両方のクラスターで登録を行う必要がある。

{% hint style="info" %}
 curlはAWSリクエスト署名をサポートしていないため、その他の方法を用いる必要がある。
{% endhint %}

### Pythonサンプル

#### ライブラリインストール

必要となるライブラリ群

* boto3
* requests
* requests-aws4auth

#### requirements.txt

```text
boto3
requests
requests-aws4auth
```

#### 認証情報の取得

```python
region = 'your_region'
credentials = boto3.Session().get_credentials()
awsauth = AWS4Auth(credentials.access_key, credentials.secret_key, region, 'es', session_token=credentials.token)
```

#### リポジトリ登録

```python
def register_repository():
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name'
  payload = {
    "type": "s3",
    "settings": {
      "bucket": "your_s3_bucket",
      "region": 'your_region',
      "role_arn": "arn:aws:iam::123456789012:role/your_role"
    }
  }
  res = requests.put(url, auth=awsauth, json=payload, headers={"Content-Type": "application/json"})
  print(res.status_code)
  print(res.text)
```

この処理は`your_ess_domain`を変えてクラスター２つで行う必要がある。

#### スナップショットの作成

復元元に対してリクエスト（クラスターA）

* インデックスを指定してスナップショットを作成する場合

```python
def take_snapshot():
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name/your_snapshot_name'
  payload = {
    "indices": "your_index_1,your_index_2",
  }
  res = requests.put(url, auth=awsauth, json=payload, headers={"Content-Type": "application/json"})
  print(res.status_code)
  print(res.text)
```

* 全インデックスをスナップショット作成する場合

```python
def take_snapshot():
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name/your_snapshot_name'
  res = requests.put(url, auth=awsauth)
  print(res.status_code)
  print(res.text)
```

#### インデックスの削除

復元先に対してリクエスト（クラスターB）

```python
def delete_index():
  url = 'http://your_ess_domain/your_index_1'
  res = requests.delete(url, auth=awsauth)
  print(res.status_code)
  print(res.text)
```

{% hint style="info" %}
AmazonESは**`_close`APIをサポートしていない**ため復元対象のインデックスを一度削除するかインデックス名を変える必要がある。
{% endhint %}

#### スナップショットの復元

復元先に対してリクエスト（クラスターB）

```python
def restore_index():
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name/_restore'
  payload = {
    "indices": 'your_index_1'
  }
  res = requests.post(url, auth=awsauth, json=payload, headers={"Content-Type": "application/json"})
  print(res.status_code)
  print(res.text)
```

{% hint style="info" %}
データ容量によっては復元に必要な時間が長くなり、504 GATEWAY\_TIMEOUTが発生することがあるが、処理自体は正常に継続される。
{% endhint %}

### その他操作メモ

#### スナップショットのステータス取得

```python
def get_snapshot_status(host):
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name/_all?pretty'
  res = requests.get(url, auth=awsauth)
  data = res.json()
  print(json.dumps(data, indent=4))
  return data
```

スナップショットの保存が正常に完了した場合`data['snapshots'][0]['state']`に`SUCCESS`が入る

```python
def is_complete(data):
  return data['snapshots'][0]['state'] == 'SUCCESS'
```

#### スナップショットの削除

```python
def delete_snapshot(host):
  url = 'http://your_ess_domain/_snapshot/your_snapshot_repo_name/snapshot'
  res = requests.delete(url, auth=awsauth)
  print(res.status_code)
  print(res.text)
```

