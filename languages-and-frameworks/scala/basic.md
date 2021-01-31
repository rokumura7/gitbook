---
description: Scala基礎
---

# 基礎

Web上でサクッと試したい場合は[ScalaFiddle](https://scalafiddle.io/)をば。

## 基本のき

### コメント

```scala
// 一行コメント

/*
  複数行コメント
  複数行コメント
*/
```

### 文字出力

```scala
// 改行なし
print("出力したい内容")
// 改行あり
println("出力したい内容")
```

### 変数/定数

```scala
// 変数
var x: Int = 1
// 定数
val y: Int = 1
```

値の型は**推論される**ため省略可能。

```scala
// 型宣言である「: Int」の部分は値から推論されるため省略可能
val x = 1
```

### 四則演算

他のプログラミング言語と同様に`+`,`-`,`*`,`/`,`%`が利用可能。

```scala
println(1 + 1)  // 2
println(3 - 1)  // 2
println(4 * 2)  // 8
println(10 / 2) // 5
println(10 % 3) // 1
```

### ブロック

`{}`を用いて処理をまとめることができる。  
**最終行が結果として評価**される。なお、Scalaにも`return`は存在するが滅多に使わないらしい。

```scala
println({
  val x = 5
  val y = 2
  val z = 3

  val a = x + y
  a % z
}) // 1
```

## 制御文

### 条件分岐

#### if文

```scala
val age = 21

if (age < 20) {
  println("未成年")
} else {
  println("成人")
}
// 成人
```

一行の場合、`{}`は省略可能。

```scala
if (age < 20) println("未成年")
else println("成人")
```

なお、Scalaに三項演算子はないが`if-else`を用いて、それっぽく表現できる。

```scala
val x = 3
val ans = if (x % 2 == 0) "even" else "odd"
println(ans) // ans
```

#### match

他のプログラミング言語の`switch`のようなもの。同じではない。

```scala
val lang = "EN"

val x = lang match {
  case "EN" => "ABCDE"
  case "JP" => "あいうえお"
  case _    => "other"
}

println(x) // ABCDE
```

色々特有の面白さがありそうだがややこいのでここでは省略。

### 繰り返し

Seqなどのコレクションタイプのメソッドを利用することが多い印象。正直あんまり使わないのでは。

#### while文

```scala
var i = 1
while(i <= 10) {
  println(i)
  i += 1
}
// 1
// 2
// 3
// 4
// 5
```

#### for文

```scala
for(x <- 1 to 5){
  println(x)
}
// 1
// 2
// 3
// 4
// 5
```

## 関数とメソッド

関数とメソッドの違いはよくわかってない。

### 関数

パラメータを受け取り、評価する式のこと。`(パラメータ: パラメータの型) => 式`と書く。  
下記の例は受け取った値を２倍にする無名関数。

```scala
(x: Int) => x * 2
```

関数には名前を付与して宣言することも可能。  
`val 名前 = (パラメータ: パラメータの型) => 式`と書く。

```scala
val double = (x: Int) => x * 2
println(double(3)) // 6
println(double(5)) // 10
```

パラメータを複数設定することや、パラメータなしにすることも可能。

```scala
val sum = (x: Int, y: Int) => x + y
println(sum(1, 2)) // 3
println(sum(5, 6)) // 11

val pi = () => 3.14
println(pi()) // 3.14
```

### メソッド

`def`を用いて宣言をし、`def 名前(パラメータ: パラメータの型): 戻り値の型 => 処理`と書く。

```scala
def sum(x: Int, y: Int): Int = x + y
println(sum(1,2))
```

メソッドは複数のパラメータリストを受け取ることができる。

```scala
def addThenMultiply(x: Int, y: Int)(z: Int): Int = (x + y) * z
println(addThenMultiply(1, 2)(3)) // 9
```

複数行の処理がある場合、最終行の結果が戻り値となる。

```scala
def addThenMultiply(x: Int, y: Int)(z: Int): Int = {
  val sum = x + y
  sum * z
}
println(addThenMultiply(1, 2)(3)) // 9
```

## クラス

### クラス

`class`を用いてクラスを定義。`class クラス名 {}`と書く。  
`new クラス名()`でインスタンスを生成

```scala
class Person {
  def greet(): Unit = println("Hi!")
}

val p = new Person()
p.greet() // Hi!
```

クラス名の後ろにコンストラクタパラメータを続けることでメンバを定義できる。

```scala
class Person(name: String) {
  def greet(): Unit = println("Hi! I'm " + name + ".")
}

val p = new Person("rokumura")
p.greet() // Hi! I'm rokumura.
```

### ケースクラス

ケースクラスは**不変であり、値で比較される**。  
上述の通常のクラスはインスタンス同士の比較となる。

```scala
// 通常のクラス
class Person(name: String) {}
val p1 = new Person("rokumura")
val p2 = new Person("rokumura")

if (p1 == p2) println("p1とp2は同じ")
else println("p1とp2は異なる")
// 値は同じだがインスタンスが異なるため「p1とp2は異なる」と判定される

// ケースクラス
case class CasePerson(name: String) {}
val c1 = new CasePerson("rokumura")
val c2 = new CasePerson("rokumura")

if (c1 == c2) println("p1とp2は同じ")
else println("p1とp2は異なる")
// インスタンスが異なるが値が同じであるため「p1とp2は同じ」と判定される
```

なお、ケースクラスは`new`なしでインスタンス化できる。

```scala
case class CasePerson(name: String) {}

val c1 = new CasePerson("rokumura")
val c2 = CasePerson("rokumura")
// どちらでもOK
```

### オブジェクト

オブジェクトは単一のインスタンスであり、シングルトンと考えることができる。  
`object 名前 {}`と書く。

```scala
object Counter {
  private var num = 0
  def add(): Unit = num += 1
  def getNum(): Int = num
}

println(Counter.getNum()) // 0
Counter.add()
println(Counter.getNum()) // 1
Counter.add()
Counter.add()
println(Counter.getNum()) // 3
```

### トレイト

フィールドとメソッドを持つ型。`trait 名前 {}`と書く。  
振る舞いだけを宣言することも、実装を持つこともできる。  
振る舞いのみを定義した場合、継承するクラス（継承には`extends`を用いる）は振る舞いの実装を強制される。  
また、複数の`trait`を継承することも可能。\(`class クラス名 extends トレイトA with トレイトB`と書く\)

```scala
trait Engineer {
  def develop(): Unit
}

class BackendEngineer extends Engineer {
  override def develop(): Unit = println("バックエンドを開発します")
}

class FrontendEngineer extends Engineer {
  override def develop(): Unit = println("フロントエンドを開発します")
}

val bEngineer = new BackendEngineer()
bEngineer.develop() // バックエンドを開発します

val fEngineer = new FrontendEngineer()
fEngineer.develop() // フロントエンドを開発します
```

同じトレイトを継承しているものは、抽象化した状態で扱うことができる。

```scala
val engineers: Seq[Engineer] = Seq(
  new BackendEngineer(),
  new FrontendEngineer()
) // Engineerの集合として扱い、それぞれがBackendかFrontendかは意識していない。

// 集合の各要素によって結果が変わる
engineers.foreach(e => e.develop())
// バックエンドを開発します
// フロントエンドを開発します
```

---

### 参考

**ぶっちゃけ下記リンクを一読したほうが良い🍣**

* [基本 \| Scala Documentation](https://docs.scala-lang.org/ja/tour/basics.html)
* [Scala研修テキスト - dwango](https://scala-text.github.io/scala_text/)

