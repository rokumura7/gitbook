# Git操作メモ

## ブランチの作成

```console
$git checkout -b 作成するブランチ名 元となるリモート名とブランチ名
```

## リモートの変更を取得（フォーク元）

```console
$git fetch リモート名
$git merge リモート名/ブランチ名
```

リモート名はoriginとかupstreamとか。

## 作業とプッシュ

```console
$git add .
$git commit
$git push --set-upstream リモート名 ブランチ名
```

## ブランチの削除

### ローカル

```console
$git branch -D ブランチ名
```

### リモート

```console
$git push リモート名 ブランチ名
```

## ブランチ名の変更

### ローカル

```console
$git branch -m 新しいブランチ名
```

### リモート

```console
$git push リモート名 :今のブランチ名
$git push --set-upstream リモート名 新しいブランチ名
```

## 変更を退避する

### 変更の退避

```console
$git stash
```

### 退避した変更の確認

```console
$git stash list
```

### 退避した変更の適用

- 最新のstashを適用する場合

```console
$git stash pop
```

- 指定する場合

```console
$git stash apply stash@{0}
```

### 退避した変更の削除

```console
$git stash clear
```
