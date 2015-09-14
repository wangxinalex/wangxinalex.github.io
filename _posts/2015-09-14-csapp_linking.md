---
layout: post
title: "重读CSAPP：关于程序的链接与运行"
description: ""
category: OS
tags: OS Compiler C
---
{% include JB/setup %}

#Chapter 7 Linking

##7.1 编译器驱动程序

1. 源程序(main.c)--C预处理器(cpp)--> 
2. ASCII码中间文件(main.i)--编译器(ccl)-->
3. ASCII汇编语言文件(main.s) --汇编器(as)-->
4. **可重定位目标文件**(main.o)--链接器(ld)-->
5. 可执行目标文件(a.out)

![](http://i.imgur.com/tVistL7.png)

##7.2静态链接

>像UNIX ld程序这样的*静态链接器*以一组可重定位目标文件和命令行参数作为输入，生成一个完全链接的可以加载和运行的可执行目标文件作为输出。

链接器的两个主要任务：

1. 符号解析：将每个符号引用刚好和一个符号定义联系起来
2. 重定位：把每个符号定义与一个存储器位置联系起来，然后修改所有对这些符号的引用，使得它们指向这个存储器的位置，从而*重定位*这些节

##7.3目标文件
目标文件有三种形式：

1. 可重定位目标文件。
2. 可执行目标文件:可以被直接拷贝到存储器并执行
3. 共享目标文件：在加载或者运行时被动态地加载到存储器并链接 

##7.4可重定位目标文件
![](http://i.imgur.com/29kJ0Ri.png)

1. `.text`已编译程序的机器代码（函数名）
2. `.rodata`只读数据（字符串字面量）
3. `.data`已初始化的全局变量和静态变量。局部变量在运行时保存在栈中，没有符号链接
4. `.bss`未初始化的全局变量和静态变量。不占据实际的空间，仅是一个占位符。未初始化的变量不占据任何磁盘空间
5. `.symtab`符号表。存放程序中定义和引用的函数和全局变量的信息，不包含局部变量的条目
6. `.rel.text`一个`.text`节中位置的列表，当链接器把这个目标文件和其他文件结合时，需要修改这些位置。
7. `.rel.data`被模块引用或定义的任何全局变量的重定位信息。
8. `.debug`调试符号表
9. `.line`源文件中的行号和`.text`节中机器指令之间的映射
10. `.strtab`一个字符串表，其内容包括`.symtab`和`.debug`节中的符号表，以及节头部中的节名字。字符串表就是以null结尾的字符串序列。


<pre class = "brush:cpp">
int global = 100;
int uninitialized_global;
const int outer_const = 100;
static int outer_static = 100;
static int uninitialized_outer_static;
int main(){
	static int inner_static = 100;
	static int uninitialized_inner_static;
	const int inner_const = 100;
	int local = 0;
	return 0;	
}
</pre>

<pre class = "brush:shell">
?> gcc -o symbol symbol.c
?> nm symbol

0000000000400594 r _ZL11outer_const
000000000060103c d _ZL12outer_static
000000000060104c b _ZL26uninitialized_outer_static
0000000000601040 d _ZZ4mainE12inner_static
0000000000601050 b _ZZ4mainE26uninitialized_inner_static
...
0000000000601038 D global
00000000004004ed T main
0000000000601048 B uninitialized_global
</pre>

|变量名|类型|初始化|链接类型|所在节|
|------|----|------|--------|------|
|global|全局变量|是|外部链接|.data|
|uninitialized_global|全局变量|否|外部链接|.bss|
|main|函数名|是|外部链接|.text|
|outer_const|全局变量（只读）|是|内部链接|.rodata|
|outer_static|静态变量（函数外部）|是|内部链接|.data|
|uninitialized_outer_static|静态变量（函数外部）|否|内部链接|.bss|
|inner_static|静态变量（函数内部）|是|内部链接|.data|
|uninitialized_inner_static|静态变量（函数内部）|否|内部链接|.bss|


##7.5符号和符号表
每个可重定位目标模块m都有一个符号表，包含m所定义和引用的符号的信息。

1. 由m定义并能够被其他模块引用的**全局符号**。全局链接器符号对应于**非静态的（non-static）** C函数以及被定义为**不带static**属性的全局变量。
2. 由其他模块定义并被模块m引用的全局符号。这些符号成为**外部符号（external）**，对应于定义在其他模块中的C函数和变量
3. 只被模块m定义和引用的**本地符号**。有的本地链接器符号对应于带**static**属性的C函数和全局变量。不能被其他模块引用。**不等于本地程序变量**

##7.6符号解析

###7.6.1如何解析多重定义的全局符号
函数和已初始化的全局变量是**强符号**，未初始化的全局变量是**弱符号**。

Unix链接器使用下面的规则来处理多重定义的符号

* 规则1：不允许有多个强符号
* 规则2：如果有一个强符号和多个弱符号，选择强符号
* 规则3：如果有多个弱符号，那么从这些弱符号中任意选择一个。

![](http://i.imgur.com/pCGxrNi.png)
![](http://i.imgur.com/3u39ZQU.png)

###7.6.2与静态库链接
在Unix系统中，静态库以一种称为**存档（archive）**的特殊文件格式存放在磁盘中。存档文件是一组连接起来的可重定位目标文件的集合，有一个头部用来描述每个成员目标文件的大小和位置。

![](http://i.imgur.com/jn6xNrL.png)

###7.6.3链接器如何使用静态库来解析引用

![](http://i.imgur.com/hALWonF.png)

##7.7重定位
重定位由两步组成：

* 重定位节和符号定义。链接器将所有相同类型的节合并为同一类型的新的聚合节。来自输入模块的`.data`节全部合并成一个小节，这个节成为输出的可执行目标文件的`.data`节。然后将运行时**存储器地址**赋给行的聚合节，赋给输入输出模块定义的每个节。
* 重定位节中的符号引用。链接器修改代码节和数据节中对每个符号的引用，使得它们指向正确的运行时地址。为了执行这一步，链接器依赖于成为**重定位条目**的可重定位目标模块中的数据结构。

###7.7.1重定位条目

无论何时汇编器遇到对最终位置未知的目标引用，它就会生成一个**重定位条目**，告诉链接器在将目标文件合并成可执行文件时如何修改这个引用。代码的重定位条目放在`.rel.text`中，已初始化数据的重定位条目放在`.rel.data`中。

![](http://i.imgur.com/npBESq5.png)

###7.7.2重定位符号引用
