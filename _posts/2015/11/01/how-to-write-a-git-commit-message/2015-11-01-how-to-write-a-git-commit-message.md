---
title: "【譯】如何撰寫Git提交訊息"
date: 2015-11-01 18:00:00 +0800
categories: [Git]
tags: [Git,Commit Message]
imgurl: /assets/imgs/2015/11/01/how-to-write-a-git-commit-message
comments: true
---

這篇筆記、翻譯自博文：[How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/)，作者Chris Beams。尊重他人勞動果實，轉載請註明！  

![2015-11-01-01.png]({{ page.imgurl }}/2015-11-01-01.png)  

- [引言]({{ page.url }}#intro)  
- [七條規則]({{ page.url }}#seven-rules)  
- [建議]({{ page.url }}#tips)  
<!--Introduction | The Seven Rules | Tips--><!-- more -->  

# <a name="intro"></a>引言：為什麼好的提交訊息很重要  
<!--Introduction: Why good commit messages matter-->

如果你隨意瀏覽一些Git存儲庫的日誌，你可能會發現提交訊息或多或少都亂成一團。例如，看看我早些時候向Spring提交的一些[gems](https://github.com/spring-projects/spring-framework/commits/e5f4b49?author=cbeams)：  
<!--If you browse the log of any random git repository you will probably find its commit messages are more or less a mess. For example, take a look at these gems from my early days committing to Spring:-->  

{% highlight text %}
$ git log --oneline -5 --author cbeams --before "Fri Mar 26 2009"

e5f4b49 Re-adding ConfigurationPostProcessorTests after its brief removal in r814. @Ignore-ing the testCglibClassesAreLoadedJustInTimeForEnhancement() method as it turns out this was one of the culprits in the recent build breakage. The classloader hacking causes subtle downstream effects, breaking unrelated tests. The test method is still useful, but should only be run on a manual basis to ensure CGLIB is not prematurely classloaded, and should not be run as part of the automated build.
2db0f12 fixed two build-breaking issues: + reverted ClassMetadataReadingVisitor to revision 794 + eliminated ConfigurationPostProcessorTests until further investigation determines why it causes downstream tests to fail (such as the seemingly unrelated ClassPathXmlApplicationContextTests)
147709f Tweaks to package-info.java files
22b25e0 Consolidated Util and MutableAnnotationUtils classes into existing AsmUtils
7f96f57 polishing
{% endhighlight %}

呀！相比之下，再看看這一存儲庫[近些時候](https://github.com/spring-projects/spring-framework/commits/5ba3db?author=philwebb)的提交訊息：  
<!--Yikes. Compare that with these more recent commits from the same repository:-->  

{% highlight text %}
$ git log --oneline -5 --author pwebb --before "Sat Aug 30 2014"

5ba3db6 Fix failing CompositePropertySourceTests
84564a0 Rework @PropertySource early parsing logic
e142fd1 Add tests for ImportSelector meta-data
887815f Update docbook dependency and generate epub
ac8326d Polish mockito usage
{% endhighlight %}

你更願意看到哪一種？  
<!--Which would you rather read?-->  

前者長短不一、格式各異；後者簡潔明了、前後一致。前者天然雕飾；後者精心構築。  
<!--The former varies wildly in length and form; the latter is concise and consistent. The former is what happens by default; the latter never happens by accident.-->  

雖然許多存儲庫日誌看起來像前者，但凡事皆有例外。[Linux內核](https://github.com/torvalds/linux/commits/master)和[Git本身](https://github.com/git/git/commits/master)都是很好的例子。看看[Spring Boot](https://github.com/spring-projects/spring-boot/commits/master)，或任何由[Tim Pope](https://github.com/tpope/vim-pathogen/commits/master)管理的存儲庫。  
<!--While many repositories' logs look like the former, there are exceptions. The Linux kernel and git itself are great examples. Look at Spring Boot, or any repository managed by Tim Pope.-->  

這些存儲庫的貢獻者們知道，一條精心撰寫的Git提交訊息是和其他開發人員就一個改動的上下文進行溝通的最佳方式（實際上也是和未來的他們自己）。一個diff會告訴你作了哪些改動，但只有提交訊息能正確地告訴你為什麼。Peter Hutterer做了很好的詮釋：  
<!--The contributors to these repositories know that a well-crafted git commit message is the best way to communicate context about a change to fellow developers (and indeed to their future selves). A diff will tell you what changed, but only the commit message can properly tell you why. Peter Hutterer makes this point well:-->  

> 重建一段程式碼的上下文是一種浪費。我們不能完全避免，所以我們應竭盡所能去[減少它](http://www.osnews.com/story/19266/WTFs_m)、越多越好。提交訊息完全可以做到這點，因此，提交訊息顯示了這個開發人員是否是一個好的協作者。  
<!--Re-establishing the context of a piece of code is wasteful. We can't avoid it completely, so our efforts should go to reducing it [as much] as possible. Commit messages can do exactly that and as a result, a commit message shows whether a developer is a good collaborator.-->  

如果你還不曾考慮過如何更好地撰寫Git提交訊息的話，這可能是因為你沒有花多少時間使用`git log`和相關工具。這是一個惡性循環：因為提交歷史是結構混亂、前後矛盾的，人們不會在使用或打理它上花太多時間。而且，因為不被常用或打理，它將保持結構混亂、前後矛盾的。  
<!--If you haven't given much thought to what makes a great git commit message, it may be the case that you haven't spent much time using git log and related tools. There is a vicious cycle here: because the commit history is unstructured and inconsistent, one doesn't spend much time using or taking care of it. And because it doesn't get used or taken care of it, it remains unstructured and inconsistent.-->  

但精心打理的日誌是優雅和有用的。 `git blame`、`revert`、`rebase`、`log`、`shortlog`等子指令都會復活。查看其他人的提交和拉取請求成為值得做的事，並可獨立完成。理解數月或數年前所發生的原委就變得不僅可能，而且高效。  
<!--But a well-cared for log is a beautiful and useful thing. git blame, revert, rebase, log, shortlog and other subcommands come to life. Reviewing others' commits and pull requests becomes something worth doing, and suddenly can be done independently. Understanding why something happened months or years ago becomes not only possible but efficient.-->  

一個專案的長期成功取決於（和其他方面相比）維護工作，而對於維護人員來說、鮮有比專案日誌功能更強大的工具。花時間去學習如何正確地打理是值得的。起初可能是個麻煩、不久變成習慣，最後成為所有參與者自豪感和生產力的來源。  
<!--A project's long-term success rests (among other things) on its maintainability, and a maintainer has few tools more powerful than his project's log. It's worth taking the time to learn how to care for one properly. What may be a hassle at first soon becomes habit, and eventually a source of pride and productivity for all involved.-->  

在這篇文章中，我將探討保持一個健康的提交歷史的最基本的要點：如何撰寫個人提交訊息。其他重要實踐如壓縮提交則不會涉及。也許我會在隨後的文章裡進行探討。  
<!--In this post, I am addressing just the most basic element of keeping a healthy commit history: how to write an individual commit message. There are other important practices like commit squashing that I am not addressing here. Perhaps I'll do that in a subsequent post.-->  

大多數編程語言都有完善的構成慣用風格的慣例，例如，命名、格式等等。當然，這些慣例千差萬別；但大多數開發者都同意：選擇一種並堅持下去、遠比各行其是而混亂不堪強的多。  
<!--Most programming languages have well-established conventions as to what constitutes idiomatic style, i.e. naming and formatting and so on. There are variations on these conventions, of course, but most developers agree that picking one and sticking to it is far better than the chaos that ensues when everybody does their own thing.-->  

一個團隊訪問提交日誌的方式應當相同。為了創建有用的修訂版本歷史記錄，團隊應首先在提交訊息的慣例上達成一致，並至少確定以下三樣事情：  
<!--A team's approach to its commit log should be no different. In order to create a useful revision history, teams should first agree on a commit message convention that defines at least the following three things:-->  

<b>樣式。</b>標記語法、自動換行間距、語法、大小寫、標點符號。把這些東西都寫出來，而不是靠臆測，並讓這一切盡可能的簡單。最終的結果將是一個非常一致的日誌，不僅讀來引人入勝，而且確實會定期進行審閱。  
<!--Style. Markup syntax, wrap margins, grammar, capitalization, punctuation. Spell these things out, remove the guesswork, and make it all as simple as possible. The end result will be a remarkably consistent log that's not only a pleasure to read but that actually does get read on a regular basis.-->  

<b>內容。</b>哪些信息應當包含在提交訊息的正文（如果有的話）中？哪些不應包含？
<!--Content. What kind of information should the body of the commit message (if any) contain? What should it not contain?-->  

<b>元資料。</b>應當如何引用議題跟踪ID、拉取請求編號等等？  
<!--Metadata. How should issue tracking IDs, pull request numbers, etc. be referenced?-->  

幸運的是，如何生成規範的Git提交訊息已經有完善的慣例。事實上，它們中相當一部分業已內置於Git命令中。你並不需要重新造輪子。只要遵循以下[七條規則]({{ page.url }}#seven-rules)，你就可以撰寫出合乎規範的提交日誌。  
<!--Fortunately, there are well-established conventions as to what makes an idiomatic git commit message. Indeed, many of them are assumed in the way certain git commands function. There's nothing you need to re-invent. Just follow the seven rules below and you're on your way to committing like a pro.-->  

# <a name="seven-rules"></a>好的Git提交訊息的七條規則  
<!--The seven rules of a great git commit message-->  

> 請謹記：[這](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)  [些](http://www.git-scm.com/book/en/Distributed-Git-Contributing-to-a-Project#Commit-Guidelines)  [都](https://github.com/torvalds/subsurface/blob/master/README#L82-109)  [曾](http://who-t.blogspot.co.at/2009/12/on-commit-messages.html)  [說](https://github.com/erlang/otp/wiki/writing-good-commit-messages)  [過](https://github.com/spring-projects/spring-framework/blob/30bce7/CONTRIBUTING.md#format-commit-messages)。  
<!--Keep in mind: This has all been said before.-->  

1.[以空行隔開主題與正文]({{ page.url }}#separate)  
2.[限制主題行長度在50個字符以內]({{ page.url }}#limit-50)  
3.[主題行首字母大寫]({{ page.url }}#capitalize)  
4.[主題行結尾不要使用句號]({{ page.url }}#end)  
5.[在主題行使用祈使語氣]({{ page.url }}#imperative)  
6.[正文在72個字符處換行]({{ page.url }}#wrap-72)  
7.[用正文來解釋是什麼以及為什麼，而不是怎麼樣]({{ page.url }}#why-not-how)  
<!--
1.Separate subject from body with a blank line
2.Limit the subject line to 50 characters
3.Capitalize the subject line
4.Do not end the subject line with a period
5.Use the imperative mood in the subject line
6.Wrap the body at 72 characters
7.Use the body to explain what and why vs. how
-->  

例如：  
<!--For example:-->  

{% highlight text %}
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
{% endhighlight %}

### <a name="separate"></a>1.以空行隔開主題與正文  
<!--1. Separate subject from body with a blank line-->  

援引`git commit`[手冊頁](https://www.kernel.org/pub/software/scm/git/docs/git-commit.html#_discussion)：  
<!--From the git commit manpage:-->  

> 雖然不是必需的，在提交訊息中、以簡短（少於50個字符）的一行文字來概述改動、緊接著空一行、然後再接上更為詳盡的描述，不失為一個好主意。在提交訊息中，空行前的文本被視為提交的標題，並且這個標題會在Git中到處使用。例如，git-format-patch(1)將提交轉換為電子郵件，會將標題作為主題，而提交的其餘部分則為正文。  
<!--Though not required, it's a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, git-format-patch(1) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.-->  

首先，並非所有提交都需要主題正文俱全。有時單獨一行即可，特別是當改動非常簡單、並不需要上下文時。例如：  
<!--Firstly, not every commit requires both a subject and a body. Sometimes a single line is fine, especially when the change is so simple that no further context is necessary. For example:-->  

{% highlight text %}
Fix typo in introduction to user guide
{% endhighlight %}

無需多言；如果讀者想知道哪裡拼寫錯了，她只需查看下改動即可，例如，使用`git show`或`git diff`或`git log -p`。  
<!--Nothing more need be said; if the reader wonders what the typo was, she can simply take a look at the change itself, i.e. use git show or git diff or git log -p.-->  

如果你在命令列下進行類似的提交時，只需在`git commit`時使用開關`-m`即可：  
<!--If you're committing something like this at the command line, it's easy to use the -m switch to git commit:-->  

{% highlight text %}
$ git commit -m "Fix typo in introduction to user guide"
{% endhighlight %}

但是，當提交需要一些解釋和上下文時，你需要撰寫正文。例如：  
<!--However, when a commit merits a bit of explanation and context, you need to write a body. For example:-->  

{% highlight text %}
Derezz the master control program

MCP turned out to be evil and had become intent on world domination.
This commit throws Tron's disc into MCP (causing its deresolution)
and turns it back into a chess game.
{% endhighlight %}

這時使用開關`-m`進行提交就不那麼容易了。你真的需要一個適當的編輯器。如果你還沒有設置在命令列下配合Git使用的編輯器，請閱讀[Pro Git的這個章節](http://git-scm.com/book/ch7-1.html#Basic-Client-Configuration)。  
<!--This is not so easy to commit this with the -m switch. You really need a proper editor. If you do not already have an editor set up for use with git at the command line, read this section of Pro Git.-->  

在任何情況下，隔開主題與正文會在瀏覽日誌時得到回報。下面是完整的日誌條目：  
<!--In any case, the separation of subject from body pays off when browsing the log. Here's the full log entry:-->  

{% highlight text %}
$ git log
commit 42e769bdf4894310333942ffc5a15151222a87be
Author: Kevin Flynn <kevin@flynnsarcade.com>
Date:   Fri Jan 01 00:00:00 1982 -0200

 Derezz the master control program

 MCP turned out to be evil and had become intent on world domination.
 This commit throws Tron's disc into MCP (causing its deresolution)
 and turns it back into a chess game.
{% endhighlight %}

現在`git log --oneline`，僅僅只會打印出主題行：  
<!--And now git log --oneline, which prints out just the subject line:-->  

{% highlight text %}
$ git log --oneline
42e769 Derezz the master control program
{% endhighlight %}

或者，`git shortlog`，將提交以使用者進行分組，同樣也只簡潔地顯示了主題行：  
<!--Or, git shortlog, which groups commits by user, again showing just the subject line for concision:-->    

{% highlight text %}
$ git shortlog
Kevin Flynn (1):
      Derezz the master control program

Alan Bradley (1):
      Introduce security program "Tron"

Ed Dillinger (3):
      Rename chess program to "MCP"
      Modify chess program
      Upgrade chess program

Walter Gibbs (1):
      Introduce protoype chess program
{% endhighlight %}

區分開主題行與正文，在git中還有其他若干應用場景——但若兩者間沒有空行、它們都不會正常工作。  
<!--There are a number of other contexts in git where the distinction between subject line and body kicks in—but none of them work properly without the blank line in between.-->  

### <a name="limit-50"></a>2.限制主題行長度在50個字符以內  
<!--2. Limit the subject line to 50 characters-->  

50個字符並不是硬性的限制，而只是經驗法則【[Rule of thumb](https://en.wikipedia.org/wiki/Rule_of_thumb)，拇指規則，又作：經驗法則】。保持主題行在這個長度內可確保其可讀性，並迫使作者花點時間考慮下以最簡潔的方式來闡述發生了什麼。  
<!--50 characters is not a hard limit, just a rule of thumb. Keeping subject lines at this length ensures that they are readable, and forces the author to think for a moment about the most concise way to explain what's going on.-->  

> 建議：如果你難於概述主題，那可能是你一次提交了太多的改動。爭取[原子提交](http://www.freshconsulting.com/atomic-commits/)（一個提交一個議題）。  
<!--Tip: If you're having a hard time summarizing, you might be committing too many changes at once. Strive for atomic commits (a topic for a separate post).-->  

GitHub的使用者介面充分照顧了這些慣例。如果你超過了50個字符的限制，它會給予警告：  
<!--GitHub's UI is fully aware of these conventions. It will warn you if you go past the 50 character limit:-->  

![2015-11-01-02.png]({{ page.imgurl }}/2015-11-01-02.png)  

而且、會以省略號截斷任何長度超過69個字符的主題行：  
<!--And will truncate any subject line longer than 69 characters with an ellipsis:-->  

![2015-11-01-03.png]({{ page.imgurl }}/2015-11-01-03.png)  

所以爭取50個字符以內，但以69為硬性限制。  
<!--So shoot for 50 characters, but consider 69 the hard limit.-->  

### <a name="capitalize"></a>3.主題行首字母大寫
<!--3. Capitalize the subject line-->  

這正如聽起來那麼簡單。所有主題行起始於大寫字母。  
<!--This is as simple as it sounds. Begin all subject lines with a capital letter.-->  

例如：  
<!--For example:-->  

- <font color="green">Accelerate to 88 miles per hour</font>

而不是：  
<!--Instead of:-->  

- <font color="red">accelerate to 88 miles per hour</font>

### <a name="end"></a>4.主題行結尾不要使用句號  
<!--4. Do not end the subject line with a period-->  

主題行結尾處的標點符號是沒有必要的。再說，當你試圖將其保持在[50個字符以內]({{ page.url }}#limit-50)時，空間就很珍貴。  
<!--Trailing punctuation is unnecessary in subject lines. Besides, space is precious when you're trying to keep them to 50 chars or less.-->  

例如：  
<!--Example:-->  

- <font color="green">Open the pod bay doors</font>  

而不是：  
<!--Instead of:-->  

- <font color="red">Open the pod bay doors.</font>  

### <a name="imperative"></a>5.在主題行使用祈使語氣  
<!--5. Use the imperative mood in the subject line-->  

祈使語氣僅僅表示「如發號施令般發言或書寫」。舉幾個例子：  
<!--Imperative mood just means "spoken or written as if giving a command or instruction". A few examples:-->  

- Clean your room  
- Close the door  
- Take out the trash  

你現在讀到的七條規則的每一條都是以祈使語氣書寫（「正文在72個字符處換行」，等等）。  
<!--Each of the seven rules you're reading about right now are written in the imperative ("Wrap the body at 72 characters", etc).-->  

祈使語氣聽起來稍顯失禮；因而我們並不常用。但它卻很適合於Git提交的主題行。原因之一、<b>Git本身在代替你創建提交時使用了祈使語氣。</b>  
<!--The imperative can sound a little rude; that's why we don't often use it. But it's perfect for git commit subject lines. One reason for this is that git itself uses the imperative whenever it creates a commit on your behalf.-->  

例如，當使用`git merge`時默認創建的訊息讀起來就像這樣：  
<!--For example, the default message created when using git merge reads:-->  

{% highlight text %}
Merge branch 'myfeature'
{% endhighlight %}

而使用`git revert`時：  
<!--And when using git revert:-->  

{% highlight text %}
Revert "Add the thing with the stuff"

This reverts commit cc87791524aedd593cff5a74532befe7ab69ce9d.
{% endhighlight %}

或者點擊GitHub拉取請求上的「合併」按鈕時：  
<!--Or when clicking the "Merge" button on a GitHub pull request:-->  

{% highlight text %}
Merge pull request #123 from someuser/somebranch
{% endhighlight %}

所以，當你使用祈使語氣撰寫提交訊息時，你在遵循git本身內置的慣例。例如：  
<!--So when you write your commit messages in the imperative, you're following git's own built-in conventions. For example:-->  

<ul><font color="green">
  <li>Refactor subsystem X for readability</li>
  <li>Update getting started documentation</li>
  <li>Remove deprecated methods</li>
  <li>Release version 1.0.0</li>
</font></ul>

起初這麼撰寫可能有點彆扭。我們更習慣於以陳述語氣發言、如同報告事實一般。這就是為什麼提交訊息最終讀起來經常像這樣：  
<!--Writing this way can be a little awkward at first. We're more used to speaking in the indicative mood, which is all about reporting facts. That's why commit messages often end up reading like this:-->  

<ul><font color="red">
  <li>Fixed bug with Y</li>
  <li>Changing behavior of X</li>
</font></ul>

並且有時、提交訊息被撰寫成了內容的描述：  
<!--And sometimes commit messages get written as a description of their contents:-->  

<ul><font color="red">
  <li>More fixes for broken stuff</li>
  <li>Sweet new API methods</li>
</font></ul>

為了消除困惑，這裡有一條簡單的屢試不爽的規則。  
<!--To remove any confusion, here's a simple rule to get it right every time.-->  

<b>格式正確的git提交的主題行應該總是能夠完成下面的句子：</b>  
<!--A properly formed git commit subject line should always be able to complete the following sentence:-->  

- If applied, this commit will <b><u>你的主題行在這</u></b>  
<!--If applied, this commit will your subject line here-->  

例如：  
<!--For example:-->  

- If applied, this commit will <b><i><font color="green">refactor subsystem X for readability</font></i></b>  
- If applied, this commit will <b><i><font color="green">update getting started documentation</font></i></b>  
- If applied, this commit will <b><i><font color="green">remove deprecated methods</font></i></b>  
- If applied, this commit will <b><i><font color="green">release version 1.0.0</font></i></b>  
- If applied, this commit will <b><i><font color="green">merge pull request #123 from user/branch</font></i></b>  

請注意，這對於其他非祈使語氣的格式是行不通的：  
<!--Notice how this doesn't work for the other non-imperative forms:-->  

- If applied, this commit will <b><i><font color="red">fixed bug with Y</font></i></b>  
- If applied, this commit will <b><i><font color="red">changing behavior of X</font></i></b>  
- If applied, this commit will <b><i><font color="red">more fixes for broken stuff</font></i></b>  
- If applied, this commit will <b><i><font color="red">sweet new API methods</font></i></b>  

> 謹記：使用祈使語氣很重要這一點、僅限於主題行。當你撰寫正文時，可以放寬這個限制。  
<!--Remember: Use of the imperative is important only in the subject line. You can relax this restriction when you're writing the body.-->  

### <a name="wrap-72"></a>6.正文在72個字符處換行  
<!--6. Wrap the body at 72 characters-->  

Git從不會自動換行。當你撰寫提交訊息正文時，你得留意右邊距，並手動換行。  
<!--Git never wraps text automatically. When you write the body of a commit message, you must mind its right margin, and wrap text manually.-->  

推薦在72個字符處換行，這樣git在保持長度在80個字符以內的同時有足夠的空間來縮進文本。  
<!--The recommendation is to do this at 72 characters, so that git has plenty of room to indent text while still keeping everything under 80 characters overall.-->  

這裡一個好的文本編輯器可以有所幫助。例如，當你撰寫git提交時，配置Vim在72個字符處換行是很容易的。不過，通常IDE們對於提交訊息的文本換行所提供的智能支持都很糟糕（儘管在最近的版本中，IntelliJ IDEA  [終於](http://youtrack.jetbrains.com/issue/IDEA-53615)  [變得](http://youtrack.jetbrains.com/issue/IDEA-53615#comment=27-448299)  [好多](http://youtrack.jetbrains.com/issue/IDEA-53615#comment=27-446912)  了）。  
<!--A good text editor can help here. It's easy to configure Vim, for example, to wrap text at 72 characters when you're writing a git commit. Traditionally, however, IDEs have been terrible at providing smart support for text wrapping in commit messages (although in recent versions, IntelliJ IDEA has finally gotten better about this).-->  

### <a name="why-not-how"></a>7.用正文來解釋是什麼以及為什麼，而不是怎麼樣  
<!--7. Use the body to explain what and why vs. how-->  

[來自Bitcoin Core的提交](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6)是一個解釋改動內容和原因的很好的例子：  
<!--This commit from Bitcoin Core is a great example of explaining what changed and why:-->  

{% highlight text %}
commit eb0b56b19017ab5c16c745e6da39c53126924ed6
Author: Pieter Wuille <pieter.wuille@gmail.com>
Date:   Fri Aug 1 22:57:55 2014 +0200

   Simplify serialize.h's exception handling

   Remove the 'state' and 'exceptmask' from serialize.h's stream
   implementations, as well as related methods.

   As exceptmask always included 'failbit', and setstate was always
   called with bits = failbit, all it did was immediately raise an
   exception. Get rid of those variables, and replace the setstate
   with direct exception throwing (which also removes some dead
   code).

   As a result, good() is never reached after a failure (there are
   only 2 calls, one of which is in tests), and can just be replaced
   by !eof().

   fail(), clear(n) and exceptions() are just never called. Delete
   them.
{% endhighlight %}

看看[完整的diff](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6)，並想想作者當時花時間去提供的這個上下文節約了同事和未來的提交者多少時間。如果他沒有這麼做，那麼它可能永遠丟失了。  
<!--Take a look at the full diff and just think how much time the author is saving fellow and future committers by taking the time to provide this context here and now. If he didn't, it would probably be lost forever.-->  

在大多數情況下，你可以略去改動的具體細節。在這方面程式碼一般都是自解釋的（如果程式碼複雜到需要額外的解釋，那就是源碼註釋的工作了）。僅專注於把改動原因言明擺在首位——改動前它們如何工作（那樣有什麼問題），現在如何工作，以及為什麼你決定以這種方式解決問題。  
<!--In most cases, you can leave out details about how a change has been made. Code is generally self-explanatory in this regard (and if the code is so complex that it needs to be explained in prose, that's what source comments are for). Just focus on making clear the reasons you made the change in the first place—the way things worked before the change (and what was wrong with that), the way they work now, and why you decided to solve it the way you did.-->  

在未來感謝你的維護者也許就是你自己！  
<!--The future maintainer that thanks you may be yourself!-->  

# <a name="tips"></a>建議
<!--Tips-->  

### 學著去熱愛命令列。把IDE丟在一邊吧。  
<!--Learn to love the command line. Leave the IDE behind.-->  

明智的做法是去擁抱命令行，理由如git子指令那樣多。Git極其強大；IDE們同樣也是，但卻各不相同。我每天都在用IDE (IntelliJ IDEA)、其它用的也比較多 (Eclipse)，但我從未見過有整合git的IDE能夠匹敵命令列的易用與強大（一旦你了解它）。  
<!--For as many reasons as there are git subcommands, it's wise to embrace the command line. Git is insanely powerful; IDEs are too, but each in different ways. I use an IDE every day (IntelliJ IDEA) and have used others extensively (Eclipse), but I have never seen IDE integration for git that could begin to match the ease and power of the command line (once you know it).-->  

某些git相關的IDE功能是極好的，比如呼叫`git rm`刪除一個檔案，以及用`git`重命名檔案。當你開始嘗試通過IDE去提交、合併、重訂、或做複雜的歷史分析時，一切都將分崩離析。  
<!--Certain git-related IDE functions are invaluable, like calling git rm when you delete a file, and doing the right stuff with git when you rename one. Where everything falls apart is when you start trying to commit, merge, rebase, or do sophisticated history analysis through the IDE.-->  

當談到充分發揮git的強大功能時，那就非命令列莫屬了。
<!--When it comes to wielding the full power of git, it's command-line all the way.-->  

謹記，不管你用Bash或Z Shell，都有[tab補齊腳本](http://git-scm.com/book/en/Git-Basics-Tips-and-Tricks)來減輕記住子指令和開關的痛苦。
<!--Remember that whether you use Bash or Z shell, there are tab completion scripts that take much of the pain out of remembering the subcommands and switches.-->  

### 閱讀Pro Git  
<!--Read Pro Git-->  

[Pro Git](http://git-scm.com/book)這本書是網上免費提供的，寫的也非常棒。好好使用吧！  
<!--The Pro Git book is available online for free, and it's fantastic. Take advantage!-->  
