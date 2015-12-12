---
title: "WordPress備份到谷歌雲端硬碟"
date: 2014-10-05 18:00:00 +0800
categories: [Website]
tags: [WordPress,Updraftplus]
imgurl: /assets/imgs/2014/10/05/wordpress-backup-to-google-drive
comments: true
---

本文介紹如何使用Updraftplus將WordPress備份到谷歌雲端硬碟，參考自這篇文章：[备份WordPress到Google Drive: UpdraftPlus](http://blog.netsh.org/posts/wordpress-google-drive_1493.netsh.html)，感興趣的可以戳進去看看。  

# 關於Updraftplus  

外掛名稱：UpdraftPlus - Backup/Restore  
外掛首頁：<https://wordpress.org/plugins/updraftplus/>  
非常強大的WordPress備份和回復外掛，一鍵備份和回復，支持多種備份方式（ Amazon S3、Dropbox、Google Drive、FTP、Email等等），支持排程自動備份，檔案和數據庫也可以分開備份。<!-- more -->  

# 申請Google Drive API  

訪問Google Developers Console：<https://console.developers.google.com/>。點擊「Create Project」以創建專案，或者也可以復用已有專案。  

注：Updraftplus間不可以共用同一個專案！  

在專案頁面下，切換到<b>APIs&auth</b>標籤下<b>APIs</b>子標籤，將<b>Driver API</b>後隨的開關打開：
![2014-10-05-01.png]({{ page.imgurl }}/2014-10-05-01.png)  

注：不是<b>Drive SDK</b>哦，雖然都帶有「Drive」……如下圖所示：  
![2014-10-05-02.png]({{ page.imgurl }}/2014-10-05-02.png)  

切換到<b>Credentials</b>子標籤，點擊「Create new Client ID」：  
![2014-10-05-03.png]({{ page.imgurl }}/2014-10-05-03.png)  

彈出窗口，「APPLICATION TYPE」採用預設即可：  
![2014-10-05-04.png]({{ page.imgurl }}/2014-10-05-04.png)  

先Hold住，因為創建 Client ID所需的「AUTHORIZED JAVASCRIPT ORIGINS」和「AUTHORIZED REDIRECT URI」還無從知曉。  

# Updraftplus設定  

回到Updraftplus設定頁面，切換到<b>Settings</b>標籤頁，在「Choose your remote storage:」後選擇「Google Drive」：  
![2014-10-05-05.png]({{ page.imgurl }}/2014-10-05-05.png)  

注意到以`http://`起始、被加以灰色背景的字符串了麼？這就是創建Client ID所需的URI！把它拷貝到剪貼板上去，再回到先前「Create new Client ID」頁面吧。  

# Create new Client ID  

「AUTHORIZED JAVASCRIPT ORIGINS」下的文本框中填入WordPress地址，而 「AUTHORIZED REDIRECT URI」下的文本框中則填入先前獲取的URI即可。點擊「Create Client ID」創建Client ID。  

注：不要開啟兩步驗證，因為這樣會給備份/回復徒增不必要的麻煩。  

待創建成功後：  
![2014-10-05-06.png]({{ page.imgurl }}/2014-10-05-06.png)  

如此，設定Updraftplus所需的Client ID和Client Secret也就得到了，將它們複製到剪貼板上，再回到Updraftplus設定頁面。  

注：若不慎填錯，仍然可以通過點擊「Edit settings」，在彈出窗口中進行修改：  
![2014-10-05-07.png]({{ page.imgurl }}/2014-10-05-07.png)  

# 完成Updraftplus設定  

再次回到Updraftplus設定頁面，將獲得的Client ID和Client Secret填入，並點擊「Save Change」以繼續：  
![2014-10-05-08.png]({{ page.imgurl }}/2014-10-05-08.png)  

Updraftplus試圖向Google Drive發出驗證請求，如果驗證成功的話、頁面會顯示「You appear to be already authenticated」：  
![2014-10-05-09.png]({{ page.imgurl }}/2014-10-05-09.png)  

這樣、Updraftplus設定也就成功了，可以進行備份了。  

# Updraftplus FAQ連結  

不要試圖在維護模式(Maintenance mode)下運作Updraftplus，因為它並不能正常工作。  
遇到其他問題了麼？參考這個連結：[My scheduled backups do nothing, or “Backup Now” stops mid-way](http://updraftplus.com/faqs/my-scheduled-backups-do-nothing-backup-now-stops-midway/)。  
