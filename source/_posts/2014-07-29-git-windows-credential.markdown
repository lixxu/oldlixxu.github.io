---
layout: post
title: "Windows下git保存密码"
date: 2014-07-29 20:27:58 +0800
comments: true
categories: git
tags: git
---
更新: 直接保存密码是不行的, 需要安装 `git-credential-winstore`. 
安装后, 就可以手动添加密码了.

越来越觉得 `SourceTree` 启动慢, 我都尽量在命令行下使用 `git` 了 (这样才是正道吧).
每次的推送都要输入用户名和密码, 用了 `_netrc` 大法也不管用, 隐约记得之前可以的, 是人品越来越好了吗? 

总感觉不甘心, 就翻看 http://stackoverflow.com/questions/6031214/git-how-to-use-netrc-file-on-windows-to-save-user-and-password, 看到 `git-credential-winstore` 这里, 就进去看看, 下载安装, 很小巧的东西, 才十几K.

接着再推送时, 弹出对话框, 输入用户名和密码, 就可以了. 看这个窗口的架势, 是在系统的密码管理里面, 进去一看, 果不其然, 在通用安全里, 静静地躺着刚刚保存的密码.
没有安装 `git-credential-winstore` (https://gitcredentialstore.codeplex.com/) 的同学可以手动添加试试, 此法仅限于 `http` 方式的验证, 不适合 `ssh` 模式.

{% img http://lixxu.qiniudn.com/windows_git_save_password.png %}

顺便换上了 `度娘` 的CDN, 还用上了雅黑字体:
```
<script src="http://libs.baidu.com/jquery/1.9.1/jquery.min.js"></script>

```

还有 `渣浪` 和 `小斗士` 的:
```
<script src="http://lib.sinaapp.com/js/jquery/1.9.1/jquery-1.9.1.min.js"></script>

<script src="http://libs.useso.com/js/jquery/1.9.1/jquery.min.js"></script>

```
