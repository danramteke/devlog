---
layout: default
published: true
---

Here's the script I got passed down from who knows where on how to show the git branch in the command prompt. I've improved it by coloring the branch name. RED means there are unstaged changes. YELLOW means the working directory and index match. And GREEN means working directory is clean (nothing to commit).

A colorblind version is forthcoming.

    RED="\033[0;31m"
    YELLOW="\033[0;33m"
    GREEN="\033[0;32m"
    NO_COLOUR="\033[0m"


    function parse_git_branch () {
       git status -b --porcelain | head -1 | sed -e 's/## //'
    }

    function parse_git_status () {
    	if [ -z "`git status --porcelain 2> /dev/null`" ]
    	then 
    		echo -e $GREEN
    	elif [ -z "`git st --porcelain 2> /dev/null | grep -v -E "^[MARC]  .*$"`" ]
    	then
    		echo -e $YELLOW
    	else
    		echo -e $RED
    	fi
    }

    export PS1="$GREEN\u$NO_COLOUR:\w\$(parse_git_status) (\$(parse_git_branch))$NO_COLOUR\$ "