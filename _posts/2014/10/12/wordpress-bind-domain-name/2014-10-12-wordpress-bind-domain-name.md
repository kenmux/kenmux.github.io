---
title: "WordPress網站綁定網域名稱"
date: 2014-10-12 18:00:00 +0800
categories: [Website]
tags: [WordPress,Domain Name,Nameserver]
imgurl: /assets/imgs/2014/10/12/wordpress-bind-domain-name
comments: true
---

本文介紹如何給WordPress網站綁定Godaddy網域名稱，涉及到三個方面的設定：  

- <a href="{{ page.url }}#vps-setting">虛擬主機</a>  
- <a href="{{ page.url }}#godaddy-domain-name-setting">Godaddy網域名稱</a>  
- <a href="{{ page.url }}#wordpress-setting">WordPress軟體</a>  

本文假定已經擁有了Godaddy網域名稱，故不再贅述如何購買Godaddy網域名稱。  
注：本文圖例中，虛擬主機為Hostinger主機、Godaddy網域名稱為XIWAN.INFO。<!-- more -->  

# <a name="vps-setting"></a>虛擬主機設定  

首先，獲取IP位址(IP Address)與名稱伺服器(nameserver)，這都由主機供應商給出：  
![2014-10-12-01.png]({{ page.imgurl }}/2014-10-12-01.png)  

接著就是設定寄放網域(Parked Domains)了。在主機後台管理程式裡面，一般都有管理「Domains」的選單列，單擊「Parked Domains」開始設定：  
![2014-10-12-02.png]({{ page.imgurl }}/2014-10-12-02.png)  

將已購Godaddy網域名稱加進來：  
![2014-10-12-03.png]({{ page.imgurl }}/2014-10-12-03.png)  
注：不要擅自加入「WWW」等主機名稱。  

# <a name="godaddy-domain-name-setting"></a>Godaddy網域名稱設定  

訪問Godaddy：<https://www.godaddy.com/>。登入帳號，開始設定網域名稱。這裡，要進行設定的有兩個地方：名稱伺服器(Nameservers)和主機名稱(Host Names)。  
![2014-10-12-04.png]({{ page.imgurl }}/2014-10-12-04.png)  

首先是名稱伺服器，單擊下方「Manage」進行設定。將之前獲取的名稱伺服器加進來：  
![2014-10-12-05.png]({{ page.imgurl }}/2014-10-12-05.png)  

接著是主機名稱，將虛擬主機的IP位址加進來。注：在這裡「Hostname」一般都採用「WWW」，這也是預設值。  
![2014-10-12-06.png]({{ page.imgurl }}/2014-10-12-06.png)  

# <a name="wordpress-setting"></a>WordPress設定  

最後便是WordPress設定了。<b>強烈建議在確定網際網路中DNS刷新之後再進行此項設定（即：能夠通過綁定的網域名稱訪問虛擬主機。當然、妳應當保證本機域名伺服器快取(DNS Cache)為最新）。</b>登入WordPress網誌管理後台，單擊「設定」標籤下「一般」子標籤，在設定頁面下將綁定的網域名稱加進來：  
![2014-10-12-07.png]({{ page.imgurl }}/2014-10-12-07.png)  

這樣造訪網站文章時，瀏覽器位址欄也會顯示綁定的網域名稱，而不是主機的原網域名稱。做到這裡，綁定網域名稱全部完工。  
