---
title: "Windows下運行Jekyll"
date: 2015-08-02 18:00:00 +0800
categories: [Website]
tags: [Jekyll,Windows]
comments: true
---

詳細教程、參見：[Run Jekyll on Windows](http://jekyll-windows.juthilo.com/)  
這裡、只是做個簡單的筆記。<!-- more -->

# Ruby支援

訪問 <http://rubyinstaller.org/downloads/>，下載並安裝Ruby（記得添加path）及DevKit。

# Ruby環境設置

在命令列下、依次執行以下命令即可（假若DevKit放置在C區）：

{% highlight text %}
cd C:\RubyDevKit
ruby dk.rb init
ruby dk.rb install
{% endhighlight %}

# 安裝Jekyll

在命令列下、執行命令：

{% highlight text %}
gem install jekyll
{% endhighlight %}

注：若通過proxy上網、則先要設置proxy：

{% highlight text %}
set http_proxy=http://user:password@proxy_ip:port
{% endhighlight %}

# 可能遇到的問題

嘛～由於Windows下的Jekyll沒有得到官方支援，因而遇到問題並不奇怪，比如這個問題：

{% highlight text %}
C:/Ruby/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require': cann
ot load such file -- hitimes/hitimes (LoadError)
{% endhighlight %}

解決方案如下：

{% highlight text %}
> gem uni hitimes
> gem ins hitimes -v 1.2.1 --platform ruby
{% endhighlight %}
