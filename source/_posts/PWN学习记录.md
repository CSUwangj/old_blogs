---
title: PWN学习记录
connments: true
date: 2018-02-10 00:17:50
tags: 
	- CTF
	- PWN
categories: Notes
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
apt-get install lib32c-dev lib32stdc++6  
apt-get install python2.7 python-pip python-dev git libssl-dev libffi-dev build-essential
pip install pwntools
git clone https://github.com/pwndbg/pwndbg /opt/pwndbg
cd /opt/pwndbg
./setup.sh
```

# 学习记录及WP地址

-    Exploit-Exercise

     -    Nebula

          - [ ] Level 00
          - [ ] Level 01
          - [ ] Level 02
          - [ ] Level 03
          - [ ] Level 04
          - [ ] Level 05
          - [ ] Level 06
          - [ ] Level 07
          - [ ] Level 08
          - [ ] Level 09
          - [ ] Level 10
          - [ ] Level 11
          - [ ] Level 12
          - [ ] Level 13
          - [ ] Level 14
          - [ ] Level 15
          - [ ] Level 16
          - [ ] Level 17
          - [ ] Level 18
          - [ ] Level 19
     -    Protostar
     -    Fusion

          - [ ] Level 00
          - [ ] Level 01
          - [ ] Level 02
          - [ ] Level 03
          - [ ] Level 04
          - [ ] Level 05
          - [ ] Level 06
          - [ ] Level 07
          - [ ] Level 08
          - [ ] Level 09
          - [ ] Level 10
          - [ ] Level 11
          - [ ] Level 12
          - [ ] Level 13
          - [ ] Level 14
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
          -    [week1](https://csuwangj.github.io/2018/02/08/HGAME2018-week1WP/)
               - [x] guess_number
               - [x] flag_server
               - [x] zazahui
          -    week2
               - [x] ez_shellcode
               - [x] ez bash jail
               - [ ] hacker_system_ver1
               - [ ] ez_shellcode_ver2
-    To be continue...