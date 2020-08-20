# TypeScript for JavaScript Programmers

[参考サイト](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

TypeScriptは、JavaScriptにレイヤーを乗せた形でJavaScriptの機能を提供しつつ、型システムを実現したもの。

---

## Types by Inference

```typescript
let helloWorld = "Hello World";
//  ^ = let helloWorld: string
```

変数の宣言時に値から型が割り当てられる。[^1]

---

## Defining Types

```typescript
const user = {
  name: "Hayes",
  id: 0,
};
```

上記は`name`が`string`、`id`が`number`を推論されるが、このオブジェクトの構成を`interface`を用いて明示的にすることができる。

```typescript
interface User {
  name: string;
  id: number;
}
const user: User = {
  name: "Hayes",
  id: 0,
};
```

ここで指定した`interface`と合致しない場合、警告が表示される。
下記の例だと、`username`というプロパティが存在しない旨のエラーが表示される。

```typescript
const user: User = {
  username: "Hayes", // nameで定義されているためエラーになる
  id: 0,
};
```

`interface`宣言は`class`にも利用ができる。

```typescript
class UserAccount {
  name: string;
  id: number;

  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}

const user: User = new UserAccount("Murphy", 1);
```

`interface`は引数や戻り値の型にも指定できる。  
`interface`と`type`の2つの構文があるが、`interface`を優先させて特定の機能が必要な時に`type`を利用すること。

### primitive types in TypeScript

TypeScriptはJavaScriptが用意しているプリミティブ型に加えていくつかの型を提供している。

- `any`

  `any`は「何でもあり」を意味するため`string`や`number`などどんな型とも相互変換可能な型を意味する。  
  ただし、これは型の安全性を放棄しかねない。

- `unknown`

  `unknown`も`any`と同様に任意の値を受け入れるが、利用時に型を特定してからではないと扱えない。型安全な`any`。  

- `void`

  `void`は主に関数の戻り値の型指定で利用。その関数が「何も返さない」ことを意味する。  
  その実、何も返さない関数を変数で受け取ると`undefined`となるため`return undefined`と書けるが意味はない。  
  返ってきた値を利用することができないという意図となる。

- `never`

  `never`は「ありえない」を意味する型で、すべての型の一番下に存在する。(一番上は`unknown`)  
  `unknown`の逆で、`never`型にはどんな値も代入することが**できない**が、どんな型にも`never`は代入できる。  
  `switch文`などですべてのパターンを網羅した後の`default文`であったり、  
  例外処理時の戻り値の型指定などで利用することができる。

#### `any`と`unknown`の違い

```typescript
const anyVal: any = 'hoge'
console.log(anyVal.length)
console.log(anyVal * 2)
console.log(anyVal + 'hoge')

const unknownVal: unknown = 'fuga'
// console.log(unknownVal.length) // Error "Object is of type 'unknown'."
if (typeof unknownVal === 'string') {
  console.log(unknownVal.length)
  // console.log(unknownVal * 2) Error "The left-hand side of an arithmetic operation must be of type 'any', 'number', 'bigint' or an enum type."
  console.log(unknownVal + 'fuga')
}
if (typeof unknownVal === 'number') {
  console.log(unknownVal * 2)
}
```

---

## Composing Types

小さい型から新しい型を作成する方法は`Unions`と`Generics`の2つがある。

### Unions

`unions`は、型が多くの型の1つである可能性があることを宣言する方法。  
最も一般的な使用例は、許容される値を列挙すること。

```typescript
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
```

また、複数の型を処理する方法も提供できる。

```typescript
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
  } else {
    return obj;
  }
}
```

### Generics

`generics`は抽象的な型引数を利用して、利用時に型の指定を行うことができる仕組み。  
`Java`などの言語で提供されているものと同じ。主に配列の要素の型指定などで使われる。  
なお、`T`を指定せずに代入を行った場合、代入した値で型推論がされる。

---

## Structural Type System

TypeScriptのコアとなる原則の1つは、型チェックを値の形状に焦点を当てることで、これを`duck typing`や`structual typing`と呼ぶ。

`structual typing`では、2つのオブジェクトの形状が同じ場合、それらを同じとみなす。  
下記の例では、変数`point`は`Point`型であることを宣言していないが、形状が同じであるため`Point`型とみなされている。

```typescript
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

const point = { x: 12, y: 26 };
printPoint(point);
```

この仕組は、オブジェクトのフィールドの部分的一致を見るため、定義に存在しないプロパティをもっていたり型が違っていても、構造さえ同じであれば同じように扱う。

```typescript
const point3 = { x: 12, y: 26, z: 89 };
printPoint(point3);

const rect = { x: 33, y: 3, width: 30, height: 80 };
printPoint(rect);
```

```typescript
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
printPoint(newVPoint);
```

[^1]:型推論
