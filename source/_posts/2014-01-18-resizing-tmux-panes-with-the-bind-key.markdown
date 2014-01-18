---
layout: post
title: "Resizing tmux panes with the bind-key"
date: 2014-01-18 14:06
comments: true
categories: tmux, resize panes 
---

<iframe align="center" width="600" height="450" src="//www.youtube.com/embed/oGs7h9ECL0Y" frameborder="0" allowfullscreen></iframe>

With small screens, resizing tmux panes efficiently is important.  Using the `resize-pane -U [number]` command each time you want to resize a pane, isn't going to cut it.

Add the following lines to your `~/.tmux.conf` file:

```
bind-key -r j resize-pane -D 5
bind-key -r k resize-pane -U 5
bind-key -r h resize-pane -L 5
bind-key -r l resize-pane -R 5
```

Instead of typing `resize-pane -U 10` into `tmux`, just hit `bind-key` and tap `k` a few times. If working with vertically split panes, type `bind-key` and `h` or `l` a few times.


