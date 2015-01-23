---
layout: post
title:  "Algorithm Design Manual (Chapter 2)"
date:   2014-09-17 20:39:40
categories: Algorithm Booknote
tags: Algorithm
---
#Chapter 2 Algorithm Analysis

##2.1 The RAM Model of Computation
Machine-independent algorithm design depends upon a hypothetical computer called the _Random Access Machine_ or RAM.

Under the RAM model, we measure run time by counting up the number of steps an algorithm takes on a given problem instance.

###2.1.1 Best, Worst, and Average-Case Complexity

![Best, worst, and average-case complexity](http://i.imgur.com/aAWPyzk.png)

 * The _worst-case complexity_ of the algorithm is the function defined by the _maximum_ number of steps taken in any instance of size n. This represents the curve passing through the highest point in each column.
 * The _best-case complexity_ of the algorithm is the function defined by the _minimum_ number of steps taken in any instance of size n. This represents the curve passing through the lowest point of each column.
 * The _average-case complexity_ of the algorithm, which is the function defined by the _average_ number of steps over all instances of size n.

##2.2 The Big Oh Notation
* $$f(n) = O(g(n))$$ means $$c \cdot g(n)$$ is an upper bound on f(n). Thus there exists some constant c such that f(n) is always $$\leqslant c \cdot g(n)$$, for large enough n(i.e., $$n \geqslant n_0$$ for some constant $$n_0$$).

![](http://i.imgur.com/XIRLaAb.png)

##2.3 Growth Rates and Dominance Relations

###2.3.1 Dominance Relations
When f and g belong to different classes (i.e., $$f(n) \neq \Theta(g(n))$$), we say g __dominates__ f when $$f(n) = O(g(n))$$, sometimes written $$g \gg f$$.

$$n! \gg 2^n \gg n^3 \gg n^2 \gg nlogn \gg n \gg logn \gg 1$$

##2.4 Working with the Big Oh

###2.4.1 Adding Functions

$$O(f(n)) + O(g(n)) \rightarrow O(max(f(n), g(n)))$$
$$\Omega(f(n)) + \Omega(g(n)) \rightarrow \Omega(max(f(n), g(n)))$$
$$\Theta(f(n)) + \Theta(g(n)) \rightarrow \Theta(max(f(n), g(n)))$$

###2.4.2 Multiplying Functions
$$O(c * f(n)) \rightarrow O(f(n))$$
$$\Omega(c * f(n)) \rightarrow \Omega(f(n))$$
$$\Theta(c * f(n)) \rightarrow \Theta(f(n))$$

$$O(f(n)) * O(g(n)) \rightarrow O(f(n) * g(n))$$
$$\Omega(f(n)) * \Omega(g(n)) \rightarrow \Omega(f(n) * g(n))$$
$$\Theta(f(n)) * \Theta(g(n)) \rightarrow \Theta(f(n) * g(n))$$

##2.5 Reasoning About Efficiency##

###2.5.3 String Pattern Matching###

<pre class = "brush:cpp">
    int findmatch(char *p, char *t){
        int i,j; /* counters */
        int m, n; /* string lengths */
        m = strlen(p);
        n = strlen(t);
        for (i=0; i<=(n-m); i=i+1) {
            j=0;
            while ((j<m) && (t[i+j]==p[j]))
                j = j+1;
            if (j == m) return(i);
        }
        return(-1);
    }
</pre>

$$O((n-m)(m+2)) \rightarrow O(n+m+(n-m)(m+2)) \rightarrow O(n+m+(n-m)m) \rightarrow O(n+m+nm-m^2) \rightarrow O(n+nm-m^2) \rightarrow O(nm)$$

##2.6 Logarithms and Their Applications

###2.6.6 Logarithms and Summations
Harmonic numbers: $$H(n) = \sum_{i=1}^{n}{1/i} \sim ln n$$

##2.7 Properties of Logarithms

##2.9 Advanced Analysis

###2.9.1 Esoteric Functions

##Chapter 2 Exercise
> 2-43 You are given a set S of n numbers. You must pick a subset S' of k numbers from S such that the probability of each element of S occurring in S' is equal (i.e., each is selected with probability k / n). You may make only one pass over the numbers. What if n is unknown?

<pre class = "brush:cpp">
	for elem in S
  		if random() < (k - S'.size)/S.size 
    		S'.add(elem)
</pre>

选中第一个元素的概率为$$\frac{k}{n}$$，选中第二个元素的概率为$$\frac{k}{n}\cdot\frac{k-1}{n-1}+\frac{n-k}{n}\cdot{k}{n-1} = \frac{k}{n}$$,以此类推……

> 2-44. We have 1,000 data items to store on 1,000 nodes. Each node can store copies of exactly three different items. Propose a replication scheme to minimize data loss as nodes fail. What is the expected number of data entries that get lost when three random nodes fail?

此题可以参考[RAID](http://en.wikipedia.org/wiki/Standard_RAID_levels)磁盘阵列的方法。将不同的数据位按照条带（striping）的方式备份在不同的节点上。

<pre class = "brush:plain">
	Node	Node1	 Node2	  Node3	 	    Node1000
	Copy1	Data1	 Data2	  Data3			Data1000	
	Copy2	Data1000 Data1	  Data2			Data999
	Copy3	Data999	 Data1000 Data1			Data998
</pre>

在1000个节点中随机选取3个节点，共有$${{1000 \choose 3}}$$种选择方式，会造成1个数据损失的选取方法有1000种，期望值为$$1000/{{1000 \choose 3}}=6.02\times10^{-6}$$

> 2-45. Consider the following algorithm to find the minimum element in an array of numbers $$A[0, \ldots, n]$$. One extra variable tmp is allocated to hold the current minimum value. Start from A[0]; "tmp" is compared against $$A[1], A[2], \ldots, A[N]$$ in order. When $$A[i] < tmp, tmp = A[i]$$. What is the expected number of times that the assignment operation $$tmp = A[i]$$ is performed?

可以推导出期望值之间的递推公式，当数列项数从k增加到k+1时，下一次迭代会发生替换的概率等于$$A[k+1]$$是$$A[0, \ldots, k+1]$$中最小值的概率，，即$$E(k+1) = E(k)+\frac{1}{k+1}$$，因此$$S(n)=\sum_{i=1}^{n}{\frac{1}{i}} \approx ln n$$

> 2-46. __You have a 100-story building and a couple of marbles. You must identify the lowest floor for which a marble will break if you drop it from this floor. How fast can you find this floor if you are given an infinite supply of marbles? What if you have only two marbles?__

这一题很有意思，如果我是面试官，我会选择这种题目……
本解答参考[DreamRunner](http://dreamrunner.org/blog/2014/05/29/The-Algorithm-Design-Manual2/)，可以归类为动态规划问题。

1. 当小球数量无限时，简单的二分查找问题，$$\lceil log_{2}100\rceil = 7$$
2. 当只有2个小球时，先考虑简单情况，如果从50楼往下扔，会产生两种情况  
	(1).碎了。那么说明会使小球碎掉的楼层在[1,50]之间，下面就只能从底层一层层往上试验  
	(2).没碎。那么说明会使小球碎掉的楼层(50,100]之间，下面就只能从50层一层层往上试验，这显然不是最好的情况
3. 那么考虑更一般的情况: n 个球时在总楼层 r 中某个楼层 x 抛，两种情况：   
	(1).破碎，剩下的总楼层 x-1 用剩下的 n-1 个球;   
	(2).没破碎，剩下的总楼层 r-x 用 n 个球  
那么可以写出如下的程序解答(最后结果14次)：    

<pre class = "brush:cpp">
        int DropMarbles(int n, int r){
		    int** marble_drop = (int**)calloc(n , sizeof(int*));  
		    for ( int i = 0; i < n ; i++) {  
		        marble_drop[i] = (int*)calloc(r , sizeof(int));  
		    }  
	
		    int i, j;
		    for (j = 0; j < r; ++j) {
		        marble_drop[0][j] = j;//在只有1个球时，只能有几层就试几次
		    }
	
		    for (i = 0; i < n; ++i) {
		        marble_drop[i][1] = 1;
		        marble_drop[i][0] = 0;
		    }//递归的初始条件
		
		    int min_sofar;
		    for (i = 1; i < n; ++i) {
		        for (j = 2; j < r; ++j) {
		            marble_drop[i][j] = INT_MAX;
		            for (int x = 1; x <= j; ++x) {
		                min_sofar = 1 + MAX(marble_drop[i - 1][x - 1], marble_drop[i][j - x]);//核心步骤，两种情况取最大值
		                if (min_sofar < marble_drop[i][j]) {
		                    marble_drop[i][j] = min_sofar;//对于不同的x取最小值
		                }
		            }
		        }
		    }
		
			dump_matrix(n,r, marble_drop);
		    int result = marble_drop[n-1][r-1];
		    for ( int k = 0; k < n ; k++) {
		        free(marble_drop[k]);
		    }
		    free(marble_drop);
		    return result;
		}
</pre>

> 2-47. You are given 10 bags of gold coins. Nine bags contain coins that each weigh 10 grams. One bag contains all false coins that weigh one gram less. You must identify this bag in just one weighing. You have a digital balance that reports the weight of what is placed on it.

小学奥数题……第1袋选择1枚金币，第2袋选择2枚金币，...,第10袋选择10枚金币，一起上秤应该是550克，最后缺几克第几袋金币就是假的……

> 2-48. You have eight balls all of the same size. Seven of them weigh the same, and one of them weighs slightly more. How can you find the ball that is heavier by using a balance and only two weighings?

还是小学奥数题……  
随机选6个球，一边3个放在天平两边，如果有一边重，重的那三个里面随机选两个放在天平两边，如果有一边重，重的那个是假的，如果一样重，剩下那个是假的。如果6个球一样重，剩下两个……

> 2-49. Suppose we start with n companies that eventually merge into one big company. How many different ways are there for them to merge?

1. 只有2个公司的时候，只有一种合并方法
2. 假设有（n-1）个公司的时候有f(n-1)种合并方法。那么当增加一个公司时，可以从先从n个公司中选择两个合并，就划归为(n-1)个公司的情况。从n个公司选择2个公司共有$${{n \choose 2}} = \frac{n(n-1)}{2}$$中方法。
3. 所以$$f(n) = \sum_{i=2}^{n}\frac{i(i-1)}{2} = \frac{n!(n-1)!}{2^{n-2}}$$

> 2-50. A Ramanujam number can be written two different ways as the sum of two cubes---i.e., there exist distinct a, b, c, and d such that $$a^3 + b^3 = c^3 + d^3$$. Generate all Ramanujam numbers where $$a,b,c,d < n$$.

遵循“固定一边”的思路，假设a < b，当a和b确定时，c和d必然在区间(a,b)之间。则必然有$$b \geq a+3$$，否则c和d无法选择。

<pre class="brush:cpp">
	using std::vector;
	bool FindEqual(const vector<int> &num_cube, int low, int high, const int &sum,
	               vector<int> *res) {
	  if (low >= high) {
	    return false;
	  }
	  int i, j;
	  i = low;
	  j = high;
	  int add;
	  while (i < j) {
	    add = num_cube[i] + num_cube[j];
	    if (add == sum) {
	      res->push_back(i);
	      res->push_back(j);
	      return true;
	    }
	    if (add > sum) {
	      --j;
	    } else {
	      ++i;
	    }
	  }
	  return false;
	}
	
	void RamanujamNum(int n, vector<vector<int> > *res) {
	  vector<int> num_cube(n);
	  int i, j;
	  for (i = 0; i < n; ++i) {
	    num_cube[i] = i*i*i;
	  }
	  vector<int> ram_num;
	  bool find;
	  for (i = 0; i < n - 1; ++i) {
	    for (j = i + 3; j < n; ++j) {
	      find = FindEqual(num_cube, i+1, j-1, num_cube[i] + num_cube[j], &ram_num);
	      if (find) {
	        ram_num.push_back(i);
	        ram_num.push_back(j);
	        res->push_back(ram_num);
	        ram_num.clear();
	      }
	    }
	  }
	}
</pre>

> 2-51 Six pirates must divide $300 dollars among themselves. The division is to proceed as follows. The senior pirate proposes a way to divide the money. Then the pirates vote. If the senior pirate gets at least half the votes he wins, and that division remains. If he doesn’t, he is killed and then the next senior-most pirate gets a chance to do the division. Now you have to tell what will happen and why (i.e., how many pirates survive and how the division is done)? All the pirates are intelligent and the first priority is to stay alive and the next priority is to get as much money as possible.

推理题……喜欢看推理小说的应该太熟悉了。逆向思维一下

* 2个海盗         (300,0)	//随便分
* 3个海盗       (299,0,1)	//必须拉拢第3个人，因为第2个人肯定反对
* 4个海盗     (299,0,1,0)	//拉拢第2个人
* 5个海盗   (298,0,1,0,1)	//拉拢第1和第3个人
* 6个海盗 (298,0,1,0,1,0)	//拉拢第2和第4个人，之后的海盗间隔着拉拢就可以了

> 2-52 Reconsider the pirate problem above, where only one indivisible dollar is to be divided. Who gets the dollar and how many are killed?

现在只有一枚金币了……

* 2个海盗           (1,0)    //怎么分都行,自己肯定同意……
* 3个海盗         (0,0,1)	//必须都给第3个人，因为第2个人肯定反对
* 4个海盗       (0,1,0,0)	//拉拢第3个人即可
* 5个海盗   		    None	//必死……
* 6个海盗   (0,0,1,0,0,0)    //无需拉拢第5个人，拉拢第4个人即可

这个游戏继续玩下去的话，之后单数的海盗都必死，双数的海盗只要拉拢自己之前的双数海盗就能活……
