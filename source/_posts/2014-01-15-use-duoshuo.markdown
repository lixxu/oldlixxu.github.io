---
layout: post
title: "Octopress使用多说评论系统"
date: 2014-01-15 09:44:48 +0800
comments: true
categories: Octopress
tags: Octopress duoshuo
---
参考来源:

 - <http://dinever.com/blog/2012/12/05/zai-octopresszhong-shi-yong-duo-shuo/>
 - <http://havee.me/internet/2013-02/add-duoshuo-commemt-system-into-octopress.html>

照着文章来就行了, 没啥好说的, 这里记一下备忘, 把 `yourname` 替换成你自己的名字.
<!--more-->
&nbsp;1. 注册多说, 不必多说, 复制生成的代码到 `source\_includes\post` 下新建 `duoshuo.html`
```html
<!-- Duoshuo Comment BEGIN -->
<div class="ds-thread"></div>
<script type="text/javascript">
    var duoshuoQuery = {short_name:"yourname"};
    (function() {
        var ds = document.createElement('script');
        ds.type = 'text/javascript';ds.async = true;
        ds.src = 'http://static.duoshuo.com/embed.js';
        ds.charset = 'UTF-8';
        (document.getElementsByTagName('head')[0] 
        || document.getElementsByTagName('body')[0]).appendChild(ds);
    })();
</script>
<!-- Duoshuo Comment END -->
```

&nbsp;2. 编辑 `_config.yml`, 添加下面两行:
```yaml
# duoshuo comments
duoshuo_comments: true
duoshuo_short_name: yourname
```

&nbsp;3. 在 `source\_layouts\post.html` disqus设置下面添加:
```html
{% raw %}{% if site.duoshuo_short_name and site.duoshuo_comments == true and page.comments == true %}{% endraw %}
  <section>
    <h1>Comments</h1>
    <div id="comments" aria-live="polite">{% raw %}{% include post/duoshuo.html %}{% endraw %}</div>
  </section>
{% raw %}{% endif %}{% endraw %}
```
<!--more-->