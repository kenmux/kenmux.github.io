---
title: "WordPress快速架站指南"
date: 2014-09-14 18:00:00 +0800
categories: [Website]
tags: [Wordpress,Hosting]
imgurl: /assets/imgs/2014/09/14/wordpress-quick-setup-reference
comments: true
---

# 架站需要什麼  

其一，WordPress軟體。訪問[WordPress台灣正體中文網站](http://tw.wordpress.org/)，下載所需的軟體壓縮檔。  
其二，WordPress運行環境。一般為虛擬主機，在網路上有許多免費的。以Linux作業系統為佳，PHP+MySQL+Apache為必要條件（不同於純粹的VPS，網路上許多虛擬主機默認會提供WordPress相容的應用環境，更有甚者直接提供cPanel等管理後台）。<!-- more -->  

# 免費虛擬主機  

網路上免費的虛擬主機多不勝數，這裡只列舉我試用過的：  

- [Byethost](https://byethost.com/index.php/free-hosting)  
- [MegaByet](http://megabyet.com/)  
- [獅子的免費虛擬主機](http://www.lionfree.net/)  
    
Byethost自不用多說，老字號的免費網頁空間供應商，經營多年，也擁有相當數量的分銷商，MegaByet、獅子的免費虛擬主機即為其二。這類供應商，一般都支援電子郵件註冊、提供至少1000MB空間、50000MB月流量、以及cPanel虛擬主機管理後台等等，同質化相當嚴重。特別值得一說的是獅子的免費虛擬主機，提供了高達10000MB的空間、以及100000MB月流量，夠慷慨的吧？而且據說，是由台灣一名國中生創立的哦，真的很了不起呢。  
  
Hostinger又是另一家免費網頁空間供應商，目前也把業務擴展到了香港。提供了2000MB主機空間、100000MB月流量，特色是支援Google、Facebook等帳號關聯註冊，而不用去額外創建帳號。不過，美中不足的是：只支援簡化字，而無正體字（我直接切換成了英語）。  
  
本站初始也是在獅子主機上的哦，不過在創立時(2014/08/30)不巧獅子主機正在搬家，所以只得去Byethost了。以下皆以ByetHost的免費主機為例進行說明。  

# <a name="byethost-account-details"></a>ByetHost帳號概覽  

成功申請帳號後，會收到如下通知：  
這裡包含了作為「虛擬主機」的一切：cPanel管理後台、MySQL資料庫、FTP主機空間、以及個人主頁。這裡有三個帳號哦，用戶名(username)和密碼(password)都一樣的喲，唯一不同的就是URL喔。千萬別弄混了！  
![2014-09-14-01.png]({{ page.imgurl }}/2014-09-14-01.png)  

# FTP空間上傳WordPress  

使用FTP帳號（不清楚？參考[上一節]({{ page.url }}#byethost-account-details)。）登入，並將WordPress上傳到主機空間。這裡推介一個非常好用的軟體：FileZilla，上傳下載非常便利。  
<b>注意：「登入型式」必須為「帳號」；「使用者」對應「username」，「帳號」可隨意填寫，但不能為空！</b>  
![2014-09-14-02.png]({{ page.imgurl }}/2014-09-14-02.png)  
  
登入空間後，可以看到根目錄「/」。不要上傳任何檔案到此目錄哦，子目錄「htdocs」才是WordPress的根目錄！  
![2014-09-14-03.png]({{ page.imgurl }}/2014-09-14-03.png)  

進入「htdocs」，初始一般會有默認html檔、以及說明檔。全部刪除、清空此檔案夾：  
![2014-09-14-04.png]({{ page.imgurl }}/2014-09-14-04.png)  

將下載的WordPress壓縮檔解壓，並將檔案夾`wordpress`下所有檔案全部上傳到「htdocs」下。<b>注：點選左側本地站台、`wordpress`檔案夾，Ctrl+A選擇「所有」，從左側拖放到右側即可。</b>   
![2014-09-14-05.png]({{ page.imgurl }}/2014-09-14-05.png)  

這裡補充說明下cPanel下的FTP工具：net2ftp。額，該怎麼評價呢？用這個詞吧：雞肋。  
看看應用限制吧：只支援檔案（想要上傳檔案夾？對不起！請打包，或者逐個上傳……），單個檔案最大10MB（實測最大也就2MB），任何作業必須在30秒內完成(……)。  
嘛～畢竟是依托於網路的喲。而且，在某些場景下（比如限制FTP使用的網域），這就非常有用了。  
![2014-09-14-06.png]({{ page.imgurl }}/2014-09-14-06.png)  

# 創建MySQL資料庫  

登入cPanel，點選「Databases」下的「MySQL資料庫」；填入合適的後綴名做標識（一般採用「wp」），點選「Create Datebase」，創建資料庫。  
![2014-09-14-07.png]({{ page.imgurl }}/2014-09-14-07.png)  

# 安裝WordPress  

裝過程簡單易懂：使用常用的瀏覽器（建議Chrome或者Firefox，不建議使用IE！）訪問個人主頁(Home Page)，按照提示一步步走下去即可。  
WordPress台灣正體中文網站提供了詳盡的[安裝教程](http://tw.wordpress.org/install/)。  
注：標記「<span style="color:red">√</span>」為必填項  
![2014-09-14-08.png]({{ page.imgurl }}/2014-09-14-08.png)  
