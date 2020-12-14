---
description: zshのプロンプト設定
---

# zsh設定

```bash
less ~/.zshrc
```

```bash
function rprompt-git-current-branch {
  local branch_name st branch_status

  if [ ! -e  ".git" ]; then
    return
  fi
  branch_name=`git rev-parse --abbrev-ref HEAD 2> /dev/null`
  st=`git status 2> /dev/null`
  if [[ -n `echo "$st" | grep "^nothing to"` ]]; then
    branch_status="%F{green}"
  elif [[ -n `echo "$st" | grep "^Untracked files"` ]]; then
    branch_status="%F{red}?"
  elif [[ -n `echo "$st" | grep "^Changes not staged for commit"` ]]; then
    branch_status="%F{red}+"
  elif [[ -n `echo "$st" | grep "^Changes to be committed"` ]]; then
    branch_status="%F{yellow}!"
  elif [[ -n `echo "$st" | grep "^rebase in progress"` ]]; then
    echo "%F{red}!(no branch)"
    return
  else
    branch_status="%F{blue}"
  fi
  echo "${branch_status}[$branch_name]"
}

setopt prompt_subst
RPROMPT='`rprompt-git-current-branch` %F{magenta}%D{%H:%M:%S}%f'
PROMPT='%F{cyan}%~%f
 🍣 >> '

alias ls='ls -aG'
alias ll='ls -l'

alias glo='git log --oneline'
alias gbl="git for-each-ref refs/heads/ --sort='committerdate' --format='%(committerdate:short) %(refname:short)'"
alias ggraph='git log --graph --date-order -C -M --pretty=format:"<%h> %ad [%an] %Cgreen%d%Creset %s" --all --date=short'
```

### `RPROMPT`

プロンプトの右側に表示

### `PROMPT`

プロンプトの左側に表示

![](../../.gitbook/assets/image%20%281%29.png)



