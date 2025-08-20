# **【草稿】使用Latex绘制电路图**

## 概述

数字电路绘制的时候使用的包为`circuitikz`。
`circuitikz`提供了`IEEE`标准的逻辑门。

## 具体绘制方法
#### 一、使用circuitikz包环境配置
**导入包：** 导言区里面写`\usepackage{circuitikz}`即可。
**全局设置：** `\ctikzset{logic ports=ieee}.`所有逻辑门使用IEEE标准。
**环境：** 使用`\begin{circuitikz}`和`\end{circuitikz}`框定指令的编写环境。
**缩放配置：** 可以在`\begin{circuitikz}`标签后面使用`[scale= 0.8, transform shape]`来确认图片的缩放比例。

**初始环境搭建案例：**
```Latex
\documentclass{article}
\usepackage{circuitikz}

\begin{document}

\begin{circuitikz}[scale = 0.7, transform shape]%这里必须添加transform shape，中间使用逗号隔开。
%绘制指令编写区间
\end{circuitikz}

\end{document}
```

#### 二、基础知识储备
由于circuitikz是tikz的一个包装，故在使用circuitikz的时候有必要了解一下关于tikz的有关知识，tikz的指令都可以在circuitikz中使用。

tikz的图形主要由三部分构成：坐标（coordinate）、路径（path）、节点（node）。他们关系大致如下：
+ 节点是位于一个坐标的二维结构
+ 路径是连接若干个坐标的一维结构

tikz指令示例：
```Latex
\command[options] <other information>; %一定要有结尾分号
```

**坐标**
`(x,y)`就表示一个坐标，我们一般会搭配路径或节点当中使用。
可以通过坐标先生成一个辅助网格。
```Latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
\end{circuitikz}
```
通过上述指令就可以得到一个6`*`6的网格。

这里暂时不必在意`\draw`指令的含义，只需要知道我们生成了一个从`(0,0)`到`(5,5)`，间隔为1的网格即可。注意到这个指令给三个选项赋值：网格的间距`step`，以及两个绘制选项`gray`和`very thin`，表示颜色和线的粗细。

声明坐标的时候，我们实际上做的事吧tikz中的“笔尖”移动到了那个位置，从哪里开始绘制图形。
在笔尖确定的情况下，我们也可以使用相对坐标，根据上一个坐标的偏移量来声明下一个坐标。如果笔尖位于`(x,y)`，那么下一个坐标可以用`+(a,b)`或者`++(a,b)`来表示，意为将笔尖向右移动`a`，向上移动`b`，即`(x+a, y+b)`。
一个加号和两个加号的区别在于两个加号声明的相对坐标会变成新的“笔尖位置”，而一个加号的坐标不会影响“笔尖位置”，笔尖仍然留在`(x, y)`上，但这只会在声明第二个相对坐标时有区别。下面带吗会生成一样的网格图案：
```Latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid +(5,5); %或者++(5,5)
\end{circuitikz}
```

**路径**
一个路径事连接若干个点的线。最简单的声明路径的方法（也就是在circuitikz）中少数派的上用场的方法之一）：
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\path[draw] (1,1) -- (4,5);
\end{circuitikz}
```

路径一般的声明方式是：
`\path[options] <cord 1> <connection> <cord 2>`
这里的`<connection>`除了`--`，还有`|-`和`-|`两种选项，前者会先垂直，再水平地从第一点走到第二个点；后者则相反，先水平，再垂直。我们一直使用的`grid`，也是一种路径的连接方式。

```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\path[draw] (1,1) -- (4,5);
	\path[draw, red] (1,1) -| (4,5);
	\path[draw, blue] (1,1) |- (4,5);
\end{circuitikz}
```

一条路径可以连续连接多个坐标，在最后使用`--cycle`会使得路径闭合。同时，一直用`\path[draw]`显得太过累赘。我们可以简单地用`\draw`替换。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (1,1) -- (4,5) -- (3,2) -- cycle;
\end{circuitikz}
```

这里最后节点使用`--cycle`没有使用`--(1, 1)`是因为闭合有区别（使用例子展示）

**节点**
一个节点是一个微型的tikz图形。一般我们用它添加文字，也可以用它画一些小图形。节点是一个二维图形，它会按预先设置好的一个猫店来对齐设定的坐标。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\node at (2, 2) {A}; %锚点默认在节点中央
\end{circuitikz}
```
每个节点自带四个锚点：`north`,`south`,`ease`,`west`。如果我们在设置节点的时候，用`south`猫店锚定坐标`(2, 2)`，那么节点的下端就会对齐`(2, 2)`，这个节点就会出现在`(2, 2)`的上方。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\node[anchor=south] at (2, 2) {A};
\end{circuitikz}
```
这里四个锚点的含义如下：
`"above" == "anchor=south"`, `"below" == "anchor=north"`
`"left" == "anchor=east"`, `"right" == "anchor=west"`
节点的声明方式：
`\node[options] {name} at <cord> {text}`

除了使用`\node`显式声明节点，还可以在路径定义中为涉及的坐标绑定一个节点：
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (1,1) node[below] {A} -- (4,5);
\end{circuitikz}
```

设置节点的时候，还可以为它指定名字，并把它作为一个坐标调用。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (1,1) node[below] (a) {A} -- (4, 5);
	\draw (a.north) -- ++(2, 1); %这里要注意使用的是a.north而不是a，因为后者代表的是坐标A中央。
\end{circuitikz}
```

#### 三、circuitikz中的元件
circuitikz中有两类元件：“路径类”和“节点类”。基本所有的数字电路元件都属于节点式，所以只举一例路径类的绘制方法。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (3,3) to[R] (1,3);   %从(3,3)到(1,3)的路径，内容是一个电阻
\end{circuitikz}
```
所有路径类元件都使用`to[]`连接。方括号中的参数除了指定元件（如电阻`R`和导线`short`），还可以指定末端的连接方式。

对于节点，我们一般先声明节点本身，再从节点的各个锚点向外绘制。
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (3,3) node[and port] (and1) {};
	%\node[and port] (and1) at (3,3) {};
\end{circuitikz}
```
这里需要注意的是每个节点都需要有一个大括号围起来的标签，哪怕是空的。接下来绘制导线，一个逻辑门有`out`和若干`in`锚点：
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (3,3) node[and port] (and1) {};
	\draw (and1.out) -- ++(1,0);
	\draw (and1.in 1) -| (1,5);
	\draw (and1.in 2) -| (1,1);
\end{circuitikz}
```

同时还可以设置不止两个借口的逻辑门，只需添加选项`number inputs`，同时也可以通过`inner inputs`指定有多少个位于逻辑门内部：
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5)
	\draw (3,3) node[and port, number inputs = 10, inner inputs = 2] (and1) {};
	\draw (and1,out) -- ++(1,0);
	\draw (and1.in 1) -| (1,5);
	\draw (and1.in 10) -| (1,1);
\end{circuitikz}
```

对于表示否定的“小圆圈”，如果是输出端，那可以使用`nand`等表示否定的门；不然，就需要动手在接口处添加一个`notcirc`节点。注意，`in`,`out`等锚点在“针脚”的末端，如果要设置在起点，要在前面加`b`，变成`bin`，`bout`：
```latex
\begin{circuitikz}[scale=0.7, transform shape]
	\draw[step=1, gray, very thin] (0,0) grid (5,5);
	\draw (3,3) node[and port, number inputs=10, inner inputs=2] (and1) {};
	\draw (and1.out) -- ++(1,0);
	\draw (and1.in 1) -| (1, 5);
	\draw (and1.in 10) -| (1,1);
	\draw (and1.bin 3) node[notcirc, left] {};
\end{circuitikz}
```
以下是所有常用逻辑门的节点名：
+ 与门：`and port`;
+ 或门：`or port`;
+ 非门：`not port`;
+ 异或门：`xor port`;
+ 同或门：`xnor port`;
+ 与非门： `nand port`;
+ 或非门：`nor port`;
+ 施密特电路：`schmitt port`;
+ 反施密特电路：`inv schmitt port`;
+ 缓冲门：`buffer port`;