# 1 概述

## 1.1 什么是Github Flavored Markdown?

Github Flavored Markdown(简称为GFM)是一个支持GitHub.com和GitHub Enterprise的一种Markdown规范。

本文基于CommmonMark规范定义了本规范的语法和语义。

GFM是CommonMark的超集。它包括了Github user支持的和CommonMark规范中未指出的。

虽然GFM支持广泛的输入，但值得注意的是GitHub.com和GitHub Enterprise在GFM转换为HTML后执行了额外的后处理和清理，以确保网站的安全性和一致性。

## 1.2 什么是Markdown

Markdown是一种基于电子邮件和新闻组帖格式规范的用于编写结构化文档的纯文本格式。它是由John Gruber(在Aaron Swartz的帮助下)开发的，并于2004年以[语法描述](https://daringfireball.net/projects/markdown/syntax)和Perl脚本(`Markdown.pl`)的形式发布，用于将Markdown转换为HTML。在接下来的十年中出现了许多语言开发的数十种实现。有些扩展了原始的Markdown语法，加入了脚注、表格和其他文档元素的约定。有些允许以HTML以外的格式呈现Markdown文档。Reddit、StackOverflow和GitHub等网站有数百万人使用Markdown。Markdown开始被用于网络之外的领域，比如撰写书籍、文章、幻灯片、信件和课堂笔记。

Markdown与许多其他轻量级标记语法(通常更容易编写)的区别在于它的可读性。正如Gruber所说:

        Markdown的格式化语法最重要的设计目标是使其尽可能具有可读性。markdown格式的文档应该是可以原样发布的，就像纯文本一样，看起来不像被标记或格式说明标记过。(http://daringfireball.net/projects/markdown/)

## 1.3 为什么需要一个规范

John Gruber的[对Markdown语法的规范描述](https://daringfireball.net/projects/markdown/syntax)并没有明确地指定语法。下面是一些它没有回答的问题:

1. 子列表需要多少缩进?规范指明连续段落需要缩进四个空格，但对子列表没有明确说明。我们很自然地认为它们也必须缩进四个空格，但是`Markdown.pl`不需要这样做。

2. 在引号或标题前需要空行吗?大多数实现不需要空行。然而，这可能会导致硬包装文本的意外结果，并且还会导致解析中的歧义(注意，有些实现将标题放在blockquote中，而其他实现则没有)。

3. 缩进代码块前需要空行吗?(`Markdown.pl`需要，但文档中没有提到，而且一些实现不需要。)

    ```Markdown
    paragraph
    code?
    ```

4. 当一个列表被\<p\>标签包括的时候的有什么明确规则？一个列表可以部分松散部分紧凑吗？就像这样的？

    ```Markdown
    1. one

    2. two
    3. three
    ```

    或者这样的？

    ```Markdown
    1. one
       - a
    
       - b
    2. two
    ```

5. 列表标记可以缩进吗?有序列表标记可以右对齐吗?

    ```Markdown
     8. item 1
     9. item 2
    10. item 2a
    ```

6. 这是一个包含分行符的列表还是两个被分行符分隔的列表？

    ```Markdown
    * a
    *****
    * b
    ```

7. 当列表标记从数字变为项目符号时，我们有两个列表还是一个列表?(Markdown语法描述建议有两个，但是perl脚本和许多其他实现产生一个。)

    ```Markdown
    1. fee
    2. fie
    - foe
    - fum
    ```

8. 内联结构的标记(tab键上面的那个键)的优先级规则是什么?例如，下面的链接是否有效，或者代码范围是否优先?

    ```Markdown
    [a backtick (`)](/url) and [another backtick (`)](/url).
    ```

9. 粗体标记和粗体标记的优先顺序是什么?例如，应该如何解析以下内容?

    ```Markdown
    *foo *bar* baz*
    ```

10. 块级和内联级结构之间的优先规则是什么?例如，应该如何解析以下内容?

    ```Markdown
    - `a long code span can contain a hyphen like this
      - and it can screw things up`
    ```

11. 列表项可以包括章节标题吗?(Markdown.pl不允许这样做，但允许块引号包含标题。)

    ```Markdown
    - # Heading
    ```

12. 列表元素可以为空吗？

    ```Markdown
    * a
    *
    * b
    ```

13. 链接引用可以在块引号或列表项中定义吗?

    ```Markdown
    > Blockquote [foo].
    >
    > [foo]: /url
    ```

14. 如果同一引用有多个定义，哪个优先?

    ```Markdown
    [foo]: /url1
    [foo]: /url2

    [foo][]
    ```

在没有规范的情况下，早期的实现者查阅了`Markdown.pl`来解决这些歧义。但是`Markdown.pl`有很多bug，在很多情况下给出的结果都很糟糕，所以它并不不能作为一个让人满意的规范。

因为没有明确的规范，实现有很大的差异。因此，用户经常惊讶地发现，在一个系统(比如GitHub wiki)上以一种方式呈现的文档在另一个系统(比如使用pandoc转换为docbook)上呈现的方式不同。更糟糕的是，由于Markdown中没有任何东西被视为“语法错误”，因此通常不会立即发现差异。

## 1.4 关于本文档

本文档是为了规定没有歧义的Markdown语法。它包含许多并列的Markdown和HTML示例。这些测试的目的是作为一致性测试。附带的脚本`spec_tests.py`可用于对任何Markdown程序运行测试:

```shell
python test/spec_tests.py --spec spec.txt --program PROGRAM
```

原文，这段话暂时没理解是什么意思：Since this document describes how Markdown is to be parsed into an abstract syntax tree, it would have made sense to use an abstract representation of the syntax tree instead of HTML. But HTML is capable of representing the structural distinctions we need to make, and the choice of HTML for the tests makes it possible to run the tests against an implementation without writing an abstract syntax tree renderer.

该文档是从一个用`带有用于并行测试插件的Markdown`编写的`spec.txt`生成的。脚本`tools/makspec.py`可用于将`spec.txt`转换为HTML或CommonMark(然后可以转换为其他格式)。

在示例中，→字符用于表示制表符。


