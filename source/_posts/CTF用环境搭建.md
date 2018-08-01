---
title: CTF用环境搭建
connments: true
tags:
  - CTF
  - environment
categories: Notes
desc: 记录步骤，方便自己动手的孩纸
summary: CTF用环境搭建，基于Kali-2018.02
abbrlink: 46723
date: 2018-07-20 14:57:49
---

# 目前工具集合

## 环境

- [x] 86架构及基本库
- [x] wine32
- [ ] pattern

## 安装式

这里的话就是那种安装以后直接用的，也有的是插件

- [x] pwndbg
- [x] r2
- [x] Audacity

## 非安装式

这里的是不用/不能安装的库，比如脚本等

统一放置在/opt/目录下，可能根据类别再分

- [x] rsatool
- [x] cloacked-pixel
- [x] volatility
- [x] routerpassview
- [x] Stegsolve 
- [ ] CTFcrackTools

## Python库

- [x] pwntools
- [x] gmpy2
- [x] unicorn
- [x] zio
- [x] angr
- [x] request 



<!--more-->

# 正文

## 环境

### 86架构及其个别基本库

```bash
dpkg --add-architecture i386 
apt-get update
apt-get -f dist-upgrade 
apt-get update
apt-get install lib32c-dev lib32stdc++6  
```

### wine32

```bash
apt-get install wine32
```



## 安装式

### pwndbg

```bash
git clone https://github.com/pwndbg/pwndbg /opt/pwndbg
cd /opt/pwndbg
./setup.sh 
```

如果对pip无法用的时候怎么安装感兴趣，可以戳[这里](http://csuwangj.top/2018/07/21/在pip连不上网的时候安装pwntools/)

### r2

```bash
git clone https://github.com/radare/radare2.git /opt/r2
cd /opt/r2/
sys/install.sh 
```

### Audacity

```bash
apt-get install audacity
```



## 非安装式

| 工具           | 指令                                                         | 备注                |
| -------------- | ------------------------------------------------------------ | ------------------- |
| rsatool        | `git clone https://github.com/ius/rsatool.git /opt/Crypto/rsatool` | Openssl RSA密钥生成 |
| cloacked-pixel | `git clone https://github.com/livz/cloacked-pixel /opt/Steganography/cloacked-pixel` | 图像隐写            |
| pattern        | 无                                                           |                     |
| volatility     | `git clone https://github.com/volatilityfoundation/volatility.git /opt/Forensics/volatility` | 内存取证            |
|                |                                                              |                     |

## Python库

默认在pip能用的情况下，默认为python2.7，其他版本会备注

| 工具     | 指令                           | 备注                               |
| -------- | ------------------------------ | ---------------------------------- |
| pwntools | `pip install pwntools`         |                                    |
| gmpy2    | `apt-get install python-gmpy2` |                                    |
| zio      | `pip install termcolor zio`    |                                    |
| angr     | `pip install angr`             |                                    |
| unicorn  | `pip install unicorn`          | pwntools自带，但是不妨碍你只安装它 |
|          | `pip install request`          |                                    |



