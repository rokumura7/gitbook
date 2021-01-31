# `class`と`case class`、`abstract class`と`trait`、それから`object`について

## TL;DR

| 種類 | 概要 |
| ---- | ---- |
| class | いわゆる普通のクラス。 |
| case class | イミュータブルで、値で比較されるクラス。 |
| abstract class | 抽象メソッドを持てるけどインスタンス化できないクラス。 |
| trait | 抽象メソッドを持てるけどコンストラクタが持てない、インターフェース的な存在。 |
| object | それ自体が単一インスタンスのクラス。 |

コンパニオンオブジェクトとかシールドクラスとかとかとかはまた今度🍣

## `class`

`class`はオブジェクトの設計図。

### 定義方法

最小の定義は`class`キーワードと`識別子`のみ。
```scala
class User

val usr = new User
```
なお、命名は大文字はじまりのキャメルケース推奨。

### メソッド
クラスのブロック内に`def`を用いて定義。
```scala
class User {
  def greet(): Unit = println("Hello.")
}

val usr = new User
usr.greet() // Hello.
```

### コンストラクタ

さきほどの`class User`は引数なしのデフォルトコンストラクタを持つ。  
引数を持つコンストラクタはクラスの識別子に続けて書く。この時、コンストラクタに指定された変数は、デフォルトでは、**`val`で定義されたプライベートな変数**となり、クラス内でのみアクセスできる。
```scala
class User(name: String, age: Int) {
  def greet(): Unit = println(s"Hello, I'm ${name}.")
  // 変更不可能
  // def changeAge(): Unit = age = age + 1
}
val usr = new User("rokumura", 30)

usr.greet() // Hello, I'm rokumura.

// アクセスできない
// println(usr.name)
```

パブリックなアクセスを持たせたい場合は`val`または`var`を付与する。  
当然、`var`で定義した場合は変更が可能となる。

```scala
class User(name: String, var age: Int) {
  def greet(): Unit = println(s"Hello, I'm ${name}.")
  // varにしたため変更可能
  def changeAge(): Unit = age = age + 1
}
val usr = new User("rokumura", 30)

println(usr.age) // 30
// varを付与ことでパブリック且つ変更可能
usr.age = 10
println(usr.age) // 10
usr.changeAge()
println(usr.age) // 11
```

### メンバー
クラス内に記述したメンバーはデフォルトでパブリックとなる。  
プライベートにしたい場合は`private`を付与する。
```scala
class User {
  var x     = 100
  private var y = 999
}
val usr = new User
println(usr.x) // 100
// println(usr.y) // エラーとなる
```

## `case class`

上述の普通の`class`と似ているが、ちょっと違う。結構違うかもしれない。

### 定義方法

最小の定義は`case class`キーワードと`識別子`と`パラメータリスト`。パラメータリストは空でもできる。  
ケースクラスはインスタンス生成を行うapplyメソッドをデフォルトで持っているため、生成時には`new`を必要としない。
```scala
case class Book()
val b = Book()
```

パラメータを用いてケースクラスを作った場合、その値はイミュータブル（=`val`）なパブリックの値として扱われる。  
なお、`var`を用いてミュータブルにすることもできるが非推奨。

```scala
case class Book(isbn: String)
val book = Book("1234-5678")
println(book.isbn)
// イミュータブルなため変更不可
// book.isbn = "9876-5432"
```

### 比較対象

`class`の比較は参照比較であるのに対して、`case class`の比較は値比較である。

```scala
// クラスの場合、参照比較
class Dog(name: String)

val d1 = new Dog("poti")
val d2 = new Dog("poti")

// 値は同じであるが、参照が異なるため「違う犬」が出力される
if (d1 == d2) println("同じ犬")
else println("違う犬")

// ケースクラスの場合、値比較
case class cDog(name: String)

val c1 = cDog("poti")
val c2 = cDog("poti")

// 参照は異なるが、値は同じであるため「同じ犬」が出力される
if (c1 == c2) println("同じ犬")
else println("違う犬")
```

ただし、値が同じでも型が違えば違うと判定される。

```scala
case class cDog(name: String)
case class cCat(name: String)

val c1 = cDog("poti")
val c2 = cCat("poti")

// ケースクラスで値は同じだが、型が違うため「違う」が出力される
if (c1 == c2) println("同じ")
else println("違う")
```

## `abstract class`

`abstract class`は、`class`が具象であるのに対して、抽象である。  
`class`と基本同じだが、抽象メソッドを持つことができる。  
また、抽象クラス（=`abstract class`）をインスタンス化することはできない。

### 定義方法

最小は`abstract class`キーワードと識別子。  
継承する場合は`extends`を用いる。

```scala
abstract class Engineer
class FrontendEngineer extends Engineer
```

### 抽象メソッド
抽象メソッドは振る舞いのみを定義し、実装は具象に任せる。  
継承したクラスが具象クラスである場合、その振る舞いを`override`して実装をしないとエラーとなる。

```scala
abstract class Engineer {
  // 振る舞いのみを宣言し、実装はしない
  def develop(): Unit
}

class FrontendEngineer extends Engineer {
  // 具象クラスで実装をしないとエラーとなる
  override def develop(): Unit = println("フロントエンドの開発をします")
}

val engineer = new FrontendEngineer()
engineer.develop() // フロントエンドの開発をします
```

## `trait`

`trait`は、Javaのinterfaceっぽいもの。  
抽象メソッドを持つことができ、前述の`abstract class`とも似ている。  

### 定義方法

`trait`キーワードと識別子。マーカーインターフェースのように使う場合はこれ。  
継承（インターフェース的に言えば実装と言ったほうが正しいか？）には、`abstract class`と同様に、`extends`を用いる。
```scala
trait Engineer
class FrontendEngineer extends Engineer
```

### `abstract class`との違い

迷ったら`trait`、必要になったら`abstract class`。

#### 継承
`abstract class`が単一継承であるのに対して、`trait`は複数実装することができる。

```scala
// abstract classの場合
abstract class Designer
abstract class Engineer
// 複数の抽象クラスを継承することはできない
class FrontendEngineer extends Engineer with Designer
```

```scala
// traitの場合
trait Designer
trait Engineer
// 複数のトレイトを実装することができる
class FrontendEngineer extends Engineer with Designer
```

なお、Mixinを用いれば、ごにょごにょはできる。

#### コンストラクタ
`abstract class`がパラメータを保有できるのに対して、`trait`は持てない。  
```scala
// トレイトはパラメータを持てない
// trait Engineer(name: String)

abstract class Engineer(name: String)
```

## `object`

一見`class`と似ているが、`object`はそれ自体が単一のインスタンス。シングルトンと考えることができる。

### 定義方法
最小は`object`キーワードと識別子。

```scala
object Counter
```

### `class`との違い
先述の通り、**`object`はそれ自体が単一のインスタンス**である。

```scala
object Counter {
  private var num = 0
  def add(): Unit = num = num + 1
  def show(): Unit = println(num)
}

Counter.add()
Counter.show() // 1
Counter.add()
Counter.add()
Counter.show() // 3
```

```scala
class Counter {
  private var num = 0
  def add(): Unit = num = num + 1
  def show(): Unit = println(num)
}

// クラスはインスタンス化が必要
val counter1 = new Counter
counter1.add()
counter1.show() // 1
counter1.add()
counter1.add()
counter1.show() // 3

// counter1とは別インスタンスであるため、値も共有されていない
val counter2 = new Counter
counter2.add()
counter2.show() // 1
counter2.add()
counter2.add()
counter2.show() // 3
```

## 参考

**ぶっちゃけ下記リンクを一読したほうが良い🍣**

* [基本 \| Scala Documentation](https://docs.scala-lang.org/ja/tour/basics.html)
* [Scala研修テキスト - dwango](https://scala-text.github.io/scala_text/)