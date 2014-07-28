---
layout: post
title: "使用 Python 设置系统时间"
date: 2014-06-22 16:53:43 +0800
comments: true
categories: Python
tags: Python
---
接上文: [使用 NSSM 安装 Windows 系统服务](/blog/2014/06/22/nssm-install-windows-service)

上一篇介绍使用了 `net time` 来同步系统时间, 后来突然想到如果没有服务器可供同步时间或电脑无法直接连接到服务器怎么办?

就去找了下使用 `Python` 设置时间的例子, 还真有: <http://stackoverflow.com/questions/12081310/python-module-to-change-system-date-and-time>

直接贴在这里:
```python set_time.py
import sys
import datetime

time_tuple = (2012, # Year
                 9, # Month
                 6, # Day
                 0, # Hour
                38, # Minute
                 0, # Second
                 0, # Millisecond
              )

def _win_set_time(time_tuple):
    import pywin32
    # http://timgolden.me.uk/pywin32-docs/win32api__SetSystemTime_meth.html
    # pywin32.SetSystemTime(year, month, dayOfWeek, day, hour, minute, second, millseconds)
    dayOfWeek = datetime.datetime(time_tuple).isocalendar()[2]
    pywin32.SetSystemTime(time_tuple[:2] + (dayOfWeek,) + time_tuple[2:])


def _linux_set_time(time_tuple):
    import ctypes
    import ctypes.util
    import time

    # /usr/include/linux/time.h:
    #
    # define CLOCK_REALTIME                     0
    CLOCK_REALTIME = 0

    # /usr/include/time.h
    #
    # struct timespec
    #  {
    #    __time_t tv_sec;            /* Seconds.  */
    #    long int tv_nsec;           /* Nanoseconds.  */
    #  };
    class timespec(ctypes.Structure):
        _fields_ = [("tv_sec", ctypes.c_long),
                    ("tv_nsec", ctypes.c_long)]

    librt = ctypes.CDLL(ctypes.util.find_library("rt"))

    ts = timespec()
    ts.tv_sec = int(time.mktime(datetime.datetime(*time_tuple[:6]).timetuple()))
    ts.tv_nsec = time_tuple[6] * 1000000 # Millisecond to nanosecond

    # http://linux.die.net/man/3/clock_settime
    librt.clock_settime(CLOCK_REALTIME, ctypes.byref(ts))


if sys.platform=='linux2':
    _linux_set_time(time_tuple)

elif  sys.platform=='win32':
    _win_set_time(time_tuple)

```
<!--more-->
可是没有可供同步用的服务器, 那时间怎么来呢?

1\. 跑一个 web, 可以返回一个 `json` 的数据, 如:
```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals
from datetime import datetime
from flask import Flask, jsonify

app = Flask(__name__)


@app.route('/api/get-time/')
def get_time():
    return jsonify(time=tuple(datetime.utcnow().timetuple()))

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')

```

客户端可以这样来获取和同步时间 (假设是 Windows 平台), 上面的 `set_time.py` 需要改一下:
```python set_time2.py
import sys
import datetime


def _win_set_time(time_tuple):
    '''time_tuple: 
    year, month, day, hour, minute, second, day_of_week, day_of_year, is_dist
    '''
    import pywin32
    # http://timgolden.me.uk/pywin32-docs/win32api__SetSystemTime_meth.html
    # pywin32.SetSystemTime(year, month, dayOfWeek, day, hour, minute, second, millseconds)
    tt = list(time_tuple)
    tt.insert(2, tt[6]) # add day_of_week 
    tt.insert(7, 0)  # set miliseconds
    pywin32.SetSystemTime(*tt[:8])


def _linux_set_time(time_tuple):
    import ctypes
    import ctypes.util
    import time

    # /usr/include/linux/time.h:
    #
    # define CLOCK_REALTIME                     0
    CLOCK_REALTIME = 0

    # /usr/include/time.h
    #
    # struct timespec
    #  {
    #    __time_t tv_sec;            /* Seconds.  */
    #    long int tv_nsec;           /* Nanoseconds.  */
    #  };
    class timespec(ctypes.Structure):
        _fields_ = [("tv_sec", ctypes.c_long),
                    ("tv_nsec", ctypes.c_long)]

    librt = ctypes.CDLL(ctypes.util.find_library("rt"))

    ts = timespec()
    ts.tv_sec = int(time.mktime(datetime.datetime(*time_tuple[:6]).timetuple()))
    ts.tv_nsec = time_tuple[6] * 1000000 # Millisecond to nanosecond

    # http://linux.die.net/man/3/clock_settime
    librt.clock_settime(CLOCK_REALTIME, ctypes.byref(ts))


def set_time(time_tuple):
    if sys.platform=='linux2':
        _linux_set_time(time_tuple)

    elif  sys.platform=='win32':
        _win_set_time(time_tuple)

```

```python sync_time.py
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals
import time
from datetime import datetime, timedelta
import requests
import set_time2

URL = 'http://a.b.c.d:5000/api/get-time/'
offset = time.timezone if (time.localtime().tm_isdst == 0) else time.altzone
LOCAL_TZ = offset / 60 / 60 * -1


def get_time():
    r = requests.get(URL)
    json = r.json()
    return json['time']


def sync_time(tz, ts):
    ts = get_time()
    to_time = datetime(*ts[:6]) + timedelta(hours=LOCAL_TZ)
    set_time2.set_time(to_time.timetuple())

if __name__ == '__main__':
    sync_time()

```

注意: 以上代码未经测试, 可能有错误.

2\. 如果无法直接连接到提供时间的服务器呢? 
比如有两个网络, 一个是测试电脑的内部网, 服务器在测试的通用网络.

可以使用一个可以连通两个网络的电脑上作为交换数据的中间人, 具体的是使用文件交换还是像 `socket` 之类的网络接口服务交换就随你的喜好了.

拙见.
<!--more-->
