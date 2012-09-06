---
layout: default
published: true
---

Here's the script I got passed down from who knows where on how to show the git branch in the command prompt. I've improved it by coloring the branch name. RED means there are unstaged changes. YELLOW means the working directory and index match. And GREEN means working directory is clean (nothing to commit).

A colorblind version is forthcoming.

    function parse_git_branch () {
       git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
    }

    RED="\[\033[0;31m\]"
    YELLOW="\[\033[0;33m\]"
    GREEN="\[\033[0;32m\]"
    NO_COLOUR="\[\033[0m\]"

    PS1="$GREEN\u$NO_COLOUR:\w$YELLOW\$(parse_git_branch)$NO_COLOUR\$ "