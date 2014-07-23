---
layout: post
title: "reload flask with gevent"
date: 2014-02-24 13:06:22 +0800
comments: true
categories: Python
tags: gevent flask Python
---
越来越感觉 Windows 下 `flask` 自带的 debug server 慢了, 于是就开始寻找替换工具,
先是看了 `gunicorn`, 但是在 Windows 下有问题. 再接着试了 `gevent`, 可以跑起来,
但是有个问题, 丰富的 debug 信息看不到了, 只提示 `internal server error`. 
这显然不行, 放狗一搜, 下面就是结果:

```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-

import werkzeug.serving
from werkzeug.debug import DebuggedApplication
from gevent.wsgi import WSGIServer
from myapp import app


@werkzeug.serving.run_with_reloader
def run():
    app.debug = True
    app = DebuggedApplication(app, evalex=True)
    ws = WSGIServer(('', 5000), app)
    ws.serve_forever()

if __name__ == '__main__':
    run()
```
