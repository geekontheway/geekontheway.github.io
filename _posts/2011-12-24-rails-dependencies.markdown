---
layout: post
title: "Rails基本环境配置"
date: 2011-12-24 06:10
comments: true
---


>Mac

Xcode,`rvm 安装1.9以上版本的ruby需要设置编译器为gcc`

>Ubuntu 10.04 LTS

{% highlight ruby %}
apt-get install wget vim build-essential openssl libssl-dev libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf automake libtool imagemagick libpcre3-dev libmysqld-dev



{% endhighlight %}

>CentOS 5

{% highlight ruby %}

yum groupinstall "Development Tools"
yum install zlib-devel wget openssl-devel pcre pcre-devel make gcc gcc-c++ curl-devel mysql-devel

# gem install mysql --no-rdoc --no-ri -- --with-mysql-dir=/usr/bin --with-mysql-lib=/usr/lib/mysql --with-mysql-include=/usr/include/mysql

{% endhighlight %}

>passenger+apache2/passenger+nginx

Apache2
{% highlight ruby %}
 apt-get install apache2 libcurl4-openssl-dev apache2-prefork-dev apache2-mpm-prefork libapr1-dev libaprutil1-dev
 gem install passenger
 passenger-install-apache2-module
{% endhighlight %}

Nginx
{% highlight ruby %}
 gem install passenger
 passenger-install-nginx-module
{% endhighlight %}

> Database

- [MySQL](http://geekontheway.github.com/blog/2011/12/24/install-mysql-server-in-unix-like-operating-system/)

- PostgreSQl

{% highlight ruby %}
geek@me$: sudo apt-get install postgre libpq-dev postgresql-contrib
geek@me$: sudo password postgres
geek@me$: sudo su postgres
	postgres@me:$ createuser pg --pwprompt
	Enter password for new role:[pg]
	Enter it again: [pg][]
	Shall the new role be a superuser? (y/n) y
	gem install pg
{% endhighlight %}

- MongoDB
- SQLite3
