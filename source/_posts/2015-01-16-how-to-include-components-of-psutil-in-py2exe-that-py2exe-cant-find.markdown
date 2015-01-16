---
layout: post
title: "py2exe打包psutil"
date: 2015-01-16 18:18:35 +0800
comments: true
categories: Python
tags: psutil py2exe
---
记录备忘.

原因:<br />
```
Traceback (most recent call last):
  File "main.py", line 10, in <module>
  File "zipextimporter.pyo", line 82, in load_module
  File "utils.pyo", line 10, in <module>
  File "zipextimporter.pyo", line 82, in load_module
  File "psutil\__init__.pyo", line 138, in <module>
  File "zipextimporter.pyo", line 82, in load_module
  File "psutil\_pswindows.pyo", line 16, in <module>
  File "zipextimporter.pyo", line 98, in load_module
ImportError: MemoryLoadLibrary failed loading _psutil_windows.pyd
```

解决办法:<br />
1\. <http://stackoverflow.com/questions/20930173/how-to-include-components-of-psutil-in-py2exe-that-py2exe-cant-find><br />
    `dll_excludes` 至少添加这几项: `"IPHLPAPI.DLL", "NSI.dll",  "WINNSI.DLL",  "WTSAPI32.dll"`

2\. <http://sourceforge.net/p/py2exe/bugs/130/><br />
    `options` 里的 `bundle_files` 改成 `3`, 不过打包出来的文件会有很多个.
