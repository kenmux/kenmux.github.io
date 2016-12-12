---
title: "二〇一五年四十七週工作拾遺"
date: 2015-11-29 18:00:00 +0800
categories: [Programming]
tags: [iOS]
comments: true
---

拾遺其二、主要記述iOS程式除錯的點點滴滴、內容稍顯瑣碎：  

- 其一、[擷取頂層視圖控制器]({{ page.url }}#get-top-view-controller)  
- 其二、[dismissViewControllerAnimated:completion:不工作]({{ page.url }}#dismissViewControllerAnimated-completion-not-work)  
- 其三、[XCode6.0中@import出錯]({{ page.url }}#xcode6-at-import-error)  
- 其四、[使用NSInteger格式字串的問題]({{ page.url }}#format-string-issue-using-NSInteger)  
- 其五、[UIPickerView的高度無法更改]({{ page.url }}#uipickerview-height-not-changeable)  
- 其六、[啟用檔案分享功能]({{ page.url }}#enable-file-sharing)  

<!-- more -->

# <a name="get-top-view-controller"></a>擷取頂層視圖控制器

{% highlight objc %}
+ (UIViewController *)topViewController {
    @synchronized(self) {
        return [self topViewController:[UIApplication sharedApplication].keyWindow.rootViewController];
    }
}

+ (UIViewController *)topViewController:(UIViewController *)rootViewController {
    if (rootViewController.presentedViewController == nil) {
        return rootViewController;
    }
    
    if ([rootViewController.presentedViewController isKindOfClass:[UINavigationController class]]) {
        UINavigationController *navigationController = (UINavigationController *)rootViewController.presentedViewController;
        UIViewController *lastViewController = [[navigationController viewControllers] lastObject];
        return [self topViewController:lastViewController];
    }
    
    UIViewController *presentedViewController = (UIViewController *)rootViewController.presentedViewController;
    return [self topViewController:presentedViewController];
}
{% endhighlight %}

# <a name="dismissViewControllerAnimated-completion-not-work"></a>dismissViewControllerAnimated:completion:不工作

在程式除錯時，發現、在顯示Alert視圖後、呼叫`dismissViewControllerAnimated:completion:`會出現異常。  
原因：呼叫的視圖控制器不在頂層，可以採用[上節方法]({{ page.url }}#get-top-view-controller)進行檢查。  

# <a name="xcode6-at-import-error"></a>XCode6.0中@import出錯

{% highlight objc %}
@import Foundation;
{% endhighlight %}

出現錯誤信息為：  

> .../Categories/NSString+Additions.h:9:1: Use of '@import' when modules are disabled

參照stackoverflow這個[解答](https://stackoverflow.com/a/25883210/2518851)，若問題還未解決，則可以做如下修改：

{% highlight objc %}
//@import Foundation;
#import <Foundation/Foundation.h>
{% endhighlight %}

即以`#import`取代`@import`。  

# <a name="format-string-issue-using-NSInteger"></a>使用NSInteger格式字串的問題  

{% highlight objc %}
label.text = [NSString stringWithFormat:@"section: %d, row: %d, text: %@", indexPath.section, indexPath.row, text];
{% endhighlight %}

出現警告信息為：  

> .../ViewController.m:103:115: Values of type 'NSInteger' should not be used as format arguments; add an explicit cast to 'long' instead

對於這個問題，stackoverflow給了[解釋](https://stackoverflow.com/a/16076218/2518851)：  

> 如果在64位元的OS X進行編譯、你就會遇到這個警告，因為在該平臺上`NSInteger`為`long`、即64位元的整數。另一方面，格式`%d`，則為`int`、即32位元的整數。因而，格式與實際參數、兩者的長度並不匹配。  
> 由於`NSInteger`在不同的平臺上可以為32或者64位元，故而編譯器建議強制轉化為`long`。  

同樣，stackoverflow也給了[解決方案](https://stackoverflow.com/a/4405041/2518851)：  

> 有符號整數使用`%zd`，無符號整數使用`%tu`，十六進制數使用`%tx`。  

# <a name="uipickerview-height-not-changeable"></a>UIPickerView的高度無法更改  

stackoverflow給的[解答](https://stackoverflow.com/a/12473679/2518851)：

> UIPickerView可用的高度僅有三種：162.0、180.0以及216.0。  

因而、當發現`UIPickerView`的高度不可更改時、請檢視下高度值是否可用。  

# <a name="enable-file-sharing"></a>啟用檔案分享功能  

參考stackoverflow這個[解答](https://stackoverflow.com/a/6029954/2518851)：  

1. 打開專案的`Info.plist`，新增名為`UIFileSharingEnabled`的鍵，並設其值為`YES`。  
2. 若仍不可用，則檢查下名為`CFBundleDisplayName`的鍵。沒有則新建之，注意其值類別為字串。  
