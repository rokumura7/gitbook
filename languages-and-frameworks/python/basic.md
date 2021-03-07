# 基礎

## Hello World

```python
print('Hello World')
```

## 空配列判定

```python
if not ary:
  print('ary is empty')
```

## map

mapで生成されるのはCollectionではなくIterable。  
配列として扱いたい場合は`list()`で囲う必要がある。

```python
before = [1,2,3]
after = list(map(lambda v: v * 2, before))
print(after) // 2, 4, 6
```

## filter

```python
before = [1,2,3,4]
after = list(filter(lambda v: v % 2 == 0, before))
print(after) // 2, 4
```

