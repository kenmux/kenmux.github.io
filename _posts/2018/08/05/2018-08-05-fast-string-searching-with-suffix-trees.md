---
title: "【譯】後綴樹快速字串搜尋"
date: 2018-08-05 18:00:00 +0800
categories: [Programming]
tags: [Suffix Tree,Trie,Algorithm]
imgurl: /assets/imgs/2018/08/05/fast-string-searching-with-suffix-trees
comments: true
---

這篇筆記、翻譯自博文：[Fast String Searching With Suffix Trees](http://facweb.cs.depaul.edu/mobasher/classes/csc575/Suffix_Trees/index.html)，作者Mark Nelson。尊重他人勞動果實，轉載請註明！  
<!--
於《Dr. Dobb's Journal》1996年8月刊[發表](http://www.drdobbs.com/database/algorithm-alley-fast-string-searching-wi/184409945?queryText=Suffix%2BTrees)
Fast String Searching With Suffix Trees
by Mark Nelson
Dr. Dobb's Journal
August, 1996
-->

> <i>I think that I shall never see</i>  
> <i>A poem lovely as a tree.</i>  
> <i>Poems are made by fools like me,</i>  
> <i>But only God can make a tree.</i>  
>  
> --Joyce Kilmer

<!--
I think that I shall never see
A poem lovely as a tree. 
Poems are made by fools like me,
But only God can make a tree.

--Joyce Kilmer
-->

> <i>A tree's a tree. How many more do you need to look at?</i>  
>  
> --Ronald Reagan-  

<!--
A tree's a tree. How many more do you need to look at?

--Ronald Reagan-
-->

字串序列匹配是電腦程式設計師經常需要面對的問題。一些編程任務，例如數據壓縮或DNA測序，可以從字串匹配演算法的改進中獲益匪淺。本文探討了一種相對未知的數據結構，即<b>後綴樹</b>，並展示如何使用它的特性着手解决字串匹配的難題。  
<!--
Matching string sequences is a problem that computer programmers face on a regular basis. Some programming tasks, such as data compression or DNA sequencing, can benefit enormously from improvements in string matching algorithms. This article discusses a relatively unknown data structure, the suffix tree, and shows how its characteristics can be used to attack difficult string matching problems.
-->
<!-- more -->

### 問題的提出  
<!--
The problem
-->

想像一下，你剛剛被聘為一名DNA測序項目的程式設計師。研究人員正在忙著分切病毒遺傳物質，產生碎片化的核苷酸序列。他們將這些序列發送到你的伺服器，期望將序列在基因組數據庫中定位。給定病毒的基因組可能有成千上萬個核苷酸鹼基，數據庫中有數百種病毒。你需要實作客戶端/伺服器系統，以提供實時反饋給等不及的博士們。什麼才是最好的方法呢？  
<!--Imagine that you've just been hired as a programmer working on a DNA sequencing project. Researchers are busy slicing and dicing viral genetic material, producing fragmented sequences of nucleotides. They send these sequences to your server, which is then expected to locate the sequences in a database of genomes. The genome for a given virus can have hundreds of thousands of nucleotide bases, and you have hundreds of viruses in your database. You are expected to implement this as a client/server project that gives real-time feedback to the impatient Ph.D.s. What's the best way to go about it?
-->

顯而易見，暴力字串搜尋將非常低效。這種搜尋需要對數據庫中每個基因組的每個核苷酸進行字串比較。測試部分匹配命中率很高的長片段將使你的客戶端/伺服器系統看起來像中古批處理機。你面臨的挑戰是提出一種高效的字串匹配解決方案。  
<!--
It is obvious at this point that a brute force string search is going to be terribly inefficient. This type of search would require you to perform a string comparison at every single nucleotide in every genome in your database. Testing a long fragment that has a high hit rate of partial matches would make your client/server system look like an antique batch processing machine. Your challenge is to come up with an efficient string matching solution.
-->

### 直觀的解決方案  
<!--
The intuitive solution
-->

由於要測試的數據庫是不變的，因此對它預處理以簡化搜尋似乎是一個好主意。一種預處理的方法是構建搜尋字典樹。對於搜尋輸入文本，構建搜尋字典樹的直觀方法產生了一種稱為<b>後綴字典樹</b>的東西。（後綴字典樹距離我的最終標的，即<b>後綴樹</b>，只有一步之遙。）字典樹是一種樹，每個節點有N個可能的分支，其中N是字母表中的字符數。使用「後綴」一詞來指代這樣的情形：字典樹包含給定文本塊（可能是病毒基因組）的所有後綴。  
<!--
【[trie](https://en.wikipedia.org/wiki/Trie)，又稱前綴樹或字典樹，是一種有序樹，用於保存關聯陣列，其中的鍵通常是字串。本文一律翻作「字典樹」】
Since the database that you are testing against is invariant, preprocessing it to simplify the search seems like a good idea. One preprocessing approach is to build a search trie. For searching through input text, a straightforward approach to a search trie yields a thing called a suffix trie. (The suffix trie is just one step away from my final destination, the suffix tree.) A trie is a type of tree that has N possible branches from each node, where N is the number of characters in the alphabet. The word 'suffix' is used in this case to refer to the fact that the trie contains all of the suffixes of a given block of text (perhaps a viral genome.)
-->

<div style="text-align:center" markdown="1">
![2018-08-05-01.gif]({{ page.imgurl }}/2018-08-05-01.gif)  
圖1 用後綴字典樹表示「BANANAS」
</div>
<!--Figure 1

The Suffix Trie Representing "BANANAS"-->  

<div style="word-wrap:break-word;word-break:break-all;">
圖1顯示了BANANAS這個詞的後綴字典樹。關於這個字典樹，有兩個重點需要注意。首先，從根節點開始，BANANAS的每個後綴都在字典樹中：從BANANAS，ANANAS，NANAS開始，然後用一個單獨的S結束。第二，由於這種組織方式，你可以搜尋這個詞的任何子串：從根開始，然後沿著樹一直向下找，直到終了。
</div>
<!--
Figure 1 shows a Suffix trie for the word BANANAS. There are two important facts to note about this trie. First, starting at the root node, each of the suffixes of BANANAS is found in the trie, starting with BANANAS, ANANAS, NANAS, and finishing up with a solitary S. Second, because of this organization, you can search for any substring of the word by starting at the root and following matches down the tree until exhausted.
-->

第二點成就了後綴字典樹這麼好的結構。如果有一個長度為<b><i>N</i></b>的輸入文本和一個長度為<b><i>M</i></b>的搜尋字串，則一次傳統的暴力搜尋需要完成N*M次字符比較。最佳化的搜尋技術，例如Boyer-Moore演算法，可以保證搜尋不超過M+N次比較，並具有更好的平均性能。但後綴字典樹完全超越了這種性能：僅需M次字符比較，無論待搜尋文本的長度如何！  
<!--
The second point is what makes the suffix trie such a nice construct. If you have a input text of length N, and a search string of length M, a traiditonal brute force search will take as many as N*M character comparison to complete. Optimized searching techniques, such as the Boyer-Moore algorithm can guarantee searches that require no more than M+N comparisons, with even better average performance. But the suffix trie demolishes this performance by requiring just M character comparisons, regardless of the length of the text being searched!
-->

這可能看起來很不錯，意味著我可以通過執行區區七次字符比較來確定BANANAS這個詞是否在威廉·莎士比亞的作品集中！當然，只有一個小問題：構建字典樹所需的時間。  
<!--
Remarkable as this might seem, it means I could determine if the word BANANAS was in the collected works of William Shakespeare by performing just seven character comparisons! Of course, there is just one little catch: the time needed to construct the trie.
-->

你不怎麼聽到後綴字典樹應用的原因是基於一個簡單的事實，即構建一個需要O(N<sup>2</sup>)的時間和空間。這種平方階性能使之無法用在最需要使用後綴字典樹的場合：搜尋長數據塊。  
<!--
The reason you don't hear much about the use of suffix tries is the simple fact that constructing one requires O(N2) time and space. This quadratic performance rules out the use of suffix tries where they are needed most: to search through long blocks of data.
-->

### 在後綴樹蔭下  
<!--
Under the spreading suffix tree
-->

1976年，Edward McCreight提出了一個合理的方法來解決這個困境。他發表了一篇論文來闡述<b>後綴樹</b>。給定數據塊的後綴樹保留與後綴字典樹相同的拓撲，但它消除了只有一個後代的節點。這個過程稱為<b>路徑壓縮</b>，意味著樹中的各個邊現在可以表示文本序列而不是單個字符。  
<!--
A reasonable way past this dilemma was proposed by Edward McCreight in 1976, when he published his paper on what came to be known as the suffix tree. The suffix tree for a given block of data retains the same topology as the suffix trie, but it eliminates nodes that have only a single descendant. This process, known as path compression, means that individual edges in the tree now may represent sequences of text instead of single characters. 
-->

<div style="text-align:center" markdown="1">
![2018-08-05-02.gif]({{ page.imgurl }}/2018-08-05-02.gif)  
圖2 用後綴樹表示BANANAS
</div>
<!--
Figure 2

A suffix tree representing BANANAS
-->

圖2顯示了圖1的後綴字典樹轉換為後綴樹的樣子。你可以看到樹仍然具有相同的一般形狀，只是擁有更少的節點。通過消除所有的僅有單個後代的節點，計數從23減少到11。  
<!--
Figure 2 shows what the suffix trie from Figure 1 looks like when converted to a suffix tree. You can see that the tree still has the same general shape, just far fewer nodes. By eliminating every node with just a single descendant, the count is reduced from 23 to 11.
-->

實際上，節點數量的減少使得構造後綴樹的時間和空間要求從O(N<sup>2</sup>)減少到O(N)。在最壞的情況下，構建後綴樹最多需要2N個節點，其中N是輸入文本的長度。因此，只需與輸入文本長度成比例的一次性投資，我們就可以創建一個用於增強字串搜尋的樹。  
<!--
In fact, the reduction in the number of nodes is such that the time and space requirements for constructing a suffix tree are reduced from O(N2) to O(N). In the worst case, a suffix tree can be built with a maximum of 2N nodes, where N is the length of the input text. So for a one-time investment proportional to the length of the input text, we can create a tree that turbocharges our string searches.
-->

### 即使你可以做一棵樹  
<!--
Even you can make a tree
-->

構造後綴樹的原始McCreight演算法有一些缺點。其中的一項原則是要求樹以相反的順序構建，這意味著字符從輸入末端開始添加。這使得在線處理成為不可能，使得它更難以用於數據壓縮等應用程式。  
<!--
McCreight's original algorithm for constructing a suffix tree had a few disadvantages. Principle among them was the requirement that the tree be built in reverse order, meaning characters were added from the end of the input. This ruled the algorithm out for on line processing, making it much more difficult to use for applications such as data compression.
-->

二十年後，來自赫爾辛基大學的Esko Ukkonen打破了窘境。他稍微修改了演算法，使其從左向右運作。我的示例代碼和後面的描述均基於Ukkonen的學術論文，該論文發表在1995年9月的<b><i>Algorithmica</i></b>雜誌上。  
<!--
Twenty years later, Esko Ukkonen from the University of Helsinki came to the rescue with a slightly modified version of the algorithm that works from left to right. Both my sample code and the descriptions that follow are based on Ukkonen's work, published in the September 1995 issue of Algorithmica.
-->

對於給定的文本字串，T，Ukkonen的演算法以空樹開始，然後逐步將T的N個前綴逐個添加到後綴樹中。例如，在為BANANAS創建後綴樹時，將B插入樹中，然後插入BA，然後插入BAN，依此類推。最後插入BANANAS時，樹已完成。  
<!--
For a given string of text, T, Ukkonen's algorithm starts with an empty tree, then progressively adds each of the N prefixes of T to the suffix tree. For example, when creating the suffix tree for BANANAS, B is inserted into the tree, then BA, then BAN, and so on. When BANANAS is finally inserted, the tree is complete. 
-->

<div style="text-align:center" markdown="1">
![2018-08-05-03.gif]({{ page.imgurl }}/2018-08-05-03.gif)  
圖3 逐步創建後綴樹
</div>
<!--
Figure 3

Progressively Building the Suffix Tree
-->

### 後綴樹運作方式  
<!--
Suffix tree mechanics
-->

向樹中添加新前綴是通過遍歷樹並訪問當前樹的每個後綴來完成的。我們從最長的後綴（圖3中的BAN）開始，然後向下遍歷到最短的後綴，即空字串。每個後綴都以這三種類型中的一種節點結束：  
<!--
Adding a new prefix to the tree is done by walking through the tree and visiting each of the suffixes of the current tree. We start at the longest suffix (BAN in Figure 3), and work our way down to the shortest suffix, which is the empty string. Each suffix ends at a node that consists of one of these three types:
-->

1. 葉節點。在圖4中，標記為1，2，4和5的節點是葉節點。  
2. 顯式節點。在圖4中，標記為0和3的非葉節點是顯式節點。它們代表樹上的一個點，其上兩條或兩條以上的邊分開。  
3. 隱式節點。在圖4中，諸如BO，BOO和OO之類的前綴都在邊的中間結束。這些位置稱為<b>隱式</b>節點。它們代表後綴字典樹中的節點，但路徑壓縮消除了它們。在構建樹時，隱式節點有時會轉換為顯式節點。  
<!--
1.A leaf node. In Figure 4, the nodes labeled 1,2, 4, and 5 are leaf nodes.
2.An explicit node. The non-leaf nodes that are labeled 0 and 3 in Figure 4 are explicit nodes. They represent a point on the tree where two or more edges part ways.
3.An implicit node. In Figure 4, prefixes such as BO, BOO, and OO all end in the middle of an edge. These positions are referred to as implicit nodes. They would represent nodes in the suffix trie, but path compression eliminated them. As the tree is built, implicit nodes are sometimes converted to explicit nodes.
-->

<div style="text-align:center" markdown="1">
![2018-08-05-04.gif]({{ page.imgurl }}/2018-08-05-04.gif)  
圖4 添加BOOK後的BOOKKEEPER
</div>
<!--
Figure 4

BOOKKEEPER after adding BOOK-->

在圖4中，在將BOOK添加進來之後，樹中有五個後綴（包括空字串）。將下一個前綴BOOKK添加到樹中意味著訪問現有樹中的每個後綴，並在後綴的末尾添加字母K。  
<!--
In Figure 4, there are five suffixes in the tree (including the empty string) after adding BOOK to the structure. Adding the next prefix, BOOKK to the tree means visiting each of the suffixes in the existing tree, and adding letter K to the end of the suffix.
-->

前四個後綴BOOK，OOK，OK和K都以葉節點結束。由於後綴樹使用了路徑壓縮，向葉節點添加新字符將始終只添加到該節點上的字串。它不會為新添加的字母創建新節點。  
<!--
The first four suffixes, BOOK, OOK, OK, and K, all end at leaf nodes. Because of the path compression applied to suffix trees, adding a new character to a leaf node will always just add to the string on that node. It will never create a new node, regardless of the letter being added.
-->

在更新了所有葉節點之後，我們仍然需要在空字串中添加字符「K」，該字串在節點0處找到。由於已經有一條以字母K開頭的離開節點0的邊，我們不需要做任何事情。新添加的後綴K將在節點0處找到，並且將在隱式節點處（沿著此處通向節點2的邊向下有一個字符）結束。  
<!--
After all of the leaf nodes have been updated, we still need to add character 'K' to the empty string, which is found at node 0. Since there is already an edge leaving node 0 that starts with letter K, we don't have to do anything. The newly added suffix K will be found at node 0, and will end at the implicit node found one character down along the edge leading to node 2.
-->

結果樹的最終形狀如圖5所示。  
<!--
The final shape of the resulting tree is shown in Figure 5.
-->

<div style="text-align:center" markdown="1">
![2018-08-05-05.gif]({{ page.imgurl }}/2018-08-05-05.gif)  
圖5 添加bookk後的同一棵樹
</div>
<!--Figure 5

The same tree after adding BOOKK-->

### 事情變得棘手了  
<!--
Things get knotty
-->

更新圖4中的樹相對容易。我們執行了兩種類型的更新：第一種只是邊的擴充，第二種是隱式更新，不做任何事情。將BOOKKE添加到圖5中所示的樹將演示另外兩種類型的更新。第一種類型，創建一個新節點以在隱式節點處拆分現有邊，然後添加新邊。第二種類型，向顯式節點添加新邊。  
<!--
Updating the tree in Figure 4 was relatively easy. We performed two types of updates: the first was simply the extension of an edge, and the second was an implicit update, which involved no work at all. Adding BOOKKE to the tree shown in Figure 5 will demonstrate the two other types of updates. In the first type, a new node is created to split an existing edge at an implicit node, followed by the addition of a new edge. The second type of update consists of adding a new edge to an explicit node.
-->

<div style="text-align:center" markdown="1">
![2018-08-05-06.gif]({{ page.imgurl }}/2018-08-05-06.gif)  
圖6 拆分並添加更新
</div>
<!--
Figure 6

The Split and Add Update
-->

當將BOOKKE添加到圖5中的樹時，我們再次以最長的後綴BOOKK開始，並遍歷到最短的空字串。只要我們更新葉節點，更新較長的後綴還是很容易的。在圖5中，以葉節點結尾的後綴是BOOKK，OOKK，OKK和KK。圖6中的第一個樹顯示了使用簡單字串擴充更新這些後綴後樹的樣子。  
<!--
When adding BOOKKE to the tree in Figure 5, we once again start with the longest suffix, BOOKK, and work our way to the shortest, the empty string. Updating the longer suffixes is trivial as long as we are updating leaf nodes. In Figure 5, the suffixes that end in leaf nodes are BOOKK, OOKK, OKK, and KK. The first tree in Figure 6 shows what the tree looks like after these suffixes have been updated using the simple string extension.
-->

圖5中未在葉節點處終止的第一個後綴是K。當更新後綴樹時，第一個非葉節點被定義為樹的活躍點。所有比活躍點定義的後綴長的後綴都將以葉節點結束。此點之後的後綴都不會在葉節點處終止。  
<!--
The first suffix in Figure 5 that doesn't terminate at a leaf node is K. When updating a suffix tree, the first non-leaf node is defined as the active point of the tree. All of the suffixes that are longer than the suffix defined by the active point will end in leaf nodes. None of the suffixes after this point will terminate in leaf nodes.
-->

後綴K終止於KKE定義的沿邊向下的隱式節點。在測試非葉節點時，我們需要查看它們是否有任何與要追加的新字符匹配的後代。在這種情況下，那將是E。  
<!--
The suffix K terminates in an implicit node part way down the edge defined by KKE. When testing non-leaf nodes, we need to see if they have any descendants that match the new character being appended. In this case, that would be E.
-->

快速查看KKE中的第一個K，發現它只有一個後代：K。所以這意味著我們必須添加一個代表字母E的葉節點。這個過程需要兩步。首先，我們拆分保持弧的邊，以使待測後綴的末尾有一個顯式節點。圖6中間的樹顯示了拆分後樹的樣子。  
<!--
A quick look at the first K in KKE shows that it only has a single descendant: K. So this means we have to add a descendent to represent Letter E. This is a two step process. First, we split the edge holding the arc so that it has an explicit node at the end of the suffix being tested. The middle tree in Figure 6 shows what the tree looks like after the split.
-->

一旦拆分了邊，並且添加了新節點，你就會看到一個類似於圖6第三個位置的樹。注意成長為KE的K節點，已成為葉節點。  
<!--
Once the edge has been split, and the new node has been added, you have a tree that looks like that in the third position of Figure 6. Note that the K node, which has now grown to be KE, has become a leaf node.
-->

### 更新顯式節點  
<!--
Updating an explicit node
-->

更新後綴K後，我們仍然需要更新下一個較短的後綴，即空字串。空字串在顯式節點0結束，所以我們只需檢查它是否有一個以字母E開頭的後代。快速查看圖6中的樹表明節點0沒有葉節點，所以添加了另一個葉節點，產生如圖7所示的樹。  
<!--
After updating suffix K, we still have to update the next shorter suffix, which is the empty string. The empty string ends at explicit node 0, so we just have to check to see if it has a descendant that starts with letter E. A quick look at the tree in Figure 6 shows that node 0 doesn't have a descendent, so another leaf node is added, which yields the tree shown in Figure 7.
-->

<div style="text-align:center" markdown="1">
![2018-08-05-07.gif]({{ page.imgurl }}/2018-08-05-07.gif)  
圖7
</div>
<!--
Figure 7
-->

### 泛化演算法  
<!--
Generalizing the algorithm
-->

通過利用後綴樹的一些特性，我們可以得到一個相當高效的演算法。第一個重要特徵是：一旦為葉節點，總是葉節點。葉節點自創建後永遠不會有後代，它只會通過字符串接進行擴充。更重要的是，每次我們向樹添加新後綴時，我們將自動地向通往每個葉節點的邊擴充單個字符。該字符將是新後綴中的最後一個字符。  
<!--
By taking advantage of a few of the characteristics of the suffix tree, we can generate a fairly efficient algorithm. The first important trait is this: once a leaf node, always a leaf node. Any node that we create as a leaf will never be given a descendant, it will only be extended through character concatenation. More importantly, every time we add a new suffix to the tree, we are going to automatically extend the edges leading into every leaf node by a single character. That character will be the last character in the new suffix.
-->

這使得對邊的管理變得容易。每當創建一個新的葉節點時，我們會自動設置其邊以表示從其起點到輸入文本末尾的所有字符。即使不知道這些字符是什麼，我們也知道它們最終會被添加到樹中。因此，一旦創建了葉節點，我們就可以忘掉它！如果邊被拆分，它的起始點可能會改變，但它仍然會一直延伸到輸入文本的末尾。  
<!--
This makes management of the edges leading into leaf nodes easy. Any time we create a new leaf node, we automatically set its edge to represent all the characters from its starting point to the end of the input text. Even if we don't know what those characters are, we know they will be added to the tree eventually. Because of this, once a leaf node is created, we can just forget about it! If the edge is split, its starting point may change, but it will still extend all the way to the end of the input text.
-->

這意味著只需要關心在活躍點（第一個非葉節點）更新顯式和隱式節點。鑑於此，我們必須從活躍點到空字串，測試每個節點是否需要更新。  
<!--
This means that we only have to worry about updating explicit and implicit nodes at the active point, which was the first non-leaf node. Given this, we would have to progress from the active point to the empty string, testing each node for update eligibility.
-->

然而，提前停止更新可以節約時間。當遍歷後綴時，我們將為每個沒有以正確字符開頭的後代邊的節點添加一個新邊。當最終找到具有正確字符作為後代的節點時，我們就可以停止更新了。了解了構造演算法的工作原理，你可以看出：如果某個字符是特定後綴的後代，那它也是每個較小後綴的後代。  
<!--
However, we can save some time by stopping our update earlier. As we walk through the suffixes, we will add a new edge to each node that doesn't have a descendant edge starting with the correct character. When we finally do reach a node that has the correct character as a descendant, we can simply stop updating. Knowing how the construction algorithm works, you can see that if you find a certain character as a descendent of a particular suffix, you are bound to also find it as a descendent of every smaller suffix.
-->

找到第一個匹配後代的點稱為終止點。終止點有一個額外的特別有用的特性。由於在活躍點和終止點之間的每個後綴中添加了葉節點，我們現在知道每個長於終止點的後綴是葉節點。這意味著終止點將在下一輪遍歷時變為活躍點!  
<!--
The point where you find the first matching descendent is called the end point. The end point has an additional feature that makes it particularly useful. Since we were adding leaves to every suffix between the active point and the end point, we now know that every suffix longer than the end point is a leaf node. This means the end point will turn into the active point on the next pass over the tree!
-->

通過將更新限制在活躍點和終止點之間的後綴，我們削減了更新樹所需的處理。通過追踪終止點，我們會自動知道下一輪的活躍點。使用此訊息的更新演算法的第一輪可能看起來像這樣（以類C的偽代碼）：
<!--
By confining our updates to the suffixes between the active point and the end point, we cut way back on the processing required to update the tree. And by keeping track of the end point, we automatically know what the active point will be on the next pass. A first pass at the update algorithm using this information might look something like this (in C-like pseudo code) :
-->

{% highlight text %}
Update( new_suffix )
{
  current_suffix = active_point
  test_char = last_char in new_suffix
  done = false;
  while ( !done ) {
    if current_suffix ends at an explicit node {
      if the node has no descendant edge starting with test_char 
        create new leaf edge starting at the explicit node
      else
        done = true;
    } else {
      if the implicit node's next char isn't test_char {
        split the edge at the implicit node
        create new leaf edge starting at the split in the edge
      } else
        done = true;
    }
    if current_suffix is the empty string
      done = true; 
    else
       current_suffix = next_smaller_suffix( current_suffix )
  }
  active_point = current_suffix
}
{% endhighlight %}

### 後綴指標  
<!--
The Suffix Pointer
-->

上面顯示的偽代碼演算法大致正確，但它掩蓋了一個問題。當我們瀏覽樹時，我們通過調用next_smaller_suffix()移動到下一個較小的後綴。該程式必須找到對應於特定後綴的隱式或顯式節點。  
<!--
The pseudo-code algorithm shown above is more or less accurate, but it glosses over one difficulty. As we are navigating through the tree, we move to the next smaller suffix via a call to next_smaller_suffix(). This routine has to find the implicit or explicit node corresponding to a particular suffix.
-->

如果簡單地順著樹從上往下直至找到正確的節點，演算法的運行時間將不是線性的。為了解決這個問題，我們必須為樹添加一個額外的指標：後綴指標。後綴指標是在每個內部節點都有的指標。每個內部節點表示從根開始的字符序列。後綴指標指向該字串的第一個後綴的節點。因此，如果特定字串包含輸入文本的字符0到N，則該字串的後綴指標將指向作為從根開始的字串（該字串表示輸入文本1到N的字符）的終止點的節點。  
<!--
If we do this by simply walking down the tree until we find the correct node, our algorithm isn't going to run in linear time. To get around this, we have to add one additional pointer to the tree: the suffix pointer. The suffix pointer is a pointer found at each internal node. Each internal node represents a sequence of characters that start at the root. The suffix pointer points to the node that is the first suffix of that string. So if a particular string contains characters 0 through N of the input text, the suffix pointer for that string will point to the node that is the termination point for the string starting at the root that represents characters 1 through N of the input text.
-->

圖8顯示了字串ABABABC的後綴樹。第一個後綴指標位於表示ABAB的節點上。該字串的第一個後綴是BAB，這是在ABAB的後綴指標所指向的。同樣，BAB有自己的指向AB的節點的後綴指標。  
<!--
Figure 8 shows the suffix tree for the string ABABABC. The first suffix pointer is found at the node that represents ABAB. The first suffix of that string would be BAB, and that is where the suffix pointer at ABAB points. Likewise, BAB has its own suffix pointer, which points to the node for AB.
-->

<div style="text-align:center" markdown="1">
![2018-08-05-08.gif]({{ page.imgurl }}/2018-08-05-08.gif)  
圖8 ABABABC後綴樹，後綴指標顯示為虛線
</div>
<!--Figure 8

The suffix tree for ABABABC with suffix pointers shown as dashed lines-->

後綴指標是在對樹進行更新的同時構建的。當從活躍點移動到終止點時，我會跟踪創建的每個新葉子的父節點。每次創建新邊時，我也會從上一個創建的葉邊的父節點創建一個後綴指標來指向當前父邊。（顯然，我不能為在更新中創建的第一條邊執行此操作，但我會對所有剩餘的邊執行此操作。）  
<!--
The suffix pointers are built at the same time the update to the tree is taking place. As I move from the active point to the end point, I keep track of the parent node of each of the new leaves I create. Each time I create a new edge, I also create a suffix pointer from the parent node of the last leaf edge I created to the current parent edge. (Obviously, I can't do this for the first edge created in the update, but I do for all the remaining edges.)
-->

隨著後綴指針的到位，從一個後綴轉到下一個後綴只要跟隨指標就行了。這是將演算法簡化為O(N)的重要補充。  
<!--
With the suffix pointers in place, navigating from one suffix to the next is simply a matter of following a pointer. This critical addition to the algorithm is what reduces it to an O(N) algorithm.
-->

### 樹屋  
<!--
Tree houses
-->

為了幫助說明這篇文章，我寫了一個簡短的程序STREE.CPP，它從標準輸入中讀取一串文本並構建一個後綴樹。清單1中顯示的是註釋完全的C++版本。第二個版本STREED.CPP加上了大量的調試輸出，可以從DDJ列表服務以及我的主頁獲得。  
<!--
To help illustrate this article, I wrote a short program, STREE.CPP, that reads in a string of text from standard input and builds a suffix tree. The version shown in Listing 1, is fully documented C++. A second version, STREED.CPP, has extensive debug output as well, and is available from the DDJ listing service, as well as my home page.
-->

理解STREE.CPP實際上只是理解它包含的數據結構的工作原理。 最重要的數據結構是Edge物件。Edge的類別定義是：  
<!--
Understanding STREE.CPP is really just a matter of understanding the workings of the data structures that it contains. The most important data structure is the Edge object. The class definition for Edge is:
-->
{% highlight c++ %}
class Edge {
    public :
        int first_char_index;
        int last_char_index;
        int end_node;
        int start_node;
        void Insert();
        void Remove();
        Edge();
        Edge( int init_first_char_index,
              int init_last_char_index,
              int parent_node );
        int SplitEdge( Suffix &s );
        static Edge Find( int node, int c );
        static int Hash( int node, int c );
};
{% endhighlight %}

每次創建後綴樹中的新邊時，都會創建一個新的Edge物件來表示它。該物件的四個數據成員定義如下：  
<!--
Each time a new edge in the suffix tree is created, a new Edge object is created to represent it. The four data members of the object are defined as follows:
-->

<table style="border-collapse:collapse;border:none;width:100%">
  <tr style="border:none;">
    <td style="border:none;vertical-align:top;width:20%">
      first_char_index,
      last_char_index:
    </td>
    <td style="border:none;vertical-align:top;width:80%">
      樹中的每個邊都有與之關聯的輸入文本的字串。為了確保每個邊的存儲大小相同，我們只在輸入文本中存儲兩個索引來表示字串。
    </td>
  </tr>
  <tr style="border:none;">
    <td style="border:none;vertical-align:top;">
      start_node:
    </td>
    <td style="border:none;vertical-align:top;">
      表示此邊的起始節點的節點數。節點0是樹的根。
    </td>
  </tr>
  <tr style="border:none;">
    <td style="border:none;vertical-align:top;">
      end_node:
    </td>
    <td style="border:none;vertical-align:top;">
      表示此邊的結束節點的節點數。每次創建新邊時，也會創建新的結束節點。每個邊的結束節點在樹的生命週期內不會改變，因此這也可以用作邊ID。
    </td>
  </tr>
</table>
<!--
first_char_index, last_char_index:	Each of the edges in the tree has a sequence of characters from the input text associated with it. To ensure that the storage size of each edge is identical, we just store two indices into the input text to represent the sequence.
start_node:	The number of the node that represents the starting node for this edge. Node 0 is the root of the tree.
end_node:	The number of the node that represents the end node for this edge. Each time an edge is created, a new end node is created as well. The end node for every edge will not change over the life of the tree, so this can be used as an edge id as well.
-->

構建後綴樹時執行的最常見任務之一是基於其字串首字符來搜尋從特定節點發出的邊。在面向位元組的計算機上，可能有多達256條邊來自單個節點。為了使搜索合理快速和簡單，我將邊存儲在雜湊表中，使用基於其起始節點編號和子字串首字符的雜湊鍵。成員函數Insert()和Remove()用於管理進出雜湊表的邊的傳輸。  
<!--
One of the most frequent tasks performed when building the suffix tree is to search for the edge emanating from a particular node based on the first character in its sequence. On a byte oriented computer, there could be as many as 256 edges originating at a single node. To make the search reasonably quick and easy, I store the edges in a hash table, using a hash key based on their starting node number and the first character of their substring. The Insert() and Remove() member functions are used to manage the transfer of edges in and out of the hash table.
-->

構建後綴樹時使用的第二個重要數據結構是後綴物件。請記住，更新樹是通過處理當前存儲在樹中的字串的所有後綴來完成的，從最長點開始，到終止點結束。後綴只是一個字串，從節點0開始，到樹中的某個點結束。  
<!--
The second important data structure used when building the suffix tree is the Suffix object. Remember that updating the tree is done by working through all of the suffixes of the string currently stored in the tree, starting with the longest, and ending at the end point. A Suffix is simply a sequence of characters that starts at node 0 and ends at some point in the tree.
-->

合情合理地，我們可以通過僅定義其末字符在樹中的位置來安全地表示任何後綴，因為我們知道首字符從節點0開始，即根。後綴物件定義了使用該系統的後綴，其定義如下：
<!--
It makes sense that we can then safely represent any suffix by defining just the position in the tree of its last character, since we know the first character starts at node 0, the root. The Suffix object, whose definition is shown here, defines a given suffix using that system:
-->
{% highlight c++ %}
    class Suffix {
        public :
            int origin_node;
            int first_char_index;
            int last_char_index;
            Suffix( int node, int start, int stop );
            int Explicit();
            int Implicit();
            void Canonize();
    };
{% endhighlight %}

後綴物件從特定節點開始，跟隨由成員first_char_index和last_char_index指向的輸入文本中的字串，定義字串中的末字符。例如，在圖8中，最長的後綴「ABABABC」的origin_node為0，first_char_index為0，last_char_index為6。  
<!--
The Suffix object defines the last character in a string by starting at a specific node, then following the string of characters in the input sequence pointed to by the first_char_index and last_char_index members. For example, in Figure 8, the longest suffix "ABABABC" would have an origin_node of 0, a first_char_index of 0, and a last_char_index of 6.
-->

Ukkonen的演算法要求我們以<b>正規</b>形式使用這些後綴定義。每次修改後綴物件時，都會調用函數<b><i>Canonize()</i></b>來執行此轉換。後綴的正規表示只需要後綴物件中的origin_node是與字串終止點最接近的父。這意味著由一對(0, "ABABABC")表示的後綴字串將通過以下步驟進行正規化：先移動到(1, "ABABC")，然後(4, "BC")，最後(8, "")。  
<!--
Ukkonen's algorithm requires that we work with these Suffix definitions in canonical form. The Canonize() function is called to perform this transformation any time a Suffix object is modified. The canonical representation of the suffix simply requires that the origin_node in the Suffix object be the closest parent to the end point of the string. This means that the suffix string represented by the pair (0, "ABABABC"), would be canonized by moving first to (1, "ABABC"), then (4, "ABC"), and finally (8,"").
-->

當後綴字串在顯式節點上結束時，正規表示將使用空字串來定義字串中的剩餘字符。通過將first_char_index設置為大於last_char_index來定義空字符串。在這種情況下，我們知道後綴在<b>顯式</b>節點上結束。如果first_char_index小於或等於last_char_index，則表示後綴字串在<b>隱式</b>節點上結束。  
<!--
When a suffix string ends on an explicit node, the canonical representation will use an empty string to define the remaining characters in the string. An empty string is defined by setting first_char_index to be greater than last_char_index. When this is the case, we know that the suffix ends on an explicit node. If first_char_index is less than or equal to last_char_index, it means that the suffix string ends on an implicit node.
-->

給定這些數據結構的定義，我認為你會發現STREE.CPP中的程式碼是Ukkonen演算法的簡單實現。為了更加清晰，請使用STREED.CPP以便在運行時吐出大量的調試訊息。  
<!--
Given these data structure definitions, I think you will find the code in STREE.CPP to be a straightforward implementation of the Ukkonen algorithm. For additional clarity, use STREED.CPP to dump copious debug information out at runtime.
-->

### 致謝  
<!--
Acknowledgments
-->

通過閱讀Jesper Larsson關於1996年IEEE數據壓縮會議的論文，我終於解決了後綴樹的構建問題。Jesper也非常友好地向我提供了示例代碼和Ukkonen論文的連結。  
<!--
I was finally convinced to tackle suffix tree construction by reading Jesper Larsson's paper for the 1996 IEEE Data Compression Conference. Jesper was also kind enough to provide me with sample code and pointers to Ukkonen's paper.
-->

### 參考書目  
<!--
References
-->

E.M. McCreight. A space-economical suffix tree construction algorithm. Journal of the ACM, 23:262-272, 1976.  
E. Ukkonen. On-line construction of suffix trees. Algorithmica, 14(3):249-260, September 1995.  

### 原始碼下載  
<!--
Source Code
-->

[stree.cpp](http://facweb.cs.depaul.edu/mobasher/classes/csc575/Suffix_Trees/stree.cpp) &emsp;&emsp; 一個簡單的程式，可以從輸入字串創建後綴樹。  
[streed.cpp](http://facweb.cs.depaul.edu/mobasher/classes/csc575/Suffix_Trees/streed.cpp) &nbsp;&ensp;&emsp; 相同的程式，添加了大量調試代碼。  
<!--
stree.cpp	A simple program that builds a suffix tree from an input string.
streed.cpp	The same program with much debugging code added.
-->
