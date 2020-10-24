# curl

## オプション一覧

使えそうなものだけピックアップ

|オプション|意味|メモ|
|:--:|:--:|:--|
|メソッド系|
|-XGET|GETメソッド|省略可能<br>`curl URL`|
|-XPOST|POSTメソッド|
|-XPUT|PUTメソッド||
|-XDELETE|DELETEメソッド||
|付与系|
|-d --data|リクエストボディ|`-d 'k1=v1&k2=v2'`<br>ファイルを付与する場合は<br>`-d @ファイルパス`|
|-H --header|リクエストヘッダ|`-H 'key: value'`|
|-A|ユーザーエージェント|`-A 'useragent'`|
|デバッグ系|
|-I --head|レスポンスヘッダのみ||
|-i --include|レスポンスヘッダ&ボディ||
|-v --verbose|ログ出力||
|-s --silent|通信状況やエラーを非表示||
|-h --help|ヘルプ表示||
