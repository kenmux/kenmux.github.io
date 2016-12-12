---
title: "Mac OS X下Git遇到編輯器vi錯誤"
date: 2015-12-06 18:00:00 +0800
categories: [Programming]
tags: [Mac OS X,Git,vi]
comments: true
---

Mac OS X下Git進行提交時，會遇到這樣的錯誤：  

> error: There was a problem with the editor 'vi'.  
> Please supply the message using either -m or -F option.  

大致搜尋了下，stackoverflow上的這個[解答](https://stackoverflow.com/a/14607585/2518851)剛好對症。<!-- more -->  

而且，解答下還附有對此進行深入探究的博文：[Fixing "There was a problem with the editor 'vi'" for Git on Mac OS X Snow Leopard](http://tooky.co.uk/there-was-a-problem-with-the-editor-vi-git-on-mac-os-x/)（也是2010年的老物，看來這是老問題呀～）  

解決方案為設置Git的編輯器為vim，具體實現有兩種：  

- [其一、修改配置檔.gitconfig]({{ page.url }}#edit-gitconfig)  
- [其二、呼叫`git config`]({{ page.url }}#call-git-config)  

# <a name="edit-gitconfig"></a>修改配置檔.gitconfig

使用vim打開配置檔.gitconfig：  

{% highlight bash %}
vim ~/.gitconfig
{% endhighlight %}

然後、確保有這兩列即可：

{% highlight text %}
[core]
  editor = /usr/bin/vim
{% endhighlight %}

`/usr/bin/vim`即vim路徑，妳可以通過指令`which vim`進行查詢。  

# <a name="call-git-config"></a>呼叫`git config`  

這是stackoverflow上給的解答：  

{% highlight bash %}
git config --global core.editor /usr/bin/vim
{% endhighlight %}

不過，我更推薦這樣用：  

{% highlight bash %}
git config --global core.editor $(which vim)
{% endhighlight %}
