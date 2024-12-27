---
layout: post
title: "Rails 日志切割"
date: 2013-09-15 23:48
comments: true
---

编辑日志切割配置 `/etc/logrotate.conf`, 新增 Rails 日志配置项

{% highlight ruby %}

{{YOUR_APP_CURRENT_PATH}}/log/*.log {
  daily
  dateext
  missingok
  rotate 65535
  delaycompress
  notifempty
  copytruncate
}

{% endhighlight %}
