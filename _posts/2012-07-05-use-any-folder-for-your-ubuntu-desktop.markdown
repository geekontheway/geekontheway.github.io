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

> 今天部署了好久没部署上去，后来想清楚忘了在新电脑上rake_github_pages.

<pre>Enter the read/write url for your repository: git@github.com:geekontheway/geekontheway.github.com.git
	  Added remote git@github.com:geekontheway/geekontheway.github.com.git as origin
	  Set origin as default remoteMaster branch renamed to 'source' for committing your blog source files
	  Initialized empty Git repository in /home/nasa/www/octopress/_deploy/.git/
	  [master (root-commit) 75592aa] Octopress init
	   1 files changed, 1 insertions(+), 0 deletions(-)
	 create mode 100644 index.html
         ## Now you can deploy to http://geekontheway.github.com with `rake deploy` ##</pre>
