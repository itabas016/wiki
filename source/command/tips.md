---
title: "Linux tips"
layout: page
date: 2017-04-16 22:22:32
---

[TOC]

## 强大grep命令 ##

``` bash
# ignore-case
grep -i

# use -f search grep result to file, but it's better use nf, will output result with line number
cat access.log | grep -nf result.txt

# grep by the header char
cat access.log | grep ^https

# grep by the end char
cat access.log | grep .com$

# other grep use regex pattern
cat access.log | grep [a-Z]{5}\.\com -f site_result.txt
```

## env相关 ##

`export` command just works for current environment, but, shell's `set` command is global env

``` bash
#  env [OPTION]... [-] [NAME=VALUE]... [COMMAND [ARG]...]

# start with an empty environment
env -i

# remove variable NAME from env
env -u
# modify env: 
export PATH=$PATH:newPath1:newPAth2
```

see a example - change oracle env to 12c:

``` bash
touch oraenv12
vim oraenv12
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
export PATH=$ORACLE_HOME/bin:$PATH:$HOME/bin
export ORACLE_SID=dev12_1

chmod +x oraenv12
```

## system core相关 ##

``` bash
# Check Linux core version
cat /proc/verion
Linux version 3.16.0-4-amd64 (debian-kernel@lists.debian.org) (gcc version 4.8.4 (Debian 4.8.4-1) ) #1 SMP Debian 3.16.39-1+deb8u2 (2017-03-07)

# other way
uname -a
Linux instance-1 3.16.0-4-amd64 #1 SMP Debian 3.16.39-1+deb8u2 (2017-03-07) x86_64 GNU/Linux

# another way
lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 8.7 (jessie)
Release:        8.7
Codename:       jessie
```

## How to open sudo privilege ##

``` bash
# Add user and grant root role:
Add user itabas
Passwd *****

Vi /etc/sudoers
# Add new line below root:
itabas   ALL=(ALL)     ALL

# Other way 
Vi /etc/sudoers
# Unmark this line:
%wheel    ALL=(ALL)    ALL this line

# Another way

Vi /etc/passwd change user ID to 0
itabas:x:0:500:itabas:/home/itabas:/bin/bash

# List user role and group:
Id itabas
```

## How to open swap ##

``` bash
# Open swap memory:
# normally, set value is twice current memory, now i set 1G
sudo fallocate -l 1G /swapfile

# lock file privilege
sudo chmod 600 /swapfile

# generate swap space
sudo mkswap /swapfile

# open the swap function
sudo swapon /swapfile

# check is work for swap
free -m

# Add auto start
sudo vim /etc/fstab
# add this record
/swapfile   none    swap    sw    0   0
```