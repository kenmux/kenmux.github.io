---
title: "二〇一五年四十二週工作拾遺"
date: 2015-10-25 18:00:00 +0800
categories: [Programming]
tags: [iOS 8,Header,Footer]
imgurl: /assets/imgs/2015/10/25/2015-the-42nd-week-work-note
comments: true
---

從本篇開始、在下會時不時整理一些名為「工作拾遺」的筆記。「遺」者、吾所闕也；「拾遺」者、補吾所闕也。  

iOS提供了UITableview這樣的使用者介面。如果畫個簡圖的話、應當是這樣的：

![2015-10-25-01.png]({{ page.imgurl }}/2015-10-25-01.png)  

這篇筆記，與Header/Footer相關，主要講述：  

- 其一、[Table Header/Footer實作]({{ page.url }}#table-header-footer-implementation)  
- 其二、[Section Header/Footer背景色設置]({{ page.url }}#section-header-footer-backgroundcolor-setting)  

<!-- more -->  

# <a name="table-header-footer-implementation"></a>Table Header/Footer實作  

方法一、在程式中呼叫`initWithFrame:`、`addSubview:`等函式直接實作。參考範例如下：  

{% highlight objc linenos %}
// Header
UIView *tableHeaderView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 50)];
[tableHeaderView setBackgroundColor:[UIColor blueColor]];
UILabel *headerLabel = [[UILabel alloc] initWithFrame:CGRectMake(10,10,320,25)];
headerLabel.text = @"Header view";
headerLabel.textColor = [UIColor whiteColor];
headerLabel.font = [UIFont boldSystemFontOfSize:22];
headerLabel.backgroundColor = [UIColor clearColor];
[tableHeaderView addSubview:headerLabel];
[self.tableView setTableHeaderView:tableHeaderView];

// Footer
UIView *tableFooterView = [[UIView alloc] initWithFrame:CGRectMake(0,0, 320,50)];
[tableFooterView setBackgroundColor:[UIColor blueColor]];
UILabel *footerLabel = [[UILabel alloc] initWithFrame:CGRectMake(10,10,320,25)];
footerLabel.text = @"Footer view";
footerLabel.textColor = [UIColor whiteColor];
footerLabel.font = [UIFont boldSystemFontOfSize:22];
footerLabel.backgroundColor = [UIColor clearColor];
[tableFooterView addSubview:footerLabel];
[self.tableView setTableFooterView:tableFooterView];
{% endhighlight %}

注：  

- Header/Footer的寬度會被拉伸/壓縮、而高度則不會！（方法二同樣適用！）  
- 「完全」隱藏Table Header/Footer的方法、是採用面積為0的空視圖：  

{% highlight objc linenos %}
// Hide table header
[self.tableView setTableHeaderView:[[UIView alloc] initWithFrame:CGRectZero]];

// Hide table footer (hide empty cells at the end of table also)
[self.tableView setTableFooterView:[[UIView alloc] initWithFrame:CGRectZero]];
{% endhighlight %}

方法二、採用xib檔、結合呼叫函式`initWithFrame:`來實作。  

以Header為例。首先、實作Table Header視圖類別：`TableHeaderView`。  

表頭檔(TableHeaderView.h)如下：  

{% highlight objc linenos %}
#import <UIKit/UIKit.h>

@interface TableHeaderView : UIView

@end
{% endhighlight %}

實作檔(TableHeaderView.m)如下：  

{% highlight objc linenos %}
#import "TableHeaderView.h"

@interface TableHeaderView ()

@property (strong, nonatomic) IBOutlet UIView *view;
@property (weak, nonatomic) IBOutlet UILabel *textLabel;

@end

@implementation TableHeaderView

- (instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];

    if (self) {
        [[NSBundle mainBundle] loadNibNamed:@"TableHeaderView" owner:self options:nil];
        self.view.frame = CGRectMake(0, 0, self.frame.size.width, self.frame.size.height);
        [self addSubview:self.view];
        
        _textLabel.text = @"table header";
    }
    
    return self;
}

@end
{% endhighlight %}

然後、建立xib檔(TableHeaderView.xib)。關鍵點有二：  

- 「File's Owener」設為視圖類別`TableHeaderView`。  
- 將整個視圖的outlet連接至實作檔內名為view的property。如下圖所示：  

![2015-10-25-02.png]({{ page.imgurl }}/2015-10-25-02.png)  

最後、實作Table Header：  

{% highlight objc %}
_tableView.tableHeaderView = [[TableHeaderView alloc] initWithFrame:CGRectMake(0, 0, _tableView.frame.size.width, 80)];
{% endhighlight %}

# <a name="section-header-footer-backgroundcolor-setting"></a>Section Header/Footer背景色設置  

同樣、這裡以Header為例、Footer設置類同。  

起初、設置背景色是件輕鬆愉快的事：可在「Attributes inspector」設置，或在程式裡這般做：  

{% highlight objc %}
headerView.backgroundColor = [UIColor blackColor];
{% endhighlight %}

自iOS6起、上述方法失效了並會產生如下警告：  

> Setting the background color on UITableViewHeaderFooterView has been deprecated. Please use contentView.backgroundColor instead.  

可行方法、是在程式裡這般做：  

{% highlight objc %}
headerView.tintColor = [UIColor blackColor];
{% endhighlight %}

想在「Attributes inspector」設置？沒門！  

自iOS8起、上述方法又失效啦！可行方法、是在程式裡這般做：  

{% highlight objc linenos %}
UIView *backgroundView = [[UIView alloc] initWithFrame:headerView.bounds];
backgroundView.backgroundColor = [UIColor blackColor];
headerView.backgroundView = backgroundView;
{% endhighlight %}

注：這裡所說iOS版本、是指用於程式開發的設備的iOS版本，並非內置於Xcode的iOS SDK版本！  
