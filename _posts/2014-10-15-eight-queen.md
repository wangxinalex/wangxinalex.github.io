---
layout: post
title: "八皇后问题的Haskell程序"
description: ""
category: Haskell
tags: Haskell
---
{% include JB/setup %}

## 问题的解释与分析 ##

> 八皇后问题是一个以国际象棋为背景的问题：如何能够在8×8的国际象棋棋盘上放置八个皇后，使得任何一个皇后都无法直接吃掉其他的皇后？为了达到此目的，任两个皇后都不能处于同一条横行、纵行或斜线上。八皇后问题可以推广为更一般的n皇后摆放问题：这时棋盘的大小变为n×n，而皇后个数也变成n。

根据Haskell解决问题的一般方法，这里需要将问题分解为以下几个子问题：

1. 数据结构，即如何表示棋子在棋盘上的位置
2. 如何判断任意两个棋子是否满足题目条件
3. 对于已经确定位置的n个棋子，如何判断第(n+1)个棋子是否满足题目条件

###数据结构
对于第一个问题，题目已经给出了具体要求，即以list中元素的位置表示棋子的行数（纵坐标），以元素的值表示列数（横坐标）。如`[1,5,8,6,3,7,2,4]`即表示如下图所示的情况。

![](http://img.blog.csdn.net/20140301165306421)

###二元判断
由于列表形式略去了形如`(x,y)`的横纵坐标表示，每个棋子的纵坐标由其在列表中的位置决定，因此我们在判断两个棋子是否处于安全状态时只能获得其横坐标，而两个横坐标之间是否出于安全状态是由其相差的纵坐标决定的，因此判断的函数也必须考虑到这一点。在下面的函数中，`x1`和`x2`是两点的横坐标，`n`是其纵坐标的相差值。

<pre class = "brush:erl">
	safe x1 x2 n = x1 /= x2 && x1 /= x2 + n && x1 /= x2 - n
</pre>

###多元判断
根据Haskell递归解决问题的原则，对于一个已经确定所有棋子都处于安全状态的list，应该判断新加入的棋子是否和其他所有棋子都处于安全状态。现在考虑新加入list的棋子横坐标为`x`，现有的list为`(x1:xs)`，那么判断`x`与`x1`时，应该将纵坐标差值`n`设为1，当判断`x`与`head xs`时，差值`n`为2，`x`与`head tail xs`的差值为3，以此类推，递归的终止条件为`xs == []`，即所有棋子判断完毕。那么可以写出如下函数
	
<pre class = "brush:erl">
	safe::Int->[Int]->Int->Bool
	safe x [] n = True
	safe x (x1:xs) n = x /= x1 && x /= x1 + n && x /= x1 - n && safe x xs (n+1)
</pre>

##由列表概括写出的一种解法
Haskell 有很强的**列表概括**能力，一个列表**生成**元素，这些元素经过 **测试**和**转化**形成结果列表的元素。根据上面的分析，不难通过列表概括写出如下的解法。

<pre class = "brush:erl">
	boardsize = 8
	queens_a::Int->[[Int]]
	queens_a 0 = [[]]
	queens_a n = [ x:y |y<-queens_a (n-1), x<-[1..boardsize], safe x y 1]
</pre>

其中`y<-queens_a (n-1)`是前面`(n-1)`行中符合题目要求的结果列表，`x<-[1..boardsize]`是指新加入的棋子（第`n`行）可能取的横坐标的值，`safe x y 1`是判断条件，指的是从相差一行的坐标开始判断，也就是y的第一个元素。`queens_a`函数进行迭代时，会从`[1..boardsize]`中判断合法的横坐标解加入解集中的元素。

这个函数是直观而有效的，但它的问题也显而易见，这里我们在函数体外定义了一个常量`boardsize = 8`，为什么要定义这样一个常量呢？因为在递归过程中，新加入的棋子必须在`[1..boardsize]`中，而不是`[1..n]`,因为`n`会随着递归深度的加深而不断减小，所以如果使用递归解决该问题的话，就必须有一个在函数体外的常量记录棋盘的大小。这显然不利于函数的复用性。

##引入Monad概念的更好解法

本解法参考[这里](http://c2.com/cgi-bin/wiki?EightQueensInManyProgrammingLanguages)。

###高阶函数foldl
有没有可能不使用递归来解决问题呢，答案是肯定的。首先想到的是高阶函数`foldl`，该函数的行为是

<pre class = "brush:erl">
	foldl f z (x : xs) = foldl f (f z x) xs
</pre>

也就是说，如果可能的话，可以定义一个函数`f`，该函数接受一个新的棋子的横坐标`x`和已有的列表`list`，如果`x`和`list`中所有的元素保持安全状态的话，就返回加入`x`的新列表`list'`，否则依然返回`list`。那么通过该函数与已有列表的不断卷积，就有可能得到完整的解集。但问题是`f`的类型应该是`Int->[Int]->[Int]`，而这是不符合`foldl`对于第一个函数参数`a->b->a`的类型要求的。那么应该选择另一种卷积函数，可以将返回值解包传递给后面的执行序列，`foldM`可以满足这个需求。

###应用foldM的解法
关于`foldM`，[官方网站](http://www.haskell.org/hoogle/?hoogle=foldM)给出了这样的解释：

> The foldM function is analogous to foldl, except that its result is encapsulated in a monad. Note that foldM works from left-to-right over the list arguments. This could be an issue commutative.
     
其中**Monad**是函数是编程语言中的重要概念，也是Haskell的精髓，这里无法在有限的篇幅和时间内理解和解释清楚，以下解释只涉及Built-in Monads中的列表操作

而根据[这份文档](http://www.cs.ut.ee/~varmo/FP2007/slides/loeng13.pdf)上给出的定义

<pre class = "brush:erl">
    foldM :: (Monad m) => (a -> b -> m a) -> a -> [b] -> m a
    foldM f a [ ] = return a
    foldM f a (x : xs) = f a x >>= \y -> foldM f y xs 
</pre>

其中`>>=`是Haskell的`bind`操作，在列表操作中，这个绑定的签名是

<pre class = "brush:erl">
	(>>=) :: [a] -> (a -> [b]) -> [b] 
</pre>

也就是将列表中的元素枚举，作为参数传递给后面的lambda表达式。`foldM`的完整操作也就是从第三个列表参数中取出第一个元素`x`，将`f a x`的结果（是一个列表）中的所有元素取出作为lambda表达式的参数继续进行下一步的计算`foldM f y xs `。根据这个形式，我们可以构造一个函数，该函数接受一个棋盘维度`n`和一个已有列表`y`，返回类型为`[[Int]]`，即在保留原有棋子的情况下，新加入一个棋子所产生的所有可能的解。

<pre class = "brush:erl">
	add_q::Int->[Int]->a->[[Int]]
	add_q n y _ = [x:y|x<-[1..n],safe x y 1]
</pre>

这个函数的输入参数`n`为棋盘维度，`y`为已有列表，第三个参数是用来约束`foldM`的执行次数，这里不参与计算。下面是执行的效果

<pre class = "brush:erl">
	> add_q 4 [] []
	[[1],[2],[3],[4]]
</pre>

这表示向一个4X4的棋盘的第一行加入一个棋子时，无论放入任何一列都可以。

<pre class = "brush:erl">
	> add_q 4 [1] []
	[[3,1],[4,1]]
</pre>

第一行的横坐标是1，向第二行加入棋子时，不难理解只能放在3,4列。

<pre class = "brush:erl">
	> add_q 4 [1,4,2] []
	[[3,1,4,2]]
</pre>

就是如下图的情况

![](http://i.imgur.com/3TkrWjI.png)

那么下面的解法也就不难写出。

<pre class = "brush:erl">
	queens::Int->[[Int]]
	queens 0 = [[]]
	queens n = foldM (add_q n) [] [1..n]
</pre>

`foldM`的第一个参数是一个Curry化函数(`add_q n`)，其函数签名是

<pre class = "brush:erl">
	add_q n :: [Int]->a->[[Int]]
</pre>

第二个参数是`[Int]`型列表，表示现有的一个合法解（棋盘上已有的棋子），第三个参数`[1..n]`其实并不参与计算，它的作用是控制`foldM`的卷积次数，因为其停止条件是第三个参数为空列表。

<pre class = "brush:erl">
    foldM f a [ ] = return a
</pre>

`add_q n`每次卷积产生的`[[Int]]`型结果的每一个元素会分别再次参与`add_q n`的计算（`f a x >>=`的意义），将解集中所有列表的长度增加一位。当`[1..n]`中所有元素被`foldM`遍历过，即`foldM f y xs`中的`xs`为空列表时，卷积结束，所有的结果合并，得到`[[Int]]`。

下面以`n=4`为例说明一下执行过程(`add_q n`简记为`f`)：

1. 第一次执行，f [] [1..4] --> [[1],[2],[3],[4]]
2. 第二次执行，
	1. f [1] [2..4] --> [[3,1],[4,1]]
	2. f [2] [2..4] --> [[4,2]]
	3. f [3] [2..4] --> [[1,3]]
	4. f [4] [2..4] --> [[1,4],[2,4]]
3. 第三次执行, 
	1. f [3,1] [3..4] --> [[]]
	2. f [4,1] [3..4] --> [[2,4,1]]
	3. f [4,2] [3..4] --> [[1,4,2]]
	4. f [1,3] [3..4] --> [[4,1,3]]
	5. f [1,4] [3..4] --> [[3,1,4]]
4. 第四次执行,
	1. f [2,4,1] [4] --> [[]]
	2. f [1,4,2] [4] --> [[3,1,4,2]]
	3. f [4,1,3] [4] --> [[2,4,1,3]]
	4. f [3,1,4] [4] --> [[]]
5. 第五次执行
	1. f [3,1,4,2] [] --> return [3,1,4,2]
	2. f [2,4,1,3] [] --> return [2,4,1,3]
6. 最终结果[[3,1,4,2],[2,4,1,3]]
