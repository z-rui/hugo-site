---
date: 2017-01-19T16:11:00+08:00
title: makeindex预处理
---

makeindex是用来编制索引的工具。在\\(\LaTeX\\)文档中，用`\index`在文中标记索引，排版时将生成相应的`.idx`文件，每一个`\index`命令对应一行。

makeindex对`.idx`文件进行排序和格式化，然后生成`.ind`文件，该文件就是索引部分的\\(\LaTeX\\)代码。之后只需再运行一次\\(\LaTeX\\)程序即可将索引包含在文档内。

这个过程本身已经有些复杂了，现在遇到一件更麻烦的事：怎样编制中文索引。汉字没有明确的排序依据，通用的做法是按拼音或者笔画排序。[zhmakeindex](https://github.com/leo-liu/zhmakeindex)提供了这些功能。

<!--more-->

就汉字的本质来说，似乎按笔画排序更为科学。但我的感觉是拼音检索更加方便。汉语拼音方案已经标准化了，而且普及率很高。相比之下，为了检索一个字特意数一遍它有几画是比较麻烦的(另：“杂”字的起笔是“丿”还是“乙”？)。

按照拼音顺序排列的一个问题是多音字。“长度”应排在C音序下，而“长辈”应排在Z音序下。如果要实现多音字的自动识别(据我所知目前还没有程序做到这一点)，那么需要有汉语的词汇表。然而，即便有了词汇表，也不能保证读音一定正确：

> 海水朝朝朝朝朝朝朝落； 
> 浮云长长长长长长长消。 


编制英文索引时，makeindex只将各拉丁字母当作符号处理，它并不认得任何英语。只是为了编制中文索引，有必要引入这么多语言学的内容吗？(参见[Yak shaving](http://projects.csail.mit.edu/gsb/old-archive/gsb-archive/gsb2000-02-11.html)，给牦牛剪毛。)所以科学的中文排序方法真的是一个值得探讨的话题。

zhmakeindex没有提供多音字的解决方案。“长度”会被排序到Z音序下面。作者表示，可以在源文件中用诸如`\index{常度@长度}`[^1]来手工指定读音。这么做虽然很丑，但是有效。我的观点是读音不应该指定在文档中，可以单独设置一个文件，保存所有特殊读音的条目。不然的话，我可以直接在文档中写`\index{chang2 du4@长度}`和`\index{zhang4 bei4@长辈}`，即，我手动指定所有关键词的读音。然后直接用makeindex编制索引就完事了。

这样做很枯燥无聊，效率很低。但是给了我一个启示：我不需要重新实现一遍makeindex所有的功能，我可以只写一个预处理程序，把`.idx`文件中的所有类似`\index{中文}`的条目[^2]改写成`\index{zhong1 wen2@中文}`。剩下的事交给makeindex去做，就可以了。

所以我写了[idxpp](https://github.com/z-rui/idxpp)，实现了上述功能。特别照顾了一下多音字。仅靠预处理，在原理上是不太可能实现按笔画排序的。另外，中英文的混排也是一个问题。


[^1]: 在makeindex的语法中，`@`符号前面的部分是用于排序的关键字，后面的部分是显示在索引中的形式。
[^2]: 仅为说明概念。`.idx`文件实际的格式并非如此。