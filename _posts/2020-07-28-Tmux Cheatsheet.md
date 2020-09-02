---
layout: post
title: "Tmux Cheatsheet"
description: "생산성 +1000"
comments: true
categories: [Cheatsheet]
tags:
- Engineering
- Cheat sheet
---

## References

* https://edykim.com/ko/post/tmux-introductory-series-summary/#fnref-2124-2
* https://man.cx/tmux
* https://gist.github.com/andreyvit/2921703



## Session

```bash
# create
tmux new-session -s work

# rename
C-a $          # rename the current session

# navigate
C-a (          # previous session
C-a )          # next session
C-a s          # choose a session from a list

# detach
C-a d

# kill
C-d 
```



## Windows

```bash
# create
C-a c          # create a new window

# rename
C-a ,          # rename the current window

# navigate 
C-a 1 ...      # switch to window 1, ..., 9, 0
C-a p          # previous window
C-a n          # next window

# kill
C-a &          # kill the current window
C-d

```



## Panes

```bash
# create
C-a "          # split vertically (top/bottom)
C-a %          # split horizontally (left/right)

# navigate
C-a left       # go to the next pane on the left
C-a right      # (or one of these other directions)
C-a up
C-a down

# kill
C-a x          # kill the current pane
```

