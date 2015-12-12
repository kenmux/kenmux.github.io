---
title: "WordPress網站手動搬家"
date: 2014-10-19 18:00:00 +0800
categories: [Website]
tags: [WordPress,Migration]
imgurl: /assets/imgs/2014/10/19/wordpress-migrate-to-new-host
comments: true
---

前面提過，本站原來是使用byethost主機的，後來出了點小插曲，改用hostinger主機了。期間手動做了次網站搬家，算是寶貴的經驗吧，於是做了下筆記。  

WordPress網站搬家，主要涉及到四個方面：  

- [MySQL資料庫搬移]({{ page.url }}#mysql-database-migrate)  
- [虛擬主機檔案搬移]({{ page.url }}#wordpress-files-migrate)  
- [wp-config.php設定]({{ page.url }}#wp-config-setting)  
- [網域重新綁定]({{ page.url }}#domain-bind-again)  

注：這裡，舊主機為byethost主機，新主機為hostinger主機。當然了，對於任何主機來說，步驟都大致相同。<!-- more -->  

# <a name="mysql-database-migrate"></a>MySQL資料庫搬移  

登入舊主機後台管理程式（這裡是vpanel面板），找到資料庫管理選單：  
![2014-10-19-01.png]({{ page.imgurl }}/2014-10-19-01.png)  

選擇「MySQL資料庫」，瀏覽所有資料庫：  
![2014-10-19-02.png]({{ page.imgurl }}/2014-10-19-02.png)  

點擊WordPress資料庫後面的「Admin」，啟動phpMyAdmin：  
![2014-10-19-03.png]({{ page.imgurl }}/2014-10-19-03.png)  

這怪怪的翻譯……不吐槽了，byethost就是這樣的。點擊「導出」以輸出SQL資料庫檔案：  
![2014-10-19-04.png]({{ page.imgurl }}/2014-10-19-04.png)  

這裡都採用預設選項，直接點擊「執行」備份資料庫檔案到本地。注：也可以採用壓縮檔，以節約傳輸時間。  
登入新主機後台管理程式（這裡是cpanel面板），找到資料庫管理選單：  
![2014-10-19-05.png]({{ page.imgurl }}/2014-10-19-05.png)  

點擊「MySQL Databases」，瀏覽所有資料庫。創建空白資料庫，用以輸入舊主機的資料庫檔案。  
注：請勿嘗試將資料庫檔案輸入到非空資料庫！因為這樣很容易導致衝突而失敗。  
![2014-10-19-06.png]({{ page.imgurl }}/2014-10-19-06.png)  

點擊「+」以展開資料庫管理選單：  
![2014-10-19-07.png]({{ page.imgurl }}/2014-10-19-07.png)  

點擊「phpMyAdmin」以啟動phpMyAdmin：  
![2014-10-19-08.png]({{ page.imgurl }}/2014-10-19-08.png)  

點擊「輸入」：  
![2014-10-19-09.png]({{ page.imgurl }}/2014-10-19-09.png)  

點擊「瀏覽…」，添加本地資料庫檔案後，點擊「執行」以繼續。當輸入成功時，提示介面如下：  
![2014-10-19-10.png]({{ page.imgurl }}/2014-10-19-10.png)  
如此，便完成了MySQL資料庫的搬移。哦，忘了說了，記住新資料庫相關的設置(Host, Name, User, Password)哦。下面有用到。  

# <a name="wordpress-files-migrate"></a>虛擬主機檔案搬移  

注：為了節約時間，完全可以只下載wp-content目錄下的所有檔案；不過，這裡採用的是全站搬移，因為有些自定義檔案（比如favicon.ico）並不在那個目錄下。  
使用FileZilla登入舊主機，在WordPress根目錄上右鍵，選擇「下載」，將檔案備份到本地：  
![2014-10-19-11.png]({{ page.imgurl }}/2014-10-19-11.png)  

待下載完成之後，再登入新主機，將WordPress檔案悉數上傳到根目錄：  
![2014-10-19-12.png]({{ page.imgurl }}/2014-10-19-12.png)  

# <a name="wp-config-setting"></a>wp-config.php設定  

因為新主機的資料庫難免會跟原來的不一樣（或者說，肯定會不一樣）。所以需要修改檔案wp-config.php內有關資料庫的設定。若不修改，WordPress網站會因讀取資料庫失敗而掛掉哦～  
上一步已經將WordPress檔案上傳到新主機上了，讓我們趁熱打鐵。在wp-config.php檔案上右鍵，選擇「檢視/編輯」：   
![2014-10-19-13.png]({{ page.imgurl }}/2014-10-19-13.png)  

以「DB_NAME」、「DB_USER」、「DB_PASSWORD」、「DB_HOST」，檢視並修改其設定值，如下圖所示：  
![2014-10-19-14.png]({{ page.imgurl }}/2014-10-19-14.png)  
保存後關閉編輯器、並將改動覆寫回主機即可。  

# <a name="domain-bind-again"></a>網域重新綁定  

既然主機已經換了，那麼網域就需要再次綁定咯。之前有篇文章是關於綁定網域的，這裡不再贅述，附上傳送門：[WordPress網站綁定網域名稱]({{ site.baseurl }}/archive/wordpress-bind-domain-name.html)。  

做到這裡，搬家算是全部完工。  
