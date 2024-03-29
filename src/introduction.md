# 概述

## 什么是Github Flavored Markdown?

Github Flavored Markdown(简称为GFM)是一个支持GitHub.com和GitHub Enterprise的一种Markdown规范。

本文基于CommmonMark规范定义了本规范的语法和语义。

GFM是CommonMark的超集。它包括了Github user支持的和CommonMark规范中未指出的。

虽然GFM支持广泛的输入，但值得注意的是GitHub.com和GitHub Enterprise在GFM转换为HTML后执行了额外的后处理和清理，以确保网站的安全性和一致性。

## 什么是Markdown

Markdown是一种基于电子邮件和新闻组帖格式规范的用于编写结构化文档的纯文本格式。它是由John Gruber(在Aaron Swartz的帮助下)开发的，并于2004年以[语法描述](https://daringfireball.net/projects/markdown/syntax)和Perl脚本(Markdown.pl)的形式发布，用于将Markdown转换为HTML。在接下来的十年中出现了许多语言开发的数十种实现。有些扩展了原始的Markdown语法，加入了脚注、表格和其他文档元素的约定。有些允许以HTML以外的格式呈现Markdown文档。Reddit、StackOverflow和GitHub等网站有数百万人使用Markdown。Markdown开始被用于网络之外的领域，比如撰写书籍、文章、幻灯片、信件和课堂笔记。

Markdown与许多其他轻量级标记语法(通常更容易编写)的区别在于它的可读性。正如Gruber所说:

        Markdown的格式化语法最重要的设计目标是使其尽可能具有可读性。markdown格式的文档应该是可以原样发布的，就像纯文本一样，看起来不像被标记或格式说明标记过。(http://daringfireball.net/projects/markdown/)

这一点可以通过比较AsciiDoc的样本和Markdown的等效样本来说明。以下是AsciiDoc手册中的一个示例:

```txt
1. 列出第一项。
+
列出第一项，接着是第二段，后面跟着一个缩进。
+
.................
$ ls *.sh
$ mv *.sh ~/tmp
.................
+
清单项以第三段继续。

2. 用一个开放块继续列出第二项。
+
--
本款是前一项的一部分。

a.此列表是嵌套的，不需要显式项延续。
+
本款是前一项的一部分。

b.列出b项。

这一段属于外表的第二项。
--
```