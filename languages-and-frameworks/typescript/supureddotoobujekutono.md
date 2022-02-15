# スプレッド構文とオブジェクトの一部変更

### スプレッド構文

配列やオブジェクトの結合や複製に利用できる。

#### 複製

```typescript
const ary = ['apple', 'orange', 'banana']
const copy = [...ary]
copy.push('kiwi')
console.log(ary)  // ["apple", "orange", "banana"] 
console.log(copy) // ["apple", "orange", "banana", "kiwi] 
```

元の配列を壊すことなく複製ができる。

```typescript
const staff = {
  id: 1,
  name: 'Pig',
}
const copyStaff = {...staff}
copyStaff.id = 2
console.log(staff)     // { "id": 1, "name": "Pig" } 
console.log(copyStaff) // { "id": 2, "name": "Pig" }
```

オブジェクトも同様。

#### 結合

```typescript
const ary1 = [ 'apple', 'orange' ]
const ary2 = [ 'apple', 'banana', 'kiwi' ]
const union = [ ...ary1, ...ary2 ]
console.log(union) // ["apple", "orange", "apple", "banana", "kiwi"] 
```

重複する値は除去されないので注意。

```typescript
const fruit1 = {
  id: 1,
  name: 'apple',
}
const fruit2 = {
  name: 'green_apple',
  color: 'green'
}
const union = { ...fruit1, ...fruit2 }
console.log(union) // { "id": 1, "name": "green_apple", "color": "green" }
```

オブジェクトの場合、同じプロパティが合った際に後勝ちで上書きされる。

これを利用して一部の値を書き換えることが可能。プロパティが多い際などに使える。

```typescript
const fruit1 = {
  id: 1,
  name: 'apple',
  color: 'red',
  price:100
}
const apple = { ...fruit1, ...{ price: 120 } }
console.log(apple) // { "id": 1, "name": "apple", "color": "red", "price": 120 }
```
