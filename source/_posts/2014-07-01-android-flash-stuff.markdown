---
layout: post
title: "Android刷机相关"
date: 2014-07-01 18:31:16 +0800
comments: true
categories: Android
tags: Android Fastboot Recovery Mokee
---
记录下来备忘:

背景:

```
1. Nexus 4

2. Mokee 4.4.4

3. TWRP
```

我保存的基带共享链接: http://pan.baidu.com/s/1gdf4pfd

原帖看这里: http://bbs.mfunz.com/thread-947343-1-1.html

1\. 刷基带, 按照帖子来就可以, 简单就是
```
1. 进入fastboot
2. flash-all.bat
```

2\. 更新TWRP http://teamw.in/project/twrp2/129

```
Download - Recovery Image Method:

Download the newest .img file here

Download the above file.  Turn off your device. Turn on the device and keep
holding volume down until a menu shows up. The device will now be in fastboot
mode. Plug the device into your computer.  If you have the right drivers
installed, your screen should now say FASTBOOT USB.  Run the following command
via the command line:

fastboot flash recovery recoveryfilename.img

Note that you will need to change the last part to match the name of the file
that you just downloaded.  You will also need adb and fastboot for your computer.
```

简单讲就是: 
```
1. 下载.img文件
2. 进入fastboot
3. 运行 fastboot flash recovery openrecovery-twrp-2.7.1.0-mako.img

注意:
1. 这里的 openrecovery-twrp-2.7.1.0-mako.img 要换成你对应的名字
2. fastboot 实际是 fastboot.exe
```

补: 如何进 `fastboot`?

```
1. 关机
2. 电源 + 音量-, 直到出现菜单
```

以上.
