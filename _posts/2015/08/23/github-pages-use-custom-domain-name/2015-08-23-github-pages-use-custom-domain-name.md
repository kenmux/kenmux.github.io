---
title: "GitHub Pages使用自訂網域名稱"
date: 2015-08-23 18:00:00 +0800
categories: [Website]
tags: [GitHub Pages,Domain Name,Godaddy]
imgurl: /assets/imgs/2015/08/23/github-pages-use-custom-domain-name
comments: true
---

前幾日，把Godaddy域名重新設置了下，從WordPress網站那兒拿來給GitHub Pages用了。趁著記憶猶新，先做個筆記！  

總的來說，設置還是蠻簡單的，可以分為兩步：  
其一，[GitHub Pages設置]({{ page.url }}#github-pages-settings)  
其二，[Godaddy域名設置]({{ page.url }}#godaddy-domain-settings)  

注：  

- 文中示例，域名為XIWAN.INFO，請酌情修改為自訂域名。  
- 參考文章：[Configuring a Godaddy domain name with github pages](http://andrewsturges.com/blog/jekyll/tutorial/2014/11/06/github-and-godaddy.html)，感興趣的可以戳進去看看。<!-- more -->  

# <a name="github-pages-settings"></a>GitHub Pages設置  
首先，將`_config.yml`檔案中變數`url`的值修改為域名:  

{% highlight text %}
url: "http://xiwan.info"
{% endhighlight %}

其次，在根目錄下創建名為`CNAME`的檔案：  

{% highlight text %}
$ cd kenmux.github.io
$ touch CNAME
{% endhighlight %}

修改`CNAME`內容，將域名填入即可：  

{% highlight text %}
xiwan.info
{% endhighlight %}

# <a name="godaddy-domain-settings"></a>Godaddy域名設置  
訪問Godaddy：<https://www.godaddy.com/>。登入帳號，開始設定域名。點選標籤頁「<b>DNS ZONE FILE</b>」，要進行設定的有兩個地方：[A (Host)]({{ page.url }}#a-host-setting)和[CNname (Alias)]({{ page.url }}#cnname-alias-setting)。  
![2015-08-23-01.png]({{ page.imgurl }}/2015-08-23-01.png)  

注：如果標籤頁「<b>DNS ZONE FILE</b>」並不是像上圖顯示的那樣，有很大可能是因為客製了名稱伺服器。回到「<b>SETTINGS</b>」標籤頁，將「<b>Nameservers</b>」回復到預設值，如下圖所示：
![2015-08-23-02.png]({{ page.imgurl }}/2015-08-23-02.png)  

<a name="a-host-setting"></a>先設置`A (Host)`吧。點擊下方連結<u><font color="blue">Add Record</font></u>，出現如下彈出框：  
![2015-08-23-03.png]({{ page.imgurl }}/2015-08-23-03.png)  

「<b>HOST:</b>」那一欄填入`@`，而「<b>POINTS TO:</b>」那一欄填入`192.30.252.153`或者`192.30.252.154`，如下圖所示：  
![2015-08-23-04.png]({{ page.imgurl }}/2015-08-23-04.png)  

填寫完畢後，點選「<b>FINISH</b>」以完成輸入。`A (Host)`那一欄看起來應該是這樣的：  
![2015-08-23-05.png]({{ page.imgurl }}/2015-08-23-05.png)  

注：記得點選「<b>Save Changes</b>」以保存修改哦。這裡採用了兩條記錄，實際上一條足矣。  

<a name="cnname-alias-setting"></a>接下來設置`CNname (Alias)`。同樣是點選<u><font color="blue">Add Record</font></u>，在出現的彈出框中選擇`CNNAME (Alias)`，如下圖所示：  
![2015-08-23-06.png]({{ page.imgurl }}/2015-08-23-06.png)  

「<b>HOST:</b>」那一欄填入`www`，而「<b>POINTS TO:</b>」那一欄填入GitHub Pages預設URL（形如`username.github.io`），如下圖所示：  
![2015-08-23-07.png]({{ page.imgurl }}/2015-08-23-07.png)  

注：  

1. 若如圖例中所示、已有「<b>www</b>」項，則直接編輯修改此項即可。  
2. 若要使用二級域名，則「<b>HOST:</b>」需要填寫為二級域名。以`blog.xiwan.info`為例，則需要填寫為`blog`。  

填寫完畢後，點選「<b>FINISH</b>」以完成輸入。再次點選「<b>Save Changes</b>」以保存修改。設置完畢後，看起來應該是這樣：  
![2015-08-23-08.png]({{ page.imgurl }}/2015-08-23-08.png)  

大功告成！如此一來，`http://xiwan.info`、`http://www.xiwan.info`都將指向`http://kenmux.github.io`。  
