# curl

## オプション一覧

|オプション|意味|サンプル|メモ|
|:--:|:--:|:--|:--|
|メソッド|
|-XGET|GETメソッド|`curl URL`|省略可能|
|-XPOST|POSTメソッド|`curl -XPOST URL`||
|-XPUT|PUTメソッド|`curl -XPUT URL`||
|-XDELETE|DELETEメソッド|`curl -XDELETE URL`||
|付与系|
|-d --data|リクエストボディ|`curl -XPOST URL -d 'k1=v1&k2=v2'`|ファイルを付与する場合は`-d @ファイルパス`|
|-H --header|リクエストヘッダ|`curl URL -H 'key: value'`||
|-A|ユーザーエージェント|`curl URL -A 'useragent'`||
|デバッグ系|
|-I --head|レスポンスヘッダのみ|`curl -I URL`||
|-i --include|レスポンスヘッダ&ボディ|`curl -i URL`||
|-v --verbose|ログ出力|`curl -v URL`||
|-s --silent|通信状況やエラーを非表示|`curl -s URL`||
