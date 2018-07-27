---
title: '*ctf magic_number WP'
connments: true
date: 2018-04-27 21:48:19
tags: 
	- CTF
	- PPC
categories: WriteUp
desc: starctf_magic_number
summary: 一道交互ACM题
---

99次内猜14个[0,1023]内的数，那肯定是不仅二分而且要整体二分了

简单说来是

<!--more-->

{% asset_img 1524837016603.png 伪代码 %}

于是可以写出代码

```python
#!/bin/python3

from pwn import *
r = remote("47.89.18.224", 10011)


nums = []


def query(l, r, rm):
    """
    Send the query and get answer
    :param l: <int> left bound
    :param r: <int> Right bound
    :param rm: <remote> Remote process
    :return: Amount of numbers
    """
    payload ="? " + str(l) + " " + str(r) + "\n"
    rm.send(payload)
    res = int(rm.recv())
    print payload + str(res)
    return res


def find(l, r, k, rm):
    """
    Guess numbers
    :param l: <int> Left bound
    :param r: <int> Right bound
    :param k: <int> Amount of numbers
    :param rm: <remote> Remote process
    :return: None
    """
    global nums
    if l == r - 1 and k == 1:
        nums.append(l)
        return None
    m = (l + r) // 2
    K = query(l, m, rm)
    if K:
        find(l, m, K, rm)
    if K - k:
        find(m, r, k - K, rm)


def gate(rm):
    """
    gate
    :param rm: <remote> Remote process
    :return: None
    """
    global nums
    rm.recvuntil("n = ")
    k = int(rm.recv())
    nums = []
    find(0, 1024, k, r)
    nums.sort()
    print(nums)
    rm.interactive()


gate(r)
gate(r)
gate(r)

# 这里gate(r)一次就过一关，多少关我也忘了
```

