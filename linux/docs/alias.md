# alias

## 追加

```console
$ cd ~
$ vim .bashrc
$ source .bash_profile
```

`alias`の書き方は`alias エイリアス名="コマンド"`

例）
```console
$ less .bashrc

# bash
alias bsrc='vim ~/.bashrc'
alias bash_profile='source ~/.bash_profile'

# ls
alias ls='ls -aG'
alias ll='ls -l'

# tree
alias tree='tree -C'
alias tree1='tree -L 1'
alias tree2='tree -L 2'

# git
alias gl="git for-each-ref refs/heads/ --sort='committerdate' --format='%(committerdate:short) %(refname:short)'"

```
