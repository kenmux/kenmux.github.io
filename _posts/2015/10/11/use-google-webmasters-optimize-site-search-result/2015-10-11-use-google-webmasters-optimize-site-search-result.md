---
title: "使用Google網站管理員最佳化網站搜尋結果"
date: 2015-10-11 18:00:00 +0800
categories: [Website]
tags: [Google Webmasters,Search Console,Search Result Optimization]
imgurl: /assets/imgs/2015/10/11/use-google-webmasters-optimize-site-search-result
comments: true
---

這篇筆記簡單介紹了如何使用Google網站管理員來最佳化網站搜尋結果，即告知Google如何正確索引網站。  

若要使用「Google網站管理員」，則需具備以下先決條件：  

- 擁有Google帳號  
- 持有網站擁有權  

內容索引：  

- [Google網站管理員]({{ page.url }}#google-webmasters)  
- [驗證網站擁有權]({{ page.url }}#verify-site-ownership)  
- [使用Search Console]({{ page.url }}#use-search-console)  
- [移除過舊的內容]({{ page.url }}#remove-outdated-content)  
- [編後語]({{ page.url }}#additional-words)<!-- more -->  

# <a name="google-webmasters"></a>Google網站管理員  

[Google網站管理員](https://www.google.com/webmasters/)是Google推出的針對網站的最佳化工具；通過它，網站維護者可以取得相關資料、工具並對網站進行診斷，從而使其成為一個可讓Google順利檢索的優質網站。概觀如下圖所示：  
![2015-10-11-01.png]({{ page.imgurl }}/2015-10-11-01.png)  

# <a name="verify-site-ownership"></a>驗證網站擁有權  

訪問[網站管理員中心](https://www.google.com/webmasters/verification/home?pli=1)，點擊「新增資源」，填入網址，點擊「繼續」以添加網站：  
![2015-10-11-02.png]({{ page.imgurl }}/2015-10-11-02.png)  

添加完畢後、「網站管理員中心」嘗試去驗證網站擁有權。若驗證失敗、則會有如下提示：  
![2015-10-11-03.png]({{ page.imgurl }}/2015-10-11-03.png)  

由於域名xiwan.info購自[GoDaddy](https://www.godaddy.com/)，而DNS代管則放在了[CloudFlare](https://www.cloudflare.com/)，驗證不過尚在預料之中。這時就需要手動進行驗證。「網域名稱供應商」那列選擇「其他」、記下TXT記錄、然後Hold住：  
![2015-10-11-04.png]({{ page.imgurl }}/2015-10-11-04.png)  

訪問CloudFlare、登入帳號、在DNS記錄裡添加一條TXT記錄，「Name」設為「@」、「Value」設為上述TXT記錄，如下圖所示：  
![2015-10-11-05.png]({{ page.imgurl }}/2015-10-11-05.png)  

靜候DNS刷新後、再次回到驗證頁面、點擊「驗證」即可：  
![2015-10-11-06.png]({{ page.imgurl }}/2015-10-11-06.png)  

注：一般情況下、若名稱伺服器採用預設值、GoDaddy會把一切料理的妥妥當當的、驗證都不會有問題的、所以安啦！  

# <a name="use-search-console"></a>使用Search Console  

在使用Search Console之前、有必要了解一下Google對網站的索引情況、做到心中有數。使用`site:`搜尋即可：  
![2015-10-11-07.png]({{ page.imgurl }}/2015-10-11-07.png)  

可以看到、「西灣筆記」已經擁有Google索引、而第一項就是[Search Console](https://www.google.com/webmasters/tools/home?pli=1)的連結。點擊連結、或者通過「Google網站管理員」，都可進入。點擊「新增內容」、添加網站；點擊網站縮略圖、查看詳情：  
![2015-10-11-08.png]({{ page.imgurl }}/2015-10-11-08.png)  

資訊主頁、網站狀態一目了然：  
![2015-10-11-09.png]({{ page.imgurl }}/2015-10-11-09.png)  

但凡新添的網站、都會收到有關改進意見的訊息，如下所示：  
![2015-10-11-10.png]({{ page.imgurl }}/2015-10-11-10.png)  

在這些意見當中、比較有用的是：1、2、4（個人意見）。需要特別注意的是，第一條指出了：對於Google而言、xiwan.info和www.xiwan.info是兩個完全不同的網站；而且、https同樣也享有這一「禮遇」。  

因而，對於支持https的網站，應該這樣添加到Search Console：  
![2015-10-11-11.png]({{ page.imgurl }}/2015-10-11-11.png)  

通過「檢索->檢索錯誤」，可以為網站把把脈、看看都存在哪些問題：  
![2015-10-11-12.png]({{ page.imgurl }}/2015-10-11-12.png)  

其他諸如sitemap.xml、robots.txt的管理，如果提示找不到相應的檔案，則可自行添加。若添加失敗、且檢查完全正確、則可重複嘗試直到成功為止。這裡不再贅述。  

# <a name="remove-outdated-content"></a>移除過舊的內容  

當網站內容、網址結構發生變動後、有些擁有Google索引的內容可能無法訪問了、這時就需要告知Google將其移除之。  

Google提供了[請求頁面](https://www.google.com/webmasters/tools/removals?pli=1)，如下圖所示：  
![2015-10-11-13.png]({{ page.imgurl }}/2015-10-11-13.png)  

輸入待移除內容的網址、點擊「提出移除要求」，如下圖所示：  
![2015-10-11-14.png]({{ page.imgurl }}/2015-10-11-14.png)  

需要注意的是、Google並不會對你「言聽計從」，而是通過一套演算法來決定是否接受要求。一般說來，如果持有網站擁有權、並且待移除內容確實無法訪問、則要求一般都會接受。  

注：  

- 切勿濫用要求。若是網站私有資源、建議採用robots.txt進行封鎖。  
- <del>提出移除要求過於頻繁則可能影響Google索引頻度。切身體會、在頻繁提出要求後、Google索引數目遲遲不見增加。</del> 【無法求證】  
- 有時標識為「已移除」的網址可能在「檢索錯誤」中再次出現（並且有可能重複多次），再次提請移除即可。  
- 作為網站管理員、時時查看[Search Console](https://www.google.com/webmasters/tools/home?pli=1)不失為一個好習慣。  

# <a name="additional-words"></a>編後語  

以上提到的各色工具、雖然都放在了`/webmasters/`下，但是、除了Search Console外、「Google網站管理員」並未提供相應的入口連結、而使其在使用上稍顯不便。  

同時、這也引人遐想：莫非、「Google網站管理員」並非完全體？Google會不會有統合計劃？且拭目以待之！  
