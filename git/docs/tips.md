# Git操作メモ

## .gitignore

### 後から.gitignoreを追加する

`.gitignore`を編集した上で、

```console
$ git rm -r --cached 追跡を解除したいファイルパス
```

---

## 取り消し

### add

```console
$ git reset
```

### commit

```console
$ git reset [ --hard | --soft ] HEAD^
```

変更点を残す場合は`--hard`の代わりに`--soft`を利用する。

### push

#### origin

強制プッシュで履歴を無理やり戻すので`upstream`には使わないほうがよい。

```console
$ git reset [ --hard | --soft ] HEAD^
$ git push -f origin HEAD
```

#### upstream

打ち消しで対応する方法。  
履歴は残るが安全。

```console
$ git revert HEAD
$ git push origin HEAD
```

---

## reflog

`log`はコミット履歴であるのに対して、`reflog`は操作履歴。  
`git reset --hard`を取り消したい場合や削除したブランチを復元したい場合などの、いざという時に役立つコマンド。

下記、共通して`コミット`には`reflog`で表示された戻りたい時点の`HEAD@{n}`を指定する。

### resetを取り消す

```console
$ git reflog
$ git reset --hard コミット
```

### 削除したブランチを復元する

```console
$ git reflog
$ git branch ブランチ名 コミット
```

---

## ブランチをコミット順に表示

```console
$ git for-each-ref refs/heads/ --sort='committerdate' --format='%(committerdate:short) %(refname:short)'
```

alias登録すると便利
```console
$ less .bashrc | grep gl
alias gl="git for-each-ref refs/heads/ --sort='committerdate' --format='%(committerdate:short) %(refname:short)'"
```
