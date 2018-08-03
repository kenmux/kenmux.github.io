---
title: "【譯】C語言「右左規則」"
date: 2017-11-05 18:00:00 +0800
categories: [Programming]
tags: [C]
comments: true
---

這篇筆記、翻譯自博文：[C Right-Left Rule](http://ieng9.ucsd.edu/~cs30x/rt_lt.rule.html)，作者Rick Ord。尊重他人勞動果實，轉載請註明！  

「右左」規則是解讀C宣告十分實用的規則。創建宣告時它也同樣有用。<!-- more -->  
<!--
The "right-left" rule is a completely regular rule for deciphering C
declarations.  It can also be useful in creating them.
-->

首先，符號。在宣告中，視  
&emsp;&emsp;* &emsp;&emsp;為「指向<u>&emsp;</u>的指標」&emsp;&emsp;&emsp;通常在左邊  
&emsp;&emsp;[]&emsp;&emsp;為「<u>&emsp;</u>的陣列」&emsp;&emsp;&emsp;&emsp;&emsp;通常在右邊  
&emsp;&emsp;()&emsp;&emsp;為「返回<u>&emsp;</u>的函式」&emsp;&emsp;&emsp;通常在右邊  
<!--
First, symbols.  Read
     *		as "pointer to"			- always on the left side
     [] 	as "array of"			- always on the right side
     ()		as "function returning"		- always on the right side
as you encounter them in the declaration.
-->

第一步  
\--\--\--\--\--  
找到標識符。這是初始點。然後妳知道，這個宣告是:「標識符是<u>&emsp;</u>」。
<!--
STEP 1
------
Find the identifier.  This is your starting point.  Then say to yourself,
"identifier is."  You've started your declaration.
-->

第二步  
\--\--\--\--\--  
查看標識符右邊的符號，如果是「()」，那就表示這是一個函式宣告：「標識符是返回<u>&emsp;</u>的函式」。抑或是「[]」，則是「標識符是<u>&emsp;</u>的陣列」。繼續查看右邊，直到沒有符號<b>或者</b>遇到一個<b>右</b>圓括號「)」。（如果遇到左圓括號，那是符號()的起始，即使括號間還有東西。更多以下詳解。）  
<!--
STEP 2
------
Look at the symbols on the right of the identifier.  If, say, you find "()"
there, then you know that this is the declaration for a function.  So you
would then have "identifier is function returning".  Or if you found a 
"[]" there, you would say "identifier is array of".  Continue right until
you run out of symbols *OR* hit a *right* parenthesis ")".  (If you hit a 
left parenthesis, that's the beginning of a () symbol, even if there
is stuff in between the parentheses.  More on that below.)
-->

第三步  
\--\--\--\--\--  
查看標識符左邊的符號。如果不是上面說的符號（比如「int」），直接代到句子裏面。否則，使用上述表格轉譯成文字。繼續查看左邊，直到沒有符號<b>或者</b>遇到<b>左</b>圓括號「(」。  
<!--
STEP 3
------
Look at the symbols to the left of the identifier.  If it is not one of our
symbols above (say, something like "int"), just say it.  Otherwise, translate
it into English using that table above.  Keep going left until you run out of
symbols *OR* hit a *left* parenthesis "(".
-->

接下來重複第二步和第三步，直到宣告解讀完畢。以下是一些範例：  
<!--
Now repeat steps 2 and 3 until you've formed your declaration.  Here are some examples:
-->

<pre>    int *p[];

1）找到標識符。
                       int *p[]
                            ^
    「p是<u>  </u>」

2）向右邊移動，直到沒有符號或者遇到右圓括號。
                       int *p[]
                             ^^
    「p是<u>  </u>的陣列」

3）不能再向右邊移動了（沒有符號），因而向左邊移動並得到：
                       int *p[]
                           ^
    「p是指向<u>  </u>的指標的陣列」

4）繼續向左邊移動並得到：
                       int *p[]
                       ^^^
    「p是指向int的指標的陣列」
    （或者「p是一個陣列，其元素型別為指向int的指標」）</pre>
<!--
     int *p[];

1) Find identifier.          int *p[];
                                  ^
   "p is"

2) Move right until out of symbols or right parenthesis hit.
                             int *p[];
                                   ^^
   "p is array of"

3) Can't move right anymore (out of symbols), so move left and find:
                             int *p[];
                                 ^
   "p is array of pointer to"

4) Keep going left and find:
                             int *p[];
                             ^^^
   "p is array of pointer to int". 
   (or "p is an array where each element is of type pointer to int")-->

另一個範例：  
<!--
Another example:
-->

<pre>    int *(*func())();

1）找到標識符。
                       int *(*func())();
                              ^^^^
    「func是<u>  </u>」

2）向右邊移動。
                       int *(*func())();
                                  ^^
    「func是返回<u>  </u>的函式」

3）遇到右圓括號，不能再向右邊移動了，因而向左邊移動：
                       int *(*func())();
                             ^
    「func是返回指向<u>  </u>的指標的函式」

4）遇到左圓括號，不能再向左邊移動了，因而繼續向右邊移動：
                       int *(*func())();
                                     ^^
    「func是返回指向返回<u>  </u>的函式的指標的函式」

5）沒有符號，不能再向右邊移動了，因而轉向左邊：
                       int *(*func())();
                           ^
    「func是返回指向返回指向<u>  </u>的指標的函式的指標的函式」

6）最後，因為右邊沒有了，繼續向左邊移動。
                       int *(*func())();
                       ^^^
    「func是返回指向返回指向int的指標的函式的指標的函式」
</pre>
<!--
   int *(*func())();

1) Find the identifier.      int *(*func())();
                                    ^^^^
   "func is"

2) Move right.               int *(*func())();
                                        ^^
   "func is function returning"

3) Can't move right anymore because of the right parenthesis, so move left.
                             int *(*func())();
                                   ^
   "func is function returning pointer to"

4) Can't move left anymore because of the left parenthesis, so keep going
   right.                    int *(*func())();
                                           ^^
   "func is function returning pointer to function returning"

5) Can't move right anymore because we're out of symbols, so go left.
                             int *(*func())();
                                 ^
   "func is function returning pointer to function returning pointer to"

6) And finally, keep going left, because there's nothing left on the right.
                             int *(*func())();
                             ^^^
   "func is function returning pointer to function returning pointer to int".
-->

正如妳所看到的，這個規則相當有用。妳也可以在創建宣告時用它來做完整性檢查，或提示下一個符號的位置以及括號是否必要。  
<!--
As you can see, this rule can be quite useful.  You can also use it to
sanity check yourself while you are creating declarations, and to give
you a hint about where to put the next symbol and whether parentheses
are required.
-->

有些宣告因原型中陣列長度和參數列表而看起來比實際複雜得多。如「[3]」，意為「<u>__</u>的（長度為3的）陣列。又如「(char *,int)」，意為「參數為(char *,int)，返回<u>__</u>的函式」。來一個比較有趣的：  
<!--
Some declarations look much more complicated than they are due to array
sizes and argument lists in prototype form.  If you see "[3]", that's
read as "array (size 3) of...".  If you see "(char *,int)" that's read
as "function expecting (char *,int) and returning...".  Here's a fun
one:
-->

<pre>                int (*(*fun_one)(char *,double))[9][20];</pre>
<!--                 int (*(*fun_one)(char *,double))[9][20];-->

這裡就不詳細羅列解讀過程，直接揭曉答案：  

<pre>「fun_one是指向參數為(char *,double)並返回指向int陣列（長度為20）的陣列（長度為9）
的指標的函式的指標。」</pre>

<!--
I won't go through each of the steps to decipher this one.

Ok.  It's:

     "fun_one is pointer to function expecting (char *,double) and 
      returning pointer to array (size 9) of array (size 20) of int."
-->

正如妳所看到的，去掉陣列長度和參數列表後，其實並不復雜：  
<pre>                int (*(*fun_one)())[][];</pre>
<!--
As you can see, it's not as complicated if you get rid of the array sizes
and argument lists:
     int (*(*fun_one)())[][];
-->

妳可以先這樣解讀，然後再添上陣列長度和參數列表。  
<!--
You can decipher it that way, and then put in the array sizes and argument
lists later.
-->

結語：  
<!--
Some final words:
-->

使用這個規則很有可能做出非法宣告，因而關於什麼在C中是合法的基本常識是必要的。例如，如果把上述語句寫作：  
<!--
It is quite possible to make illegal declarations using this rule,
so some knowledge of what's legal in C is necessary.  For instance,
if the above had been:
-->

<pre>                int *((*fun_one)())[][];</pre>
<!--
     int *((*fun_one)())[][];
-->

這樣就解讀為「fun_one是指向<span style="border-bottom-style:dotted;">返回</span>指向int的指標的陣列的<span style="border-bottom-style:dotted;">陣列的函式</span>的指標」。因為函式不能返回陣列，而是指向陣列的指標，因而這樣宣告是非法的。  
<!--span style="border-bottem:2px dotted
it would have been "fun_one is pointer to function returning array of array of
                                          ^^^^^^^^^^^^^^^^^^^^^^^^
pointer to int".  Since a function cannot return an array, but only a 
pointer to an array, that declaration is illegal.-->

非法的組合還有：  
<!--Illegal combinations include:-->

<pre style="margin-left:2em;">
[]() - 不存在函數陣列
()() - 不存在返回函式的函式
()[] - 不存在返回陣列的函式
</pre>
<!--
	 []() - cannot have an array of functions
	 ()() - cannot have a function that returns a function
	 ()[] - cannot have a function that returns an array
-->

在以上所有情況下，為了使宣告合法，妳需要用一對括號去結合左邊\*符號和右邊()和[]間的符號。  
<!--
In all the above cases, you would need a set of parens to bind a *
symbol on the left between these () and [] right-side symbols in order
for the declaration to be legal.
-->

以下是一些合法和非法的例子：  
<pre style="margin-left:2em;">
int i;                  int的變數
int *p;                 int的指標（指向int的指標）
int a[];                int的陣列
int f();                返回int的函式
int **pp;               指向int的指標的指標
int (*pa)[];            指向int的陣列的指標
int (*pf)();            指向返回int的函式的指標
int *ap[];              int的指標的陣列（指向int的指標的陣列）
int aa[][];             int的陣列的陣列
int af[]();             返回int的函式的陣列（非法）
int *fp();              返回int的指標的函式
int fa()[];             返回int陣列的函式（非法）
int ff()();             返回返回int的函式的函式（非法）
int ***ppp;             指向指向int的指標的指標的指標
int (**ppa)[];          指向指向int的陣列的指標的指標
int (**ppf)();          指向指向返回int的函式的指標的指標
int *(*pap)[];          指向int的指標的陣列的指標
int (*paa)[][];         指向int的陣列的陣列的指標
int (*paf)[]();         指向返回int的函式的陣列的指標（非法）
int *(*pfp)();          指向返回int指標的函式的指標
int (*pfa)()[];         指向返回int的陣列的函式的指標（非法）
int (*pff)()();         指向返回返回int的函式的函式的指標（非法）
int **app[];            指向int指標的指標的陣列
int (*apa[])[];         指向int的陣列的指標的陣列
int (*apf[])();         指向返回int的函式的指標的陣列
int *aap[][];           int指標的陣列的陣列
int aaa[][][];          int的陣列的陣列的陣列
int aaf[][]();          返回int的函式的陣列的陣列（非法）
int *afp[]();           返回int指標的函式的陣列（非法）
int afa[]()[];          返回int的陣列的函式的陣列（非法）
int aff[]()();          返回返回int的函式的函式的陣列（非法）
int **fpp();            返回指向int指標的指標的函式
int (*fpa())[];         返回指向int的陣列的指標的函式
int (*fpf())();         返回指向返回int的函式的指標的函式
int *fap()[];           返回int指標的陣列的函式（非法）
int faa()[][];          返回int的陣列的陣列的函式（非法）
int faf()[]();          返回返回int的函式的陣列的函式（非法）
int *ffp()();           返回返回int指標的函式的函式（非法）
</pre>
<!--
Here are some legal and illegal examples:

int i;                  an int
int *p;                 an int pointer (ptr to an int)
int a[];                an array of ints
int f();                a function returning an int
int **pp;               a pointer to an int pointer (ptr to a ptr to an int)
int (*pa)[];            a pointer to an array of ints
int (*pf)();            a pointer to a function returning an int
int *ap[];              an array of int pointers (array of ptrs to ints)
int aa[][];             an array of arrays of ints
int af[]();             an array of functions returning an int (ILLEGAL)
int *fp();              a function returning an int pointer
int fa()[];             a function returning an array of ints (ILLEGAL)
int ff()();             a function returning a function returning an int
                                (ILLEGAL)
int ***ppp;             a pointer to a pointer to an int pointer
int (**ppa)[];          a pointer to a pointer to an array of ints
int (**ppf)();          a pointer to a pointer to a function returning an int
int *(*pap)[];          a pointer to an array of int pointers
int (*paa)[][];         a pointer to an array of arrays of ints
int (*paf)[]();         a pointer to a an array of functions returning an int
                                (ILLEGAL)
int *(*pfp)();          a pointer to a function returning an int pointer
int (*pfa)()[];         a pointer to a function returning an array of ints
                                (ILLEGAL)
int (*pff)()();         a pointer to a function returning a function
                                returning an int (ILLEGAL)
int **app[];            an array of pointers to int pointers
int (*apa[])[];         an array of pointers to arrays of ints
int (*apf[])();         an array of pointers to functions returning an int
int *aap[][];           an array of arrays of int pointers
int aaa[][][];          an array of arrays of arrays of ints
int aaf[][]();          an array of arrays of functions returning an int
                                (ILLEGAL)
int *afp[]();           an array of functions returning int pointers (ILLEGAL)
int afa[]()[];          an array of functions returning an array of ints
                                (ILLEGAL)
int aff[]()();          an array of functions returning functions
                                returning an int (ILLEGAL)
int **fpp();            a function returning a pointer to an int pointer
int (*fpa())[];         a function returning a pointer to an array of ints
int (*fpf())();         a function returning a pointer to a function
                                returning an int
int *fap()[];           a function returning an array of int pointers (ILLEGAL)
int faa()[][];          a function returning an array of arrays of ints
                                (ILLEGAL)
int faf()[]();          a function returning an array of functions
                                returning an int (ILLEGAL)
int *ffp()();           a function returning a function
                                returning an int pointer (ILLEGAL)
-->
