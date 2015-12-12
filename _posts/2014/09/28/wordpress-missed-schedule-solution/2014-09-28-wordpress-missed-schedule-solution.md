---
title: "WordPress錯失排程解決方案"
date: 2014-09-28 18:00:00 +0800
categories: [Website]
tags: [WordPress,Missed Schedule]
imgurl: /assets/imgs/2014/09/28/wordpress-missed-schedule-solution
comments: true
---

明明設定好了排程，可到了時間、文章卻遲遲不能發表出來。登入後台查看了下狀態：「錯失排程(missed schedule)」。蝦米？！這是什麼狀況？鬱悶之餘，在網路上搜尋了一番，總算找到解決方案。  

注：  

- 這篇文章的方法來自於[香腸炒魷魚](https://sofree.cc)的一篇文章：[解決WordPress自動排程失敗的三個方法(missed schedule)](http://sofree.cc/wp-missed-schedule/)，感興趣的可以戳進去看看。  
- 請盡量只採用一個解決方案！這裡羅列的方案，並不能保證能夠互不干擾地協同工作！<!-- more -->  

# 解決方案一：修改wp-config.php  

在WordPress根目錄下有個設定檔：wp-config.php：  
![2014-09-28-01.png]({{ page.imgurl }}/2014-09-28-01.png)  

在其中添加如下語句：  

{% highlight text %}
/** 解決 WordPress 排程問題 **/
define('ALTERNATE_WP_CRON', true);
{% endhighlight %}

如下圖所示：
![2014-09-28-02.png]({{ page.imgurl }}/2014-09-28-02.png)  

此方案基本能夠解決問題，但偶爾還會出現「錯失排程」，原因未知（初步估計是網路錯誤）。此方案優點在於不需要外掛協助，但當WordPress軟體更新時、需要重新進行修改（更新會覆寫此檔案，原有改動會消失不見）。  

# 解決方案二：修改/wp-includes/cron.php  

這次折騰的是/wp-includes下的作業排程設定檔：cron.php，呵呵。  
![2014-09-28-03.png]({{ page.imgurl }}/2014-09-28-03.png)  

以「timeout」作為關鍵字搜尋以下內容：  
![2014-09-28-04.png]({{ page.imgurl }}/2014-09-28-04.png)  

將「timeout」後面的「0.01」換成較大的數，比如：「20.00」，讓超時時間變得長一點。  
不過呢，經過測試，此方案基本不能解決問題，原因亦未知。  

# 解決方案三：使用外掛修正  

外掛名稱：WP Missed Schedule Fix Future Posts Scheduled Failed  
外掛首頁：<https://wordpress.org/plugins/wp-missed-schedule/>  
登入WordPress後台，點選：外掛->安裝外掛。在安裝外掛頁面上，以「WP Missed Schedule」或「WP Missed Schedule Fix Future Posts Scheduled Failed」搜尋外掛，即可找到此外掛。安裝並啟用之，「錯失排程」的問題應該就不會出現。目前採用的正是這個方案。  

# 最終方案B：手動更新文章  

上述方案（方案二pass），基本可以解決「錯失排程」的問題；不過呢，萬一要是還是出現了，也可以手動修正哦，方法如下：  

進入需要修正的文章編輯頁面，在「發表」的設定框下，單擊「更新」以更新文章的狀態。  
![2014-09-28-05.png]({{ page.imgurl }}/2014-09-28-05.png)  

記得之前說過的麼？[如果排程時間落後於當前時間，WordPress會將文章加入「立刻發表」佇列，並將其發表]({{ site.baseurl }}/archive/wordpress-schedule-auto-post.html#schedule-time-before-current-time)：  
![2014-09-28-06.png]({{ page.imgurl }}/2014-09-28-06.png)  

待WordPress更新完狀態，就會發現文章已經發表啦：  
![2014-09-28-07.png]({{ page.imgurl }}/2014-09-28-07.png)   

雖然不能一勞永逸，但絕對可以保證將問題解決。嘛～這就是所謂的最終殺手鐧啦～  
