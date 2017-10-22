---
title: "Vps开启bbr加速"
layout: page
date: 2017-08-29 23:06:12
---

[TOC]

## One-Step ##

> Quick installation(懒人一键安装)

**PS:** Requirments

* OS: CentOS 6+，Debian 7+，Ubuntu 12+
* Virtual platform: Except OpenVZ, such as KVM, Xen, VMware etc.
* Memory: ≥128M

```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```

## Check-BBR ##

```bash
# check linux kernel version
[root@vps-d ~]# uname -a
Linux vps-d 4.10.4-1.el7.elrepo.x86_64 #1 SMP Sat Mar 18 12:50:10 EDT 2017 x86_64 x86_64 x86_64 GNU/Linux

# check network info
[root@vps-d ~]# sysctl net.ipv4.tcp_available_congestion_control
net.ipv4.tcp_available_congestion_control = bbr cubic reno
[root@vps-d ~]# sysctl net.ipv4.tcp_congestion_control
net.ipv4.tcp_congestion_control = bbr
[root@vps-d ~]# sysctl net.core.default_qdisc
net.core.default_qdisc = fq

# check bbr module is load
[root@vps-d ~]# lsmod | grep bbr
tcp_bbr                16384  24
```

**PS:** If the kernel version less than 4.12, you need upgrade.

## Upgrade-Kernel##

Sample with CentOS 7:

```bash
# add repo and install
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml kernel-ml-devel -y

# check kernel install
rpm -qa | grep kernel

# update grub system
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
# default 0
# Run by the first core(表示第一个内核设置为默认运行)
grub2-set-default 0

# restart
reboot
```

Reference: 

[开启TCP BBR拥塞控制算法](https://github.com/iMeiji/shadowsocks_install/wiki/%E5%BC%80%E5%90%AFTCP-BBR%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6%E7%AE%97%E6%B3%95)

[一键安装最新内核并开启 BBR 脚本](https://teddysun.com/489.html)