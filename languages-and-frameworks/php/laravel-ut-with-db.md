# LaravelでDBを使った単体テスト

参考：[Laravel5.4：データベースのテスト](https://readouble.com/laravel/5.4/ja/database-testing.html)

## データのリセット
前のテストデータが次のテスト結果へ影響を与えないように、  
各テストごとにデータベースをリセットする

`DatabaseTransactions`を使う

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use Illuminate\Foundation\Testing\DatabaseTransactions;

class ExampleTest extends TestCase
{
  use DatabaseTransactions;
  public function testCaseA()
  {
    // do something...
  }
}
```