# **初识Latex**

## 概述

Latex粗略的看是一种标记式排版语言。

整个文档通过一些标记（命令）分成结构化的部分，Latex的命令以反斜线`\`开头，命令一般以**英文单词**命名，有的可以带**参数**。

Latex的编译过程：
编写 -> TEX源文档 -> 输入 -> 编译程序 - > 输出 -> PDF文档 -> 发布（可以根据输出结果修改源文件）

## 例子
```Latex
% 提纲例子
% -*- coding ：utf-8 -*-
% gougu.tex
% 勾股定理
\documentclass[UTF8]{ctexart}
% 多参数不能另起中括号另写，应该写在同一个中括号内，用逗号隔开

\title{杂谈勾股定理}
\author{张三}
\date{\today}

\bibliography{plain}

\begin{document}

\maketitle
\tableofcontents
\section{勾股定理在古代}
\section{勾股定理的近代形式}

\bibliography{math}
\end{document}
```

`%`：以此符号开头开头的内容是注释，`%`后面的内容都会忽略
`\documentclass`：文档类，中文短文所以使用的是ctexart，并且用`[UTF8]`选项说明编码。
`\title`：声明文章的标题
`\author`：声明文章的作者
`\date`：写作日期。`\today`表示的是今天。
`\bibliographystyle`：声明参考文件格式。
上述内容均处于`\begin{document}`标签前面，这部分称为导言区。导言区通常对文章性质做设置或者自定义一些命令。

`\begin{document}`和`\end{document}`标签声明了一个document环境，里面的内容是正文部分，也就是直接输出的部分。
`\maketitle`：用于实际展示文章的标题。
`\tableofcontents`：用于输出文章目录。

> Latex对空格的处理
> - 单独使用换行符对连续文字换行，并不会使文字另起一段，而只是起到使源代码更容易读的作用。只有空白行（且这个空白行里面只能含有空格），会使文字另起一段，且作用只是使文字另起一段，并不是用来增大行间距。
> - 段落前面不需要打空格，即使打了空格也会被Latex忽略，Latex会自动完成段落的首行缩进。（不要用文字的全角空格）
> - 通常汉字后面的空格会被忽略，其他符号后面的空格会被保留。

## Latex命令与环境
```Latex
.....见于欧几里得\footnote{欧几里得，约公元前 330-275 年。}《几何原本》的作者.....

\begin{quote}
\zihao{-5} 这是一段引用测试
\end{quote}
```
Latex命令的格式为：
无参数：`\command`
有n个参数：`\command<arg1><arg2>...`
有可选参数：`\command[<arg.opt>]<arg1><arg2>...`

Latex环境：
`\zihao`字号命令与`\footnote{内容}`脚注命令不一样，脚注命令只是在对括号内的内容发生效果，但是字号命令会影响命令后面的所有文字，直到分组结束，这种命令又称声明。
分组限定了声明的作用范围。`\begin{quote}`和`\end{quote}`这一组命令就产生了一个分组。

Latex环境的格式：
```Latex
\begin{环境名}
环境内容
\end{环境名}

% 有参数或可选参数的环境格式
\begin{环境名}[可选参数]其他参数
环境内容
\end{环境名}
```