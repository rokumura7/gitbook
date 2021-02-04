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

上記の`eq`では、クラスが同じであることも条件に加えている。
単純に値が等価であるかだけを比較したければ、下記で十分。
```typescript
eq(vo: ValueObject<T>): boolean {
  return this.get() === vo.get()
}
```
