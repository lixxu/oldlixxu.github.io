---
layout: post
title: "Windows下升级pip"
date: 2014-05-07 09:08:11 +0800
comments: true
categories: Python
tags: Python pip
---
在Windows下由于pip是个exe的可执行文件, 如果使用`pip install -U pip`的方式升级pip, 将会失败, 因为无法在执行pip时再覆盖它. 这时就需要使用导入包的方式:
`python -m pip install -U pip`

记录下备忘.

Linux下是没有这个问题的, 暂时没研究是什么原理.
