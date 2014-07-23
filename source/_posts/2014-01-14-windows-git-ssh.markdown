---
layout: post
title: "Windows下使用ssh连接github"
date: 2014-01-14 18:56:34 +0800
comments: true
categories: Github
tags: Windows&nbsp;ssh Github
---
在[上一篇](/blog/2014/01/14/first-post/)中大概介绍了初始环境的搭建, 不过发现在Windows下部署时出了点问题,
问了下狗, 找到解决方法了, 暂时记下来.

**注:** 我用的是 [SmartGitHg](http://www.syntevo.com/smartgithg/) 带的git程序.

<!--more-->
####1. 生成ssh key文件
键入文件名, 如 `id_rsa.github`
```
ssh-keygen -C your_email_address@email.com -t rsa
```

####2. 添加到github
到<https://github.com/settings/ssh>添加刚刚创建的key, 标题随便写, 把 `id_rsa.github.pub` 公钥里的内容复制粘贴到 `Key` 里面

####3. 复制密钥文件
复制 `id_rsa.github` 和 `id_rsa.github.pub` 到`[SmartGitHg folder]\git\.ssh`文件夹下

####4. 测试
运行`[SmartGitHg folder]\git\git-bash.bat` 或 `git-cmd.bat`进行测试
```
ssh -T git@github.com
```
如果出现`Hi yourname! You've successfully authenticated, but GitHub does not provide shell access.`就说明成功了. 以后就可以直接提交了

<font color="red">**注**: 不能直接使用 **cmd.exe** 进行测试和提交</font>
<!--more-->