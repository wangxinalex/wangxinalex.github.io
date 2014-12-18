---
layout: post
title:  "100年后的编程语言"
date:   2014-09-21 20:39:39
categories: Booknote
tags: 
---

人类生命的有限性决定了人类对于尚未发生的事物有着极为浓郁的好奇心。三次工业革命让人类物质文明的发展进入了一个前所未有的快车道，人类的生活方式和生产方式可能在十年之内发生彻底性的变革。这似乎使得预测未来成为一种愈发困难的任务，想象19世纪一个在伦敦等待蒸汽火车的旅客，根据他对铁路和机车的专业知识预测一百年后开往曼彻斯特的火车会从一天一班增加到一小时一班，但是一百年之后，蒸汽火车已经不复存在了，取而代之的是磁悬浮、跨海隧道和喷气式飞机，他的后人也许会在十分钟之内完成他用一天才能完成的旅程。就好像下图所展示的20世纪初人类对于21世纪现代生活的预测一样，在打电话过程中随时看到对方的实时动态，的确是十分具有前瞻性的设想，今天的我们已经能够做到这一点，不过我们不需要投影仪和留声机，只需要一台苹果手机和Facetime。

![Movie](http://www.chinadaily.com.cn/hqzx/images/attachement/jpg/site1/20120911/0023ae98874811b985362f.jpg)

在电气技术和信息技术革命中诞生的软件产业和种种编程语言，很有可能也会面临这样的情况。编程语言中的发展脉络基本上伴随并适应[Von Neumann结构](http://en.wikipedia.org/wiki/Von_Neumann_architecture)计算机的发展历史。然而硬件发展的速率在[摩尔定律](http://en.wikipedia.org/wiki/Moore%27s_law)的作用下快速发展并很快进入硬件资源相对过剩的状态，除了嵌入式开发者以外已经很少有程序员会担心硬件资源的分配效率问题，那么我们是否有能力预测编程语言在此后一个世纪中的发展呢？

##道生一，一生二——公理体系的简洁性
>  I have a hunch that the main branches of the evolutionary tree pass through the languages that have the smallest, cleanest cores. The more of a language you can write in itself, the better.

David Gries在*The Science of Programming*中阐明了一套由符号逻辑和自然演绎法支撑的程序正确性证明体系。依靠在近代数学中归纳总结出的[公理化系统](http://en.wikipedia.org/wiki/Axiomatic_system)，我们可以以一套机械性的方法证明和写出bug-free的程序。同样，在设计一门编程语言时，同样需要界定其公理系统。在*The Science of Programming*一书中，作者归纳出的公理化形式系统和推导规则(Formal Systems of Axioms and Inference Rule说)总共不超过20条。公理(Axiom)及由推导规则(Inference Rule)推导出的命题(Proposition)构成了定理(Theorem)。通过这些公理和定理，我们可以推导并证明任何一个程序（主要是指令式编程语言）和正确性。应该说公理化的简明性是一套设计优秀的编程语言的重要标志，否则学习者可能会花很多时间学习一些有悖于正常思维模式的逻辑。

##日光之下并无新事——缓慢进化

> So the rate of evolution in programming languages is more like the rate of evolution in mathematical notation than, say, transportation or communications. Mathematical notation does evolve, but not with the giant leaps you see in technology.
 
自1957年世界上第一种编程语言Fortran诞生以来，包括领域专用编程语言在内的数千种编程语言被发明并推广应用，其中绝大部分已经随着时间的流逝慢慢淡出大众的视野或者仅仅为某一领域的专业人士熟知。然而由于Von Neumann结构计算机始终占据主流，一些控制计算资源的基本原语（栈操作、寄存器操作、访存操作等等）并未改变，反映在编程语言上，其基本指令和控制逻辑也并没有发生实质性的改变，五十年前的Fortran语言至今保持着相当的生命力。因此作者认为正如今天的中学生依然需要学习几百年前发现的基本数学概念和定理一样，一百年后的计算机，如果没有在体系结构和设计方法上发生革命性变化，那么一百年后的人类使用的编程语言与今天也不会有太多变化。

##欲速则不达——关于过早优化
> Premature optimization is the root of all evil. 

这里作者讨论了关于语言的语义(semantic)和实现(implementation)的问题。正如上文中的公理化系统一样，语言的语义是内核、公理，应当尽量由简明、自证、相互独立的准则构成，公理之间不应当存在相互从属、整体与部分的关系。因此在语义层面同时实现字符串(string)和列表(list)是不明智的，字符串在语义层面应该隶属于列表的范畴，正如一些脚本语言(比如Python)中所实现的一样。而在实际运行中，字符串的出现频率远远高于其他列表，这一部分针对字符串的特殊优化应当由语言下游的编译器或解释器实现。语义和实现的分层使得程序员在构建程序原型时可以面临较少的障碍和限制，实现更高效率的生产。

##圣杯战争——可重用性
> Bottom-up programming means writing a program as a series of layers, each of which serves as a language for the one above. This approach tends to yield smaller, more flexible programs. It’s also the best route to that holy grail, reusability.

这里涉及了一个在20世纪80年代兴起的概念——面向对象编程。面向对象编程目前业已成为软件设计的实际标准，面向对象的设计模式推动了软件工程领域的深刻变革，使得可重用性成为一种易于表达和实践的概念。然而作者认为面向对象并不是可重用性的必要条件，可重用性的实现，更多地源自于自下而上(bottom-up)的开发方法和分层式的设计模式。面向对象的设计之所以能够长期存在并成为事实上的设计标准，是因为它符合软件产业的商业需求，即流水线和迭代式开发，有利于软件产品的长期维护和增值。毕竟软件工程依然是工程实践，theorist的“正确”必须转化为engineer的“可用”才能够转化为实际的产业价值。

##后之视今，亦犹今之视昔——编程语言的百年预测
> Now we have two ideas that, if you combine them, suggest interesting possibilities: (1) the hundred-year language could, in principle, be designed today, and (2) such a language, if it existed, might be good to program in today.

设计一门编程语言的目的究竟是发挥人的最大效率还是计算机的最大效率？显然在这本书中，作者坚定地认为编程语言的作用是让人类更好地发挥工作效率。先进的编程语言应当具有但不限于以下几个特点：

* 开发者友好，注重于发挥编程者的开发效率
* 语言的公理体系具有简洁性
* 语义和实现分离，杜绝过早优化
* 具有良好的可重用性，符合当代软件工程领域的要求
* 低学习成本，快速构建原型

值得注意的是，当今的编程语言发展已经在向这样一种趋势发展，脚本语言和开源语言的蓬勃兴起，让一些新潮甚至超前的设计概念开始真正投入应用。循着这样一条道路向前探索，我们或许能够猜测到编程语言的进化方向。当然，猜测仅仅是猜测，IT产业距离真正成为一门成熟的工业毕竟还有一段发展的路程，也许一百年后，我们会以一种完全不同的视角和维度看待编程这种生产活动，那么现在我们所坚持的一些基本准则和设计理念都将发生翻天覆地的变化。不过即便如此，编程语言走过的半个世纪的发展历程必将构成一门成熟工业的基本支柱，依然具有十分珍贵并且值得研究的意义。