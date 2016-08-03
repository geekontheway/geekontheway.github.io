---
layout: post
title: "Apache Rewrite 规则详解"
date: 2016-02-01 03:00
comments: true
categories: [apache, rewrite]
---

[原文地址](http://www.ha97.com/3460.html), 作者：terrysco

Apache mod_rewrite 规则重写的标志一览

  R[=code](force redirect) 强制外部重定向

  强制在替代字符串加上http://thishost[:thisport]/前缀重定向到外部的URL.如果code不指定，将用缺省的302 HTTP状态码。

  F(force URL to be forbidden)禁用URL,返回403HTTP状态码。

  G(force URL to be gone) 强制URL为GONE，返回410HTTP状态码。

  P(force proxy) 强制使用代理转发。

  L(last rule) 表明当前规则是最后一条规则，停止分析以后规则的重写。

  N(next round) 重新从第一条规则开始运行重写过程。

  C(chained with next rule) 与下一条规则关联

  如果规则匹配则正常处理，该标志无效，如果不匹配，那么下面所有关联的规则都跳过。

  T=MIME-type(force MIME type) 强制MIME类型

  NS (used only if no internal sub-request) 只用于不是内部子请求

  NC(no case) 不区分大小写

  QSA(query string append) 追加请求字符串

  NE(no URI escaping of output) 不在输出转义特殊字符

  例如：RewriteRule /foo/(.*) /bar?arg=P1%3d$1 [R,NE] 将能正确的将/foo/zoo转换成/bar?arg=P1=zed

  PT(pass through to next handler) 传递给下一个处理

  例如：

  RewriteRule ^/abc(.*) /def$1 [PT] # 将会交给/def规则处理

  Alias /def /ghi

  S=num(skip next rule(s)) 跳过num条规则

  反向引用 $N 用于 RewriteRule 中匹配的变量调用(0 <= N <= 9)

  反向引用 %N 用于 RewriteCond 中最后一个匹配的变量调用(1 <= N <= 9)

使用mod_rewrite时常用的服务器变量：

  HTTP headers:HTTP_USER_AGENT, HTTP_REFERER, HTTP_COOKIE, HTTP_HOST, HTTP_ACCEPT
  connection & request: REMOTE_ADDR, QUERY_STRING
  server internals: DOCUMENT_ROOT, SERVER_PORT, SERVER_PROTOCOL
  system stuff: TIME_YEAR, TIME_MON, TIME_DAY

RewriteRule规则表达式的说明：
  . 匹配任何单字符
  [chars] 匹配字符串:chars
  [^chars] 不匹配字符串:chars
  text1|text2 可选择的字符串:text1或text2
  ? 匹配0到1个字符
  * 匹配0到多个字符
  + 匹配1到多个字符
  ^ 字符串开始标志
  $ 字符串结束标志
  n 转义符标志

RewriteCond适用的标志符

  ‘nocase|NC’ (no case)忽略大小
  ‘ornext|OR’ (or next condition)逻辑或，可以同时匹配多个RewriteCond条件

RewriteRule适用的标志符

  ‘redirect|R [=code]’ (force redirect)强迫重写为基于http开头的外部转向(注意URL的变化) 如：[R=301,L]
  ‘forbidden|F’ (force URL to be forbidden)重写为禁止访问
  ‘proxy|P’ (force proxy)重写为通过代理访问的http路径
  ‘last|L’ (last rule)最后的重写规则标志，如果匹配，不再执行以后的规则
  ‘next|N’ (next round)循环同一个规则，直到不能满足匹配
  ‘chain|C’ (chained with next rule)如果匹配该规则，则继续下面的有Chain标志的规则。
  ‘type|T=MIME-type’ (force MIME type)指定MIME类型
  ‘nosubreq|NS’ (used only if no internal sub-request)如果是内部子请求则跳过
  ‘nocase|NC’ (no case)忽略大小
  ‘qsappend|QSA’ (query string append)附加查询字符串
  ‘noescape|NE’ (no URI escaping of output)禁止URL中的字符自动转义成%[0-9]+的形式。
  ‘passthrough|PT’ (pass through to next handler)将重写结果运用于mod_alias
  ’skip|S=num’ (skip next rule(s))跳过下面几个规则
  ‘env|E=VAR:VAL’ (set environment variable)添加环境变量

实战

例子：

  RewriteEngine on
  RewriteCond %{HTTP_USER_AGENT} ^MSIE [NC,OR]
  RewriteCond %{HTTP_USER_AGENT} ^Opera [NC]
  RewriteRule ^.* – [F,L] 这里”-”表示没有替换，浏览器为IE和Opera的访客将被禁止访问。

例子：

  RewriteEngine On
  RewriteBase /test
  RewriteCond %{REQUEST_FILENAME}.php -f
  RewriteRule ([^/]+)$ /test/$1.php
  #for example: /test/admin => /test/admin.php
  RewriteRule ([^/]+).html$ /test/$1.php [L]
  #for example: /test/admin.html => /test/admin.php

限制目录只能显示图片

  < IfModule mod_rewrite.c>
  RewriteEngine on
  RewriteCond %{REQUEST_FILENAME} !^.*.(gif|jpg|jpeg|png|swf)$
  RewriteRule .*$ – [F,L]
  < /IfModule>

更多：

[APACHE REWRITE规则（内部重定向、外部重定向）](http://smilejay.com/2012/10/apache-rewrite/)



  E=VAR:VAL(set environment variable) 设置环境变量

