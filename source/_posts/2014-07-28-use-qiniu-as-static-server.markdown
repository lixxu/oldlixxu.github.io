---
layout: post
title: "使用七牛作为博客静态文件存放地"
date: 2014-07-28 22:53:29 +0800
comments: true
categories: Octopress
tags: Octopress qiniu
---
之前的图片放在 `images` 目录下, 不过随着博客的文章会越来越多, 将导致整个库变得越来越大, 而这些静态文件是没必要放在库里的. 

碰巧在网上看到一篇提问说" 博客的静态文件都放在哪里托管? ", 回答有各种, 也有 `七牛`, 而我对七牛算是有点了解, 知道这是一家使用 `go` 语言作为后台的云存储公司. 又在上海GDG组织的一次活动上听过创始人许式伟的演讲, 所以对七牛比较熟悉, 并且每月有10G的免费流量可以用, 个人用的话应该足够了, 就决定使用七牛放放静态文件.

好了, 不废话了.

1\. 首先到官网注册账号 (推荐绑定手机号, 可以有更多的免费资源可用): https://portal.qiniu.com/signup
<!--more-->
{% img http://lixxu.qiniudn.com/qiniu_signup.png %}

2\. 创建存储空间, 博客用的话就创建一个公开空间

{% img http://lixxu.qiniudn.com/qiniu_add_space.png %}

3\. 简单设置一下

{% img http://lixxu.qiniudn.com/qiniu_basic_setting.png %}

{% img http://lixxu.qiniudn.com/qiniu_advanced_setting.png %}

4\. 把密钥记录下来, 待会的上传脚本需要用到

{% img http://lixxu.qiniudn.com/qiniu_key.png %}

5\. 到这里下载客户端工具 (http://developer.qiniu.com/docs/v6/tools/qrsync.html), 有命令行和图形两种, 参照右面的说明就可以了.

需要注意的是配置文件里 `dest` 的部分不要像文档里的那样分行显得好看, 全部合并到一行里, 否则会出错. 把红框里的换成你自己的目录, 密钥和空间名.

更多详情可以参考官方文档, 比如本地文件上传后就可以删除了, 如果需要同步删除的应该怎么做等

{% img http://lixxu.qiniudn.com/qiniu_rsync_config.png %}

6\. 创建一个批处理文件, 方便上传, 如 `qiniu_pub.bat`

{% img http://lixxu.qiniudn.com/qiniu_rsync_bat.png %}

7\. 以后有需要上传的文件, 就丢到你定义好的 `public` 文件夹里, 再运行 `qiniu_pub.bat` 就可以了, 已经同步的文件如果没有修改就不会同步的

{% img http://lixxu.qiniudn.com/qiniu_upload.png %}

附我的七牛上传的目录结构, 仅供参考:

{% img http://lixxu.qiniudn.com/qiniu_folders.png %}

好了, 码完收工, 早睡早起.
<!--more-->
