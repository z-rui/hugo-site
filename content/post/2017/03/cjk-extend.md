---
date: 2017-03-29T00:15:27+08:00
title: 扩展汉字的编码
---

2013 年的《通用规范汉字表》是目前简体中文字形的最新标准。其中有若干字为新的类推简化字，如字典末尾均有的「<ruby>𬭊<rt>dù</rt></ruby>」「<ruby>𬭳<rt>xǐ</rt></ruby>」「<ruby>𬭛<rt>bō</rt></ruby>」「<ruby>𬭶<rt>hēi</rt></ruby>」「<ruby>鿏<rt>mài</rt></ruby>」（105--109号元素名）。

除「鿏」外，其余均位于 Unicode 8.0 新增的 Ext-E 扩展分区内。在较新的操作系统上，安装有相应的字体，这些汉字均可正常显示。可能并非所有输入法均能输入这些字。（可以根据上面给出的拼音测试一下自己的输入法是否符合国家标准。）

Ext-E 的范围是2B820--2CEAF。使用UTF-8或GB18030编码均需4个字节：

| 字  |  GB18030   |     UTF-8     |
| --- | ---------- | ------------- |
| 𬭊  | `9933a838` | `f0 ac ad 8a` |

<!--more-->

中学教科书上也有一些类推简化字在计算机上是较难录入的：

> 绝𪩘[^1]多生怪柏，悬泉瀑布，飞漱其间，清荣峻茂，良多趣味。
<p style="text-align: right;">——《水经注·三峡》</p>

> 木直中绳，𫐓[^2]以为轮，其曲中规。
<p style="text-align: right;">——《荀子》</p>

类推简化字是一个大坑。《字表》只有8000余字，而汉字数以万计。表外字如何处理，一直存在争议。大量古代用字现已不再使用，为它们类推简化写法毫无意义（在信息化时代，意味着要为它们制作计算机字体——尽管从没有、也不会有人用它们）。不做类推，又限制了某些汉字在日常生活中的使用，化学元素名即为一例。必须佩服化学家们的创造力。104--109号元素名称的简体字在1998年版的《新华字典》中就已经存在了，难以置信的是，直至2014年以前，计算机系统竟均无法处理（一般用临时拼字或造字的方法解决，不规范的场合也有用繁体字「𨧀𨭎𨨏𨭆䥑」的，例如：百度百科的[「元素周期表」](http://baike.baidu.com/item/%E5%85%83%E7%B4%A0%E5%91%A8%E6%9C%9F%E8%A1%A8/282048)[「䥑」](http://baike.baidu.com/item/%E4%A5%91)[「钅杜」](http://baike.baidu.com/item/%E9%92%85%E6%9D%9C)等词条）。

新闻和《健康教育读本》上经常见到的「二<ruby>𫫇<rt>è</rt></ruby>英」，竟有四种写法，除了正规的写法外，还有：[「二恶英」](http://baike.baidu.com/item/%E4%BA%8C%E6%81%B6%E8%8B%B1)（「恶」为「<ruby>噁<rt>ě</rt></ruby>」的简化字）、[「二⿰口恶英」](http://jyjgs.aqsiq.gov.cn/zxbs/bszn/200610/t20061026_6727.htm)（拼字）和[「二噁英」](http://baike.baidu.com/item/%E4%BA%8C%E5%99%81%E8%8B%B1)（用繁体）。

我这种深受应试教育毒害的读书人，经常会把这些没用的东西当成学问。

中学教科书的做法是一方面谨慎选材（尽量规避未简化字），另一方面总是做类推简化。如古人名「赵孟<ruby>𫖯<rt>fǔ</rt></ruby>」等。事实上《字表》亦收录了教科书中出现的类推简化字。

今天我还恰好在景点里见到了一种名为「红花<ruby>檵<rt>jì</rt></ruby>木」的植物，「㡭」这个形状还出现在「繼」中，后者被简化为了「继」，但「檵」要不要类推简化？不知道。《字表》里既没有「檵」字（但该字包含于1988年的《现代汉语通用字表》中），又没有（不知哪儿来的类推简化字）「𪲛」。那么这种植物的名字的规范写法是什么？


[^1]: yǎn，常被录入为繁体字「巘」。例如：百度百科[《三峡》](http://baike.baidu.com/item/%E4%B8%89%E5%B3%A1/9799182)词条。
[^2]: róu，常被录入为繁体字「輮」。例如：百度百科[《劝学》](http://baike.baidu.com/item/%E5%8A%9D%E5%AD%A6/1055)词条。
