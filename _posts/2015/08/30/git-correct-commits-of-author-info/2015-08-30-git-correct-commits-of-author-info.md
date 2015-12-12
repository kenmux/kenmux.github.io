---
title: "Git修改提交(commits)的作者資訊"
date: 2015-08-30 18:00:00 +0800
categories: [Git]
tags: [Git,Change Author Info]
comments: true
---

前幾日，在Github上新做存儲庫時、一時失察、居然使用了工作專用的作者資訊。發現時、已經推送了多個提交……怎麼辦？難道要砍掉重做麼？  

緊急Google了一番、發現完全不用！解決方法也可以有兩種：  

- 其一、使用Github提供的方法：[Changing author info](https://help.github.com/articles/changing-author-info/)。  
- 其二、使用Stack Overflow提供的方法：[Change commit author at one specific commit](https://stackoverflow.com/a/3042512/2518851)。  

由於是新做的存儲庫、所慮極少，故選用了第一種方法。<!-- more -->步驟如下：  

1) 從存儲庫複製一份裸倉庫(bare repository)：  

{% highlight text %}
$ git clone --bare https://github.com/kenmux/kenmux.github.io.git
$ cd kenmux.github.io.git
{% endhighlight %}

2) 創建名為`git-author-rewrite.sh`的程式腳本：  

{% highlight text %}
$ touch ~/git-author-rewrite.sh
$ chmod u+x ~/git-author-rewrite.sh
$ vim ~/git-author-rewrite.sh
{% endhighlight %}

並加入如下內容：  

{% highlight bash %}
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
{% endhighlight %}

注：  

- 腳本來源：[git-author-rewrite.sh](https://gist.github.com/octocat/0831f3fbd83ac4d46451#file-git-author-rewrite-sh)。  
- 替換變數：<b>OLD_EMAIL</b>、<b>CORRECT_NAME</b>、<b>CORRECT_EMAIL</b>。  
- 這裡依據：郵箱查找替換，亦可改為依據名字、不再贅述。  
- 這裡假定：作者(author)=提交者(committer)，注意甄別；如有必要，可只改其一。  

3) 執行腳本，以修改作者資訊：  

{% highlight text %}
$ chmod u+x ~/git-author-rewrite.sh
$ ~/git-author-rewrite.sh
{% endhighlight %}

4) 將改動強行推送(force push)到存儲庫，以及善後、清理：  

{% highlight text %}
$ git push --force --tags origin 'refs/heads/*'
$ cd ..
$ rm -rf kenmux.github.io.git
$ rm -rf ~/git-author-rewrite.sh
{% endhighlight %}

5) 為了不再重蹈覆轍，複製存儲庫後第一件事便是設置作者：  

{% highlight text %}
$ git clone https://github.com/kenmux/kenmux.github.io.git
$ cd kenmux.github.io
$ git config user.name <user-name>
$ git config user.email <user-email>
{% endhighlight %}

當然、如果不嫌煩絮的話、也可以在每次創建提交時指定作者（在以下示例中，名字為kenmux，郵箱為kenmux@gmail.com，請酌情修改）：  

{% highlight text %}
$ git commit --author="kenmux <kenmux@gmail.com>" -m "commit message"
{% endhighlight %}
