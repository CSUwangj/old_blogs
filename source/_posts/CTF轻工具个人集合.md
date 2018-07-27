---
title: CTF轻工具个人集合
connments: true
date: 2018-02-08 13:12:03
tags: 
	- Misc
	- Crypto
categories: Tools
desc:
summary:
typora-copy-images-to: ..\..\..\ForBetterMe\Image
---

持续更新ing

可能以后还会重新整理

<!-- more -->

# 古典密码学工具

## 在线解密

http://www.practicalcryptography.com/ciphers/

直链列表：

-    [Atbash Cipher](http://www.practicalcryptography.com/ciphers/classical-era/atbash-cipher/)
-    [ROT13 Cipher](http://www.practicalcryptography.com/ciphers/classical-era/rot13/)
-    [Caesar Cipher](http://www.practicalcryptography.com/ciphers/classical-era/caesar/)
-    [Affine Cipher](http://www.practicalcryptography.com/ciphers/classical-era/affine/)
-    [Rail-fence Cipher](http://www.practicalcryptography.com/ciphers/classical-era/rail-fence/)
-    [Baconian Cipher](http://www.practicalcryptography.com/ciphers/classical-era/baconian/)
-    [Polybius Square Cipher](http://www.practicalcryptography.com/ciphers/classical-era/polybius-square/)
-    [Simple Substitution Cipher](http://www.practicalcryptography.com/ciphers/classical-era/simple-substitution/)
-    [Codes and Nomenclators Cipher](http://www.practicalcryptography.com/ciphers/classical-era/codes-and-nomenclators/)
-    [Columnar Transposition Cipher](http://www.practicalcryptography.com/ciphers/classical-era/columnar-transposition/)
-    [Autokey Cipher](http://www.practicalcryptography.com/ciphers/classical-era/autokey/)
-    [Beaufort Cipher](http://www.practicalcryptography.com/ciphers/classical-era/beaufort/)
-    [Porta Cipher](http://www.practicalcryptography.com/ciphers/classical-era/porta/)
-    [Running Key Cipher](http://www.practicalcryptography.com/ciphers/classical-era/running-key/)
-    [Vigenère and Gronsfeld Cipher](http://www.practicalcryptography.com/ciphers/classical-era/vigenere-gronsfeld-and-autokey/)
-    [Homophonic Substitution Cipher](http://www.practicalcryptography.com/ciphers/classical-era/homophonic-substitution/)
-    [Four-Square Cipher](http://www.practicalcryptography.com/ciphers/classical-era/four-square/)
-    [Hill Cipher](http://www.practicalcryptography.com/ciphers/classical-era/hill/)
-    [Playfair Cipher](http://www.practicalcryptography.com/ciphers/classical-era/playfair/)
-    [ADFGVX Cipher](http://www.practicalcryptography.com/ciphers/classical-era/adfgvx/)
-    [ADFGX Cipher](http://www.practicalcryptography.com/ciphers/classical-era/adfgx/)
-    [Bifid Cipher](http://www.practicalcryptography.com/ciphers/classical-era/bifid/)
-    [Straddle Checkerboard Cipher](http://www.practicalcryptography.com/ciphers/classical-era/straddle-checkerboard/)
-    [Trifid Cipher](http://www.practicalcryptography.com/ciphers/classical-era/trifid/)
-    [Fractionated Morse Cipher](http://www.practicalcryptography.com/ciphers/classical-era/fractionated-morse/)
-    [Enigma Cipher](http://www.practicalcryptography.com/ciphers/mechanical-era/enigma/)
-    [Lorenz Cipher](http://www.practicalcryptography.com/ciphers/mechanical-era/lorenz/)

另一个网站： [WordCounter 英文论文单词统计](http://tool.yovisun.com/wordcounter/)

另一个维吉尼亚：https://atomcated.github.io/Vigenere/

另一个维吉尼亚：http://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx

另一个维吉尼亚：https://f00l.de/hacking/vigenere.php

文字加密解密： http://www.qqxiuzi.cn/bianma/wenbenjiami.php

单表替换：https://quipqiup.com/

## 库

[pycipher](http://pycipher.readthedocs.io/en/master/#)

# 数论计算器

https://www.alpertron.com.ar/CALTORS.HTM

# Brainfuck

http://esoteric.sange.fi/brainfuck/impl/interp/i.html

# PWN-cheatsheet

https://github.com/Naetw/CTF-pwn-tips

# PHP代码在线执行

http://sandbox.onlinephpfunctions.com/

# 在线扫码

https://online-barcode-reader.inliteresearch.com/

# MD5破解

https://somd5.com/

http://www.md5online.org/

# HASH破解

https://www.onlinehashcrack.com/

https://crackstation.net/

# 在线OCR

http://jinapdf.com/cn/image-to-text-file.php

http://ocr.wdku.net/

# base家族自动解

```python
# -*- coding: utf-8 -*-
# Python3
# AutoBase.py
from base64 import *
s = input()
lis1 = [s]
lis2 = []
lis3 = []
lis4 = []
while(1):
	for a in lis1:
		ok = 0
		try:
			lis2.append(b64decode(a).decode('ascii'))
			ok = 1
		except:
			pass
		try:
			lis2.append(b32decode(a).decode('ascii'))
			ok = 1
		except:
			pass
		try:
			lis2.append(b16decode(a).decode('ascii'))
			ok = 1
		except:
			pass
		if not ok:
			lis3.append(a)
	if not len(lis2):
		break
	lis1=lis2.copy()
	lis2.clear()
for a in range(0,len(lis3)):
	ok = 1
	for b in lis3[a]:
		if ord(b)>126 or ord(b)<32:
			ok = 0
			break
	if ok:
		lis4.append(lis3[a])
print(lis4)
```

# 漏洞平台

http://www.cnnvd.org.cn/

http://www.cnvd.org.cn/

http://exploit-db.com

http://www.exploit-id.com/

http://cve.mitre.org/

http://www.securiteam.com/

http://securityvulns.com/ (更新至2015.02.11)

http://securityvulns.ru/

http://www.securityfocus.com/

http://marc.info/?l=bugtraq

http://www.securitytracker.com/

https://packetstormsecurity.com/

# 题目

https://ctftime.org/

http://shell-storm.org/repo/

https://www.jarvisoj.com/

http://oj.xctf.org.cn/web/login/?next=/

http://www.shiyanbar.com/ctf/practice

http://ctf.nuptsast.com/

https://cgctf.nuptsast.com

https://skidophrenia.ctfd.io/

https://www.ichunqiu.com/battalion

http://cxsecurity.com/exploit/

http://reversing.kr/

http://pwnable.kr/

http://codeengn.com/challenges/

https://exploit-exercises.com/

https://io.netgarage.org/

http://hackinglab.cn/

http://captf.com/

http://www.baimaoxueyuan.com/ctf

http://hkyx.myhack58.com/index.html

http://overthewire.org/wargames/krypton/

https://backdoor.sdslabs.co/

http://www.hetianlab.com/CTFrace.html

<http://security.cs.rpi.edu/courses/binexp-spring2015/> 

http://www.wechall.net/challs

http://smashthestack.org/wargames.html

https://microcorruption.com/login

https://www.hackthissite.org/pages/index/index.php

https://exploit-exercises.com/protostar/

http://ctf.bugku.com/challenges

https://id0-rsa.pub/

