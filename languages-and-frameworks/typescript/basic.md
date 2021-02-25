---
description: TypeScript基礎
---

# 基礎

[Playground](https://www.typescriptlang.org/play)

## Hello World

```typescript
console.log('Hello World.')
```

## 制御文

### for文

```typescript
for (let i = 0; i < 10; i++) {
  console.log(`${i}回目`)
}
```

### while文

```typescript
let i = 0
while (i < 10) {
  console.log(`${i}回目`)
  i++
}
```

### if文

```typescript
const age = 30
if (age >= 20) {
  console.log('adult')
} else {
  console.log('child')
}
```

### switch文

```typescript
const v = 1

switch (v) {
  case 1:
    console.log("A");
    break;
  case 2:
    console.log("B");
    break;
  case 3:
    console.log("C");
    break;
  default:
    console.log("default");
    break;
}
```

## 変数/定数

```typescript
const str: string = 'Hello World'
```

型推論が効くため型アサーションは省略可能

```typescript
const str = 'Hello World'
```

## 関数

```typescript
function sum(n1: number, n2: number): number {
  return n1 + n2
}
```

```typescript
const sum = (n1: number, n2: number): number => n1 + n2
```

## 型ガード

```typescript
function func(arg: unknown): void {
  // console.log(arg.length) エラーになる
  // console.log(arg * 2)    エラーになる
  if (typeof arg == 'string') {
    console.log(arg.length) // stringとして扱われる
  } else if (typeof arg == 'number') {
    console.log(arg * 2)    // numberとして扱われる
  } else {
    console.log(typeof arg)
  }
}
```

## Type Alias

複数の箇所で利用が想定される型や型の組み合わせに別名を付ける

```typescript
type User = {
  id: number
  name: string
}
```

### 交差型（intersection types）

```typescript
type StrAndNum = { str: string } & { num: number }
function funcIntersection(arg: StrAndNum): string {
  return `${arg.str} : ${arg.num}`
}
console.log(funcIntersection({str: 'hoge', num: 2}))
// console.log(funcIntersection({str: 'hoge'))
```

この仕組みを用いて継承を実現できる

### 共用体型（union types）

```typescript
type StrOrNum = string | number
function funcUnion(arg: StrOrNum): string {
  return `${arg} ${arg} ${arg}`
}
console.log(funcUnion('fuga'))
console.log(funcUnion(2))
```

取りうる値を制御することもできる

```typescript
type Fruits = 'apple' | 'banana' | 'orange'
const func = (f: Fruits): void => console.log(f)
func('apple')
// func('pork') エラーになる
```

### Mapped Types

```typescript
type Mapped = {
}
const m: Mapped = {
  id: 1,
  age: 20
}
```

## Interface

```typescript
interface User {
  id: number
  name: string
}
```

同名のinterfaceを定義するとマージされる

```typescript
interface User {
  id: number
  name: string
}
interface User {
  age: number
}
const u: User = {
  id: 1,
  name: 'hoge',
  age: 30
}
```

## Type Alias vs. Interface

|  | Type Alias | Interface |
| :--- | :--- | :--- |
| 用途 | 型の定義 | オブジェクトの構造や振る舞いを定義 |
| 継承 | 交差型を利用すれば可能 | 可能 |
| 同名宣言 | エラー | マージ |
| 交差型 | 可能 | 不可能 |
| 共用体型 | 可能 | 不可能 |
| Mapped Types | 可能 | 不可能 |

## Class

```typescript
class CUser {
  private id: number
  private name: string
  constructor(id: number, name: string) {
    this.id = id
    this.name = name
  }

  getInfo():string {
    return `${this.id} : ${this.name}`
  }
}
```

