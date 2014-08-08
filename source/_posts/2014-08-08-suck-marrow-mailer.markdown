---
layout: post
title: "令人崩溃的 marrow.mailer 的 py2exe 打包历程"
date: 2014-08-08 16:10:51 +0800
comments: true
categories: python
tags: python py2exe marrowmailer
---
今天在打包一个小工具时, 由于使用 `marrow.mailer`, 碰到了经典的 `py2exe 找不到使用 pip 安装的包` 的问题.
于是使用了以前的大法: http://www.py2exe.org/index.cgi/ExeWithEggs
再加上又碰到的各种各种的问题. 虽然最后解决了, 但是过程太崩溃了, 不想多说, 只记下过程.

1\. 下载 `marrow.mailer` 压缩包, 解压, 用上面的大法安装
```
python setup.py install_lib
```

2\. 同上, 依次安装 `marrow.util`, `marrow.interface`, `futures`

3\. 程序里类似这样的

```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals

from marrow.mailer.manager import futures
from marrow.mailer.transport import smtp
from marrow.mailer import Mailer, Message

host = 'localhost'
mailer = Mailer({'manager.use': futures.FuturesManager,
                 'transport.use': smtp.SMTPTransport,
                 'transport.host': host,
                 })
mailer.start()
author = (('Sender', 'no-reply@example.com'))
msg = Message(author=author)
msg.subject = 'Subject'
msg.to = ['aaa@example.com']
msg.plain = 'plain mail'
msg.rich = 'rich html'

mailer.send(msg)
mailer.stop()

```

备注:

由于 `marrow.mailer` 的 bug: https://github.com/marrow/marrow.mailer/issues/36 , 导致 `manager.use`, `transport.use` 要写成上面的格式, 看了源码才发现.当然这种写法在不需要打包时是不需要的:

```python site-packages\marrow\mailer\__init__.py

# ...
if not isinstance(Manager, IManager):
    raise TypeError("Chosen manager does not conform to the manager API.")

# ...
if not isinstance(Transport, ITransport):
    raise TypeError("Chosen transport does not conform to the transport API.")

# ...

```
