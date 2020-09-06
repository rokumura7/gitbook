# About .tsconfig

[公式](https://www.typescriptlang.org/tsconfig)

## 俺雛形

```json
{
  "compilerOptions": {
    "target": "es2018",
    "module": "commonjs",
    "outDir": "./target/",
    "removeComments": true,
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "esModuleInterop": true
  }
}
```

## compilerOptions

コンパイルオプションのマッピング

---
---

### Basic Options

---

#### incremental

前回ビルドからの差分のみビルドする。

#### tsBuildInfoFile

`incremental`を`true`にした際に生成される`.tsbuildinfo`を任意のパスに設定。

#### target

出力するjsのバージョン指定。  
ES3であれば、ES3に準拠したjsが出力される。

#### downlevelIteration

`target`に`ES3`あるいは`ES5`を指定している場合に、`for-of`等の構文をサポートする設定。

#### module

出力するjsのモジュール指定。

#### lib

コンパイル時に使用する組み込みライブラリ指定。  
`target`で指定したjsに含まれていない組み込みライブラリを利用する場合は記述する。

#### allowJs

jsファイルをコンパイル対象とする設定。  
JavaScriptから徐々にTypeScriptへ移行する際などに利用できる。  
当然jsファイルはTypeScriptの恩恵を受けない。

#### checkJs

jsファイルの型チェックを行う設定。  
ただし、JSDocに依存する。

#### jsx

`.tsx`拡張子をJSXやJSにコンパイルする際の出力形式設定。  
Reactなどを書く際に利用する。

#### declaration

`export`した型定義ファイルをファイル毎に作成する。

#### declarationMap

型定義のマッピングファイル生成設定。

#### sourceMap

jsのマッピングファイル生成設定。

#### outFile

指定した場合、指定したファイル1つにビルドされる。  
`outDir`より優先される。

#### outDir

出力先フォルダ設定。  
デフォルトではtsファイル群と同じ階層に構成を保ちつつ出力される。

#### rootDir

出力する際のディレクトリ構造設定。

#### composite

// TODO: よくわからん。

#### noEmit

コンパイル結果出力の禁止設定。  
型チェックのみを目的として他のツールでビルドを行う場合などに利用。

#### importHelpers

// TODO: よくわからん

#### isolatedModules

// TODO: よくわからん

---
---

### Strict Type-Checking Options

---

#### strict

すべての厳密型チェック設定。  
この設定を`true`にした上で、個別にオフを設定することもできる。

#### noImplicitAny

暗黙的に`any`型となる値をエラーとする。

#### strictNullChecks

`null`になりうる値へのアクセスをエラーとする。

#### strictFunctionTypes

関数の引数不一致をエラーとする。

#### strictBindCallApply

// TODO: よくわからん

#### strictPropertyInitialization

インスタンスのメンバの初期化が行われていない場合エラーとする。

#### noImplicitThis

`this`が暗黙的に`any`となりうる場合エラーとする。

#### alwaysStrict

`use strict`をすべてのファイルに付与する。

---
---

### Additional Checks

---

#### noUnusedLocals

宣言後に呼び出しがない変数をエラーとする。

#### noUnusedParameters

関数内で呼び出しがない引数が存在する場合エラーとする。

#### noImplicitReturns

条件分岐ですべてのパターンを見た際に`return`がないルートが存在する場合エラーとする。

#### noFallthroughCasesInSwitch

`switch`文において、`break`が設定されず後続の`case`文に処理が流れてしまう場合にエラーとする。

---
---

### Module Resolution Options

---

#### moduleResolution

モジュールの解決方法。  
TypeScript 1.6以前の場合は`classic`、それ以外は`node`。

#### baseUrl

モジュール名を解決するためのベースディレクトリ。

#### paths

`baseUrl`からの相対パス指定マッピング。  
下記の例では、`import "jquery"`という記述が可能になる。

```json
{
  "compilerOptions": {
    "baseUrl": ".", // this must be specified if "paths" is specified.
    "paths": {
      "jquery": ["node_modules/jquery/dist/jquery"] // this mapping is relative to "baseUrl"
    }
  }
}
```

#### rootDirs

仮想的にルートディレクトリとして扱いたいディレクトリ群指定。  
コーディング時にあたかもルートディレクトリが1つにマージされたかのように扱うことができる。

#### typeRoots

// TODO: よくわからん

#### types

// TODO: よくわからん

#### allowSyntheticDefaultImports

`default export`を持たないモジュールからのデフォルトインポートを許可する。  
ランタイムに影響はなく、型チェックのみを提供する。

#### esModuleInterop

CommonJSとESModule間の相互運用を有効にする。  
この設定をオンにすると`allowSyntheticDefaultImports`も自動的にオンとなる。

#### preserveSymlinks

モジュールが参照するパスをシンボリックリンク自体の場所を基準とする。

#### allowUmdGlobalAccess

UMDモジュールを`import`なしでグローバルにアクセス可能にする。

---
---

### Source Map Options

---

#### sourceRoot

// TODO: よくわからん

#### mapRoot

// TODO: よくわからん

#### inlineSourceMap

// TODO: よくわからん

#### inlineSources

// TODO: よくわからん

---
---

### Experimental Options

---

#### experimentalDecorators

`ES7`デコレーターの実験的なサポートを可能にする。

#### emitDecoratorMetadata

デコレーターのための型メタデータの実験的なサポートを可能にする。
