---
layout: post
title: "使用 Thin 作为 Octopress 预览服务器"
date: 2014-01-16 10:06:15 +0800
comments: true
categories: Octopress
tags: Octopress Thin
---
Windows 平台下 Octopress 自带的 `WEBrick` 有点慢, 于是就想使用 `Thin` 代替.
放狗一搜, 这里有答案了:

- <http://blog.geoffpetrie.com/blog/2012/10/14/octopress-change-the-default-preview-server/>

####操作步骤:####
<!--more-->
&nbsp;1. 安装 Thin
```
gem install thin
```

&nbsp;2. 更改 `Gemfile`, 在 `development` 里面最后添加一行
``` ruby Gemfile
source "http://ruby.taobao.org"

group :development do
  gem 'rake', '~> 0.9'
  gem 'jekyll', '~> 0.12'
  gem 'rdiscount', '~> 2.0.7'
  gem 'pygments.rb', '~> 0.3.4'
  gem 'RedCloth', '~> 4.2.9'
  gem 'haml', '~> 3.1.7'
  gem 'compass', '~> 0.12.2'
  gem 'sass', '~> 3.2'
  gem 'sass-globbing', '~> 1.0.0'
  gem 'rubypants', '~> 0.2.0'
  gem 'rb-fsevent', '~> 0.9'
  gem 'stringex', '~> 1.4.0'
  gem 'liquid', '~> 2.3.0'
  gem 'directory_watcher', '1.4.1'
  gem 'thin'
end

gem 'sinatra', '~> 1.4.2'
```

&nbsp;3. 重新运行 `rake preview` 就会使用 `Thin` 了
<!--more-->