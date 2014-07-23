---
layout: page
title: "About Me"
comments: false
sharing: false
footer: true
---
{% img http://www.gravatar.com/avatar/20d98692a314c016ab84b6304e78467c?s=200 %}
```python about_me.py
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals, print_function


def about_me():
    print("Hey there!")
    print("I'm Lix Xu, and I'm working for Jabil Shanghai TE now.")
    print("You can call me Lix and contact me via xuzenglin@gmail.com")
    print("I have some toys on Github (https://github.com/lixxu).")
    print("I usually use these tools for work: ", end='')
    print("[Python, Flask, MongoDB, PostgreSQL, redis, PySide, ...]")
    print("I love Web/GUI development.")

if __name__ == '__main__':
    about_me()

```
