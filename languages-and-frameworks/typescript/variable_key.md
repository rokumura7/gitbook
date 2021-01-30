# 定数・変数をキーに設定する

例えばこんな列挙された定数があるとして、

```typescript
export default {
  ホゲホゲ: 'hogehoge',
  フガフガ: 'fugafuga',
} as const;
```

その定数を連想配列のキーに設定するにはスクエアブラケットで囲う

```typescript
import 定数LIST from './定数LIST'

const targetSets = {
  [定数LIST.ホゲホゲ]: { id: 1, foo: 'foofoo' },
  [定数LIST.フガフガ]: { id: 2, foo: 'barbar' },
}
```

