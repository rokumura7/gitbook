# Git操作メモ

## ブランチの作成

```console
$git checkout -b 作成するブランチ名 元となるリモート名とブランチ名
```

## リモートの変更を取得（フォーク元）

```console
$git fetch リモート名
$git pull リモート名/ブランチ名
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

追跡していないファイルも含めてstashを行う場合は`-u`オプションを付与する。

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

`pop`した場合は、同時に`stash`から削除も行われるが、  
`apply`した場合は、残る。

### 退避した変更の削除

```console
$git stash clear
```

## .gitignore

### 後から.gitignoreを追加する

`.gitignore`を編集した上で、

```console
git rm -r --cached 追跡を解除したいファイルパス
```

## 取り消し

### add

```console
git reset
```

### commit

```console
git reset [ --hard | --soft ] HEAD^
```

変更点を残す場合は`--hard`の代わりに`--soft`を利用する。

### push

#### origin

強制プッシュで履歴を無理やり戻すので`upstream`には使わないほうがよい。

```console
git reset [ --hard | --soft ] HEAD^
git push -f origin HEAD
```

#### upstream

打ち消しで対応する方法。  
履歴は残るが安全。

```console
git revert HEAD
git push origin HEAD
```

## reflog

`log`はコミット履歴であるのに対して、`reflog`は操作履歴。  
`git reset --hard`を取り消したい場合や削除したブランチを復元したい場合などの、いざという時に役立つコマンド。

下記、共通して`コミット`には`reflog`で表示された戻りたい時点の`HEAD@{n}`を指定する。

### resetを取り消す

```console
git reflog
git reset --hard コミット
```

### 削除したブランチを復元する

```console
git reflog
git branch ブランチ名 コミット
```
