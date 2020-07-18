---
title: Regex Golf Challenge
connments: true
date: 2019-03-18 21:23:34
tags: 
	- RegularExpression
	- WARGAME
categories: WriteUp
desc:
summary:
---

Interesting challenges for regular expression newbie like me~

TBC...

<!--more-->

# Classic

## Warmup

`foo`

## Anchors

`k$`

## It never ends

`u\b`

---

should work on PCRE: `u\Z`

## Ranges

`^[a-f]*$`

## Backrefs

I think [this](http://www.aihanyu.org/cncorpus/CpsTongji.aspx) is useful.

---

noooooooooo

We need back references.

`(...).*\1`

## Abba

This is hard for me ...

`^(?!.*(\w)(\w)\2\1)`

How does these guys use 14 character but not 19?

---

ok, `^(?!.*(.)(.)\2\1)` for 17

`^(?!.*(.)\1)|ef` for 15

`^(?!(.)+\1)|ef` for 14,,, WTF

## A man, a plan

It's Palindrome.

But it seems doesn't support PCRE,,,

`^(.)(.).*\2\1$` for 14. Not perfect answer.

`(.)[^p].*\1$` for 13, but not fun.

## Prime

WTF???

Wow, really interesting!!!

`^(?!(..+)\1+$)`

## Four

`(\w).\1.\1.\1` for 13. How to achieve 11?

Oh I got it:`(.)(.\1){3}`

## Order

`^[^o].{4,5}$` for 12

`^[^o]?.{5}$` for 11 or `^.{5}[^e]?$`

## Triples



## Powers

^(?!(.(..)+)\1*$)`