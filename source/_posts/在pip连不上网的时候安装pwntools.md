---
title: 在pip连不上网的时候安装pwntools
connments: true
date: 2018-07-21 14:38:02
tags: 
	- PWN
categories: 
	- Notes
desc: 本文描述了这样一种特殊情况下安装pwntools的解决方案：有方法联网，但是pip连不上网。 
summary: 手动下载安装
---

本文描述了这样一种特殊情况下安装pwntools的解决方案：有方法联网，但是pip连不上网。 

<!--more-->

解决方法很简单，就是将pwntools和其依赖包下载下来，然后手动安装。选择对应版本，有whl直接下载whl，没有就下载源代码用命令python setup install安装，安装的时候可能会遇到有预先依赖，那就先安装依赖的包就行。 

下图是我在2018年7月22日下载的一套，有需要的度盘链接：https://pan.baidu.com/s/1efC82WX_TdAMoS7aVBFi1w 密码：lnv8

{% asset_img 1532396878037.png 压缩包 %}

安装效果如下图

{% asset_img 1532397057219.png 安装效果 %}