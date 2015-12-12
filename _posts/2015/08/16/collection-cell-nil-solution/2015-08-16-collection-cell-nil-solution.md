---
title: "Collection Cell為nil的解決方案"
date: 2015-08-16 18:00:00 +0800
categories: [iOS]
tags: [iOS7,Xcode6,UICollectionViewCell]
comments: true
---

最近、遇到一個奇怪的問題：在storyboard中作了個UICollectionViewController，在其中客製了包含UILabel的UICollectionViewCell，並在函式-collectionView:cellForItemAtIndexPath:中設置UILabel；但是程式總是莫名其妙地在此處掛掉、而排查的結果是一切設置並無不妥之處？  

百思不得其解之餘、在Stack Overflow上搜尋了下，總算找到答案了、原來這種情景下從storyboard獲取的UICollectionViewCell為nil：[Why is UICollectionViewCell's outlet nil?](https://stackoverflow.com/a/25166762/2518851) <!-- more -->  

細細追究起來，這事還得怪Xcode自動生成的程式碼，並沒有考慮到使用storyboard這種情景。請看這段程式碼：  

{% highlight objective-c linenos %}
static NSString * const reuseIdentifier = @"Cell";

- (void)viewDidLoad {
    [super viewDidLoad];
    
    // Uncomment the following line to preserve selection between presentations
    // self.clearsSelectionOnViewWillAppear = NO;
    
    // Register cell classes
    [self.collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:reuseIdentifier];
}
{% endhighlight %}

罪魁禍首就在第10列註冊單元格類別那裡。當使用storyboard時，那是多餘的，不僅無用、而且有害：這會覆寫（而不是合併）storyboard中的設置，直接導致拿取的collection cell為nil！因而、直接註釋掉即可。  

同樣，也是很好奇、為何Xcode生成這樣的程式碼？官方挖坑麼？（笑！）  
