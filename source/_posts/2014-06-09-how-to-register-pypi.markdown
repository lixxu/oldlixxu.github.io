---
layout: post
title: "在 PyPi 上发布软件包"
date: 2014-06-09 14:33:10 +0800
comments: true
categories: Python
tags: PyPi Python
---

距上次发布软件包过去了很久, 都忘了操作步骤了, 今天碰巧发布一个小包, 放狗搜了一下, 记下来备忘:

来源: <http://peterdowns.com/posts/first-time-with-pypi.html>

基本操作:

1\. 准备好 `setup.py`, 提供一下 `setup.cfg`

{% codeblock lang:ini setup.cfg %}
[metadata]
description-file = README.md
{% endcodeblock %}

2\. 注册:

{% codeblock %}
python setup.py register -r pypi
{% endcodeblock %}

按提示做选择就可以了:

{% img http://lixxu.qiniudn.com/pypi.png %}

3\. 上传, 一串字符滚过之后看到 `200` 就 OK 了

{% codeblock %}
python setup.py upload -r pypi
{% endcodeblock %}

{% img http://lixxu.qiniudn.com/pypi_upload.png %}
