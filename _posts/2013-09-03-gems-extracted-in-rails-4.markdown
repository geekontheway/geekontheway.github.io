---
layout: post
title: "Rails4中剥离的gem"
date: 2013-09-03 00:16
comments: true
categories: [rails4]
---
原文链接：[Upgrading Rails: Gems Extracted in Rails 4](http://www.andylindeman.com/2013/03/05/gems-extracted-in-rails-4.html "Upgrading Rails: Gems Extracted in Rails 4")

一些特定的组件服务被抽离出来成了独立的gem，一方面和Rails本身的发展方向和定位有关，一方面，抽离出来之后的gem，有了独立的维护组织，能更快的进行开发维护。

#### protected_attributes

Rails4 推荐使用 `strong_parameters` 来做 `mass-assignment` 保护 , Rails3 中的 `attr_accessible` 和 `attr_protected` 已经被抽离出来，这个应该是每个 Rails3 项目升级到 Rails4 的时候最先遇到的，短期内不适应或者项目太大，可以添加 `strong_parameters` 这个 gem 。

#### activeresource

没用过，`RESTful API` 的 ActiveRecord 式调用。

#### actionpack-action_caching actionpack-page_caching

Rails4 对片段缓存做了优化，俄罗斯套娃缓存已经很快了。如果需要，还是可以把这两个 gem 添加进来。

#### activerecord-session_store

如果想把 session 存储在数据库中，加上这个。

#### rails-observers

不再推荐使用。

#### actionpack-xml_parser

之前爆过相关漏洞，而且如果项目里面不需要接受 xml 请求，也用不上。

#### rails-perftest

性能测试，没用过。

#### actionview-encoded_mail_to

如果 mail_to 方法中的参数需要 encode, 加上这个。
