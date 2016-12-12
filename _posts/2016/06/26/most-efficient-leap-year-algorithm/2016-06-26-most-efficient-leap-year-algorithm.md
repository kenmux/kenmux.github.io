---
title: "閏年的最佳效率演算法"
date: 2016-06-26 18:00:00 +0800
categories: [Programming]
tags: [leap year,algorithm]
comments: true
---

所謂閏年，維基百科做了如是[介紹](https://zh.wikipedia.org/wiki/%E9%97%B0%E5%B9%B4)：  

> 閏年是比普通年分多出一段時間的年分，在各種曆法中都有出現，目的是為了彌補人為規定的紀年與地球公轉產生的差異。  
> 
> 目前使用的格里曆閏年規則如下：  
> 
> 1. 西元年分除以400可整除，為閏年。  
> 2. 西元年分除以4可整除但除以100不可整除，為閏年。  
> 3. 西元年分除以4不可整除，為平年。  
> 4. 西元年分除以100可整除但除以400不可整除，為平年。  

在C，C++，C#，Java，以及許多類C編程語言的入門書籍裏，舉凡講到運算子一節，一般都會提及閏年的演算法；且不出意外、一般皆為：  

{% highlight c %}
if (((year % 4) == 0 && (year % 100) != 0) || (year % 400) == 0)
{
    /* leap year */
}
{% endhighlight %}

這演算法簡單明瞭、但在效率上卻顯得差強人意；因而、在stackoverflow上有[討論](https://stackoverflow.com/a/11595914/2518851)，認為若以效率考量、則最佳演算法為：

{% highlight c %}
if ((year & 3) == 0 && ((year % 25) != 0 || (year & 15) == 0))
{
    /* leap year */
}
{% endhighlight %}

那麼、這「最佳效率」演算法如何得來呢？以下便是推演過程。<!-- more -->  

# <a name="short-circuit-evaluation"></a>短路求值  

有以下兩點：  

- 許多編程語言都有[短路求值](https://zh.wikipedia.org/wiki/%E7%9F%AD%E8%B7%AF%E6%B1%82%E5%80%BC)的策略  
- 不能被4整除的數、亦不能被400整除  

則演算法可以重寫為：  

{% highlight c %}
if ((year % 4) == 0 && ((year % 100) != 0 || (year % 400) == 0))
{
    /* leap year */
}
{% endhighlight %}

這樣、舉凡不能被4整除的年份皆為平年（格里曆閏年規則#3）；不用再去運算<span style="background-color:#333333;color:white">(year % 400) == 0</span>、大大提高了演算法的效率。  

# <a name="factoring"></a>因式分解  

因有等式<span style="background-color:#333333;color:white">100=4×25</span>，則可被100整除等同於可被4和25整除。依據短路求值策略，在進行運算<span style="background-color:#333333;color:white">(year % 100) != 0</span>時必有先決條件：運算式<span style="background-color:#333333;color:white">(year % 4) == 0</span>為真，即年份可被4整除。  

故而、演算法可以重寫為：  

{% highlight c %}
if ((year % 4) == 0 && ((year % 25) != 0 || (year % 400) == 0))
{
    /* leap year */
}
{% endhighlight %}

同理、因有<span style="background-color:#333333;color:white">400=25×16</span>，演算法進一步可以重寫為：  

{% highlight c %}
if ((year % 4) == 0 && ((year % 25) != 0 || (year % 16) == 0))
{
    /* leap year */
}
{% endhighlight %}

# <a name="bitwise-and-in-place-of-modulo"></a>位元與取代模除  

執行模除運算需要除法。而除法運算在某些低端CPU上非常耗時。因而、最好避免不必要的模除運算。  

特別地、當模數為2的指數冪時、模除運算可用位元與[代替](https://en.wikipedia.org/wiki/Modulo_operation#Performance_issues)，即：<span style="background-color:#333333;color:white">x % 2^n == x & (2^n - 1)</span>。  

因而、演算法可以重寫為：  

{% highlight c %}
if ((year & 3) == 0 && ((year % 25) != 0 || (year & 15) == 0))
{
    /* leap year */
}
{% endhighlight %}

至此、推演告一段落；不過、以下兩點更加有趣哦～  

---

# <a name="15-or-12"></a>15還是12  

有以下三點：  

1. 運算式<span style="background-color:#333333;color:white">(year & 3) == 0</span>檢查年份的[0..1]位元是否皆為0  
2. 運算式<span style="background-color:#333333;color:white">(year & 12) == 0</span>檢查年份的[2..3]位元是否皆為0  
3. 運算式<span style="background-color:#333333;color:white">(year & 15) == 0</span>檢查年份的[0..3]位元是否皆為0  

因而、#3可由#1與#2協同完成；而依據短路求值策略，在進行運算<span style="background-color:#333333;color:white">(year & 15) == 0</span>時必有先決條件：運算式<span style="background-color:#333333;color:white">(year & 3) == 0</span>為真。  

故15也可以12替代、演算法也可重寫為：  

{% highlight c %}
if ((year & 3) == 0 && ((year % 25) != 0 || (year & 12) == 0))
{
    /* leap year */
}
{% endhighlight %}

# <a name="year-4000-problem"></a>4000年問題  

按照現行閏年規則，西元4000年應當為閏年；但到西元8000時，時日又會有所差錯，故有提議西元4000年為平年，並修改規則為：   

> <ol start="0">
>   <li>西元年分除以4000可整除，為平年。</li>
>   <li>西元年分除以400可整除但除以4000不可整除，為閏年。</li>
>   <li>西元年分除以4可整除但除以100不可整除，為閏年。</li>
>   <li>西元年分除以4不可整除，為平年。</li>
>   <li>西元年分除以100可整除但除以400不可整除，為平年。</li>
> </ol>

據此、又有人提出了「最終極」「最佳效率」[演算法](https://gist.github.com/rindeal/8ed6fdb24650c9d17416)：  

{% highlight c %}
if ((year & 3) == 0 && ((year % 25) != 0 || ((year & 15) == 0 && (year % 4000) != 0)))
{
    /* leap year */
}
{% endhighlight %}
