---
title: "移除Project中缺失的Target"
date: 2014-08-31 18:00:00 +0800
categories: [iOS]
tags: [iOS7,Xcode5,Scheme]
imgurl: /assets/imgs/2014/08/31/remove-missing-target-in-project
comments: true
---

在Project初創和後續開發過程中，難免會增刪Target，但這樣會殘留很多無用的設置；有時會遇到如下情形，在Project中，還殘留著已經移除的Target (<span style="color:red">missing</span>)：  
![2014-08-31-01.png]({{ page.imgurl }}/2014-08-31-01.png)  
那麼，該如何做、才能移除呢？答案是：修改Scheme檔！<!-- more -->  

眾所週知，Project檔(`*.xcodeproj`)實為檔案夾，Scheme檔(`*.xcscheme`)就存放其中。在Project檔上右鍵單擊，選擇「顯示套件內容」，即可查看其內容。  
![2014-08-31-02.png]({{ page.imgurl }}/2014-08-31-02.png)  

共享的Scheme檔存放在：xcshareddata/xcschemes/，而非共享的則存放在：xcuserdata/$USER.xcuserdatad/xcschemes。  
![2014-08-31-03.png]({{ page.imgurl }}/2014-08-31-03.png)  

用Xcode打開Scheme檔，以Target的名字（圖示為NawixWorksUITests）為關鍵字查找，刪除相關列即可：  
1)「<span style="color:magenta;font-weight:bold"><BuildeAction</span>」與「<span style="color:magenta;font-weight:bold"></BuildeAction></span>」之間：  
![2014-08-31-04.png]({{ page.imgurl }}/2014-08-31-04.png)  

2)以及「<span style="color:magenta;font-weight:bold"><TestableReference</span>」與「<span style="color:magenta;font-weight:bold"></TestableReference></span>」之間：  
![2014-08-31-05.png]({{ page.imgurl }}/2014-08-31-05.png)  

注：有時Scheme只有如上情形之一，此亦為正常。  

保存後退出，重新打開Project，可以看到，缺失的Target已經不見啦：LOL  
![2014-08-31-06.png]({{ page.imgurl }}/2014-08-31-06.png)  
