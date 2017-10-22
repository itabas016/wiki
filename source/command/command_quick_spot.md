---
title: "Linux Command扫盲点"
layout: page
date: 2017-04-07 01:22:19
---

[TOC]

## 日常快捷系 ##

* `CTRL + U` – 剪切光标前的内容
* `CTRL + K` – 剪切光标至行末的内容
* `CTRL + Y` – 粘贴
* `CTRL + E` – 移动光标到行末
* `CTRL + A` – 移动光标到行首
* `CTRL + W` – 剪切光标前一个单词
* `ALT + F` – 跳向下一个空格
* `ALT + B` – 跳回上一个空格
* `ALT + Backspace` – 删除前一个单词
* `Shift + Insert` – 向终端内粘贴文本

## su sudo相关系 ##

* su

可译为switch user

* su -

切换到specific user下执行相关命令

* sudo

可译为Super-User-DO, 当然不同的linux内核也有所区别, 详细可以参照`/etc/sudoers`的配置

* sudo su

先看下详细的解释：

The often-used sudo su combination works as follows: first sudo asks you for your password, and, if you're allowed to do so, invokes the next command (su) as a super-user. Because su is invoked by root, it does not require you to enter the target user's password. So, sudo su allows you to open a shell as another user (including root), if you're allowed super-user access by the `/etc/sudoers` file

* sudo -i

侧重于acquires the root user's environment

Run the shell specified by the target user's password database entry as a login shell.  This means that login-specific resource files such as .profile or .login will be read by the shell.  If a command is specified, it is passed to the shell for execution via the shell's -c option.  If no command is specified, an interactive shell is executed.  sudo attempts to change to that user's home directory before running the shell.  The command is run with an environment similar to the one a user would receive at log in.  The Command Environment section in the sudoers(5) manual documents how the -i option affects the environment in which a command is run when the sudoers policy is in use.

* sudo -s

主要是runs a shell with root privileges

Run the shell specified by the SHELL environment variable if it is set or the shell specified by the invoking user's password database entry.  If a command is specified, it is passed to the shell for execution via the shell's -c option.  If no command is specified, an interactive shell is executed.

## 实用终端系 ##

* `nohup` 执行命令如要耗费长时间 可加此参数, 让提交的命令忽略`hangup`信号, 可再后台运行。 

Sample: `nohup wget xxxxxx &`, 如果重定向输出文件 `nohup wget xxxxxx > output.log 2>&1 &`
