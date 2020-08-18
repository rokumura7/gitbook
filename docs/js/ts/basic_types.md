# Basic Types

有用なプログラムのために、数値や文字列などの最もシンプルな小さいデータの単位が必要である。  
TypeScriptでは、JavaScriptで利用できる型をサポートし、これに列挙型が追加されている。

[参考サイト](https://www.typescriptlang.org/docs/handbook/basic-types.html)

## boolean

`boolean`はtrue/falseの真偽値を表す、最も単純なデータ構造。

```typescript
let isDone: boolean = false
```

## number

JavaScriptと同様に、浮動小数点やBigIntegerを表せる。`number`は浮動小数点を、`bigint`はBigIntegerを取りうる。  
TypeScriptでは16進数と10進数のリテラルに加えて、ECMAScript2015で導入された2進数と8進数のリテラルもサポートしている。

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

## string

`string`は文字列を表し、シングルクォート`'`またはダブルクォート`"`で囲う。

```typescript
let color: string = "blue";
color = "red";
```

複数行や変数の埋め込みを行う場合はバッククォート'`'で囲う。  
埋め込みは`${ expr }`の形で行うことができる。

```typescript
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}.

I'll be ${age + 1} years old next month.`;
```

## Array

`Array`は配列を示し、2つの方法で定義が行える。

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

## Tuple

`Tuple`は要素数と型が固定された配列を表現できる。例えば、stringとnumberのペアなどを表現できる。

```typescript
let status_code: [number, string];
status_code = [200, "OK"];
status_code = [404, "Not Found"];
// status_code = ["unknown status", 999]; // Error
```

値の利用にはインデックスを指定する。

```typescript
let data: [string, number, number]
data = ['hoge', 100, 999];
console.log(data[0]); // hoge
console.log(data[1]); // 100
console.log(data[2]); // 999
```

## enum

`enum`はデータの標準セットを示すのに役立つ列挙型。  
他の言語と同様に、数値セットに名前を付ける方法。

```typescript
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
```

デフォルトでは`enum`の各メンバーは0のインデックスからはじまるが、手動で設定することも可能である。  
0の代わりに1からはじめるように設定することや、各メンバーに任意の値を設定することもできる。  

```typescript
enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;

enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;
```

名称がわからずともインデックスから取得することもできる。

```typescript
enum Color {
  Red = 1,
  Green,
  Blue,
}
let colorName: string = Color[2];

// Displays 'Green'
console.log(colorName);
```

## unknown

`unknown`は型が未知であることを示す。  
変数はどんな値も受け入れるが、型が特定できるまで操作が制限される。

型の特定には、`typeof`チェック、比較チェック、またはタイプガードを実行することで具体的な型に絞り込むことができる。

```typescript
declare const maybe: unknown;

// const aNumber: number = maybe; Error "Type 'unknown' is not assignable to type 'number'."

if (maybe === true) {
  const aBoolean: boolean = maybe;

  // const aString: string = maybe; Error "Type 'boolean' is not assignable to type 'string'."
}

if (typeof maybe === "string") {
  const aString: string = maybe;
  // const aBoolean: boolean = maybe; Error "Type 'string' is not assignable to type 'boolean'."
}
```

## any

`any`は何かであることを示す。  
`unknown`と異なり存在の有無に関係なくプロパティにアクセスができる。  
TypeScriptのその存在チェックを行わない。

```typescript
let looselyTyped: any = {};
let d = looselyTyped.a.b.c.d;
```

つまり、`any`はTypeScriptの恩恵である型システムを放棄するものであるため、  
その利用シーンは極めて限定的であり、原則利用を避けるべき型。

## void

`void`は、`any`の反対のようなもので、型が全くないことを意味する。  
戻り値のない関数の宣言に使われる。

```typescript
function warnUser(): void {
  console.log("This is my warning message");
}
```

## null and undefined

TypeScriptには、`null`と`undefined`というそれぞれ独自の型が存在するが、それ自体は有用ではない。  
これら２つの型はその他すべての型のサブクラスであり、例えば`number`の変数に`null`をアサインすることができる。  
ただし、`--strictNullChecks`を使用した場合は、`unknown`または`any`にそみ割り当てが可能となる。  
なお、`undefined`はvoidにも割り当てられる。

関数が文字列かnullを受け取りたい場合は、`string | null`というように共用体型を使用する。

## never

`never`は発生しえない値の型を表す。  
例えば、必ず例外処理となる関数や無限ループに入る関数などの戻り値の型となりうる。

`never`はすべての型のサブタイプであり、どんな型の変数にも割り当てが可能だが、  
`never`型の変数にはすべての値が割り当て不可能である。

```typescript
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

## object

`object`は非プリミティブ型であることを示すが、一般的にこれを使うことはない。

```typescript
declare function create(o: object | null): void;

create({ prop: 0 });
create(null);

// create(42); Error "Argument of type '42' is not assignable to parameter of type 'object | null'."
```

## Type assertions

TypeScriptの型チェックよりも具体的な型を想定できる場合、コンパイラに型について指示を出すことができる。これをタイプアサーションと呼ぶ。  
これは他言語のキャストに類似するが、特別なチェックやデータの再構築は行わず、純粋にコンパイラが使用する。  

この機能を利用する場合は特別な型チェックを開発者自身が行うように。

実装方法は、`as-syntax`と`angle-bracket syntax`の2種類用意されている。  
2つの実装方法に差はないため好みの問題となるが、JSXでTypeScriptを使用する場合は`as-syntax`のみ許容される。

```typescript
// as-syntax
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;

// angle-bracket syntax
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

## About Number, String, Boolean, Symbol and Object

`number`などのlowercaseの代わりに`Number`を使うこともできるが、  
これらの型は言語プリミティブを参照しないため、型として使用することはほとんどない。  
`number`などのlowercaseを使うように。
