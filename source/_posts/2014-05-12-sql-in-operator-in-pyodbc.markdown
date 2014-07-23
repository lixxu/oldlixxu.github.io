---
layout: post
title: "pyodbc in 查询"
date: 2014-05-12 11:37:58 +0800
comments: true
categories: Python
tags: pyodbc Python
---

参考链接, 看 `geographika` 的答案即可:

<http://stackoverflow.com/questions/4819356/sql-in-operator-using-pyodbc-and-sql-server>

记录一下如何在 `pyodbc` 中使用 `in` 查询:

```python
#!/usr/bin/env python

import pyodbc

alist = [1, 2, 3]
sql = 'select * from table where field1 in (%s)'
conn = pyodbc.connect(...)
cur = conn.cursor()
cur.execute(sql % (','.join('?' * len(alist))), alist)
cur.fetchall()
conn.close()
```
