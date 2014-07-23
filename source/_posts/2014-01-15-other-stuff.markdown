---
layout: post
title: "Octopress的其他杂项"
date: 2014-01-15 14:13:56 +0800
comments: true
categories: Octopress
tags: Octopress
---
放了半天狗, 添加许多小东西到 Octopress, 如 `tags`, `相关文章`, `添加多说评论到边栏` 等等.

参考文章: <http://blog.csdn.net/lcliliil/article/details/13725895>

<del>不过, 多说的侧边栏评论一直不显示标题, 找了半天也没找到解决方法. 有知道的达人麻烦告知一下.</del>
**更新**: 已经搞定了, 需要添加 `data-thread-key` 属性

暂时不想再弄了, 以后再说吧.

- 在 `source\_includes\post\duoshuo.html` 中使用 `page`
- 在 `source\_includes\article.html` 中如果是首页使用 `post`, 否则使用 `post`

我把加载 js 的部分放到 `source\_layouts\default.html` 里了, 加在 `</body>` 前即可.

<!--more-->
```html source\_layouts\default.html
{% raw %}{% if site.duoshuo_short_name and site.duoshuo_comments == true %}{% endraw %}
  <!-- 多说js加载开始，一个页面只需要加载一次 -->
  <script type="text/javascript">
    var duoshuoQuery = {short_name:"{% raw %}{{ site.duoshuo_short_name }}{% endraw %}"};
    (function() {
        var ds = document.createElement('script');
        ds.type = 'text/javascript';ds.async = true;
        ds.src = 'http://static.duoshuo.com/embed.js';
        ds.charset = 'UTF-8';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(ds);
    })();
  </script>
  <!-- 多说js加载结束，一个页面只需要加载一次 -->
{% raw %}{% endif %}{% endraw %}
```

```html source\_includes\post\duoshuo.html
<!-- Duoshuo Comment BEGIN -->
<div class="ds-thread" data-thread-key="{% raw %}{{ page.url }}{% endraw %}" data-title="{% raw %}{{ page.title }}{% endraw %}" data-url="{% raw %}{{ page.url }}{% endraw %}"></div>
<!-- Duoshuo Comment END -->
```
```html source\_includes\article.html
{% raw %}{% if site.duoshuo_short_name and page.comments != false and post.comments != false and site.duoshuo_comments == true %}{% endraw %}
    | <a href="{% raw %}{% if index %}{{ root_url }}{{ post.url }}{% endif %}{% endraw %}#comments"><span class="ds-thread-count" data-thread-key="{% raw %}{% if index %}{{ post.url }}{% else %}{{ page.url }}{% endif %}{% endraw %}" data-count-type="comments">暂无评论</span></a>
{% raw %}{% endif %}{% endraw %}
```
<!--more-->