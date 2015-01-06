---
layout: post
title: "Windows下彩色化终端文字"
date: 2014-01-15 08:48:35 +0800
comments: true
categories: Python
tags: Python ctypes
---

最近有在 Windows 命令行程序下打印彩色文字的需要, 放了半天狗, 试了几个包, 都不是很满意, 最后还是使用决定 `ctypes`.

先看下例子 (超过 15 好像就是背景色了):
{% img http://7ktor3.com1.z0.glb.clouddn.com/color_terminal.png %}
<!--more-->
具体用法(Python 2.7.6下):
```python color_terminal_test.py
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals, print_function
from ctypes import windll, c_ulong

HANDLER = windll.Kernel32.GetStdHandle(c_ulong(0xfffffff5))
COLOR_MAPS = dict(black=0, dark_blue=1, dark_green=2,
    dark_navy=3, dark_red=4, dark_purple=5,
    dark_yellow=6, dark_white=7, gray=8,
    blue=9, green=10, navy=11,
    red=12, purple=13, yellow=14,
    white=15, default=7,
    )


def set_color(color='default'):
    windll.Kernel32.SetConsoleTextAttribute(HANDLER,
        COLOR_MAPS.get(color, 7))


def show_text(text, color='default', new_line=True):
    set_color(color)
    print('{}{}'.format(text, '\n' if new_line else ''), end='')
    set_color()

show_text('yellow', color='yellow')
```

`COLOR_MAPS` 里多了一个 `default`, 这个就是默认终端下的文字颜色.
<!--more-->
