---
title: "Markdown/HTML常用語法小結"
date: 2015-09-13 18:00:00 +0800
categories: [Website]
tags: [Markdown,HTML,Syntax]
comments: true
---

Markdown語法請參考[這裡][1]，HTML語法請參考[這裡][2]。  
以下是類比HTML語法做的小結，只涵蓋了最基本最通用的語法。  

  [1]: http://markdown.tw/
  [2]: http://www.powmo.com/

<!-- more -->  

<table border="1" width="100%" height="100%" style="table-layout:fixed;word-wrap:break-word;word-break;break-all;">
  <colgroup>
    <col span="1" style="width:20%;">
    <col span="1" style="width:80%;">
  </colgroup>
  <tr>
    <th>
      顯示
    </th>
    <th>
      語法
    </th>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      斷行
    </td>
    <td>
      在行尾加上兩個以上的<span style="background-color:lightgrey">空格</span>然後按<span style="background-color:lightgrey">回車</span><br />
      <ul class="noindent" type="circle">
        <li>Markdown預設</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;br</span> <span class="nt">/&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>或可寫作<span style="background-color:lightgrey">&lt;br></span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td rowspan="2">
      <h1>標題一</h1>
      <h2>標題二</h2>
      <h3>標題三</h3>
      <h4>標題四</h4>
      <h5>標題五</h5>
      <h6>標題六</h6>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p"># 標題一</span>

<span class="p">## 標題二</span>

<span class="p">### 標題三</span>

<span class="p">#### 標題四</span>

<span class="p">##### 標題五</span>

<span class="p">###### 標題六</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li><span style="background-color:lightgrey">#</span>與標題文字間須有<span style="background-color:lightgrey">空格</span></li>
        <li>標題前後須有空行</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;h1&gt;</span>標題一<span class="nt">&lt;/h1&gt;</span>
<span class="nt">&lt;h2&gt;</span>標題二<span class="nt">&lt;/h2&gt;</span>
<span class="nt">&lt;h3&gt;</span>標題三<span class="nt">&lt;/h3&gt;</span>
<span class="nt">&lt;h4&gt;</span>標題四<span class="nt">&lt;/h4&gt;</span>
<span class="nt">&lt;h5&gt;</span>標題五<span class="nt">&lt;/h5&gt;</span>
<span class="nt">&lt;h6&gt;</span>標題六<span class="nt">&lt;/h6&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <hr />
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p">---</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>前後須有空行</li>
        <li>或可寫作<span style="background-color:lightgrey">***</span></li>        
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;hr</span> <span class="nt">/&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
      <li>或可寫作<span style="background-color:lightgrey">&lt;hr></span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <blockquote>引言</blockquote>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="gt">&gt; 引言  </span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>引言區塊前後須有空行</li>
        <li>行尾須有兩個以上的<span style="background-color:lightgrey">空格</span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;blockquote&gt;</span>引言<span class="nt">&lt;/blockquote&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="3">
      <b>粗體</b>
    </td>
    <td>
      <div class="highlight"><pre><code class="language-md" data-lang="md"><span class="gs">**粗體**</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;b&gt;</span>粗體<span class="nt">&lt;/b&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>STRONG標籤效果類同</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;span</span> <span class="na">style=</span><span class="s">"font-weight:bold;"</span><span class="nt">&gt;</span>粗體<span class="nt">&lt;/span&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center">
      <i>italics</i>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;i&gt;</span>italics<span class="nt">&lt;/i&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
      <li>傳統上漢字並沒有斜體、故而不建議對其使用</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <font color="blue">藍色</font>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;font</span> <span class="na">color=</span><span class="s">"blue"</span><span class="nt">&gt;</span>藍色<span class="nt">&lt;/font&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;span</span> <span class="na">style=</span><span class="s">"color:blue;"</span><span class="nt">&gt;</span>藍色<span class="nt">&lt;/span&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center">
      <font style="text-decoration:overline">上劃線</font>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;font</span> <span class="na">style=</span><span class="s">"text-decoration:overline"</span><span class="nt">&gt;</span>上劃線<span class="nt">&lt;/font&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center">
      <u>下劃線</u>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;u&gt;</span>下劃線<span class="nt">&lt;/u&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>不要採用在文字兩端加<span style="background-color:lightgrey">_</span>的方式</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center">
      <del>刪除線</del>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;del&gt;</span>刪除線<span class="nt">&lt;/del&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>不要採用在文字兩端加<span style="background-color:lightgrey">~~</span>的方式</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center">
      文字<sub>下標</sub>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html">文字<span class="nt">&lt;sub&gt;</span>下標<span class="nt">&lt;/sub&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center">
      文字<sup>上標</sup>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html">文字<span class="nt">&lt;sup&gt;</span>上標<span class="nt">&lt;/sup&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center">
      <ruby>
      注<rp>(</rp><rt>ㄓㄨˋ</rt><rp>)</rp>
      音<rp>(</rp><rt>ㄧㄣˉ</rt><rp>)</rp>
      </ruby>
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;ruby&gt;</span>
注<span class="nt">&lt;rp&gt;</span>(<span class="nt">&lt;/rp&gt;&lt;rt&gt;</span>ㄓㄨˋ<span class="nt">&lt;/rt&gt;&lt;rp&gt;</span>)<span class="nt">&lt;/rp&gt;</span>
音<span class="nt">&lt;rp&gt;</span>(<span class="nt">&lt;/rp&gt;&lt;rt&gt;</span>ㄧㄣˉ<span class="nt">&lt;/rt&gt;&lt;rp&gt;</span>)<span class="nt">&lt;/rp&gt;</span>
<span class="nt">&lt;/ruby&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>RP標籤或可省略</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <ul class="noindent">
        <li>週一</li>
        <li>週二</li>
        <li>週三</li>
      </ul>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p">-</span> 週一  
<span class="p">-</span> 週二  
<span class="p">-</span> 週三  </code></pre></div>
      <ul class="noindent" type="circle">
        <li><span style="background-color:lightgrey">-</span>與項目間須有<span style="background-color:lightgrey">空格</span></li>
        <li>項目行尾須有兩個以上的<span style="background-color:lightgrey">空格</span></li>
        <li>項目清單前後須有空行</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;ul&gt;</span>
  <span class="nt">&lt;li&gt;</span>週一<span class="nt">&lt;/li&gt;</span>
  <span class="nt">&lt;li&gt;</span>週二<span class="nt">&lt;/li&gt;</span>
  <span class="nt">&lt;li&gt;</span>週三<span class="nt">&lt;/li&gt;</span>
<span class="nt">&lt;/ul&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>項目符號的種類由type屬性指定：</li>
          <ul type="square">
            <li>disc、實心圓形、預設</li>
            <li>circle、空心圓形</li>
            <li>square、實心方形</li>
          </ul>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <ol class="noindent" start="1">
        <li>蘋果</li>
        <li>香蕉</li>
        <li>葡萄</li>
      </ol>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p">1.</span> 蘋果  
<span class="p">2.</span> 香蕉  
<span class="p">3.</span> 葡萄  </code></pre></div>
      <ul class="noindent" type="circle">
        <li>編號不可為0</li>
        <li>編號與項目間須有<span style="background-color:lightgrey">空格</span></li>
        <li>項目行尾須有兩個以上的<span style="background-color:lightgrey">空格</span></li>
        <li>項目清單前後須有空行</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;ol</span> <span class="na">start=</span><span class="s">"1"</span><span class="nt">&gt;</span>
  <span class="nt">&lt;li&gt;</span>蘋果<span class="nt">&lt;/li&gt;</span>
  <span class="nt">&lt;li&gt;</span>香蕉<span class="nt">&lt;/li&gt;</span>
  <span class="nt">&lt;li&gt;</span>葡萄<span class="nt">&lt;/li&gt;</span>
<span class="nt">&lt;/ol&gt;</span></code></pre></div>
      <ul class="noindent" type="circle">
        <li>項目編號由type與start屬性指定</li>
        <li>start屬性指定起始編號、預設為1</li>
        <li>type屬性可為：</li>
          <ul type="square">
            <li>1、阿拉伯數字、預設</li>
            <li>a、小寫英文字母</li>
            <li>A、大寫英文字母</li>
            <li>i、小寫羅馬數字</li>
            <li>I、大寫羅馬數字</li>
          </ul>
      </ul>
    </td>
  </tr>
  <tr>
    <td rowspan="2">
<div class="highlight"><pre><code class="language-text" data-lang="text">程式碼</code></pre></div>
    </td>
    <td>
<div class="highlight"><pre><code class="language-text" data-lang="text"><span class="nt">{</span>% highlight text <span class="nt">%</span>}
程式碼
<span class="nt">{</span>% endhighlight <span class="nt">%</span>}</code></pre></div>
      <ul class="noindent" type="circle">
        <li>highlight後隨字串指定程式碼所採編程語言</li>
        <li>程式碼所採編程語言或可略去不寫</li>
        <li>rouge所支援編程語言清單參考<a href="https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers">這裡</a></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-text" data-lang="text">```text
程式碼
```</code></pre></div>
      <ul class="noindent" type="circle">
        <li><span style="background-color:lightgrey">```</span>後隨字串指定程式碼所採編程語言</li>
        <li>不太推薦採用這種風格</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center">
      空格
    </td>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&amp;nbsp;</span> : 空 格(x1)<br><span class="nt">&amp;ensp;</span> : 空  格(x2)<br><span class="nt">&amp;emsp;</span> : 空    格(x4)</code></pre></div>
      <ul class="noindent" type="circle">
        <li>當需要向MD檔插入複數空格時格外有用</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <img src="/assets/icons/favicon-32x32.png" alt="favicon-32x32.png"/>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p">![</span><span class="nv">favicon-32x32.png</span><span class="p">](</span><span class="sx">/assets/icons/favicon-32x32.png</span><span class="p">)</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;img</span> <span class="na">src=</span><span class="s">"/assets/icons/favicon-32x32.png"</span> <span class="na">alt=</span><span class="s">"favicon-32x32.png"</span><span class="nt">/&gt;</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <a href="https://xiwan.info">西灣筆記</a>
    </td>
    <td>
<div class="highlight"><pre><code class="language-md" data-lang="md"><span class="p">[</span><span class="nv">西灣筆記</span><span class="p">](</span><span class="sx">https://xiwan.io</span><span class="p">)</span></code></pre></div>
    </td>
  </tr>
  <tr>
    <td>
<div class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;a</span> <span class="na">href=</span><span class="s">"https://xiwan.io"</span><span class="nt">&gt;</span>西灣筆記<span class="nt">&lt;/a&gt;</span></code></pre></div>
    </td>
  </tr>  
</table>
