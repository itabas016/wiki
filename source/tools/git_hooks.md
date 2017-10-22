---
title: "Git Hooks快捷使用"
layout: page
date: 2017-04-06 23:05:01
---

[TOC]

## Quick Start ##

通过git hooks快速将blog push到vps, 懒人必备。

### Prepare vps ###

``` bash
# add git user and grant sudo
addusr git

# if your OS is debian/ubuntu
gpasswd -a git sudo

# else centos (precondition: enable wheel group have sudo privileges)
usermod -aG wheel git

# upload your ssh key
ssh-copy-id -i ~/.ssh/id_rsa.pub git@ip(xxx.xxx.xxx.xxx)

# prepare git and nginx env
sudo apt-get install git-core nginx
```

### Configure repo ###

``` bash
# configure git repo
mkdir /home/git/wiki.git
cd /home/git/wiki.git/
git init --bare

# configure wiki site
mkdir /var/www/wiki

# chown folder owner to git
chown git:git -R /var/www/wiki
```

### Configure git hooks ###

``` bash
# vim post-receive file
cd /home/git/wiki.git/hooks
vim post-receive

#!/bin/bash
GIT_REPO=/home/git/wiki.git
TMP_GIT_CLONE=/tmp/wiki
PUBLIC_WWW=/var/www/wiki
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

``` bash
# grant execute privileges
chmod +x post-receive
```

### Configure nginx ###

``` bash
vi /etc/nginx/conf.d/wiki.conf
server {
  listen         80 ;
  root /var/www/wiki;
  server_name wiki.itabas.com;
  access_log  /var/log/nginx/wiki_access.log;
  error_log   /var/log/nginx/wiki_error.log;
  location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ {
    root /var/www/wiki;
    access_log   off;
    expires      1d;
  }
  location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
    root /var/www/wiki;
    access_log   off;
    expires      10m;
  }
  location / {
    root /var/www/wiki;
    if (-f $request_filename) {
    rewrite ^/(.*)$  /$1 break;
    }
  }
}
```

### Restart service ###

``` bash
service nginx restart
```

### Configure blog yml ###

无论你使用哪个静态博客框架, 基本上都支持一键部署, 详细参数参照`_config.yml`

``` yml
deploy:
  type: git
  repo: git@wiki.itabas.com:wiki.git
```