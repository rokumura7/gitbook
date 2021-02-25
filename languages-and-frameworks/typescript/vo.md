# TypeScriptでValueObject

## TypeScriptでValueObjectの表現を考えてみた

### TL;DR

```typescript
abstract class ValueObject<T> {
  protected constructor(protected readonly val: T) {}

  get(): T {
    return this.val;
  }

  eq(vo: ValueObject<T>): boolean {
    if (this.constructor.name !== vo.constructor.name) return false;
    return this.get() === vo.get()
  }
}

abstract class StringValueObject extends ValueObject<string> {}

class UserName extends StringValueObject {
  static of(_val: string): UserName {
    return new this(_val);
  }
}

class AdminName extends StringValueObject {
  static of(_val: string): UserName {
    return new this(_val);
  }
}

const userName1 = UserName.of('rokumura')
const userName2 = UserName.of('rokumura')
const adminName = AdminName.of('rokumura')

console.log(userName1 == userName2)  // false
console.log(userName1.eq(userName2)) // true
console.log(userName1.eq(adminName)) // false
```

### 不変性

`readonly`にすることで、値の変更を防ぐ。

```typescript
protected constructor(protected readonly val: T) {}
```

`Object.freeze`を使っているサンプルも見かけたが、`readonly`だけでも十分なのでは。

### 等価判定

上記の`eq`では、コントラクタの名前を利用して、クラスが同じであることも条件に加えている。 単純に値が等価であるかだけを比較したければ、下記で十分。

```typescript
eq(vo: ValueObject<T>): boolean {
  return this.get() === vo.get()
}
```

### プリミティブの指定

他のプリミティブを指定することももちろん可能。

```typescript
abstract class NumberValueObject extends ValueObject<number> {}
class Price extends NumberValueObject {
  static of(_val: number): Price {
    return new this(_val)
  }
}
```

### バリデーション

バリデーションは、コンストラクタに仕込んでもいいし、抽象クラスに用意してもいいだろうし、`of`の中に設けてもいいだろうし。

#### コンストラクタに仕込んだ場合

```typescript
abstract class NumberValueObject extends ValueObject<number> {
  constructor(val: number) {
    if (val < 0) throw new Error('The value must be more than 0.')
    super(val)
  }
}

class Price extends NumberValueObject {
  static of(_val: number): Price {
    return new this(_val)
  }
}

const price1 = Price.of(100)
const price2 = Price.of(-1) // Error: The value must be more than 0.
```

#### 抽象クラスに用意した場合

```typescript
abstract class NumberValueObject extends ValueObject<number> {
  protected static validate(_val: number): void {
    if (_val < 0) throw new Error('The value must be more than 0.')
  }
}

class Price extends NumberValueObject {
  static of(_val: number): Price {
    super.validate(_val)
    return new this(_val)
  }
}

const price1 = Price.of(100)
const price2 = Price.of(-1) // Error: The value must be more than 0.
```

#### ofの中に設けた場合

```typescript
abstract class NumberValueObject extends ValueObject<number> {}

class Price extends NumberValueObject {
  static of(_val: number): Price {
    if (_val < 0) throw new Error('The value must be more than 0.')
    return new this(_val)
  }
}

const price1 = Price.of(100)
const price2 = Price.of(-1) // Error: The value must be more than 0.
```

## 最後に

お寿司食べたい🍣

## 参考

* [TypeScript DeepDive：クラス](https://typescript-jp.gitbook.io/deep-dive/future-javascript/classes)
* [TypeScript DeepDive：readonly](https://typescript-jp.gitbook.io/deep-dive/type-system/readonly)
* [TypeScript DeepDive：等価演算子の同一性](https://typescript-jp.gitbook.io/deep-dive/recap/equality)

