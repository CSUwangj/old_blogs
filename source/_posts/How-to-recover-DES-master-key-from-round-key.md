---
title: How to recover DES master key from round key
connments: true
date: 2019-11-09 22:13:16
tags: 
	- CTF
	- Cryptography
	- fuck
categories: Notes
desc:
summary:
---

# TLDR

Reverse the picking(or called compressing) process where extract round key from part of master key to get most of master key(56bits), use parity to recover last 8bits.

If the dumbass/fuckhead/shithead who created this question forgot or not known parity, just fuck it.

<!--more-->

# We Got the Key!

I came across this problem when I participated in one shity CTF.

Here goes problem:

```python
import pyDes
import base64
deskey = "********"
DES = pyDes.des(deskey)
DES.setMode('ECB')
DES.Kn = [
			[1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0],
			[1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0], 
			[0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0],
			[1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1], 
			[0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1],
			[0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0],
			[0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0],
			[0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 0],
			[0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0],
			[0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0],
			[0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1],
			[0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0],
			[1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 0, 0],
			[1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1],
			[1, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1],
			[1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1]
		]
#cipher_list = base64.b64encode(DES.encrypt(mes))
cipher_list= "gAN5RT1XWKI0OyUayZj35SlKQ+if2PAJ"
#flag = mes+deskey
```

I checked [pyDes](https://github.com/twhiteman/pyDes/blob/master/pyDes.py) and found it a normal implementation, so problem is easy, add one more line to decrypt the cipher, recover master key from round keys.

So we don't have to care about how DES encrypt data, but how these round key are generated.

# DES key schedule

## drop parity

Firstly, we have 64 bits key, every byte should have odd parity.

So we would let key go through permuted choice one, where we drop 8th bit of every byte.

```python
key = [0] * 64 # 64bit key
pc1 = [
  56, 48, 40, 32, 24, 16,  8,
  0, 57, 49, 41, 33, 25, 17,
  9,  1, 58, 50, 42, 34, 26,
 18, 10,  2, 59, 51, 43, 35,
 62, 54, 46, 38, 30, 22, 14,
  6, 61, 53, 45, 37, 29, 21,
 13,  5, 60, 52, 44, 36, 28,
 20, 12,  4, 27, 19, 11,  3
]
mid_key = list(map(lambda x: key[x], pc1))
```

## derive round key

We chopped mid key into two even part, every round left shift it and let it go through permuted choice two, which compress 56 bit into 48 bit.

```python
left_rotations = [
    1, 1, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 1
]
pc2 = [
	13, 16, 10, 23,  0,  4,
	 2, 27, 14,  5, 20,  9,
	22, 18, 11,  3, 25,  7,
	15,  6, 26, 19, 12,  1,
	40, 51, 30, 36, 46, 54,
	29, 39, 50, 44, 32, 47,
	43, 48, 38, 55, 33, 52,
	45, 41, 49, 35, 28, 31
]

L = mid_key[:28]
R = mid_key[28:]

round_key = []

for i in range(len(left_rotation)):
    for j in range(left_rotation[i]):
        L = L[1:] + L[:1]
        R = R[1:] + R[:1]
    round_key.append(list(map(lambda x: (L+R)[x], pc2)))
```

# Reverse it

We could get only 48 out of 56 bits, does it mean that we need to use brute force to get last 8 bits?

Off course not! We got 16 round keys, Which means we actually got every bit of 56-bit-key.

We just need rerun the key assign process, to a opposite direction.

```python
# Extract part of master key from round key
mid_key = [-1] * 56 # -1 for key not appeared
for i in range(len(left_rotation)):
  for j in range(left_rotation[i]):
    mid_key = mid_key[1:28] + mid_key[:1] + mid_key[29:] + mid_key[28:29]
  for k in range(len(pc2)):
    if mid_key[pc2[k]] != -1 and mid_key[pc2[k]] != round_key[i][k]:
      print("This should not happen!")
    mid_key[pc2[k]] = round_key[i][k]

# Reverse permute process of dropping parity

key = [-1] * 64
for i in range(len(pc1)):
  key[pc1[i]] = mid_key[i]

# Recover master key according to parity
for i in range(8):
  sum = 0
  for j in range(7):
    sum += key[i*8+j]
  key[i*8+7] = 1 - sum % 2

# Print key ascii
print(''.join([chr(int(key[i:i+8],2)) for i in range(0, len(key), 8)]))
```

