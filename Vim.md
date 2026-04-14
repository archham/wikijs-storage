---
title: Vim
description: 
published: true
date: 2026-04-14T21:22:03.912Z
tags: linux, vim
editor: markdown
dateCreated: 2026-03-16T13:52:34.098Z
---

Vim cheat sheet [1](https://vim.rtorr.com/)
**Vim Cheat Sheet**
| Command           | Description                                               |
| ----------------- | --------------------------------------------------------- |
| `u`               | undo                                                      |
| `Ctrl`+`r`        | redo                                                      |
| `12Ctrl`+`r`      | redo to 12 steps before                                   |
| `v`               | mark regulary                                             |
| `Ctrl`+`v`        | mark in a block style                                     |
| `i`               | insert at this character                                  |
| `I`               | insert at the beginning of the line                       |
| `dd`              | delete line                                               |
| `yy`              | yank (copy) current line                                  |
| `y$`              | yank (copy) from current character to the end of the line |
| `c$`              | cut from current character to the end of the line         |
| `d$`              | delete from current character to the end of the line      |
| `:w`              | write (save and stay)                                     |
| `:wq`             | write (save) and quit                                     |
| `:x`              | same as :wq                                               |
| `:wq!`            | force write (save) and quit                               |
| `:q`              | quit                                                      |
| `:q!`             | force quit (close without saving)                         |
| `.`               | redo last action                                          |

**Useful Combinations**
| Command                                   | Description                   |
| ----------------------------------------- | ----------------------------- |
| `Ctrl`+`v` & `I` & `make changes` & `Esc` | mark in a block style, insert |
