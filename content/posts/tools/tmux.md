---
title: "Tmux-Ease your terminal workflow"
date: 2022-12-16T16:32:59
description: "This post goes over what is Tmux, and why should we use it if we are working with a terminal for quite a while"
tags: ["Unix tools", "Ease of access", "tmux"]
categories: ["Unix tools"]
author: "Tanmay"
showToc: true
TocOpen: true
cover:
    image: "https://www.redhat.com/sysadmin/sites/default/files/styles/full/public/2020-02/blur-bright-business-codes-207580.jpg?itok=eUyGgeea"
    alt: Docker
    caption: "General Unix system terminals. [Image Source](https://www.redhat.com/sysadmin/sites/default/files/styles/full/public/2020-02/blur-bright-business-codes-207580.jpg?itok=eUyGgeea)"
ShowBreadCrumbs: true
---
 ## Why `Tmux`⁉️

Every software engineer, or people working with software development, have had some sort of interaction with a `terminal`. Now, terminals are great! If one really masters it, there is no need to use your default OS file explorer to get around files that you need. 

However, I personally find it quite `chaotic` to use many many many terminal windows when trying to work on a single project. The problem of now knowing which terminal window has what can be mind-boggling, and if you have even 5 of such windows, you are already at a trouble to know which terminal has what process running.

Enter `Tmux`

## What is `Tmux`❓

`Tmux` is a terminal multiplexer for `Ubuntu` (and other Linux-based operating systems). It allows users to run `multiple` terminal sessions within a single terminal window and switch between them easily. `Tmux` also allows users to `detach` and `reattach` sessions, which makes it useful for running long-running commands or keeping a session open even when you are not actively using it.

In short, we can split a single terminal into many terminal windows, without having to open many terminal windows for each separate tasks. We can also name our terminal sessions in `Tmux`, which makes it easier to keep track of which terminal window is for which job. The `detach` and `reattach` functions are also super convenient, since we can temporarily suspend our terminals and resume back to them whenever we want. 

So, let's now see how one would go about using `Tmux`!!

### Installing `Tmux`

To install Tmux on Ubuntu, you can use the following command in your terminal:

```bash
sudo apt-get install tmux
```

Once finished, you should have `Tmux` installed on your machine. Once installed, you can start `Tmux` by running the command `Tmux` in the terminal. Once you are in `Tmux`, you can use various key commands to navigate and manage your sessions, windows, and panes. You can find more information on how to use `Tmux` by running `man Tmux` in the terminal. However, I can help you get started with a few useful things to move around tmux.

## Tmux shortcuts

### Create Splits
Once you type `tmux` in your favourite terminal, you should see the terminal on the default directory of your machine. From here, type in your first command using:

```bash
: ctrl + b %
```

When you type the colon `:`, look at the bottom of the window to see the prompt changing to `:`. The `ctrl + b` is the default prefix that `Tmux` has when you want to execute `Tmux` commands in a window. The `%` symbol is used to split the terminal horizontally. Similarly, replacing `%` with `'` will split the window vertically. 

### Navigating Panes
Now that you have your first split, start typing your commands in any of the splits. To switch between the terminal windows, there are two ways:

1) `prefix + <direction>`: Here prefix is the default prefix `Ctrl + b` and direction is one of the arrow keys (up, down, left, or right) to move the focus to the corresponding pane.
2) `prefix + o`: To move the focus to the next pane in the current window.
3) `prefix + { or prefix + }` : To move the focus to the next or previous pane in the current window
4) `prefix + q + <pane-number>`: Prompts the number of the panes on screen, and you choose the number where you want to switch. 
### Resize Windows

We can also resize our splits that we created with the following commands:

1) `prefix + <direction> + <arrow key>`: to resize the current pane in the specified direction.
2) `prefix + <direction> + <arrow key> + <Shift>`: to resize the current pane more quickly in the specified direction.

## `Tmux` configuration file

The default `prefix` that `Tmux` provides is a bit unnatural to many programmers, and this can be fixed very easily. To fix this, you just need to create a `tmux.conf` file in the root system, and add the following line there:

```bash
set-hook -g after-new-session "source-file ~/.tmux.conf" 

unbind-key C-b 
set-option -g prefix C-<enter new prefix key here>
bind-key C-g send-prefix 
```

Once you enter your favourite prefix key in the `set-option` command, you will have changed your prefix. For the changes to appear, either restart tmux server, or type the following in any of the tmux panes:

```bash
: source ~/.tmux.conf
```

This will source your changes from the config file, and your prefix should be changed. 

### Some popular changes to replace default behaviour in Tmux

Generally, these are the configuration files that I have seen programmers use quite often:

```bash
set-hook -g after-new-session "source-file ~/.tmux.conf"

unbind-key C-b
set-option -g prefix C-g
bind-key C-g send-prefix

set-window-option -g mode-keys vi
bind e setw synchronize-panes on\; display-message "Panes are synchronized"
bind E setw synchronize-panes off\; display-message "Panes not synchronized"

bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
```

To know what each of them do, type the `prefix` key that you set, and then the `alphabet` following the `bind` keyword in the above configuration.

This can be used as a good starting point for your `tmux.conf` file.

# Congratulations

You are now a pro terminal user who uses tmux to set up their workflow. Hopefully, you should soon see a boost in your productivity, and will also fall in love with your terminal :)

Until next time!!

