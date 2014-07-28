---
layout: post
title: "使用 NSSM 安装 Windows 系统服务"
date: 2014-06-22 16:15:55 +0800
comments: true
categories: NSSM
tags: NSSM
---
工厂里的某些测试电脑存在时间不对的问题, 最开始使用简单的批处理加上 
[nssm](http://nssm.cc/) 安装成 service 的方式, 比如下面的示例:

{% codeblock sync_time.bat %}
sync_time:
net time \\a.b.c.d /set /y

sleep 3600
goto sync_time

{% endcodeblock %}

但在使用的过程中出现了这个 service 吃掉许多 CPU 的问题. 
按道理说在 sleep 后应该会进入休眠状态, 不应该再占用 CPU.
不清楚什么原因, 就使用 `Python` 重写了.
<!--more-->
1\. 主程序 `main.py`

{% codeblock main.py lang:python %}
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals
import subprocess
import time
import click

__version__ = '0.1'
DEFAULT_SERVERS = 'server_a,server_b'
IP_MAPS = dict(server_a='a.b.c.d', server_b='a.b.c.e')
CMD = r'net time \\{} /set /y'


def run_cmd(server):
    status = subprocess.call(CMD.format(server))
    if status == 0:
        return True

    ip = IP_MAPS.get(server)
    if ip:
        return subprocess.call(CMD.format(ip), shell=True) == 0

    return False


@click.command()
@click.option('--servers', default=DEFAULT_SERVERS,
              help='servers name or IP list, seperated by ","')
@click.option('--sleep', default=600,
              help='sleep seconds till next run, default is 600s (10 minutes)')
def sync_time(servers, sleep):
    while 1:
        for server in servers.split(','):
            if run_cmd(server.strip()):
                break

        time.sleep(sleep)

if __name__ == '__main__':
    sync_time()

{% endcodeblock %}

2\. 使用 `py2exe` 或其他工具打包 `main.py` 为可执行文件, 比如 `SyncTimer.exe`

3\. 然后是安装/删除 service 的批处理:

3\.1 `install.bat`
{% codeblock install.bat %}
@echo off
set service=SyncTimer
echo %PROCESSOR_ARCHITECTURE% | find /i "x86" > nul
if %errorlevel%==0 (
    set nssm=nssm32.exe
) else (
    set nssm=nssm64.exe
)

%nssm% install "%service%" "%~dp0SyncTimer.exe"

net start %service%

sleep 5
{% endcodeblock %}

3\.2 `remove.bat`
{% codeblock remove.bat %}
@echo off
set exename=SyncTimer
net stop %exename%

echo %PROCESSOR_ARCHITECTURE% | find /i "x86" > nul
if %errorlevel%==0 (
    set nssm=nssm32.exe
) else (
    set nssm=nssm64.exe
)

%nssm% remove "%exename%" confirm
sleep 5
{% endcodeblock %}

当然了, 你需要到 <http://nssm.cc/download> 下载 `nssm` 的执行程序.
分别把 32 位和 64 位的执行文件和你的 EXE 文件放到一起, 并改名为 `nssm32.exe` 和 `nssm64.exe`. 在安装 service 后也不能删除这些文件, 因为 service 关联的程序是 `nssm`.

这里有个注意的地方:
`nssm` 的文档上说使用命令行安装时如果不想弹出确认对话框, 可以写成这样的:
```
%nssm% install "%service%" "%~dp0SyncTimer.exe" confirm
```
但是这样安装的 service 始终处于 `paused` 状态, 一度让我以为是程序里 `sleep` 导致的. 但在使用对话框安装时却一点问题都没有.
后来看了下系统日志, 发现是路径的问题. 后面就使用 exe 的绝对路径并且把后面的 `confirm` 给去掉了才算正常.

本来打算写如何使用 `Python` 设置系统时间的怎么变成 `nssm` 了? 
<!--more-->
