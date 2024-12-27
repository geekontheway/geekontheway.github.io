---
layout: post
title: "添加turbolinks"
date: 2014-08-25 23:14
comments: true
categories: [ruby, rails, turbolinks]
---

![title](http://i2.tietuku.cn/2431fbe2cee381fd.jpg  )

Turbolinks使站内跳转更快，原理是Turbolinks监听了页面内的链接，跳转页面用ajax请求并替换body内容，不用重新解析已加载的静态资源, 同时通过pushState修改地址栏地址。

####有多快？

看你的网站有多少静态资源，越多提升越大。

#### 有依赖吗?

没有。


#### 属性

* data-turbolinks-track 检查服务器端静态文件是否已修改，如有重载页面
* data-no-turbolink 关闭元素Turbolinks访问
* data-turbolinks-eval 设置为false的时候，直接访问的页面时才会执行，通过Turbolinks访问则不会执行

#### 事件

使用Turbolinks，站内跳转没有重新刷新整个页面，因而 DOMContentLoaded和jQuery.ready()会失效。

请求页面，加入了以下事件：

* page:before-change 点击链接
* page:fetch 客户端开始ajax请求新页面
* page:receive 服务器端已接受ajax请求
* page:before-unload 客户端准备渲染新页面
* page:change 客户端已切换到新页面
* page:load 客户端处理完成

默认，Turbolinks会缓存10个单位的资源， 你也可以自行设置。

{% highlight ruby %}

  // View the current cache size
  Turbolinks.pagesCached();

  // Set the cache size
  Turbolinks.pagesCached(20);

{% endhighlight %}

####手动触发Turbolinks跳转：

{% highlight ruby %}
  //js
  Turbolinks.visit(path)

  //rails
  redirect_via_turbolinks_to

{% endhighlight %}


#### 兼容性

* Safari 6.0+
* IE10+
* 最新版Chrome和Firefox

#### 使用

初次使用，还没遇到大坑，页面浏览体验提升了很多。

{% highlight ruby %}
# Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
gem 'turbolinks'

# incase turbolinks do not call document’s ready event.
# Usage: //= require jquery.turbolinks
gem 'jquery-turbolinks'

# Usage: //= require google-analytics-turbolinks
gem 'google-analytics-turbolinks'

# Usage: //= require turbolinks.redirect
gem 'turbolinks-redirect'
{% endhighlight %}

{% highlight ruby %}

//= require jquery
//= require jquery.turbolinks
//= require jquery_ujs
//= require turbolinks
//= require google-analytics-turbolinks
//= require turbolinks.redirect
//= require_directory .

Turbolinks.enableProgressBar();
Turbolinks.enableTransitionCache();

{% endhighlight %}

{% highlight ruby %}
  <%= stylesheet_link_tag    'application', media: "all", "data-turbolinks-track" => true %>
  <%= javascript_include_tag 'application', "data-turbolinks-track" => true %>
{% endhighlight %}

更多阅读

[Seriously. Numbers. Use them.](http://blog.steveklabnik.com/posts/2012-09-27-seriously--numbers--use-them- "Seriously. Numbers. Use them. ")

[jQuery, Turbolinks and Asset Pipeline](http://blog.zamith.pt/blog/2013/03/09/jquery-turbolinks-and-asset-pipeline/ "jQuery, Turbolinks and Asset Pipeline")


[Faster page loads with Turbolinks](https://coderwall.com/p/ypzfdw/faster-page-loads-with-turbolinks "Faster page loads with Turbolinks")


[Turbolinks 后端逻辑分析](https://ruby-china.org/topics/12224 "https://ruby-china.org/topics/12224")


[Pros and Cons of Turbolinks in Rails 4 Applications](http://wlowry88.github.io/blog/2014/07/28/pros-and-cons-of-turbolinks-in-rails-4-applications/ "Pros and Cons of Turbolinks in Rails 4 Applications")

[我会说 Turbolink 的体验就是一坨屎吗](https://ruby-china.org/topics/13543 "我会说 Turbolink 的体验就是一坨屎吗")


[Dangers of Turbolinks](http://staal.io/blog/2013/01/18/dangers-of-turbolinks/ "Dangers of Turbolinks")


[Working with JavaScript in Rails](http://guides.rubyonrails.org/working_with_javascript_in_rails.html "Working with JavaScript in Rails ")


