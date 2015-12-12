---
title: "創建Git遠端存儲庫並做初次推送"
date: 2014-11-23 18:00:00 +0800
categories: [Git]
tags: [Git]
comments: true
---

本文介紹如何創建存儲庫並做初次推送。注：本文參考自這篇文章：[GIT – SETTING UP A REMOTE REPOSITORY AND DOING AN INITIAL PUSH](http://thelucid.com/2008/12/02/git-setting-up-a-remote-repository-and-doing-an-initial-push/)，感興趣的可以戳進去看看。<!-- more -->  

# 創建遠端存儲庫  

Git伺服器上（遠端），創建裸倉庫。注：請用專案名稱替換PROJECT_NAME，下同，不再贅述。  

{% highlight text %}
$ mkdir PROJECT_NAME.git
$ cd PROJECT_NAME.git
$ git init --bare
{% endhighlight %}

# 添加專案到本地存儲庫  

本地，將專案添加到倉庫。  

{% highlight text %}
$ cd PROJECT_NAME
$ git init
$ git add *
$ git commit -m "initial version"
{% endhighlight %}

# 存儲庫間建立連接  

將遠端存儲庫添加到本地存儲庫。注：請用遠端的存儲庫URL替換REPO_URL。  

{% highlight text %}
$ git remote add origin REPO_URL
{% endhighlight %}

# 初次推送  

由於遠端存儲庫可能含有本地存儲庫所沒有的預設檔案（例如：README、.gitignore等），故需要先做一次拉取，然後再做推送。  

{% highlight text %}
$ git pull origin master
$ git push -u origin master
{% endhighlight %}
