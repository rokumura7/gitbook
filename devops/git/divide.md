# リポジトリ分割

## やりたいこと

```text
dir
├── subdirA
└── subdirB
```

dirから、subdirBだけ取り出して他のリポジトリで管理したい。  
例えば、フロントエンドとAPIのソースコードが共存しているリポジトリからフロントエンドのリポジトリだけ他のリポジトリに分割する、とか。

この際に履歴は引き継ぎたいし、masterはブランチプロテクションされている想定。

dirを管理しているのがArepo、subdirBを管理したいのがBrepoとする。

## リポジトリの準備

### 分割したいリポジトリを手元にcloneする（A）

**※このリポジトリがBrepoの実体となるので、現状手元にあるAporeとは別の場所にcloneすること**

```bash
git clone ArepoのURL
```

### チェックアウト（A）

最終的にはPRで変更を適用するため`master`や`develop`から分岐させる。

```bash
git checkout -b divide_repository
```

### Brepoで管理したいもの以外を削除する（A）

```bash
git filter-branch --tree-filter 'rm -rf 消したいファイルやフォルダ' -f --prune-empty
```

消したいファイルやフォルダが複数存在する場合はスペース区切りで指定することができる。

上述の例で書くと`subdirB`を残したいので`subdirA`を指定する。

```bash
git filter-branch --tree-filter 'rm -rf subdirA' -f --prune-empty
```

この時点で`ls`などしてみると`subdirA`が消えていることを確認できる。

### 新しいリポジトリをremoteに設定（A→B）

```bash
git remote set-url origin BrepoのURL
```

（いつものupstream、originの構成を想定）

### Brepoに分割したものだけを一度push（B）

```bash
git push --set-upstream origin divide_repository
```

この時点ではBrepoとは全く異なる歴史をもったブランチであるため、このブランチはBrepoの`master`あるいは`develop`にPRを出すことができない。

### Brepoの歴史を取り込む（B）

手元の`master`があるとややこいので一度消す。

```bash
git branch -D master
git fetch
git checkout -b master origin/master
git log
```

`git log`でBrepoの履歴が表示されればOK。  
`divide_repository`に戻る。

```bash
git checkout divide_repository
```

ここまで出来たらArepoの歴史をBrepoから始まったことにする。

```bash
git rebase master
```

おそらく`README.md`がコンフリクトするはずなので修正して`rebase`を完了させる。

```bash
git add .
git rebase --continue
```

なお、`rebase`ではなく`merge`でも対応可能。  
`rebase`の場合、Brepoの`master`が始点になるのに対して、  
`merge`の場合、Arepoの歴史の途中にBrepoの`master`が取り込まれた形になる。

`merge`の場合、歴史が違うためそのままでは通らない。

```bash
git merge master
fatal: refusing to merge unrelated histories
```

`--allow-unrelated-histories`というオプションを付与する。

```bash
git merge master --allow-unrelated-histories
```

### pushしてPR（B）

`rebase`を実施しているので`-f`で`push`する。

```bash
git push -f
```

Brepoの対応はこれで完了。

### ArepoからsubdirBを消す（A）

無事`subdirB`をBrepoで管理できたことを確認できたらArepoから`subdirB`を消す。

```text
git rm subdirB
```

あとは`push`してPRでOK。

