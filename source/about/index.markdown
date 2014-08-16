---
layout: page
title: "About me"
comments: false
sharing: false
footer: true
---
```python about_me.py
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals, print_function


def about_me():
    print("Hey there!")
    print("I'm Lix, and working for Jabil Shanghai TE now.")
    print("You can find me @ Github (https://github.com/lixxu).")
    print("I usually use these tools at work: ", end='')
    print("[Python, Flask, MongoDB, PostgreSQL, redis, PySide, ...]")
    print("And, I'm planning move to tornado in the future.")

if __name__ == '__main__':
    about_me()

```
