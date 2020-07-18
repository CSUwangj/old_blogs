---
title: PWN学习记录
connments: true
tags:
  - CTF
  - PWN
categories: Notes
abbrlink: 52311
date: 2018-02-10 00:17:50
desc:
summary:
---

学习PWN的记录

可能会有路线，看填坑进度……

<!-- more -->

# 环境配置

按理说第一篇是要说一下这个

我是Kali 2.0，所以就按照如下指令输进去就行了

```bash
dpkg --add-architecture i386 
apt-get update
apt-get -f dist-upgrade 
apt-get install lib32c-dev lib32stdc++6 libc6:i386 gcc-multilib
apt-get install python2.7 python-pip python-dev git libssl-dev libffi-dev build-essential
pip install pwntools
git clone https://github.com/pwndbg/pwndbg /opt/pwndbg
cd /opt/pwndbg
./setup.sh
```

# 学习记录及WP地址

-    Exploit-Exercise

     - [ ] Nebula
     - [x] [Protostar](http://csuwangj.top/2018/07/22/Exploit-Exercise-Protostar%E5%85%A8%E6%94%BB%E7%95%A5-%E8%A7%A3%E6%9E%90/)（解析还没写XD）
     - [ ] [Fusion](https://csuwangj.github.io/2018/08/03/Exploit-Exercise-Fusion%E5%85%A8%E6%94%BB%E7%95%A5-%E8%A7%A3%E6%9E%90/)
     -    Main Sequence
          - [ ] Main Sequence
          - [ ] Story line
          - [ ] Setup instructions
          - [ ] Irate Manticore
          - [ ] Touchy Owl
          - [ ] Wild Amphibian
          - [ ] Storming Bear
          - [ ] Screaming Jesus
          - [ ] Fabled Scorpion
          - [ ] Selfish Dragonfly
          - [ ] Vicious Platypus
-    CTF
     -    HGAME2018
          -    [week1](http://csuwangj.github.io/2018/02/08/HGAME2018-week1WP/)
               - [x] guess_number
               - [x] flag_server
               - [x] zazahui
          -    [week2](http://csuwangj.top/2018/02/16/HGAME2018-week2%E9%83%A8%E5%88%86WP/)
               - [x] ez_shellcode
               - [x] ez bash jail
               - [ ] hacker_system_ver1
               - [ ] ez_shellcode_ver2
-    To be continue...