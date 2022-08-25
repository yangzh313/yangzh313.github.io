---
alias:
  - ncat
title: ncat端口转发
tags:
  - linux
categories: linux
date: 2022-05-08
lastMod: 2022-08-21
---

## 参考

[Linux端口转发的几种常用方法](https://cloud.tencent.com/developer/article/1688152)

## ncat

安装ncat

```Shell
yum install nmap-ncat
```

监听本机11180端口，将数据转发到179.19.3.111得8080端口

```Shell
ncat --sh-exec "ncat 179.19.3.111 8080" -l 11180  --keep-open
```
