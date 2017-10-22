---
title: "SSH Key相关问题"
layout: page
date: 2017-04-08 11:32:53
---

[TOC]

## Summary ##

SSH Key分公钥和私钥, 私钥请自己妥善保管, 不要外泄。
另外采用ssh public key authentication是提升服务器安全与快捷措施之一。

### Generate ssh key ###

以github为例使用ssh-keygen生成相对应的key

``` bash
# please make sure .ssh folder is exist
cd ~/
mkdir .ssh
cd /.ssh

# use ssh-keygen generate key
# if you want sign key name please add -f parameter, default is id_rsa
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):

# please input your password for private key, default is empty
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
### Configure public key to Server ###

上传公钥到server有很多种方式了, 推荐使用`ssh-copy-id`

* `ssh-copy-id -i ~/.ssh/id_rsa.pub git@ip -p xx`

有可能会遇到一些问题比如说: `Permission denied (publickey).` 或者`bash: ssh-copy-id: command not found`

If it's permission problem, please check current user whether has execute privilege to `.ssh` folder

Else if you use windows command line, maybe you didn't install this tiny script, please execute the below script:

Maybe is works, good luck for you!

``` bash
GET_ID="cat ${ID_FILE}"
{ eval "$GET_ID" ; } | ssh ${1%:} "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys" || exit 1
```

* `scp ~/.ssh/ ~/.ssh/id_rsa.pub -p xx user@ip:`

Attention please `:`, then login server execute:

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
rm ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys
```

### Keep safe authorized_keys ###

为了保证安全，您应该阻止其他用户添加新的公钥。
将 authorized_keys 的权限设置为对拥有者只读，其他用户没有任何权限：

``` bash
chmod 400 ~/.ssh/authorized_keys
```

### Disable password authentication ###

采用公钥登录server后, 最好禁用密码登录.
``` bash 
vim /etc/ssh/sshd_config
# set PasswordAuthentication value is no
RSAAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
ChallengeResponseAuthentication no

# restart sshd service
service sshd restart
```

### How to use in the tool ###

* 使用 alias

``` bash
$ cat ~/.bash_profile
alias ssh vps-gce ='ssh itabas016@xxx.xxx.xxx.xxx'
```

* 在 ssh client端import public key,这个就比较简单了。常用的客户端比如Putty, SecureCRT, XShell, Mobile client推荐JuiceSSH