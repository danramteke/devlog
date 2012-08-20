---
layout: default
published: true
---

# How does it work?

Made some keymappings in Sublime Text 2.

Open the keymap files by going to "Sublime Text 2" > "Preferences" > "Key Bindings - User"

This is what my key bindings file looks like:
  [
  { "keys": ["super+shift+c"], "command": "copy_path" },
  { "keys": ["super+shift+1"], "command": "reveal_in_side_bar"},
  { "keys": ["super+alt+l"], "command": "reindent", "args": {"single_line": false}}
  ]