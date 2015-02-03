---
layout: post
title: "Haskell 的有向图查询路径"
description: ""
category: Haskell
tags: Haskell Graph
---
{% include JB/setup %}

> 用Haskell寻找在一个有向图中查询任意两点间所有可能的路径，并考虑有环的情况

##有向图的表示
在本程序中，有向图以边的邻接链表形式表示，如果以整数表示节点的话，那么有向图按照边的方向可以表示为[(Int, Int)]的形式。

如下的有向图可以表示为如下形式：

<pre class = "brush:erl">
	graph = [(1,2),(1,3),(2,3),(3,4),(3,5),(4,5)]
<pre>

![](http://i.imgur.com/g4esgIZ.png)

##无环有向图的路径计算

对于任意一个有向图，任意两个节点之间的所有可能路径可以表示为[[Int]]型，其中每一个列表代表一条路径所经过的节点。比如上图中从1到5的路径可以表示为

<pre class = "brush:erl">
	[[1,3,5],[1,2,3,4,5],[1,2,3,5],[1,3,4,5]]
</pre>

根据迭代计算的思路，从起点到终点的路径可以表示为两部分：

1. 从起点开始，沿路径寻找从该节点可以继续访问的下一个节点
2. 以下一个节点为起点开始继续寻路
3. 当起点等于终点时,寻路结束

按照这个思路，写出下面的函数

<pre class = "brush:erl">
	findpathSimple::Int->Int->[[Int]]
	findpathSimple start dest 
	    |start == dest      = [[start]]
	    |otherwise          = [start:path | (x, node) <- graph, x == start, path <- findpathSimple node dest]
</pre>
		
##有环有向图的处理

![](http://i.imgur.com/vTi1qRN.png)

对于如上图所示的带有环的有向图，上文的方法就不再适用，因为在寻找下一个节点的过程中，会不断沿着换的路径不断寻找下去无限循环。因此我们需要在寻找下一个节点的过程中维护一个*已访问过*的节点的列表，在寻找下一个节点的过程中，不再从这个列表中查询节点，同时将已访问的新节点加入这个列表。通过这种方法，可以防止节点重复访问。

如下面的代码所示，`detectRing`函数会在寻找下一个节点时不再从已访问过的节点中寻找(`not (elem node visited)`)，并且在递归调用时将现在访问的节点加入这个列表(`(node:visited)`)。

<pre class = "brush:erl">
	detectRing::Int->Int->[Int]->[[Int]]
	detectRing start dest visited
	    | start == dest     = [[start]]
	    | otherwise         = [start:path | (x, node) <- graph, x == start, 
								not (node `elem` visited), path <- detectRing node dest (node:visited)]
	findpath::Int->Int->[[Int]]
	findpath start dest = detectRing start dest [start]
</pre>
	
运行结果如下：

<pre class = "brush:erl">
	*Main> findpath 1 5
	[[1,2,3,4,5],[1,2,3,5],[1,3,4,5],[1,3,5]]
</pre>