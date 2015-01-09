---
layout: post
title: "小米1/1S刷魔趣4.4.4"
date: 2015-01-09 19:30:01 +0800
comments: true
categories: android
tags: mione xiaomi mokee
---
小米1已经是很老的机器了, 以至于 `MIUI` 都放弃了它 (最新系统是开发版 V5 4.12.5).
而民间的系统魔趣却有最新的 `Android 4.4.4` 可以刷 (<http://download.mokeedev.com/?device=mione_plus>), 于是没忍住, 今天查了下教程, 开刷, 这里记下步骤.

<font color="red">刷前确保电量充足</font>

主要参考: <http://bbs.xiaomi.cn/thread-9531742-1-1.html>

附上需要用到的文件:<br />
1\. 小米1驱动<br />
    <http://lixxu.qiniudn.com/miDriver.rar>

2\. 双系统recovery文件<br />
    <http://lixxu.qiniudn.com/ClockWorkModDualSystem_mione_plus_6.0.3.3.zip>
    <br />参考: <http://bbs.xiaomi.cn/thread-9531742-1-1.html>

3\. 线刷recovery文件<br />
    <http://lixxu.qiniudn.com/mione_recovery_upgrade.7z>

4\. 魔趣ROM<br />
    <http://lixxu.qiniudn.com/MK44.4-mione_plus-141223-RELEASE.zip>
    <br />或到官网下载最新
    <http://download.mokeedev.com/?device=mione_plus>

<!--more-->
步骤:<br />
1\. 安装小米1驱动, 一定要确保adb驱动正常全装, 把魔趣的 ROM 和`ClockWorkModDualSystem_mione_plus_6.0.3.3.zip` 放入手机根文件夹
    {% img http://lixxu.qiniudn.com/mione_adb.jpg %}

2\. 刷入第三方recovery<br />
    直接用MIUI的 `更新系统`, 选择安装包, 找到zip文件刷入即可, 刷完重启
    <br /><font color="red">注意: <br />这时候重启后进入的是系统2, 需要手动重启进新recovery, 新recovery里面有高级功能, 步骤如下: 高级 -- 调整分区大小 -- 500M</font>

3\. 重启或卸电池, 进fastboot线刷新recovery<br />
    <font color="red">(1) 网上说正常会重启, 重启后进入线刷模式. 但是个人经验一定会卡死, 所以显示刷机成功后重启, 卡死后大家卸电池, 再装回电池, 然后米1按组合键进入线刷模式 (电源键+米键+音量减), 也就是fastboot模式下.</font><br />
    <font color="red">(2) 再刷入新recovery, 用的是 `mi-one recovery upgrade.7z`, 解压, 执行 `shuaji.bat`, 成功后, 重启进recovery (电源键+音量加)</font>

4\. 刷入魔趣ROM<br />
    {% img http://lixxu.qiniudn.com/mi_rom.jpg %}
    <br />高级-> 打开双系统 -> 返回 -> 安装系统 -> 从SD卡选择系统 -> 找到魔趣ROM -> 开刷 (我的是系统2, 可能你的不同), 选择刷进系统2 -> 清空虚拟机缓存 -> 重启, 选择系统2. 大功告成

5\. 安装谷歌服务包<br />
    启动后, 到 `设置 -> 魔趣中心 -> 扩展组件`, 刷新, 下载 `谷歌服务包`
    <font color="red">注意:<br />下载完不要选安装, 因为会装进系统1, 需要手动进recovery 刷入 (下载的文件放在 `mkextras` 文件夹), 和刷ROM一个步骤, 不需要双清</font>

至此, 所有的步骤都完了, 进系统体验 `google服务`吧, `Gmail, G+` 就等你来折腾了!
<!--more-->
