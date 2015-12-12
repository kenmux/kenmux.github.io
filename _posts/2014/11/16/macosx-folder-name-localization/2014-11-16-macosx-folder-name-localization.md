---
title: "Mac OS X檔案夾名稱本土化"
date: 2014-11-16 18:00:00 +0800
categories: [OS]
tags: [Mac OS X,Localization,Chinese]
imgurl: /assets/imgs/2014/11/16/macosx-folder-name-localization
comments: true
---

當打開Finder時，會看到一些系統預設檔案夾名稱顯示為中文（如「下載項目」、「圖片」等等），而實際上它們都是英文的。從英文到中文，這就是「本土化」。當然，我們也可以對自己的檔案夾做同樣的「本土化」。本文即以Mac OS X 10.9.4為例，講解如何進行檔案夾名稱的「本土化」。<!-- more -->  

首先需要找到本土化字串的資源檔案，其路徑為：  

{% highlight text %}
/System/Library/CoreServices/SystemFolderLocalizations/zh_TW.lproj/SystemFolderLocalizations.strings
{% endhighlight %}

<a name="edit-the-file"></a>第二步是編輯此檔案。由於此檔案是二進制格式的，所以需要先使用`plutil`轉換為xml格式，轉換時需管理者權限（Mac OS X El Capitan請先[移步這裡]({{ page.url }}#mac-os-x-el-capitan-modify-system-files)）：  

{% highlight text %}
$ sudo plutil -convert xml1 SystemFolderLocalizations.strings
{% endhighlight %}

轉換後，就可以直接採用文字編輯器編輯了。下圖採用`Vim`進行編輯：  
![2014-11-16-01.png]({{ page.imgurl }}/2014-11-16-01.png)  

然後增加「key」和「string」，例如我添加了「MyApps」和「我的應用」、「Workspaces」和「工作臺」：  

{% highlight text %}
<key>MyApps</key>
<string>我的應用</string>
<key>Workspaces</key>
<string>工作臺</string>
{% endhighlight %}

之後，再把檔案轉換回二進制格式：  

{% highlight text %}
$ sudo plutil -convert binary1 SystemFolderLocalizations.strings
{% endhighlight %}

最後一步，在需要本土化的檔案夾下，新建名為`.localized`的檔案（實測：同名的檔案夾也可以），標明此檔案夾需要本土化。  

{% highlight text %}
$ touch .localized
{% endhighlight %}

如此這般、檔案夾本土化就算是完工了。

注：當系統更新時，所有設置都會回復，需要重新進行設置。  

---

# <a name="mac-os-x-el-capitan-modify-system-files"></a>2015/10/11更新、Mac OS X El Capitan適用  

Mac OS X El Capitan引入了[SIP (System Integrity Protection)](https://en.wikipedia.org/wiki/System_Integrity_Protection)，對系統檔案的保護較先前版本更進一步，因而任何對檔案SystemFolderLocalizations.strings的修改都會失敗（即使是管理者也不行）：

{% highlight text %}
nawix-osx:zh_TW.lproj kenmux$ sudo plutil -convert xml1 SystemFolderLocalizations.strings 
Password:
SystemFolderLocalizations.strings: Operation not permitted
nawix-osx:zh_TW.lproj kenmux$ sudo chmod a+w SystemFolderLocalizations.strings 
chmod: Unable to change file mode on SystemFolderLocalizations.strings: Operation not permitted
nawix-osx:zh_TW.lproj kenmux$ ls -l
total 8
-rw-r--r--  1 root  wheel  1819 10 10 15:02 SystemFolderLocalizations.strings
{% endhighlight %}

由此可見、除了root使用者外，其他使用者都無法修改；而且、管理者也無法修改此檔案的權限。  

為了能夠順利修改此檔案，首先想到的是：[啟用root使用者](https://support.apple.com/zh-tw/HT204012)。  

然而，切換到root使用者後、再次進行嘗試，依舊不行：  

{% highlight text %}
nawix-osx:zh_TW.lproj kenmux$ su
Password:
sh-3.2# plutil -convert xml1 SystemFolderLocalizations.strings
SystemFolderLocalizations.strings: Operation not permitted
sh-3.2# 
{% endhighlight %}

此時、唯一能想到的就是：關閉SIP（僅在回復系統下）。  

[進入回復系統](https://support.apple.com/zh-tw/HT201314)後，從上方的選單列、選擇「工具程式->終端機」，並執行：  

{% highlight text %}
-bash-3.2# csrutil disable
Successfully disabled System Integrity Protection. Please restart the machine for the changes to take effect.
-bash-3.2# 
{% endhighlight %}

注：請勿嘗試在回復系統下修改系統檔案（測試了下、無法修改）。

接下來，選擇「![2014-11-16-02.png]({{ page.imgurl }}/2014-11-16-02.png)->重新開機」以重啟機器、並登入系統。此時，切換到root使用者、就可以修改此檔案，與[上述步驟]({{ page.url }}#edit-the-file)相同，只是無需`sudo`指令；修改完畢後，再切換回標準使用者。  

注：  

- 由此可知：在Mac OS X El Capitan下，若要修改系統檔案、需要關閉SIP且啟用root使用者。  
- 無論是關閉SIP，還是啟用root使用者，都只是暫時救急，應在完工後立即回復。  

再次啟用SIP（同樣是在回復系統下）：  

{% highlight text %}
-bash-3.2# csrutil enable
Successfully enabled System Integrity Protection. Please restart the machine for the changes to take effect.
-bash-3.2# 
{% endhighlight %}

下一步，選擇「![2014-11-16-02.png]({{ page.imgurl }}/2014-11-16-02.png)->重新開機」以重啟機器即可。  

---
