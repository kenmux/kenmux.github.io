---
title: "使用subtree將子目錄分離為獨立的Git存儲庫"
date: 2014-11-30 18:00:00 +0800
categories: [Git]
tags: [Git,Subtree,Submodule]
comments: true
---

當一個專案在開發一段時間後，可能會有將某個子目錄單獨分離出來的需求。在subtree之前，可能需要比較繁瑣的步驟；不過現在，這也成為了一件輕鬆之事。<!-- more -->  

# 分離出子目錄  

使用subtree將子目錄分離出來，做成一個新的分支：  

{% highlight text %}
$ git subtree split -P FOLDER_NAME_WITH_PATH -b BRANCH_NAME
{% endhighlight %}

注：  

1. 此命令需在存儲庫根目錄下執行（即.git所在目錄）。  
2. 請用子目錄的實際路徑（相對於存儲庫根目錄）替換FOLDER_NAME_WITH_PATH。  
3. 請用實際分支名稱替換BRANCH_NAME。  

下同，不再贅述。  

# 創建獨立存儲庫  

創建一個新的存儲庫，並拉取以上創建的分支：  

{% highlight text %}
$ mkdir REPO_NAME
$ cd REPO_NAME
$ git init
$ git pull ORIGIN_REPO_NAME_WITH_PATH BRANCH_NAME
{% endhighlight %}

注：  

1. 請用實際存儲庫名稱替換REPO_NAME。  
2. 請用子目錄所屬存儲庫的路徑替換ORIGIN_REPO_NAME_WITH_PATH。  

下一步要做的，就是將新創建的存儲庫推送到遠端存儲庫，可以參考之前的文章：[創建Git遠端存儲庫並做初次推送]({{ site.url }}/archive/git-set-up-remote-repo-do-initial-push.html)，在此恕不贅述。  

# 清理原存儲庫  

刪除子目錄、以及為了分離子目錄而創建的分支：  

{% highlight bash %}
$ git rm -rf FOLDER_NAME_WITH_PATH
$ git commit -m "remove related folders;"
$ git branch -D BRANCH_NAME
{% endhighlight %}

做到這裡，子目錄算是從專案存儲庫中分離出來了。  

# 添加為子專案  

途徑為二：其一、[使用subtree]({{ page.url }}#use-subtree)；其二、[使用submodule]({{ page.url }}#use-submodule)。   

### <a name="use-subtree"></a>使用subtree：  

{% highlight text %}
$ git subtree add -P FOLDER_NAME_WITH_PATH REPO_URL BRANCH_NAME
$ git push origin PROJECT_BRANCH_NAME
{% endhighlight %}

注：  

1. 請用子專案路徑（相對於存儲庫根目錄）替換FOLDER_NAME_WITH_PATH。若存在同名檔案夾，則會出錯，無論非空與否。  
2. 請用新分離出的存儲庫url替換REPO_URL。  
3. 請用2中所指存儲庫的開發分支名稱替換BRANCH_NAME（比如：master）。  
4. 請用添加subtree的專案存儲庫的分支名稱替換BRANCH_NAME（比如：master）。  

### <a name="use-submodule"></a>使用submodule：  

呃，寫這個會不會偏離主題啊？嘛～這是筆記啊，細節什麼的就不要在意啦～  

{% highlight text %}
$ git submodule add REPO_URL FOLDER_NAME_WITH_PATH
$ git commit -m "first commit with submodule;"
$ git submodule init
$ git submodule update
$ git push origin PROJECT_BRANCH_NAME
{% endhighlight %}

注：命令列下，subtree使用起來較submodule簡潔。不過，若你使用圖形介面工具source tree，則後者可能比較好用，因為截至此篇文章寫就時source tree對subtree的支持依舊差強人意。  
