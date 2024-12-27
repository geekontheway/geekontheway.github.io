---
layout: post
title: "在SUSE上部署RubyOnRails应用环境"
date: 2013-08-18 00:33
comments: true
categories: [suse,ruby,rails]
---
>1.设置IP,hostname,dns

{% highlight ruby %}

#第一种SUSE Linux IP设置方法

ifconfig eth0 192.168.1.22 netmask 255.255.255.0 up
route add default gw 192.168.1.2

#第二种SUSE Linux IP设置方法
#在suse操作系统中每个网卡都有一个配置文件，在/etc/sysconfig/network/目录下。编辑ifcfg-eth0-你的网卡的物理地址的那个文件：

BOOTPROTO=static
IPADDR=192.168.1.110
NETMASK=255.255.255.0
NETWORK=192.168.1.0
BROADCAST=192.168.1.255

#重启应用

/etc/init.d/network restart

{% endhighlight %}

>2.安装apache2，mysql，mongodb，redis，以apache2为例

{% highlight ruby %}
#sudo zypper install apache2
#sudo /usr/sbin/rcapache2 start
{% endhighlight %}

>3.安装rvm(会自动安装开发工具包依赖，比ubuntu和centos赞啦),采用rvm.io通用安装方式即可

>4.安装ruby，如果ruby-lang访问不了，可以设置从ruby-china下载binaries。

>5.安装git, zip，unzip，nodejs等其他需要的环境

{% highlight ruby %}
# sudo zypper install nodejs zip unzip git imagemagick
{% endhighlight %}
