---
title: "Vps性能检测"
layout: page
date: 2017-08-27 21:19:01
---

[TOC]

> 折腾了两天Vps, 真可恨呢~

几个月前挂了个hello-world在gce上, 也忘记关了. 今天一看账单扣了我60多刀, 我滴个神呢. 白花花的银子就这么没了. 

可是手贱又把gcp的内存从1.7改成了0.6, 想着能省就省点吧. 鬼知道重新分配了个ip, 原来直联日本, 现在搞得要绕美国了, 延迟老高了. 哎~

![detail-trace](../static/images/vps-trace-detail-1.png)

![trace-gui](../static/images/vps-trace-detail-2.png)

> 不管怎么说gcp好是好，可还是太贵了, 于是又想着以前的Bandwagon Vps内存256还是OVZ的, 反正也快到期了, 要不换一个大点内存的. 就选了个年付LA直联中国的.

> 鬼知道测试下来的指标太不满意了. 广大同胞们都把Bandwagon玩坏了. 哎~

![detail-trace](../static/images/vps-d-trace-detail-3.png)

![trace-gui](../static/images/vps-d-trace-detail-4.png)

## Vps 指标 ##

### Bench Detail ###

直接用[秋水逸冰](https://teddysun.com/444.html)的[bench.sh](https://github.com/teddysun/across/blob/master/bench.sh)脚本跑出的结果很是一般:

```bash
wget -qO- bench.sh | bash

or
curl -Lso- bench.sh | bash
```

![bench-detail](../static/images/vps-d-bench-detail.png)

于是又单独查看了Cpu, Memory, Disk等各项参数, swap也没开, 硬盘读写简直了...

### Cpu ###

```bash
cat /proc/cpuinfo
```
![vps-d-cpu](../static/images/vps-d-cpu.png)

### Disk IO ###

```bash
dd if=/dev/zero of=test bs=64k count=4k oflag=dsync
dd if=/dev/zero of=test bs=8k count=256k conv=fdatasync
```
![vps-d-io](../static/images/vps-d-io.png)

相比之前的vps简直差太多了, LA机房直联中国严重超售了: 

![vps-io](../static/images/vps-io.png)

### Network ###

首先测试vps的download和upload, [speedtest.py](https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py)

```bash
wget --no-check-certificate https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod +x speedtest.py
./speedtest.py
```
![vps-d-speedtest](../static/images/vps-d-speedtest.png)

然后用mtr分别测试联通和电信：

![vps-d-mtr](../static/images/vps-d-mtr.png)

### Unix Bench Score ###

最后用[秋水逸冰](https://teddysun.com/245.html)的[unixbench.sh](https://github.com/teddysun/across/raw/master/unixbench.sh)脚本跑分测试了一下：

```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/unixbench.sh
chmod +x unixbench.sh
./unixbench.sh
```
![vps-d-unix-bench](../static/images/vps-d-unix-bench.png)

跑分倒还可以，只是这个磁盘读写不能让人满意. 现在倒是不知道该如何取舍了~
