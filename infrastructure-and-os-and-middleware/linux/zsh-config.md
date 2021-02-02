---
description: zshのプロンプト設定
---

# zsh設定

```bash
less ~/.zshrc
```

```bash
function rprompt-git-current-branch {
  local branch_name
  branch_name=`git rev-parse --abbrev-ref HEAD 2> /dev/null`
  if [ -n "$branch_name" ]; then
    echo "%B%F{29}◀%f%K{29}%F{15} $branch_name %f%k%b"
  fi
}

function prompt-working-time {
  local now
  now=$(date '+%H')
  if [ $now -eq 14 ]; then
    echo "🥦"
  elif [ $now -ge 19 -o $now -lt 10 ]; then
    echo "🤤"
  else
    echo "🍣"
  fi
}

setopt prompt_subst
RPROMPT='%F{99}%D{%H:%M:%S}%f'
PROMPT='%F{33}%~%f `rprompt-git-current-branch`
 `prompt-working-time`  ▶  '

##### ALIAS #####
## ls
alias ls='ls -aG'
alias ll='ls -l'

## git
alias glo='git log --oneline'
alias gbl="git for-each-ref refs/heads/ --sort='committerdate' --format='%(committerdate:short) %(refname:short)'"
alias ggraph='git log --graph --date-order -C -M --pretty=format:"<%h> %ad [%an] %Cgreen%d%Creset %s" --all --date=short'
```

## `RPROMPT`

プロンプトの右側に表示

## `PROMPT`

プロンプトの左側に表示

![&#x30D7;&#x30ED;&#x30F3;&#x30D7;&#x30C8;&#x30A4;&#x30E1;&#x30FC;&#x30B8;](../../.gitbook/assets/image%20%283%29%20%282%29.png)

## 参考リンク

* [256 COLORS - CHEAT SHEET](https://jonasjacek.github.io/colors/)
* [Zshでデキるプロンプト](https://www.slideshare.net/tetutaro/zsh-20923001)

