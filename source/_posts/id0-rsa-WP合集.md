---
title: id0-rsa WP合集
connments: true
date: 2018-04-19 15:53:15
tags: 
	- CTF
	- Crypto
categories: WriteUp
desc: id0-rsa WP合集
summary: id0-rsa WP合集
---
忙里偷闲做做题wwwwwwwwwwwww


<!-- more -->


# 进度表

- [x] [Intro to Hashing](https://id0-rsa.pub/problem/18/)
- [x] [Intro to PGP](https://id0-rsa.pub/problem/19/)
- [x] [Hello PGP](https://id0-rsa.pub/problem/1/)
- [x] [Hello OpenSSL](https://id0-rsa.pub/problem/3/)
- [x] [Intro to RSA](https://id0-rsa.pub/problem/21/)
- [x] [Caesar](https://id0-rsa.pub/problem/32/)
- [x] [Hello Bitcoin](https://id0-rsa.pub/problem/2/)
- [x] [Ps and Qs](https://id0-rsa.pub/problem/8/)
- [x] [Affine Cipher](https://id0-rsa.pub/problem/5/)
- [x] [Cut and Paste Attack On AES-ECB](https://id0-rsa.pub/problem/26/)
- [x] [Rail Fence](https://id0-rsa.pub/problem/34/)
- [x] [Factoring RSA With CRT Optimization](https://id0-rsa.pub/problem/15/)
- [x] [Easy Passwords](https://id0-rsa.pub/problem/38/)
- [x] [RSA Modulus Factorization](https://id0-rsa.pub/problem/45/)
- [x] [Fast Hashing Passwords](https://id0-rsa.pub/problem/24/)
- [x] [Vigenère](https://id0-rsa.pub/problem/33/)
- [x] [Monoalphabetic Cipher](https://id0-rsa.pub/problem/12/)
- [x] [Salt Alone Won't Save You](https://id0-rsa.pub/problem/25/)
- [x] [CCA on Textbook RSA](https://id0-rsa.pub/problem/23/)
- [ ] [AES-CTR with Nonce Reuse](https://id0-rsa.pub/problem/4/)
- [x] [Bad Entropy](https://id0-rsa.pub/problem/30/)
- [ ] [Double Strength Affine](https://id0-rsa.pub/problem/6/)
- [ ] [Rainbow Table Hash Chain](https://id0-rsa.pub/problem/16/)
- [ ] [Elliptic Curve Private Key Recovery](https://id0-rsa.pub/problem/10/)
- [ ] [ECDSA Nonce Recovery](https://id0-rsa.pub/problem/17/)
- [ ] [Slightly harder passwords](https://id0-rsa.pub/problem/41/)
- [ ] [Upgraded Affine](https://id0-rsa.pub/problem/40/)
- [ ] [Fvtavat Xrl Erpbirel](https://id0-rsa.pub/problem/39/)
- [x] [Insufficient Key Size](https://id0-rsa.pub/problem/9/)
- [ ] [Håstad's Broadcast Attack](https://id0-rsa.pub/problem/11/)
- [ ] [CBC Padding Attack](https://id0-rsa.pub/problem/22/)
- [ ] [Breaking PDF Passwords](https://id0-rsa.pub/problem/29/)
- [x] [Vigenère + Rail Fence](https://id0-rsa.pub/problem/35/)
- [x] [Recover the secret phone number](https://id0-rsa.pub/problem/43/)
- [ ] [Optimal Backpack Allocation](https://id0-rsa.pub/problem/42/)
- [ ] [Insecure PRNG](https://id0-rsa.pub/problem/27/)
- [ ] [Playfair](https://id0-rsa.pub/problem/13/)
- [ ] [CRIMEs against TLS](https://id0-rsa.pub/problem/20/)
- [ ] [Bleichenbacher's CCA2 on RSA](https://id0-rsa.pub/problem/14/)
- [ ] [Backdoored PRNG](https://id0-rsa.pub/problem/31/)
- [ ] [Not So Safe Primes](https://id0-rsa.pub/problem/37/)
- [ ] [DSA with LCG nonces](https://id0-rsa.pub/problem/44/)

# Intro to Hashing

```python
# Python

>>> import hashlib
>>> hashlib.md5(hashlib.sha256('id0-rsa.pub').hexdigest()).hexdigest()
# 'b25d449d86aa07981d358d3b71b891de'
```



# Intro to PGP

```bash
# bash

root@kali:~/Documents/id0-rsa# touch Intro_to_PGP.key
root@kali:~/Documents/id0-rsa# gpg --import Intro_to_PGP.key 
gpg: key 2503D0F1A81B09D4: public key "id0-rsa.pub (http://id0-rsa.pub) <id0rsa.pub@gmail.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
root@kali:~/Documents/id0-rsa# touch Intro_to_PGP.txt
root@kali:~/Documents/id0-rsa# gpg -d Intro_to_PGP.txt

# Thank you Phil Zimmermann!
```

# Hello PGP

```bash
#!/bin/bash

count=1

while read WORD; do
	gpg --batch --passphrase $WORD --decrypt test.txt 2>/dev/null
	if [ $? -eq 0 ]; then
		echo 
		echo $WORD
		exit
	fi
	count=$(( count + 1 ))
	if [ $((count % 1000)) -eq 0 ]; then
		echo $count
	fi
done < words

# passionately apathetic
# seamanship
# gpg: AES256 encrypted data
# gpg: encrypted with 1 passphrase

```

# Hello OpenSSL

## 繁琐的解法

```bash
# bash
root@kali:~/Documents/id0-rsa/Hello_OpenSSL# openssl rsa -in priva.pem -text
Private-Key: (256 bit)
modulus:
    00:e6:dc:a0:a5:26:5d:39:95:0c:7e:e3:b7:a1:31:
    96:47:87:00:2c:1b:56:ba:2e:54:ce:b4:30:db:ff:
    09:95:9d
publicExponent: 65537 (0x10001)
privateExponent:
    00:8f:67:e1:8a:75:28:57:ca:94:76:85:f1:dd:79:
    b6:05:0e:35:05:e7:f9:ed:da:23:e6:de:14:aa:22:
    d9:78:a9
prime1:
    00:fd:99:07:3e:67:03:c1:72:2a:96:81:ab:9a:29:
    db:d7
prime2:
    00:e9:0c:76:fe:de:98:c1:9d:d3:c8:30:c0:e4:3a:
    8b:ab
exponent1:
    00:b4:a6:37:17:c7:d0:50:14:20:ac:58:30:c2:c0:
    00:bf
exponent2:
    00:c5:87:27:25:07:8e:fa:2c:c7:e0:9a:52:24:1f:
    eb:59
coefficient:
    00:e3:bd:9b:a2:47:11:68:33:2d:80:fe:7d:ed:34:
    de:fc
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MIGtAgEAAiEA5tygpSZdOZUMfuO3oTGWR4cALBtWui5UzrQw2/8JlZ0CAwEAAQIh
AI9n4Yp1KFfKlHaF8d15tgUONQXn+e3aI+beFKoi2XipAhEA/ZkHPmcDwXIqloGr
minb1wIRAOkMdv7emMGd08gwwOQ6i6sCEQC0pjcXx9BQFCCsWDDCwAC/AhEAxYcn
JQeO+izH4JpSJB/rWQIRAOO9m6JHEWgzLYD+fe003vw=
-----END RSA PRIVATE KEY-----

```

```python
# Python
>>> n = 0xe6dca0a5265d39950c7ee3b7a131964787002c1b56ba2e54ceb430dbff09959d

>>> d = 0x8f67e18a752857ca947685f1dd79b6050e3505e7f9edda23e6de14aa22d978a9

>>> c = 0x6794893f3c47247262e95fbed846e1a623fc67b1dd96e13c7f9fc3b880642e42

>>> hex(pow(c,d,n))
'0x310f2eb0634ed1ab'

```

## 一行

```bash
# bash
root@kali:~/Documents/id0-rsa/Hello_OpenSSL# openssl rsautl -decrypt -in <(xxd -r -p cipher) -inkey priva.pem -raw | xxd -p -c 8 | tail -n1
# 310f2eb0634ed1ab
```

# Intro to RSA

```python
# Python
>>> (e, N) = (0x3, 0x64ac4671cb4401e906cd273a2ecbc679f55b879f0ecb25eefcb377ac724ee3b1)
>>> d = 0x431d844bdcd801460488c4d17487d9a5ccc95698301d6ab2e218e4b575d52ea3

>>> c = 0x599f55a1b0520a19233c169b8c339f10695f9e61c92bd8fd3c17c8bba0d5677e

>>> hex(pow(c,d,N))
'0x4d801868d894740b2be29309fcd3edcd51bd2c2a685028b89290f9268c727581'
```

# Caesar

{% asset_img 1524188467870.png Caesar %}

但是交了以后就显示错误，不知道为什么……

---

破案了，因为凯撒之后的原文是

{% asset_img 1525509896561.png Caesar_solution %}



# Hello Bitcoin

按资料算就是了

然而我找到了在线计算的[工具](http://gobittest.appspot.com/PrivateKey);D

按照比特币wiki的说法，计算过程大概是这样的：

1. 首先我们生成一个私钥
2. 获取对应的公钥
3. 获得公钥的SHA256散列值
4. 获取上一步结果的RIPEMD-160散列值
5. 在散列值前加入版本号
6. 计算上一步结果的SHA-256散列
7. 取第二个SHA-256散列值的前四字节作为地址校验和
8. 在第四步的结果末尾加入校验和
9. 进行Base28编码

`You already solved this one! Solution:  18GZRs5nx8sVhF1xVAaEjKrYJga4hMbYc2`

# Ps and Qs

factordb.com 完成 

噫，年老体衰也就算了，眼神也不好了，其实没有分解成功

然后开了个yafu，结果还是很不错的，我做出来了以后依然没有分解出答案

先是打开题目看了下，说是生成的素数`low entropy`(低熵)吗，然后打开论文大概看了下，就说生成素数有毛病，就猜测是不是共同素数的锅

结果还真是

{% asset_img 1524830782157.png Ps and Qs1 %}

先读两个公钥，然后gcd了一波，成功

```python
# python
def gcd(a, b):
	if b == 0:
		return a
	else:
		return gcd(b, a%b)

#q = gcd(3367646059138877442579820972831876412006279917097809082279412851693123955964282545145500497393579598954859534731890460229194372339215098506788375050698427369, 9055404640500300109405801152935663267176218320785348541566663982172162265778445107320065187449062375525002632043722734566593185461999286625234528036605141)
#print(q)
```

然后懒得写求拓展欧几里得求逆元了，直接拿工具了

{% asset_img 1524830884875.png Ps and Qs2 %}

补全代码

```python
# python
def gcd(a, b):
	if b == 0:
		return a
	else:
		return gcd(b, a%b)

#q = gcd(3367646059138877442579820972831876412006279917097809082279412851693123955964282545145500497393579598954859534731890460229194372339215098506788375050698427369, 9055404640500300109405801152935663267176218320785348541566663982172162265778445107320065187449062375525002632043722734566593185461999286625234528036605141)
#print(q)

#print(9055404640500300109405801152935663267176218320785348541566663982172162265778445107320065187449062375525002632043722734566593185461999286625234528036605141//q)

n = 9055404640500300109405801152935663267176218320785348541566663982172162265778445107320065187449062375525002632043722734566593185461999286625234528036605141

d = 5595429548525262877923879998920954875376781621351655927785581040054379710925982991727871918848397312612397095534700830662119225899345003601005897300178945

c = 0xf5ed9da29d8d260f22657e091f34eb930bc42f26f1e023f863ba13bee39071d1ea988ca62b9ad59d4f234fa7d682e22ce3194bbe5b801df3bd976db06b944da

print(hex(pow(c,d,n))[2:])

# f6a1df363229c6ec
```



# Affine Cipher

暴力一下肉眼看吧

是的，暴力以后搜索THE，出现次数最多的那个基本就确定了

不过得感叹一下 ……自己越来越蠢了……求个逆元，不是a*b%c==1则mod c下ab互为逆元么，我™a的b次方是要闹哪样……

```python
# python
plain = "BOHHIKBI,OZ,REI,WZRIKZIR,EX.,BOHI,RO,KISU,XSHO.R,ICBSG.WYISU,OZ, WZXZBWXS,WZ.RWRGRWOZ.,.IKYWZP,X.RKG.RIT,REWKT,DXKRWI.,RO,DKOBI..,ISIBRKOZWB,DXUHIZR.F,NEWSI,REI,.U.RIH,NOKA.,NISS,IZOGPE, OKHO.R,RKXZ.XBRWOZ.Q,WR,.RWSS,.G  IK., KOH,REI,WZEIKIZR,NIXAZI..I.,O ,REI,RKG.R,MX.IT,HOTISF,BOHDSIRISU,ZOZKIYIK.WMSI,RKXZ.XBRWOZ.,XKI,ZOR,KIXSSU,DO..WMSIQ,.WZBI, WZXZBWXS,WZ.RWRGRWOZ.,BXZZORXYOWT,HITWXRWZP,TW.DGRI.F,REI,BO.R,O ,HITWXRWOZ,WZBKIX.I.,RKXZ.XBRWOZ,BO.R.Q,SWHWRWZP,REIHWZWHGH,DKXBRWBXS,RKXZ.XBRWOZ,.WJI,XZT,BGRRWZP,O  ,REI,DO..WMWSWRU, OK,.HXSS,BX.GXS,RKXZ.XBRWOZ.QXZT,REIKI,W.,X,MKOXTIK,BO.R,WZ,REI,SO..,O ,XMWSWRU,RO,HXAI,ZOZKIYIK.WMSI,DXUHIZR., OK,ZOZKIYIK.WMSI.IKYWBI.F,NWRE,REI,DO..WMWSWRU,O ,KIYIK.XSQ,REI,ZIIT, OK,RKG.R,.DKIXT.F,HIKBEXZR.,HG.RMI,NXKU,O ,REIWK,BG.ROHIK.Q,EX..SWZP,REIH, OK,HOKI,WZ OKHXRWOZ,REXZ,REIU,NOGST,OREIKNW.I,ZIITF,X,BIKRXWZ,DIKBIZRXPI,O , KXGT,W.,XBBIDRIT,X.,GZXYOWTXMSIF,REI.I,BO.R.,XZT,DXUHIZR,GZBIKRXWZRWI.BXZ,MI,XYOWTIT,WZ,DIK.OZ,MU,G.WZP,DEU.WBXS,BGKKIZBUQ,MGR,ZO,HIBEXZW.H,ICW.R.,RO,HXAI,DXUHIZR.OYIK,X,BOHHGZWBXRWOZ.,BEXZZIS,NWREOGR,X,RKG.RIT,DXKRUF"
mp = "ABCDEFGHIJKLMNOPQRSTUVWXYZ ,."
dict = {}
inv_dict = {}
for i in range(0, len(mp)):
    dict[i] = mp[i]
    dict[mp[i]] = i

# print(dict)

for i in range(1, 29):
    for j in range(1, 29):
        if i*j%29 == 1:
            inv_dict[i] = j
            break

print(inv_dict)

newarr = []

for i in range(0, len(plain)):
    newarr.append(dict[plain[i]])

# print(newarr)


def decry(c, a, b):
    s = ""
    for i in range(0, len(c)):
        s += dict[(c[i]-b+29)*inv_dict[a]%29]

    return s


for i in range(1, 29):
    for j in range(0, 29):
        print(decry(newarr, i, j))
        print(i, j)

for j in range(0, 29):
    print(decry(newarr, 0, j))
    print(0, j)

# plain = "RLZZC SCIZJB"
#
# newarr = []
#
# for i in range(0, len(plain)):
#     newarr.append(dict[plain[i]])
#
# print(decry(newarr, 2, 3))
```

```python
λ python
Python 3.6.1 on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib
>>> plain = "COMMERCE ON THE INTERNET HAS COME TO RELY ALMOST EXCLUSIVELY ON FINANCIAL INSTITUTIONS SERVING ASTRUSTED THIRD PARTIES TO PROCESS ELECTRONIC PAYMENTS. WHILE THE SYSTEM WORKS WELL ENOUGH FORMOST TRANSACTIONS, IT STILL SUFFERS FROM THE INHERENT WEAKNESSES OF THE TRUST BASED MODEL. COMPLETELY NONREVERSIBLE TRANSACTIONS ARE NOT REALLY POSSIBLE, SINCE FINANCIAL INSTITUTIONS CANNOTAVOID MEDIATING DISPUTES. THE COST OF MEDIATION INCREASES TRANSACTION COSTS, LIMITING THEMINIMUM PRACTICAL TRANSACTION SIZE AND CUTTING OFF THE POSSIBILITY FOR SMALL CASUAL TRANSACTIONS,AND THERE IS A BROADER COST IN THE LOSS OF ABILITY TO MAKE NONREVERSIBLE PAYMENTS FOR NONREVERSIBLESERVICES. WITH THE POSSIBILITY OF REVERSAL, THE NEED FOR TRUST SPREADS. MERCHANTS MUSTBE WARY OF THEIR CUSTOMERS, HASSLING THEM FOR MORE INFORMATION THAN THEY WOULD OTHERWISE NEED. A CERTAIN PERCENTAGE OF FRAUD IS ACCEPTED AS UNAVOIDABLE. THESE COSTS AND PAYMENT UNCERTAINTIESCAN BE AVOIDED IN PERSON BY USING PHYSICAL CURRENCY, BUT NO MECHANISM EXISTS TO MAKE PAYMENTSOVER A COMMUNICATIONS CHANNEL WITHOUT A TRUSTED PARTY."
>>> hashlib.md5(plain.encode()).hexdigest()
'880cabd53df2f03050a7214d3ae30a07'
```



# Cut and Paste Attack On AES-ECB

截取一哈就行

ECB的问题就在于同样的明文总是对应相同的密文，而AES又是分组加密，所以我们只需要16字节截一段，然后从截出来的字段里拼出`Deposit amount: One million dollars`就吼啦

{% asset_img 1524833328837.png Cut and Paste Attack On AES-ECB %}

然后把对应的密文组合提交就好啦

# Rail Fence

栅栏密码

不是栅栏密码，丢人

就是这个名字，密码是17

然而提交又失败……是不是要加空格？？？

---

和凯撒那题一样的问题

{% asset_img 1525618734518.png Rail Fence %}

# Factoring RSA With CRT Optimization

看看论文

---

论文结论放得挺快的，喜欢

```python
>>> e = 0x10001
>>> n = 0x90def3c2c91ae9bf6089ec8857960d567fdbcd7c2c3ea713046977231e65f44e1b91550971d4e5d43b51675fae4ba640add3af02dad4bf68c3ddef3a98907e1e01156de7a4474d9fce2ba8c055f44673c703a72a111a06f8a7b2fe582463938d802e91630e1e1b5483b1774e608eb4368c6bbf4da375319d9a2799bf8a5ae453
>>> x = 0xdeadc0de
>>> y = 0x17d7f90a4597fb2bbbb41d1a70f505f0d8c5cb53faaafea259150eb6910fb08fbf1ba40e42de70c596fb0032d132c9c6ce46c650999ad5f14a990d205984260146e2949b819dc8732beceed452701d88b2c8723b410fce739009df89930424c566af5102403981c26c3e75d9c62065a347e815b26984dcd3b5f02fc8a8092051
>>> def gcd(a,b):
...     if b == 0:
...             return a
...     else:
...             return gcd(b,a%b)
...
>>> gcd(pow(y,e,n)-x,n)
9966524937284363425885222065976539626835093420150865305435384403545526100948308275937186399398105835105908301433416877915451315435074364352622884631548129
>>> p = gcd(pow(y,e,n)-x,n)
>>> q = n//p
>>> n%p
0
>>> print(p)
9966524937284363425885222065976539626835093420150865305435384403545526100948308275937186399398105835105908301433416877915451315435074364352622884631548129
>>> print(q)
10207350221528968021374239537567355734439270587735020516681487302155761468885207397982418037573103389660473080952277752719230542349569271803978030721660851
```



{% asset_img 1525631225843.png Factoring RSA With CRT Optimization %}

# Easy Passwords

暴力/去他们推特看看

> Hint for 1: the "easy" passwords are all standard English words

很惭愧，原本没思路的，又学习了一下别人怎么用John the Ripper的

```bash
root@kali:~# touch crackit
root@kali:~# john --wordlist=/usr/share/dict/words crackit 
Created directory: /root/.john
Warning: detected hash type "md5crypt", but the string is also recognized as "aix-smd5"
Use the "--format=aix-smd5" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 10 password hashes with no different salts (md5crypt, crypt(3) $1$ [MD5 128/128 SSE2 4x3])
Press 'q' or Ctrl-C to abort, almost any other key for status
each             (?)
in               (?)
letter           (?)
list             (?)
of               (?)
order            (?)
second           (?)
the              (?)
this             (?)
word             (?)
10g 0:00:00:04 DONE (2018-05-07 07:53) 2.421g/s 24502p/s 24502c/s 176147C/s woolly..word's
Warning: passwords printed above might not be all those cracked

# 这里直接看是不知道交什么的，id0-rsa好像都是这样做的
root@kali:~# john --show crackit
?:the
?:second
?:letter
?:of
?:each
?:word
?:in
?:this
?:list
?:in
?:order

11 password hashes cracked, 0 left
```



# RSA Modulus Factorization

有公钥、私钥，分解N

有人提示

> You may want to look at end of section 1 of [this paper](https://crypto.stanford.edu/~dabo/papers/RSA-survey.pdf) to get an idea of how to tackle this.

但是懒人选择直接用工具

```python
λ python rsatool.py -f PEM key.pem -n 2575379069244619122811590270693932708961682118615086945187580561953482470842235 5391264253236312033342930767629026955594380914791828872789640882880456756317355718989951748134910495292317107822354638062542536621806610548119524360577494257082085388892230653871347635789064083634194556923152378748383913055689941464591534636606264605505256200032003073128045380287795929441895659897956729518691264242454587153279087011099643014705274937963488514996712634166563179373202236856266518026166326484283485368106496495400924862033175617707157820195751946135848383888076352630565287208026865192435711425148588227314034233872575399 -d 17105402368913594732268140576016021009260884834703468214454459966320267707293844817833310635571150638333054368126620841170740802501167335402566350357845884586982802288930597441429262542859551082213714762867946512961388972561696317770839625342858127315991460791480206023627821121730752214777828377914024156239192555174948311918712437146465305330571121417401154082076772433310857936602097318370602240544385942653856271345622469333448979309288295132145434251447399809135938564905989171217097472641153308592366164932415033656821923922166211242186853476413476873462490965233576571621275866183349806774845359965178419609761
Using (n, d) to initialise RSA instance

n =
cc0262c3764f6d1bfa485a88c0566a6d68cb89ac382fbbc577a4862bdbce111bb59960ea787f132e3fddb9c914d0d11d156dd433a2adab9084d48cf58f58b42804805966ecd318ad19218791e14e19a8d0cf3441e219e5e1395eb5dba1fba11d94321a34eb536c51f4c44c5987e74a467b5fe2eae8d2725d63f24feebfd9ca746de93b6fd74ed82fed7dbfc3a84b0a425f52503ab71908a13ac11a9d52211042290ae2886626f67935dc78b7d86ba76d3c5e085a003dff06f914187acc368f904613cbd3d52f36c8b0535f9dfeec2737744ad8be51f66e1d470261a7fd7c6da277dbd2f213b44d9e8242a8a5b121acc8710baed3d244004e4ca560d58e756ba7

e = 65537 (0x10001)

d =
87803a2b0b44dbfa8e354a74b41371a2f3cce4c74f965cc85e9c1745c03bd15f2f320d8e0eb4907fd289a9a16642fff1aa4f0577ba6051a8aea12272e3600e60da0489dcf4058dc942fce337c0870841f956f6a59fd085c01f43c9d474755660f81283178d0a1ed31c98d9014a6414105657acb74c26a3316676062354a80a70335a670675f439dec08803def4892c4d99e20fbe1975d7673679ac6f16835307ce59971c865d71edeea9ed1a93c70a37e283f270dea8499271268d86f7fc4a1b4f64041833c62a057dfb0b48f1c4f6d351673238a3de3b4506eb2472aaf90b914e791c3a723464d9169d5eab8e0a3f8a42dedb065829e8b533a89dd3ba0a00a1

p =
cd8872868e62777d31e8cf8a694df3096bfa246463d4cfbda7f22f31d0b0f2d6d99996a099c79f312915018c475b2a5090f08ffa67400985dbbc8c04466ddf07414b40be264b98f8422ae053dfd53d4845eda2fda826cabb9237bb432f9e99862c6b75aeebc1b79e3a780fd813e05e73c9bbb521c05539e998031c876a2e7497

q = fe1a2971eee591830144ccb525befca29aa5a84c7b63510c60a491f09a185a787250bc5ea8c024896a4ec6312a8bf391fe76e15a50d7609cfb27ac18342b7dfd3d20ed94a85d62f83454ea77aa26471881aad95c63b0e890daabc71991f8051c2ec5956f8855a13e34c1e11e3a8d9abc43d813c6f9f42e34e0c75b35d2f15371
```
# Fast Hashing Passwords

母鸡

---

就是给了一个简单密码表，顺着跑个散列值表，然后排序，找出散列值最大最小的两个

有点考验电脑

Python写的排序那一步会有Memory Error（主要是ACM遗留下来的习惯，写算法写cpp比较习惯）

首先是下载rockyou list，算SHA-256值（其实这里没必要，但是我觉得可能会有用，所以算一个保存着了）

```python
import hashlib


def hash(strings):
    return str(hashlib.sha256(strings).hexdigest())


f = open(r"rockyou.txt", "rb")
w = open(r"rawsha256.txt", "w")

s = f.readline()[:-1]
arr = []
print(hash("".encode()))
i = 1
while s != "":
    w.write(hash(s)+'\n')
    arr.append(hash(s))
    s = f.readline()[:-1]

f.close()
w.close()
```

然后排序输出最低最高的位置

```c++
#include <bits/stdc++.h>
using namespace std;
int arr[14344391];
vector<string> st;
inline bool cmp(int a, int b){
	return st[a]<st[b];
}
int main()
{
	ios::sync_with_stdio(0); //加速输入输出用，其实为了效率应该考虑用scanf/printf + char*的组合，不过也能接受了
	freopen("rawsha256.txt", "r", stdin);
	string a;
	int pos=0;
	while(cin>>a){
		st.push_back(a);
		arr[pos]=pos;
		++pos;
	}
	sort(arr,arr+14344391,cmp);
	cout<<arr[0]<<endl<<arr[14344390]<<endl;
	return 0;
}

//2637658
//9682632
//--------------------------------
//Process exited after 67.14 seconds with return value 0
```

然后找到对应单词测试一下就行

```python
>>> import hashlib
>>> def hash(strings):
...     return str(hashlib.sha256(strings.encode()).hexdigest())
...
>>> print(hash("yame1bore"))
0000010e433cfc497373957df2ea9af41ec17edc43672c413558f62d09842190
>>> print(hash("bert7quinn,3"))
fffffdaa4034dadd8b4cf8a18f92a230a92423ee24ad6dfb24adb758a8487ca2
```



# Vigenère

多表，工具即可

https://f00l.de/hacking/vigenere.php

成功

`You already solved this one! Solution: BARLEY`

# Monoalphabetic Cipher

单表，工具解决

https://quipqiup.com/

成功

`You already solved this one! Solution: c21c7f2384dabd723c6d265b1315ddb5`

# Salt Alone Won't Save You

暴力？字典暴力？

---

是字典

先去hashcat了解了形式以后，写个脚本读

```python
import base64, binascii
f = open("crack.txt", "r")
s = f.readlines()
f.close()
f = open("crack.txt", "w")
for ss in s:
	ss = ss.split("$")[1:]
	ss[1] = ss[1][:-1]
	f.write(binascii.hexlify(base64.b64decode(ss[1])).decode('ascii')+':'+ss[0]+'\n')
f.close()
	
```

然后上hashcat

```bash
A:\Downloads\Compressed\hashcat-4.1.0
λ hashcat64.exe -m 1410 crack.txt rockyou.txt
hashcat (v4.1.0) starting...

* Device #1: WARNING! Kernel exec timeout is not disabled.
             This may cause "CL_OUT_OF_RESOURCES" or related errors.
             To disable the timeout, see: https://hashcat.net/q/timeoutpatch
* Device #2: Intel's OpenCL runtime (GPU only) is currently broken.
             We are waiting for updated OpenCL drivers from Intel.
             You can use --force to override, but do not report related errors.
nvmlDeviceGetFanSpeed(): Not Supported

OpenCL Platform #1: NVIDIA Corporation
======================================
* Device #1: GeForce GTX 965M, 512/2048 MB allocatable, 8MCU

OpenCL Platform #2: Intel(R) Corporation
========================================
* Device #2: Intel(R) HD Graphics 530, skipped.
* Device #3: Intel(R) Core(TM) i7-6700HQ CPU @ 2.60GHz, skipped.

Hashes: 7 digests; 7 unique digests, 7 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers:
* Zero-Byte
* Early-Skip
* Not-Iterated
* Raw-Hash

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimim salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

ATTENTION! Pure (unoptimized) OpenCL kernels selected.
This enables cracking passwords and salts > length 32 but for the price of drastically reduced performance.
If you want to switch to optimized OpenCL kernels, append -O to your commandline.

Watchdog: Temperature abort trigger set to 90c

Dictionary cache built:
* Filename..: rockyou.txt
* Passwords.: 14344390
* Bytes.....: 139921496
* Keyspace..: 14344383
* Runtime...: 1 sec

0603ae874b416862ad705b1c42f770141b1c802fa960a5b5aa91430f04c94400:kPD)T)=~1K{r:totoloco1990
6f2da1836ac6d7c40a93da4ca9afc56fdbe7279fcd12f479aa983d495772de73:4.9.mHSbiQ]^:phuck123
97ddc547c46af1af9821475c0b629d6a47bbfeb952535cc0afff72022459548d:b*.m,%~&<"^6:nc27360
e91ba9f0ff28267c4af7c6976bc1e119138b76fc2cf719a06b097baec8761391:(y3]<+9zmi4|:imberly1999
573fa001661fffc3c82aeec82f16951670dc342cc071eadc8bc71a8826209b66:{4[1m"WqdR0s:cowmen7
Cracking performance lower than expected?

* Append -O to the commandline.
  This lowers the maximum supported password- and salt-length (typically down to 32).

* Append -w 3 to the commandline.
  This can cause your screen to lag.

* Update your OpenCL runtime / driver the right way:
  https://hashcat.net/faq/wrongdriver

* Create more work items to make use of your parallelization power:
  https://hashcat.net/faq/morework

Approaching final keyspace - workload adjusted.


Session..........: hashcat
Status...........: Exhausted
Hash.Type........: sha256($pass.$salt)
Hash.Target......: crack.txt
Time.Started.....: Fri May 11 15:37:43 2018 (5 secs)
Time.Estimated...: Fri May 11 15:37:48 2018 (0 secs)
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.Dev.#1.....: 11989.9 kH/s (9.69ms) @ Accel:128 Loops:1 Thr:384 Vec:1
Recovered........: 5/7 (71.43%) Digests, 5/7 (71.43%) Salts
Progress.........: 100410681/100410681 (100.00%)
Rejected.........: 0/100410681 (0.00%)
Restore.Point....: 14344383/14344383 (100.00%)
Candidates.#1....: $HEX[3032313342] -> $HEX[042a0337c2a156616d6f732103]
HWMon.Dev.#1.....: Temp: 63c Util: 50% Core:1151MHz Mem:2505MHz Bus:16

Started: Fri May 11 15:37:31 2018
Stopped: Fri May 11 15:37:48 2018

```

排序输入得到答案

# CCA on Textbook RSA



RSA的选择密文攻击。

假设爱丽丝创建了密文 $C=P^emodn$并且把 $C$ 发送给鲍勃，同时假设我们要对爱丽丝加密后的任意密文解密，而不是只解密 $C$，那么我们可以拦截$ C$，并运用下列步骤求出 $P$：

1. 选择任意的 $X∈Z^∗_n$，即 $X$ 与 $N$ 互素
2. 计算 $Y=C×X^emod n$
3. 由于我们可以进行选择密文攻击，那么我们求得 $Y$ 对应的解密结果 $Z=Y^d$
4. 那么，由于 $Z=Y^d=(C×X^e)^d=C^dX=P^{ed}X=PXmodn$，由于 $X$ 与 $N$ 互素，我们很容易求得相应的逆元，进而可以得到 $P$

---

于是先读公钥

```bash
λ openssl rsa -pubin -in pubkey.pem -text -modulus
Public-Key: (2048 bit)
Modulus:
    00:a9:a9:60:8d:4b:d1:be:ee:e3:ce:11:51:cc:2d:
    1a:72:b8:76:df:63:e4:9f:70:aa:c8:da:e5:04:f5:
    91:7c:aa:f7:9f:5a:16:ae:5a:0b:8f:c2:e5:59:b6:
    46:8f:b4:9b:64:8d:02:96:6e:a2:30:03:f2:08:d7:
    56:b6:31:11:2d:86:ae:85:5e:04:82:e7:df:db:d5:
    e9:0d:06:d5:6b:58:12:36:db:9a:9b:d5:24:2f:2e:
    ca:82:b5:c2:f9:52:88:50:ad:be:78:b7:d1:83:e2:
    b1:ee:3b:2e:89:f2:45:33:9e:88:04:67:40:a0:d0:
    50:49:f5:76:a3:7a:ca:a9:45:0c:51:c7:7c:f5:b5:
    ba:85:e5:72:39:c2:a0:55:8e:eb:a1:f0:e6:06:7a:
    d3:b9:c6:50:08:7b:6b:35:ea:72:eb:8a:c6:8c:00:
    5d:4a:38:a4:c7:d8:d4:0c:b2:91:59:4a:83:21:ff:
    6c:08:78:81:2a:65:9a:83:59:f7:67:65:cd:55:97:
    15:96:ec:20:1a:30:63:8f:39:c5:23:08:5e:13:c7:
    55:9d:07:22:44:ea:70:df:84:f3:7b:a8:13:19:51:
    76:fc:62:c7:58:63:78:38:3f:5a:52:93:b0:3f:54:
    43:26:5d:f8:e7:50:5f:27:9f:e5:60:6d:36:bc:f1:
    47:f9
Exponent: 65537 (0x10001)
Modulus=A9A9608D4BD1BEEEE3CE1151CC2D1A72B876DF63E49F70AAC8DAE504F5917CAAF79F5A16AE5A0B8FC2E559B6468FB49B648D02966EA23003F208D756B631112D86AE855E0482E7DFDBD5E90D06D56B581236DB9A9BD5242F2ECA82B5C2F9528850ADBE78B7D183E2B1EE3B2E89F245339E88046740A0D05049F576A37ACAA9450C51C77CF5B5BA85E57239C2A0558EEBA1F0E6067AD3B9C650087B6B35EA72EB8AC68C005D4A38A4C7D8D40CB291594A8321FF6C0878812A659A8359F76765CD55971596EC201A30638F39C523085E13C7559D072244EA70DF84F37BA813195176FC62C7586378383F5A5293B03F5443265DF8E7505F279FE5606D36BCF147F9
writing RSA key
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqalgjUvRvu7jzhFRzC0a
crh232Pkn3CqyNrlBPWRfKr3n1oWrloLj8LlWbZGj7SbZI0Clm6iMAPyCNdWtjER
LYauhV4Eguff29XpDQbVa1gSNtuam9UkLy7KgrXC+VKIUK2+eLfRg+Kx7jsuifJF
M56IBGdAoNBQSfV2o3rKqUUMUcd89bW6heVyOcKgVY7rofDmBnrTucZQCHtrNepy
64rGjABdSjikx9jUDLKRWUqDIf9sCHiBKmWag1n3Z2XNVZcVluwgGjBjjznFIwhe
E8dVnQciROpw34Tze6gTGVF2/GLHWGN4OD9aUpOwP1RDJl3451BfJ5/lYG02vPFH
+QIDAQAB
-----END PUBLIC KEY-----
```

```python
λ python
Python 3.6.1 (v3.6.1:69c0db5, Mar 21 2017, 17:54:52) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> n =0xA9A9608D4BD1BEEEE3CE1151CC2D1A72B876DF63E49F70AAC8DAE504F5917CAAF79F5A16AE5A0B8FC2E559B6468FB49B648D02966EA23003F208D756B631112D86AE855E0482E7DFDBD5E90D06D56B581236DB9A9BD5242F2ECA82B5C2F9528850ADBE78B7D183E2B1EE3B2E89F245339E88046740A0D05049F576A37ACAA9450C51C77CF5B5BA85E57239C2A0558EEBA1F0E6067AD3B9C650087B6B35EA72EB8AC68C005D4A38A4C7D8D40CB291594A8321FF6C0878812A659A8359F76765CD55971596EC201A30638F39C523085E13C7559D072244EA70DF84F37BA813195176FC62C7586378383F5A5293B03F5443265DF8E7505F279FE5606D36BCF147F9
>>> x = 3
>>> c = 0x912fcd40a901aa4b7b60ec37ce6231bb87783b0bf36f824e51fe77e9580ce1adb5cf894410ff87684969795525a63e069ee962182f3ff876904193e5eb2f34b20cfa37ec7ae0e9391bec3e5aa657246bd80276c373798885e5a986649d27b9e04f1adf8e6218f3c805c341cb38092ab771677221f40b72b19c75ad312b6b95eafe2b2a30efe49eb0a5b19a75d0b31849535b717c41748a6edd921142cfa7efe692c9a776bb4ece811afbd5a1bbd82251b76e76088d91ed78bf328c6b608bbfd8cf1bdf388d4dfa4d4e034a54677a16e16521f7d0213a3500e91d6ad4ac294c7a01995e1128a5ac68bfc26304e13c60a6622c1bb6b54b57c8dcfa7651b81576fc
>>> d = pow(x,0x10001,n)*c%n
>>> hex(d)
'0x5eff6db089a86b058bbfd34f01cbe256fc9cc5b18c0999bd215048f8ca3de6f3250191417e502ea15aac5c7cf167c9be60944361163b13b96b1262dfc4bcbd695a61dcedd74192d9bfc5c0b9c8399cba3f6e6fc9f4adaf6d65b0594c696a32ab53913c06be6de9ee68b030b0aaef65bec0cbec7ad2439057cf30f7cd083138f98be11e93e54168acbc4bacb6b87b0a4a3d84e761f58e7605a0ae01b72c1344e96d7c5401be91bbe78a3527672c073c4c355ee340a1d2ea5d178c31c40561bccfb9ef47e0c984bd8227a187f492489b0292b506edf992950c009b59d2093b43f5601affa6b5b0b39a4b848c654ad242c55e62b358c03d91f889e7ab5e5d7e7f02'
```

提到网站去，得到`1395d5d50ae8d8e24572a393c628b3c4b335a30298b305d396e8b2a388e302d602a245d3c4e4a8e335990a88e5a302a5a30478de8244b36305690938b512d32`

求$3^{-1}modn$，最后计算得到明文`0x687474703a2f2f6172636869762e696e667365632e6574687a2e63682f656475636174696f6e2f667330382f73656373656d2f4d616e67657230312e706466`

# AES-CTR with Nonce Reuse

就是Two-Time-Pad攻击咯

---



# Bad Entropy

就是爆破咯

---

```python
from hashlib import md5
from Crypto.Cipher import AES
import binascii
c = binascii.unhexlify("a99210d796a1e37503febf65c329c1b2")
print(c)

i = 1453651200
while i <= 1454169600:
    s = md5(str(i).encode()).hexdigest()
    s = binascii.unhexlify(s)
    cipher = AES.new(s, AES.MODE_ECB)
    try:
        cipher.decrypt(c).decode('ascii')
        print(cipher.decrypt(c))
        a = input()
        if int(a) == 0:
            print(i)
            break
    except:
        pass
    i += 1


```

爆破成功，密钥时间是`1453862488`

# Rainbow Table Hash Chain

直觉告诉我hashcat可解

---

# Elliptic Curve Private Key Recovery

貌似是公钥选得腊鸡？但是我需要复（yu）习一下ECC

---

# ECDSA Nonce Recovery

同上

---

# Double Strength Affine

直觉告诉我，暴力依然可以解

---

# Slightly harder passwords

Twitter上有hint，直觉告诉我说hashcat可以解决

---

# Upgraded Affine

这个的CBC需要考虑一下子了

---

# Fvtavat Xrl Erpbirel

印象里是RSA，慢慢来把

---

# Insufficient Key Size

这题这么水还放这么后面……yafu/factordb秒解好吧

都懒得写WP了……看到这里还不会的可以退群了（明示【误

# Håstad's Broadcast Attack

RSA广播攻击诶，好嗦

---

# CBC Padding Attack

标题明示，我要慢慢学习去

# Breaking PDF Passwords



# Vigenère + Rail Fence

手动一个个试，然后得到密钥是`21，trader`



{% asset_img 1525698998368.png Vigenère + Rail Fence %}

# Recover the secret phone number

这题还是比较有意思的

首先很明显是一个base64

```python
λ python
Python 3.6.1 (v3.6.1:69c0db5, Mar 21 2017, 17:54:52) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> a = "H4sIAIq6o1cAAy2QS5OiMBSF9/yaIDrVLnrRQfJS000gAbILhCJAUMryMfrrB6tndRf3u+ecexR2nuLQNxF3eiU7u0IvitWtitimIcLrGEamFGdKxNmUx64hqq+xHyi++jZbd3JSQx1p3/QQBAbLBWCznRDQGbzocuws/ug0VkNz8g8br/fxKO51b8Ei+GwzCOon7JuTevNTHbFhmcBg/woo3t704qCWhM0kT80TurqHvir5rEv6TrpZmNDG7wP1fLNFsQ0p5m9uMsVmpNiDQ3feB4cMXA9ZGAtpWd7DVEl2zOSxy4DSvzuwz6WLLBCikPOhDmdmiB7yEQkhWSwlSqrSwsUUBtnkYF4yYxJdfOPHJc2VaPF2rkox1IPjlVep9VAJoJzuZtJOi8aKsQa7hEdfF0H4VO/4TxbZW6B2ycbsPMyIpy2+Si7/mpzwtUX6zndfoRmT1/4FYROKix05q4mjQvE/sbdeK9cLUN3b0X3LhIWBJLBKw5kuL5CjfDyKUnkl5cYixESCRCrp/n8VKB/TTiIV54nf0eSKit8arnH3+Rn8A9M9NCIXAgAA"
>>> import base64
>>> f = open("file","wb")
>>> f.write(base64.b64decode(a))
429
```

然后去鉴定去

```bash
root@kali:~# binwalk file

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             gzip compressed data, from Unix, last modified: 2016-08-04 21:58:34

```

接着解压

```bash
root@kali:~# gzip -d file
```

接着又是很明显的base64，解出来是个公钥

去分解一下，成功了

{% asset_img 1525700559472.png Recover the secret phone number %}

求得私钥解出号码


# Optimal Backpack Allocation


# Insecure PRNG


# Playfair


# CRIMEs against TLS


# Bleichenbacher's CCA2 on RSA


# Backdoored PRNG


# Not So Safe Primes


# DSA with LCG nonces