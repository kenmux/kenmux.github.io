---
title: "Windows磁碟管理工具：Diskpart"
date: 2014-11-09 18:00:00 +0800
categories: [Software]
tags: [Windows,Diskpart]
imgurl: /assets/imgs/2014/11/09/windows-disk-manager-tool-diskpart
comments: true
---

本文介紹的是，Windows內建的基於命令列的磁碟管理工具：Diskpart。  

# 關於 Diskpart  

Diskpart，亦即 Diskpart.efi 公用程式，內建於Windows XP及其後的視窗作業系統中；此公用程式可讓您從指令碼、遠端工作階段、或其他命令提示字元來設定儲存裝置。  
Diskpart和許多指令行公用程式不同，因為它不是在「單行」模式中操作。相反的，此公用程式是先啟動，然後從標準輸入/輸出(I/O)讀取指令。這些指令可以導向任何磁碟、磁碟分割或磁碟區。  
參見：<http://support.microsoft.com/kb/300415/zh-tw> <!-- more -->  

# 使用Diskpart  

在命令提示字元中輸入「diskpart」並回車即可啟動Diskpart。  
注：a)Diskpart需要在管理員權限下運作；b)命令不分大小寫。  
![2014-11-09-01.png]({{ page.imgurl }}/2014-11-09-01.png)   

不知道命令？輸入「?」並回車看看吧？  
![2014-11-09-02.png]({{ page.imgurl }}/2014-11-09-02.png)  

首先呢，羅列出目前電腦裡面所有的磁碟吧。輸入「list disk」並回車即可：  
![2014-11-09-03.png]({{ page.imgurl }}/2014-11-09-03.png)  

接下來選擇要管理的磁碟。這裡以8G（實際容量7498MB）容量的隨身碟為例吧：Diskpart給予的編號為「Disk 1」，因而輸入「select disk 1」即可：  
![2014-11-09-04.png]({{ page.imgurl }}/2014-11-09-04.png)  

為了避免選錯對象，可以再次輸入「list disk」並回車以確認。被選中的磁碟最前方有「*」標記：  
![2014-11-09-05.png]({{ page.imgurl }}/2014-11-09-05.png)  

接下來，就是使用命令來管理磁碟啦，比較常用的是：clear和extend。輸入「exit」並回車即可退出Diskpart。  
