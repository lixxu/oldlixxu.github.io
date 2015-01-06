---
layout: post
title: "抓取 <<码农故事>> 文本"
date: 2014-07-25 08:46:39 +0800
comments: true
categories: Python
tags: Python requests bs4 BeautifulSoup
---
偶然看到这样一篇关于码农的小说, 感觉还可以, 就想收集下来慢慢看.
但是都是网页而且还挺多, 就看看有没有整理好的全本, 搜了一下没找到.

接着就手动复制粘贴, 贴了几篇感觉太枯燥, 就琢磨着如何使用 `requests` 和 `BeautifulSoup` 来提取文本.

很少使用 `BeautifulSoup`, 所以临时去看了一下文档, 开始走起.

1\. 先用浏览器的 `审查元素` 获取分页的页面结构, 这里我们只需要 `标题` 和 `链接地址` 即可

{% img http://7ktor3.com1.z0.glb.clouddn.com/coder_page.png %}

<!--more-->

2\. 分析页面内容抓取 `标题` 和 `链接地址`, 还一并抓取当前的章节, 否则就没法知道顺序了

3\. 根据获取的链接抓取全文

4\. 分析全文, 在 `entry` 下面的 `p` 和 `span` 里, 一开始我以为都在 `p` 结构里, 结果最后发现有的章节是空的, 看了下空的章节的结构, 原来用的是 `<div><span>...</span></div>` 的结构

{% img http://7ktor3.com1.z0.glb.clouddn.com/coder_post.png %}

{% img http://7ktor3.com1.z0.glb.clouddn.com/coder_post2.png %}

5\. 这些都确定了后就可以开工了, 当然我是逐步确定的. 我的流程是: 按章节写到单独的文件里, 最后再合并. 好了, `Talk is cheap, show me the code.`:

```python coder_story.py
#!/usr/bin/env python
#-*- coding: utf-8 -*-

import os
import os.path
import requests
from bs4 import BeautifulSoup
import re

digit_re = re.compile('(\d+-?\d*)')
url = r'http://blog.jobbole.com/tag/%E7%A0%81%E5%86%9C%E6%95%85%E4%BA%8B/page/{}/'
if not os.path.exists('coder_story'):
    os.makedirs('coder_story')

os.chdir('coder_story')
for page in (1, 2, 3):
    r = requests.get(url.format(page))
    # soup = BeautifulSoup(r.content)  # lxml未安装时用这行
    soup = BeautifulSoup(r.content, 'lxml')
    for meta_title in soup.find_all('a', class_='meta-title'):
        # 我们要的都必须有title
        if 'title' not in meta_title.attrs:
            continue

        # 不是这个小说不要
        if u'老码农原创小说' not in meta_title['title']:
            continue

        print(meta_title)  # 打印一下标题
        href = meta_title.get('href')  # 获取文章链接
        # 获取章节序号
        found = digit_re.findall(meta_title['title'])
        if not found:
            continue

        # 个位数章节前补0, 为了后面的合并
        page_no = found[0]
        if int(page_no.split('-')[0]) < 10:
            page_no = '0' + page_no

        # 获取文章的内容
        r2 = requests.get(href)
        # s2 = BeautifulSoup(r2.content)  # lxml未安装时用这行
        s2 = BeautifulSoup(r2.content, 'lxml')
        entry = s2.find('div', class_='entry')
        with open('{}.txt'.format(page_no), 'w') as f:
            # 内容在 <p>...</p> 或 <div><span>...</span></div>里
            for p in entry.find_all(['p', 'span']):
                # copyright-meta部分, 跳过
                if p.get('class'):
                    continue

                # 空白行, 文件里写两行空白, 便于阅读
                text = p.get_text()
                if not text:
                    f.write('\r\n\r\n')
                    continue

                f.write(text.encode('utf-8') + '\r\n')
                # "待续" 的地方, 接着就是另一章了, 插入空行
                if text.endswith(u'<\u5f85\u7eed>'):
                    f.write('\r\n')
                    
```

6\. 合并到一个文件, 用 `Python` 碰到编码错误, 时间紧, 就用批处理好了, 还简单
```
copy *.txt 码农故事.txt
```

7\. 打完收工.

后记:

就在合并时, 看到最后的一句话, 马上吐出一升老血:

{% img http://7ktor3.com1.z0.glb.clouddn.com/coder_end.png %}

{% img http://7ktor3.com1.z0.glb.clouddn.com/tuxie.gif %}

不过, 在 `kindle` 上还是 txt 的效果好一些, pdf 转换的有些乱码, 不完美.

{% img http://7ktor3.com1.z0.glb.clouddn.com/kindle_bug.png %}

附下载地址:

1\. TXT http://7ktor3.com1.z0.glb.clouddn.com/码农故事.txt

2\. PDF http://7ktor3.com1.z0.glb.clouddn.com/码农故事.pdf

<!--more-->
