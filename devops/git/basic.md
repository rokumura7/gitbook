# 基本操作

## ブランチの作成

```bash
git checkout -b 作成するブランチ名 元となるリモート名とブランチ名
```

## リモートの変更を取得（フォーク元）

```text
git fetch リモート名
git pull リモート名/ブランチ名
```

リモート名はoriginとかupstreamとか。

## 作業とプッシュ

```text
git add .
git commit
git push --set-upstream リモート名 ブランチ名
```

2回目以降は`git push`のみで良い。

## コミットメッセージの変更

```text
git commit --amend -m "新しいコミットメッセージ"
```

## コミットの整理

細かくコミットしすぎた時に。

```text
git log --oneline
5441aba hogehoge
55aa20b fugafuga
e029c5c foo
7658b8f bar
f1c1591 develop
445f637 ・・・
```

`5441aba`〜`7658b8f`を一つにまとめて、`f1c1591 develop`から一回だけのコミットとしたいと想定すると、

```text
git rebase -i f1c1591
```

で、vimエディターが開くので、

```text
pick 7658b8f bar
f e029c5c foo
f 55aa20b fugafuga
f 5441aba hogehoge
```

とする。

リモートに既にpushしている場合、履歴のズレが発生しているため`git push -f`を行う。

## ブランチの削除

### ローカル

```text
git branch -D ブランチ名
```

### リモート

```text
git push --delete リモート名 ブランチ名
# もしくは
git push リモート名 :ブランチ名
```

（ブラウザから操作したほうが早いし確実な気もする。。。）

## ブランチ名の変更

### ローカル

```text
git branch -m 新しいブランチ名
```

### リモート

```text
git push リモート名 :今のブランチ名
git push --set-upstream リモート名 新しいブランチ名
```

リモートブランチを削除して新しく作成しているだけ。

## 変更を退避する

### 変更の退避

```text
git stash
```

追跡していないファイルも含めてstashを行う場合は`-u`オプションを付与する。

### 退避した変更の確認

```text
git stash list
```

### 退避した変更の適用

* 最新のstashを適用する場合

```text
git stash pop
```

* 指定する場合

```text
git stash apply stash@{0}
```

`pop`した場合は、同時に`stash`から削除も行われるが、  
`apply`した場合は、残る。

### 退避した変更の削除

```text
git stash clear
```

