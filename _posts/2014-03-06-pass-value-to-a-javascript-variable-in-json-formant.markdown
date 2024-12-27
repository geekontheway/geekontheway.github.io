---
layout: post
title: "Rails的hash对象传递到javascript变量"
date: 2012-10-01 14:55
comments: true
categories: [Rails, javascripts]
description: "Rails的hash对象传递到javascript变量"
---

{% highlight javascript %}
  var obj = JSON.parse('<%= hash.to_json.html_safe %>');
{% endhighlight %}

