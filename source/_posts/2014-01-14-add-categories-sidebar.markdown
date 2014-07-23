---
layout: post
title: "添加分类列表到边栏"
date: 2014-01-14 23:09:10 +0800
comments: true
categories: Octopress
tags: Octopress
---

来源: <http://www.dotnetguy.co.uk/post/2012/06/25/octopress-category-list-plugin/>

简单记下过程
<!--more-->
1. 在 `plugins` 目录下新建一个 ruby 代码文件, 如 `category_list_tag.rb`
```ruby category_list_tag.rb
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```
2. 在 `source/_includes/asides` 下新建一个 html 文件, 如 `category_list.html`
{% codeblock category_list.html lang:html %}
<section>
  <h1>Categories</h1>
  <ul id="categories"> 
    {% raw %}{% category_list %}{% endraw %}
  </ul>
</section>
{% endcodeblock %}

我还顺手添了一个外部链接列表的边栏 `asides/other_links.html`, 先添一个, 后续再加
```html asides/other_links.html
<section>
  <h1>Links</h1>
  <ul id="links">
    <li><a href="https://github.com/lixxu/">GitHub projects</a></li>
  </ul>
</section>
```
3. 修改 `_config.yml`, 添加分类和外部链接
```yaml _config.yml
default_asides: [asides/recent_posts.html, asides/category_list.html, asides/other_links.html, asides/twitter.html, asides/delicious.html, asides/pinboard.html, asides/googleplus.html]
```

另外, 如果想在分类里使用空格, 直接按 html 语法写 `CAT1&nbsp;CAT11`
<!--more-->