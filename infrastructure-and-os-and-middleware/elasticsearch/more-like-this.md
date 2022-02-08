# More Like This

### 基本形

`GET` : `localhost:9xxx/{index_name}/_search`

```json
{
    "query": {
        "more_like_this": {
            "fields": [
                "field_name"
            ],
            "like": [
                {
                    "doc": {
                        "field_name": "{検索文字列}"
                    }
                }
            ]
        }
    }
}
```

`fields`は配列なので複数指定可能。

その場合、検索文字列は下記のようにする。

```json
            "fields": [
                "field1",
                "field2"
            ],
            "like": [
                {
                    "doc": {
                        "field1": "hogehoge",
                        "field2": "fugafuga"
                    }
                }
            ],
```

### ＋絞り込み

`term`や`match`で絞り込むことができる

```json
{
    "query": {
        "bool": {
            "must": [
                {
                    "more_like_this": {
                        "fields": [
                            "field_name"
                        ],
                        "like": [
                            {
                                "doc": {
                                    "field_name": "{検索文字列}"
                                }
                            }
                        ]
                    }
                },
                {
                    "term": {
                        "絞り込みに使いたいフィールド.keyword": "hogehoge"
                    }
                }
            ]
        }
    }
}
```
