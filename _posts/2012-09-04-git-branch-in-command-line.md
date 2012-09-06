---
layout: default
published: true
---

Here's the script I got passed down from who knows where on how to show the git branch in the command prompt. I've improved it by coloring the branch name. RED means there are unstaged changes. YELLOW means the working directory and index match. And GREEN means working directory is clean (nothing to commit).


<script src="https://gist.github.com/3658683.js?file=.bash_login"> </script>

RED, YELLOW, GREEN, and NO_COLOUR are color settings.

The function parse_git_branch parses the branch from the `git status -b --porcelain` command. I used `head` to take the first lineusing sed to run the regular expressions

parse_git_status echo's RED or GREEN depending if the directory is clean or not. `git status --porcelain` is a minimalist git status that should be used for scripting.

The PS1 line sets the prompt. The \$(function_name) runs the function every time the prompt is accessed, which is what you want for a prompt. If it were merely $(function_name) or `function_name`, the function would be run only once.