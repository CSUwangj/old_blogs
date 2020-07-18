---
title: OverTheWire-Bandit全攻略+解析
connments: true
date: 2018-08-23 16:10:57
tags: 
	- WARGAME
	- LinuxNotes
categories: WriteUp
desc: 
summary: 

---

- [x] [Level 0](http://overthewire.org/wargames/bandit/bandit0.html)
- [x] [Level 0 → Level 1](http://overthewire.org/wargames/bandit/bandit1.html)
- [x] [Level 1 → Level 2](http://overthewire.org/wargames/bandit/bandit2.html)
- [x] [Level 2 → Level 3](http://overthewire.org/wargames/bandit/bandit3.html)
- [x] [Level 3 → Level 4](http://overthewire.org/wargames/bandit/bandit4.html)
- [x] [Level 4 → Level 5](http://overthewire.org/wargames/bandit/bandit5.html)
- [x] [Level 5 → Level 6](http://overthewire.org/wargames/bandit/bandit6.html)
- [x] [Level 6 → Level 7](http://overthewire.org/wargames/bandit/bandit7.html)
- [x] [Level 7 → Level 8](http://overthewire.org/wargames/bandit/bandit8.html)
- [x] [Level 8 → Level 9](http://overthewire.org/wargames/bandit/bandit9.html)
- [x] [Level 9 → Level 10](http://overthewire.org/wargames/bandit/bandit10.html)
- [x] [Level 10 → Level 11](http://overthewire.org/wargames/bandit/bandit11.html)
- [x] [Level 11 → Level 12](http://overthewire.org/wargames/bandit/bandit12.html)
- [x] [Level 12 → Level 13](http://overthewire.org/wargames/bandit/bandit13.html)
- [x] [Level 13 → Level 14](http://overthewire.org/wargames/bandit/bandit14.html)
- [ ] [Level 14 → Level 15](http://overthewire.org/wargames/bandit/bandit15.html)
- [ ] [Level 15 → Level 16](http://overthewire.org/wargames/bandit/bandit16.html)
- [ ] [Level 16 → Level 17](http://overthewire.org/wargames/bandit/bandit17.html)
- [ ] [Level 17 → Level 18](http://overthewire.org/wargames/bandit/bandit18.html)
- [ ] [Level 18 → Level 19](http://overthewire.org/wargames/bandit/bandit19.html)
- [ ] [Level 19 → Level 20](http://overthewire.org/wargames/bandit/bandit20.html)
- [ ] [Level 20 → Level 21](http://overthewire.org/wargames/bandit/bandit21.html)
- [ ] [Level 21 → Level 22](http://overthewire.org/wargames/bandit/bandit22.html)
- [ ] [Level 22 → Level 23](http://overthewire.org/wargames/bandit/bandit23.html)
- [ ] [Level 23 → Level 24](http://overthewire.org/wargames/bandit/bandit24.html)
- [ ] [Level 24 → Level 25](http://overthewire.org/wargames/bandit/bandit25.html)
- [ ] [Level 25 → Level 26](http://overthewire.org/wargames/bandit/bandit26.html)
- [ ] [Level 26 → Level 27](http://overthewire.org/wargames/bandit/bandit27.html)
- [ ] [Level 27 → Level 28](http://overthewire.org/wargames/bandit/bandit28.html)
- [ ] [Level 28 → Level 29](http://overthewire.org/wargames/bandit/bandit29.html)
- [ ] [Level 29 → Level 30](http://overthewire.org/wargames/bandit/bandit30.html)
- [ ] [Level 30 → Level 31](http://overthewire.org/wargames/bandit/bandit31.html)
- [ ] [Level 31 → Level 32](http://overthewire.org/wargames/bandit/bandit32.html)
- [ ] [Level 32 → Level 33](http://overthewire.org/wargames/bandit/bandit33.html)
- [ ] [Level 33 → Level 34](http://overthewire.org/wargames/bandit/bandit34.html)

<!--more-->

# Level 0
```bash
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```
# Level 0 → Level 1
```
cat ~/readme
```
# Level 1 → Level 2
```
cat ~/-
```
## 参考资料
http://tldp.org/LDP/abs/html/special-chars.html
# Level 2 → Level 3
这一关其实,,,应该关了tab补全
```bash
cat ~/spaces\ in\ this\ filename
```
或者在windows用命令行的方式也行
```bash
cat "/home/bandit2/spaces in this filename"
cat "space in this filename
```
# Level 3 → Level 4
```bash
cat ~/inhere/.hidden
```
# Level 4 → Level 5
```bash
i=0; while [ $i -le 9 ]; do file `python -c "print '/home/bandit4/inhere/-file%02d' % $i"`; i=$((i+1)); done
```
# Level 5 → Level 6
难度不够啊，同大小的就一个...
```bash
 find . -type f -size 1033c -print | xargs cat
```
还要过滤的话文件加-perm参数未必有用，目前考虑用ls -l和file命令做。
# Level 6 → Level 7
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
```
很见鬼的是，-print后面放哪些东西会出问题...
# Level 7 → Level 8
```bash
 cat ~/data.txt | grep millionth
```
# Level 8 → Level 9
```bash
sort ~/data.txt | uniq -u
```
# Level 9 → Level 10
```bash
strings ~/data.txt | grep ==
```
# Level 10 → Level 11
```bash
base64 -d ~/data.txt
```
# Level 11 → Level 12
```bash
 cat ~/data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
```
# Level 12 → Level 13
```bash
xxd -r ~/data.txt | gzip -d - | bzip2 -d - | gzip -d - | tar -xOf - | tar -xOf - | bzip2 -d - | tar -xOf - | gzip -d -
```
# Level 13 → Level 14
# Level 14 → Level 15
# Level 15 → Level 16
# Level 16 → Level 17
# Level 17 → Level 18
# Level 18 → Level 19
# Level 19 → Level 20
# Level 20 → Level 21
# Level 21 → Level 22
# Level 22 → Level 23
# Level 23 → Level 24
# Level 24 → Level 25
# Level 25 → Level 26
# Level 26 → Level 27
# Level 27 → Level 28
# Level 28 → Level 29
# Level 29 → Level 30
# Level 30 → Level 31
# Level 31 → Level 32
# Level 32 → Level 33
# Level 33 → Level 34