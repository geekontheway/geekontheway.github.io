---
layout: post
title: "找回ubuntu桌面"
date: 2012-07-05 23:51
comments: true
---
晚上打开电脑发现桌面上堆满了文件和文件夹，仔细一看，全是HOME文件夹中的文件，打开~/.config/user-dirs.dirs，果然配置被修改了
{% highlight ruby %}
XDG_DESKTOP_DIR="$HOME"
{% endhighlight %}
把这行设置改回来即可
{% highlight ruby %}
XDG_DESKTOP_DIR="$HOME/Desktop"
{% endhighlight %}
当然也可以在桌面上显示任意的其他文件夹中的内容。
> 改完了之后重启nautilus文件夹管理器
{% highlight ruby %}
killall nautilus
{% endhighlight %}
