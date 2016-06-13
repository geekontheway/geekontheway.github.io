---
layout: post
title: "使用farady上传图片到贴图库"
date: 2015-01-25 01:37
comments: true
categories: [ruby, faraday, tietuku]
---

![title](http://i2.tietuku.cn/048176fa46259d35.jpg)

[贴图库](http://tietuku.cn/)是国内一款免费、高速、稳定、专业图片外链、专注图片云存储服务，相比于七牛云存储，使用门槛较低。


[Faraday](https://github.com/lostisland/faraday) 是一个轻量灵活的 HTTP client.

一直想写个贴图库的ruby sdk出来,今天抽空看了下[文档](http://open.tietuku.cn/doc)，比较简单，先整理了下通过几行ruby code实现上传图片到贴图库的思路。

开始之前，你可以先注册一个账号，登陆之后，可以在管理中心中找到自己的AccessKey和SecretKey，复制下来，后面我们用来生成操作签名(sign)。

上传图片接口调用地址：

http://up.tietuku.cn/

HTTP请求方式：Post


![title](http://i2.tietuku.cn/4c1dbe8c2dc826ef.jpg)

下面的代码跑在`ruby-1.9.3-p392`上，我们先打开`irb`, 引用需要用到的库

{% highlight ruby %}
require 'hmac-sha1'
require 'uri'
require 'cgi'
require 'json'
require 'faraday'
{% endhighlight %}

1.参数数组

{% highlight ruby %}
param = { deadline: Time.now.to_i + 60, aid: '25421', from: 'file',
          file: Faraday::UploadIO.new('/Users/leslie/github.jpg', 'image/jpeg')}
{% endhighlight %}

2.将参数数组序列化为JSON格式(jsoncode)

{% highlight ruby %}
require 'json'
jsoncode = param.to_json
{% endhighlight %}

3.对JSON编码的参数数组(jsoncode)进行URL安全的Base64编码，得到待签名字符串(encodedParam)
{% highlight ruby %}
# URL安全的Base64编码适用于以URL方式传递Base64编码结果的场景。该编码方式的基本过程是先将内容以Base64格式编码为字符串，
# 然后检查该结果字符串，将字符串中的加号+换成中划线-，并且将斜杠/换成下划线_，同时尾部保持填充等号=。
def urlsafe_base64_encode content
  Base64.encode64(content).strip.gsub('+', '-').gsub('/','_').gsub(/\r?\n/, '')
end
encoded_param =  urlsafe_base64_encode(jsoncode)
{% endhighlight %}

4.使用SecertKey对上一步生成的待签名字符串(encodedParam)计算HMAC-SHA1签名(sign)

{% highlight ruby %}
sign = HMAC::SHA1.new(secret_key).update(encoded_param).digest
{% endhighlight %}

5.对签名(sign)进行URL安全的Base64编码(encodedSign)

{% highlight ruby %}
encoded_sign = urlsafe_base64_encode(sign)
{% endhighlight %}

6.将AccessKey、encodedSign和encodedParam用:连接起来
{% highlight ruby %}
token = access_key + ':' + encoded_sign + ':' + encoded_param
{% endhighlight %}

7.请求上传图片接口，记录返回数据

{% highlight ruby %}
conn = Faraday.new do |faraday|
  faraday.request :multipart
  faraday.request :url_encoded
  faraday.adapter :net_http
end

param = param.merge({Token: token })

response = conn.post 'http://up.tietuku.cn', param

result = JSON.parse response.body
{% endhighlight %}


更多阅读


[Faraday: advanced HTTP requests made easy](http://mislav.uniqpath.com/2011/07/faraday-advanced-http/ "Faraday: advanced HTTP requests made easy")


[Faraday: One HTTP Client to Rule Them All](http://www.intridea.com/blog/2012/3/12/faraday-one-http-client-to-rule-them-all "Faraday: One HTTP Client to Rule Them All")


[Ruby Patterns: Webservice object](http://blog.rlmflores.me/blog/2013/07/16/ruby-patterns-webservice-object/ "Ruby Patterns: Webservice object")


[Oh My Faraday](http://www.bjoernrochel.de/2013/02/09/oh-my-faraday/ "Oh My Faraday")


[Parallel HTTP Requests with Faraday and Typhoeus](http://blog.carbonfive.com/2013/03/15/parallel-http-requests-with-faraday-and-typhoeus/ "Parallel HTTP Requests with Faraday and Typhoeus")


[HTTP Monkey an alternative to Faraday](http://rogerleite.github.io/http_monkey/http_monkey_an_alternative_to_faraday.html "HTTP Monkey an alternative to Faraday")


[Wrapping Your API In A Custom Ruby Gem](https://quickleft.com/blog/wrapping-your-api-in-a-custom-ruby-gem/ "Wrapping Your API In A Custom Ruby Gem")


[Common interface for Ruby's HTTP clients](http://httpirb.com/ "Common interface for Ruby's HTTP clients ")


[Testing Faraday clients for APIs in your Rails app](https://firmhouse.com/blog/using-faraday-stubs-for-testing-rails-testing "Testing Faraday clients for APIs in your Rails app")


[HTTP + Client Side Middleware Stack == Win](http://www.bjoernrochel.de/2013/01/13/http-plus-middlewarestack-equals-win/ "HTTP + Client Side Middleware Stack == Win")


[Qiniu Resource (Cloud) Storage SDK for Ruby](https://github.com/qiniu/ruby-sdk "Qiniu Resource (Cloud) Storage SDK for Ruby")
