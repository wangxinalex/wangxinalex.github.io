---
layout: post
title:  "Algorithm Design Manual"
date:   2014-09-16 20:39:39
categories: Algorithm Booknote
tags: Algorithm
---
#Chapter 1 Introduction to Algorithm Design

##1.1 Robot Tour Optimization

_Traveling Salesman Problem_  

> _Algorithms_: always produce a correct result  
> _Heuristics_: do a good job but without providing any guarantee

##1.2 Selecting the Right Jobs

<pre class = "brush:cpp">
    OptimalScheduling(I)
        While (I != ∅) do
            Accept the job j from I with the earliest completion date.
            Delete j, and any interval which intersects j from I.
</pre>

##1.3 Reasoning about Correctness

###1.3.2 Problems and Properties
Problem specifications have two parts:
 
1. the set of allowed input instances,  
2. the required properties of the algorithm’s output

There are two common __traps__ in specifying the output requirements of a problem.

1. asking an ill-defined question  
2. creating compound goals

###1.3.3 Demonstrating Incorrectness

Hunting for counter-examples:  

1. Think small
2. Think exhaustively
3. Hunt for the weakness
4. Go for a tie
5. Seek extremes

###1.3.4 Induction and Recursion
> Recursion is mathematical induction

###1.3.5 Summations

1. Arithmetic progressions
2. Geometric series

##1.4 Modeling the Problem

###1.4.1 Combinatorial Objects  
1. Permutations: arrangements, or orderings, of items.
2. Subsets: selections from a set of items.
3. Trees: hierarchical relationships between items
4. Graphs: relationships between arbitrary pairs of objects.
5. Points: locations in some geometric space
6. Polygons: regions in some geometric spaces
7. Strings: sequences of characters or patterns

###1.4.2 Recursive Objects
> Learning to think recursively is learning to look for big things that are made from smaller things of __exactly the same type as the big thing__.

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
本解答参考[DreamRunner](http://dreamrunner.org/blog/2014/05/29/The-Algorithm-Design-Manual2/)

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

<pre class = "brush:cpp">
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

#Chapter 3 Data Structures
##3.1 Contiguous and Linked Data Structures 
* Contiguously-allocated structures: single slabs of memory, including arrays, matrices, heaps and hash tables
* Linked data structures: distinct chunks of memory bound together by _pointers_, including list s, trees, graph adjacency lists

###3.1.1 Arrays
* Dynamic arrays

###3.1.2 Pointers and Linked Structures
_Pointers_ are the connections that hold the pieces of linked structures together.
![](http://i.imgur.com/LGCPx8c.png)

* searching a list 

<pre class = "brush:cpp">
		list *search_list(list *l, item_type x){
			if(l == NULL) return NULL;
			if(l->item == x)
				return l;
			else
				return search_list(l->next, x);
		}
</pre>

* insertion into a list (at the beginning)
	
<pre class = "brush:cpp">
        void insert_list(list **l, item_type x){
			list *p;
			p = malloc(sizeof(list));
			p->item = x;
			p->next = *l;
			*l = p;
		}
</pre>

* Deletion from a list
		
<pre class = "brush:cpp">
		//find the predecessor in a single-linked list first
		list *predecessor_list(list* l, item_type x){
			if((l == NULL)||(l->next == NULL)){
				//ERROR
				return NULL;
			}
			if(l->next->item == x){
				return l;
			else
				return predecessor_list(l->next, x);
		} 

		delete_list(list **l, item_type x){
			list *p;
			list *pred;
			p = search_list(*l, x);
			if(p != NULL){
				pred = predecessor_list(*l, x);
				//p is the head of list
				if(pred == NULL){
					*l = p->next;
				}else{
					pred->next = p->next;
				}
				free(p);
			}
		}
</pre>

###3.1.3 Comparison
* List: Chopping the first element off a linked list leaves a smaller linked list. This same argument works for strings, since removing characters from string leaves a string. Lists are recursive objects.
* Arrays: Splitting the first k elements off of an n element array gives two smaller arrays, of size k and n−k, respectively. Arrays are recursive objects

##3.2 Stacks and Queues

* Stacks
	* Push(x,s): Insert item x at the top of stack s.
	* Pop(s): Return (and remove) the top item of stack s.
* Queues
	* Enqueue(x,q): Insert item x at the _back_ of queue q.
	* Dequeue(q): Return (and remove) the _front_ item from queue q.

##3.3 Dictionaries
The _dictionary_ data type permits access to data items by content

* Search(D,k) – Given a search key k, return a pointer to the element in dictionary D whose key value is k, if one exists.
* Insert(D,x) – Given a data item x, add it to the set in the dictionary D.
* Delete(D,x) – Given a pointer to a given data item x in the dictionary D, remove it from D.
* Max(D) or Min(D) – Retrieve the item with the largest (or smallest) key from D.
* Predecessor(D,k) or Successor(D,k) – Retrieve the item from D whose key is immediately before (or after) k in sorted order.

###Comparing Dictionary Implementations(I)
![](http://i.imgur.com/vr9XpIR.png)

* Deletion: The following idea is better: just write over A[x] with A[n], and decrement n. This only takes constant time.

###Comparing Dictionary Implementations(II)
![](http://i.imgur.com/sa2YcXd.png)

##3.4 Binary Search Trees

###3.4.1 Implementing Binary Search Tree

![](http://i.imgur.com/vsqn5yf.png)

* Data Structure
* 
<pre class = "brush:cpp">
	typedef struct tree {
		item_type item; /* data item */
		struct tree *parent; /* pointer to parent */
		struct tree *left; /* pointer to left child */
		struct tree *right; /* pointer to right child */
	} tree;
</pre>

* Searching in a Tree

<pre class = "brush:cpp">
	tree *search_tree(tree *l, item_type x){
		if (l == NULL) return(NULL);
		if (l->item == x) return(l);
		if (x < l->item)
			return( search_tree(l->left, x) );
		else
			return( search_tree(l->right, x) );
	}
</pre>

This search algorithm runs in $$O(h)$$ time, where $$h$$ denotes the height of the tree.

* Finding Minimum and Maximum Elements in a Tree
<pre class = "brush:cpp">
	tree *find_minimum(tree *t){
		tree *min; /* pointer to minimum */
		if (t == NULL) return(NULL);
		min = t;
		while (min->left != NULL)
			min = min->left;
		return(min);
	}
</pre>

* Traversal in a Tree

<pre class = "brush:cpp">
	void traverse_tree(tree *l){
		if (l != NULL) {
			traverse_tree(l->left);
			process_item(l->item);//in-order traversal
			traverse_tree(l->right);
		}
	}
</pre>

* Insertion in a Tree
		
<pre class = "brush:cpp">
	insert_tree(tree **l, item_type x, tree *parent){
		tree* p;
		if(*l == NULL){
			p = malloc(sizeof(tree));
			p->item = x;
			p->left = p->right = NULL;
			p->parent = parent;
			*l = p;
			return;
		}
		if(x < (*l)->item)
			insert(&((*l)->left), x, *l);
		else
			insert(&((*l)->right), x, *l);
	}
</pre>

* Deletion from a Tree

![](http://i.imgur.com/yBDHXhf.png)

###3.4.3 Balanced Search Trees

Exploiting Balanced Search Trees

1. How can you sort in $$O(n logn)$$ time using only insert and in-order traversal?
2. How can you sort in $$O(n logn)$$ time using only minimum, successor, and insert?
3. How can you sort in $$O(n logn)$$ time using only minimum, insert, delete, search?

![](http://i.imgur.com/ZJNpvaI.png)

##3.5 Priority Queues

* Insert(Q,x)– Given an item x with key k, insert it into the priority queue Q.
* Find-Minimum(Q) or Find-Maximum(Q)– Return a pointer to the item whose key value is smaller (larger) than any other key in the priority queue Q.
* Delete-Minimum(Q) or Delete-Maximum(Q)– Remove the item from the priority queue Q whose key is minimum (maximum).

![](http://i.imgur.com/IMGKXIN.png)

##3.7 Hashing and Strings
A _hash function_ is a mathematical function that maps keys to integers. We will use the value of our hash function as an index into an array, and store our item at that position.

 $$H(S) = \sum^{|S|-1}_{i=0}{\alpha^{|S|-(i+1)}\times char(s_i)}$$

###3.7.1 Collision Resolution

* Chaining
![](http://i.imgur.com/Kc9KKMZ.png)
* Open addressing
![](http://i.imgur.com/n4H93KC.png)

![](http://i.imgur.com/c9IhAc7.png)

###3.7.2 Efficient String Matching via Hashing

##3.10 Exercises
> 3-1. A common problem for compilers and text editors is determining whether the parentheses in a string are balanced and properly nested. For example, the string ((())())() contains properly nested pairs of parentheses, which the strings )()( and ()) do not. Give an algorithm that returns true if a string contains properly nested and balanced parentheses, and false if otherwise. For full credit, identify the position of the first offending parenthesis if the string is not properly nested and balanced.

<pre class = "brush:cpp">
	using namespace std;
	int main(int argc, char* argv[]) {
		if(argc != 2){
			cerr<< "argc != 2"<<endl;
			return 1;
		}
	    string input(argv[1]);
		stack<int> parentheses;
	    for(int i = 0; i < input.size(); i++) {
	        if(input[i] == '(') {
	            parentheses.push(i);
	        } else if(input[i] == ')') {
				if(parentheses.empty()){
					cout << "ERROR position: " << i << endl;
					return -1;
				}else{
					parentheses.pop();
				}
	        }
	    }
		if(!parentheses.empty()){
			cout << "ERROR position: "<< parentheses.top()<<endl;
			return -1;
		}
	    cout << "RIGHT" << endl;
	    return 0;
	}
</pre>

> 3-2. Write a program to reverse the direction of a given singly-linked list. In other words, after the reversal all pointers should now point backwards. Your algorithm should take linear time.

<pre class = "brush:cpp">
	Node* reverse_list(Node * head){
		queue<int> node_queue;
		Node * this_node = head;
		while(this_node != NULL){
			Node* old_node = this_node;
			node_queue.push(this_node->value);
			this_node = this_node->next;
			free(old_node);
		}
		int x = node_queue.front();
		node_queue.pop();
		Node * new_head = (Node*)calloc(1, sizeof(Node));
		new_head->value = x;
		while(!node_queue.empty()){
			push_list(&new_head, node_queue.front());
			node_queue.pop();
		}
		return new_head;
	}

	//push to the head of the list
	int push_list(Node ** head, int x){
		Node * new_node = (Node*)calloc(1, sizeof(Node));
		new_node->value = x;
		new_node->next = (*head);
		*head = new_node;
		return 0;
	}
</pre>

或者还有种空间复杂度更好的（参考[DreamRunner](http://dreamrunner.org/wiki/public_html/Misc/Train/TheAlgorithmDesignManual/The-Algorithm-Design-Manual3.html)）……
	
<pre class = "brush:cpp">
	void ReverseLinkedList(Node **head) {
	  if (!head || *head == NULL) {
	    return;
	  }
	  Node *prev, *p, *next;
	  prev = *head;
	  p = prev->next;
	  prev->next = NULL;
	  while (p != NULL) {
	    next = p->next;
	    p->next = prev;
	    prev = p;
	    p = next;
	  }
	  *head = prev;
	}
</pre>

> 3-3. We have seen how dynamic arrays enable arrays to grow while still achieving constant-time amortized performance. This problem concerns extending dynamic arrays to let them both grow and shrink on demand.
> 
(a) Consider an underflow strategy that cuts the array size in half whenever the array falls below half full. Give an example sequence of insertions and deletions where this strategy gives a bad amortized cost.
>
(b) Then, give a better underflow strategy than that suggested above, one that achieves constant amortized cost per deletion.

Underflow问题……，针对(a)，一种比较常见的情况就是有一个数组的元素数量正好是其容量的一半，那么这个数组减少一个元素就会使数组长度减半，增加一个元素就会使数组元素溢出，该数组会频繁地伸长一倍和缩短一半……

(b)比如说当数组元素数量小于数组长度一半时，该数组缩短一个元素而不是缩短一半，当元素数量超过数组长度一半时，数组增加一个元素而不是增长一倍……

> 3-4. Design a dictionary data structure in which search, insertion, and deletion can all be processed in O(1) time in the worst case. You may assume the set elements are integers drawn from a finite set 1, 2, .., n, and initialization can take O(n) time.

没仔细想……既然key是不重复的整数，那就用一个长度为n的数组好了……

> 3-5. [3] Find the overhead fraction (the ratio of data space over total space) for each of the following binary tree implementations on n nodes:
> 
(a) All nodes store data, two child pointers, and a parent pointer. The data field requires four bytes and each pointer requires four bytes.
> 
(b) Only leaf nodes store data; internal nodes store two child pointers. The data field requires four bytes and each pointer requires two bytes.

(a)$$\frac{1}{4}$$
(b)如果叶子节点的数目是n，那么非叶子结点的数目是（n-1），所以$$\frac{4n}{4n+4(n-1)} = \frac{n}{2n-1}$$

> 3-6. Describe how to modify any balanced tree data structure such that search, insert, delete, minimum, and maximum still take O(log n) time each, but successor and predecessor now take O(1) time each. Which operations have to be modified to support this?

可以参考B+ Tree的实现，为所有的节点添加指向successor 和 predecessor的指针……

![](http://blog.panictank.net/wp-content/uploads/2011/11/3-6.png)

> 3-7. Suppose you have access to a balanced dictionary data structure, which supports each of the operations search, insert, delete, minimum, maximum, successor, and predecessor in O(log n) time. Explain how to modify the insert and delete operations so they still take O(log n) but now minimum and maximum take O(1) time. (Hint: think in terms of using the abstract dictionary operations, instead of mucking about with pointers and the like.)

存储指向maximum和minimum和指针
insert时，比较新插入的值和这两个指针指向的值，需要时更新
delete时，如果删除的是minimum，用他的successor代替，如果删除的是maximum，用它的predecessor代替。

> 3-8. Design a data structure to support the following operations:
>
>insert(x,T) – Insert item x into the set T.
>
>delete(k,T) – Delete the kth smallest element from T.
>
>member(x,T) – Return true iff x ∈ T. 
>
>All operations must take O(log n) time on an n-element set.

一般的Balanced Binary Search Tree也可以满足第一个和第三个条件，至于第二个条件，[panictak](http://blog.panictank.net/tag/data-structure/)给出了这样一种解答：
![](http://blog.panictank.net/wp-content/uploads/2011/11/3-8.2.png)
在一般的Balanced BST中加入两个counter，分别记录其左子树和右子树的节点数量，这样在查找K-th smallest的节点时可以直接从跟节点计数

> 3-9. A concatenate operation takes two sets S1 and S2, where every key in S1 is smaller than any key in S2, and merges them together. Give an algorithm to concatenate two binary search trees into one binary search tree. The worst-case running time should be O(h), where h is the maximal height of the two trees.

S1中的所有元素小于S2,用O（logn）的时间找出S2的最小元素，然后S1成为它的左子树，S2成为它的右子树，组成新的搜索树。

> 3-10. In the __bin-packing problem__, we are given n metal objects, each weighing between zero and one kilogram. Our goal is to find the smallest number of bins that will hold the n objects, with each bin holding one kilogram at most.
>
The __best-fit heuristic__ for bin packing is as follows. Consider the objects in the order in which they are given. For each object, place it into the partially filled bin with the smallest amount of extra room __after__ the object is inserted.. If no such bin exists, start a new bin. Design an algorithm that implements the best-fit heuristic (taking as input the n weights $$w_1, w_2, ...,w_n$$ and outputting the number of bins used) in O(n log n) time.
>
Repeat the above using the __worst-fit heuristic__, where we put the next object in the partially filled bin with the largest amount of extra room __after__ the object is inserted.

__best-fit heuristic__ strategy 要找到满足条件的剩余空间最小的bin，因此不是一个简单的查找最小值的问题，比较适合BST数据结构

__worst-fit heuristic__ strategy 要找到的是剩余空间最大的bin，因此使用max-heap比较合适

> 3-11. [5] Suppose that we are given a sequence of n values $$x_1, x_2, ..., x_n$$ and seek to quickly answer repeated queries of the form: given i and j, find the smallest value in $$x_i, \cdots , x_j$$ .
> 
> (a) Design a data structure that uses $$O(n^2)$$ space and answers queries in O(1) time.
> 
> (b) Design a data structure that uses O(n) space and answers queries in $$O(log n)$$ time. For partial credit, your data structure can use $$O(n log n)$$ space and have $$O(log n)$$ query time.

(a)直接构造一个n*n的矩阵，其中$$a_{i,j}$$元素就是$$x_i, \cdots, x_j$$中最小的值

(b)这里需要借助[Cartesian tree](http://en.wikipedia.org/wiki/Cartesian_tree)数据结构

<pre class = "brush:cpp">
	using namespace std;
	struct node {
	    int value;
	    int index;
	    struct node* left;
	    struct node* right;
	    struct node* parent;
	};				/* ----------  end of struct node  ---------- */
	
	typedef struct node Node;
	
	int left_predecessor(Node* node);
	int right_predecessor(Node* node);
	int find_smallest_interval(Node* node, int left, int right, int *result);
	
	/* 
	 * ===  FUNCTION  ======================================================================
	 *         Name:  left_minimal_index
	 *  Description:  find the index of the minimal value in the interval [left, right)
	 * =====================================================================================
	 */
	int find_minimal_index(int arr[], int length, int left, int right) {
	    int minimal = INT_MAX;
	
	    if(right < 0 || left >= length || right < left + 1) {
	        return -1;
	    }
	
	    if(left == length - 1) {
	        return left;
	    } else if(right >= length) {
	        right = length;
	    }
	
	    int minimal_index = left;
	
	    for(int i = left; i < right; i++) {
	        if(arr[i] < minimal) {
	            minimal_index = i;
	            minimal = arr[i];
	        }
	    }
	
	    return minimal_index;
	}
	
	void construct_tree(Node* parent, int arr[], int length, int left, int right) {
	    //cout << "parent->index: " << parent->index << " left: " << left << " right:" << right << endl;
	    if(parent == NULL) {
	        return;
	    }
	
	    int min_left_index, min_right_index;
	
	    if((min_left_index = find_minimal_index(arr, length, left, parent->index)) == -1) {
	        parent->left = NULL;
	    } else {
	        Node* left = (Node*)calloc(1, sizeof(Node));
	        parent->left = left;
	        left->value = arr[min_left_index];
	        left->index = min_left_index;
	        left->parent = parent;
	    }
	
	    if((min_right_index = find_minimal_index(arr, length, parent->index + 1, right)) == -1) {
	        parent->right = NULL;
	    } else {
	        Node *right = (Node*)calloc(1, sizeof(Node));
	        parent->right = right;
	        right->value = arr[min_right_index];
	        right->index = min_right_index;
	        right->parent = parent;
	    }
	
	    if(parent->left)
	        construct_tree(parent->left, arr, length, left_predecessor(parent->left) + 1, right_predecessor(parent->left));
	
	    if(parent->right)
	        construct_tree(parent->right, arr, length, left_predecessor(parent->right) + 1, right_predecessor(parent->right));
	}
	
	/* 
	 * ===  FUNCTION  ======================================================================
	 *         Name:  left_predecessor
	 *  Description:  find the nearest left predecessor
	 * =====================================================================================
	 */
	int left_predecessor(Node* node) {
	    int left_nearest = node->index;
	    Node *this_node = node;
	
	    while(this_node != NULL) {
	        if(this_node->index < left_nearest) {
	            left_nearest = this_node->index;
	            break;
	        }
	
	        this_node = this_node->parent;
	    }
	
	    if(left_nearest == node->index) {
	        left_nearest = -1;
	    }
	
	    return left_nearest;
	}
	
	/* 
	 * ===  FUNCTION  ======================================================================
	 *         Name:  right_predecessor 
	 *  Description:  find the nearest right predecessor
	 * =====================================================================================
	 */
	int right_predecessor(Node* node) {
	    int right_nearest = node->index;
	    Node* this_node = node;
	
	    while(this_node != NULL) {
	        if(this_node->index > right_nearest) {
	            right_nearest = this_node->index;
	            break;
	        }
	
	        this_node = this_node->parent;
	    }
	
	    if(right_nearest == node->index) {
	        right_nearest = INT_MAX;
	    }
	
	    return right_nearest;
	}
	
	void in_order_traversal(Node* node) {
	    if(node == NULL) {
	        return;
	    }
	
	    in_order_traversal(node->left);
	    cout << node->value << endl;
	    in_order_traversal(node->right);
	
	}
	
	int main() {
	    int arr[] = {5, 2, 6, 9, 10, 3, 7, 1, 21, 8, 12, 14};
	    Node* root = (Node*)calloc(1, sizeof(Node));
	    root->index = find_minimal_index(arr, 12, 0, 12);
	    root->value = arr[root->index];
	    root->parent = NULL;
	    construct_tree(root, arr, 12, 0, 12);
	    //in_order_traversal(root);
	    int a = -1;
	    //cout << root->index<<" "<<root->value<<endl;
	    find_smallest_interval(root, 8, 9, &a);
	    cout << a << endl;
	    return 0;
	}
	
	/* 
	 * ===  FUNCTION  ======================================================================
	 *         Name:  find_smallest_interval
	 *  Description:  find the smallest value in the interval [left, right)
	 * =====================================================================================
	 */
	int find_smallest_interval(Node* node, int left, int right, int* result) {
	    if(right < left + 1) {
	        *result = -1;
	        return -1;
	    }
	
	    if(node->index >= left && node->index < right) {
	        *result = node->value;
	        return 0;
	    } else if(node->index < left) {
	        find_smallest_interval(node->right, left, right, result );
	    } else {
	        find_smallest_interval(node->left, left, right, result);
	    }
	
	    return 0;
	}
</pre>

#Chapter 4 Sorting and Searching #
##4.1 Applications of Sorting
* Searching: binary search
* Closest pair
* Element uniqueness
* Frequency distribution
* Selection: k-th largest element in an array
* Convex hulls

![](http://i.imgur.com/uDYXxYF.png)
 
##4.2 Pragmatics of Sorting
* Increasing or decreasing order
* Key or an entire record
* Equal key
* Non-numerical data

##4.3 Heap sort
###4.3.1 Heaps

In this spirit, a _heap-labeled tree_ is defined to be a binary tree such that the key labeling of each node _dominates_ the key labeling of each of its children. In a _min-heap_, a node dominates its children by containing a smaller key than they do, while in a _max-heap_ parent nodes dominate by being bigger.

<pre class = "brush:cpp">
	typedef struct {
		item_type q[PQ_SIZE+1]; /* body of queue */
		int n; /* number of queue elements */
	} priority_queue;

	pq_parent(int n){
		if (n == 1) 
			return(-1);
		else 
			return((int) n/2); /* implicitly take floor(n/2) */
	}

	pq_young_child(int n){
		return(2 * n);
	}
</pre>

Thus we can move around the tree __without any pointers__.
$$\sum^{h}_{i=0}{2^i} = 2^{h+1}-1 \geqslant n, \therefore h = \lfloor lg n \rfloor$$

###4.3.2 Constructing Heaps

<pre class = "brush:cpp">
	pq_insert(priority_queue *q, item_type x){
		if (q->n >= PQ_SIZE)
			printf("Warning: priority queue overflow insert x=%d\n",x);
		else {
			q->n = (q->n) + 1;
			//insert into the left-most open spot
			q->q[ q->n ] = x;
			bubble_up(q, q->n);
		}
	}

	bubble_up(priority_queue *q, int p){
		if (pq_parent(p) == -1) return; /* at root of heap, no parent */
		if (q->q[pq_parent(p)] > q->q[p]) {
			pq_swap(q,p,pq_parent(p));
			bubble_up(q, pq_parent(p));
		}
	}
</pre>

This swap process takes constant time at each level. Since the height of an nelement heap is $$\lfloor lgn \rfloor$$, each insertion takes at most $$O(logn)$$ time. Thus an initial heap of n elements can be constructed in $$O(n logn)$$ time through n such insertions:

<pre class = "brush:cpp">
	make_heap(priority_queue *q, item_type s[], int n){
		int i; /* counter */
		pq_init(q);
		for (i=0; i<n; i++)
			pq_insert(q, s[i]);
	}
</pre>

###4.3.3 Extracting the Minimum

<pre class = "brush:cpp">
	item_type extract_min(priority_queue *q){
		int min = -1; /* minimum value */
		if (q->n <= 0) printf("Warning: empty priority queue.\n");
		else {
			min = q->q[1];
			q->q[1] = q->q[ q->n ];
			q->n = q->n - 1;
			bubble_down(q,1);
		}
		return(min);
	}

	bubble_down(priority_queue *q, int p){
		int c; /* child index */
		int i; /* counter */
		int min_index; /* index of lightest child */
		c = pq_young_child(p);
		min_index = p;
		for (i=0; i<=1; i++){
			if ((c+i) <= q->n) {
				if (q->q[min_index] > q->q[c+i]) min_index = c+i;
			}
		}
		if (min_index != p) {
			pq_swap(q,p,min_index);
			bubble_down(q, min_index);
		}
	}
</pre>

We will reach a leaf after $$\lfloor lg n\rfloor$$ bubble down steps, each constant time. Thus root deletion is completed in $$O(log n)$$ time.
Exchanging the maximum element with the last element and calling heapify
repeatedly gives an $$O(n logn)$$ sorting algorithm, named _Heapsort_.

<pre class = "brush:cpp">
	heapsort(item_type s[], int n){
		int i; /* counters */
		priority_queue q; /* heap for heapsort */
		make_heap(&q,s,n);
		for (i=0; i<n; i++)
			s[i] = extract_min(&q);
	}
</pre>

It is an _inplace_ sort, meaning it uses _no extra memory_ over the array containing the elements to be sorted.

###4.3.4 Faster Heap Construction

<pre class = "brush:cpp">
	make_heap(priority_queue *q, item_type s[], int n) {
		int i; /* counter */
		q->n = n;
		for (i=0; i<n; i++) q->q[i+1] = s[i];
		for (i=q->n; i>=1; i--) bubble_down(q,i);
	}
</pre>

Multiplying the number of calls to _bubble down_ (n) times an upper bound on the cost of each operation $$O(log n)$$ gives us a running time analysis of $$O(n log n)$$.
But note that it is indeed an upper bound, because only the last insertion will actually take $$lg n$$ steps.
In a full binary tree on n nodes, there are n/2 nodes that are leaves (i.e. , height 0), n/4 nodes that are height 1, n/8 nodes that are height 2, and so on. In general, there are at most $$\lceil \frac{n}{2^{h+1}} \rceil$$ nodes of height h, so the cost of building a heap is:
$$\sum_{h=0}^{\lfloor lg n\rfloor}{\lceil \frac{n}{2^{h+1}} \rceil h} \leqslant n \sum_{h=0}^{\lfloor lg n\rfloor}{\frac{h}{2^h}}\leqslant 2n$$

> Problem: Given an array-based heap on n elements and a real number x, efficiently determine whether the kth smallest element in the heap is greater than or equal to x. Your algorithm should be $$O(k)$$ in the worst-case, independent of the size of the heap. Hint: you do not have to find the k-th smallest element; you need only determine its relationship to x.

<pre class = "brush:cpp">
	int heap_compare(priority_queue *q, int i, int count, int x){
		if ((count <= 0) || (i > q->n) return(count);
		//keep searching the successors
		if (q->q[i] < x) {
			count = heap_compare(q, pq_young_child(i), count-1, x);//left
			count = heap_compare(q, pq_young_child(i)+1, count, x);//right
		}
		return(count);
	}
</pre>

###4.3.5 Sorting by Incremental Insertion
	
<pre class = "brush:cpp">
	InsertionSort(A)
		A[0] = −$$\infinity$$
		for i = 2 to n do
			j = i
			while (A[j] < A[j − 1]) do
			swap(A[j],A[j − 1])
				j = j − 1
</pre>

##4.5 Mergesort

<pre class = "brush:cpp">
	Mergesort(A[1, n])
		Merge( MergeSort(A[1, ceil(n/2)]), MergeSort(A[ceil(n/2) + 1, n]) )
</pre>

![](http://i.imgur.com/2CTJt3V.jpg)

<pre class = "brush:cpp">
	mergesort(item_type s[], int low, int high){
		int i; /* counter */
		int middle; /* index of middle element */
		if (low < high) {
			middle = (low+high)/2;
			mergesort(s,low,middle);
			mergesort(s,middle+1,high);
			merge(s, low, middle, high);
		}
	}

	merge(item_type s[], int low, int middle, int high){
		int i; /* counter */
		queue buffer1, buffer2; /* buffers to hold elements for merging */
		init_queue(&buffer1);
		init_queue(&buffer2);
		for (i=low; i<=middle; i++) enqueue(&buffer1,s[i]);
		for (i=middle+1; i<=high; i++) enqueue(&buffer2,s[i]);
		i = low;
		//both not empty
		while (!(empty_queue(&buffer1) || empty_queue(&buffer2))) {
			if (headq(&buffer1) <= headq(&buffer2))
				s[i++] = dequeue(&buffer1);
			else
				s[i++] = dequeue(&buffer2);
		}
		while (!empty_queue(&buffer1)) s[i++] = dequeue(&buffer1);
		while (!empty_queue(&buffer2)) s[i++] = dequeue(&buffer2);
	}
</pre>

##4.6 Quicksort

<pre class = "brush:cpp">
	quicksort(item_type s[], int l, int h) {
		int p; /* index of partition */
		if ((h-l)>0) {
			p = partition(s,l,h);
			quicksort(s,l,p-1);
			quicksort(s,p+1,h);
		}
	}

	int partition(item_type s[], int l, int h){
		int i; /* counter */
		int p; /* pivot element index */
		int firsthigh; /* divider position for pivot element */
		p = h;
		firsthigh = l;
		for (i=l; i<h; i++){
			if (s[i] < s[p]) {
				swap(&s[i],&s[firsthigh]);
				firsthigh ++;
			}
		}
		swap(&s[p],&s[firsthigh]);
		return(firsthigh);
	}
</pre>

![](http://i.imgur.com/JOI8wH0.png)

##4.7 Distribution Sort: Sorting via Bucketing

###4.7.1 Lower Bounds for Sorting

##4.9 Binary Search and Related Algorithms

<pre class = "brush:cpp">
	int binary_search(item_type s[], item_type key, int low, int high){
		int middle; /* index of middle element */
		if (low > high) return (-1); /* key not found */
		middle = (low+high)/2;
		if (s[middle] == key) return(middle);
		if (s[middle] > key)
			return(binary_search(s,key,low,middle-1));
		else
			return(binary_search(s,key,middle+1,high));
	}
</pre>

###4.9.1 Counting Occurrences
A faster algorithm results by modifying binary search to search for the _boundary_ of the block containing k, instead of k itself. 

* Suppose we delete the equality test

<pre class = "brush:cpp">
		if (s[middle] == key) return(middle);
</pre>

* All searches will now be unsuccessful, since there is no equality test. The search will proceed to the right half whenever the key is compared to an identical array element, eventually terminating at the right boundary. 
* Repeating the search after reversing the direction of the binary comparison will lead us to the left boundary.

###4.9.2 One-sided Binary Search
###4.9.3 Square and Other Roots

> Binary search and its variants are the quintessential divide-and-conquer algorithms.

##4.10 Divide-and-Conquer

> To use divide-and-conquer as an algorithm design technique, we must divide the problem into two smaller sub-problems, solve each of them recursively, and then meld the two partial solutions into one solution to the full problem.

###4.10.1 Recurrence Relations
###4.10.2 Divide-and-Conquer Recurrences
Divide-and-conquer algorithms tend to break a given problem into some number of smaller pieces (say a), each of which is of size $$n/b$$. Further, they spend $$f(n)$$ time to combine these sub-problem solutions into a complete result.
$$T(n) = aT(n/b)+f(n)$$

* _Sorting_: $$T(n) = 2T(n/2)+f(n) \rightarrow T(n) = O(n lgn)$$
* _Binary Search_: $$T(n) = T(n/2)+O(1) \rightarrow T(n) = O(lg n)$$
* _Fast Heap Construction_: $$T(n) = 2T(n/2)+O(lg n) \rightarrow T(n) = O(n)$$
* _Matrix Multiplication_ 

###4.10.3 Solving Divide-and-Conquer Recurrences
$$T(n) = aT(n/b)+f(n)$$

1. If $$f(n) = O(n^{log_{b}{a-\epsilon}})$$ for some constant $$\epsilon > 0$$, then $$T(n) = \Theta(n^{log_ba})$$.
2. If $$f(n) = \Theta(n^{log_ba})$$, then $$T(n) = \Theta(n^{log_ba}lg n)$$.
3. If $$f(n) = \Omega(n^{log_ba+\epsilon})$$ for some constant $$\epsilon > 0$$, and if $$af(n/b) \leqslant cf(n)$$ for some $$c < 1$$, then $$T(n) = \Theta(f(n))$$.

![](http://i.imgur.com/SybO2vS.png)

其实关于各种排序的问题，wiki上有一组动态图可以很好地具象化这些过程：

* BubbleSort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif)
* Selection Sort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)
* Insertion Sort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
* Heap Sort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)
* Merge Sort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)
* Quick Sort: 
	* ![](http://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

15种排序的条带图简单演示：[https://www.youtube.com/watch?v=kPRA0W1kECg](https://www.youtube.com/watch?v=kPRA0W1kECg)

匈牙利土风舞版的具象演示：

* [Bubble Sort](https://www.youtube.com/watch?v=lyZQPjUT5B4)
* [Selection Sort](https://www.youtube.com/watch?v=Ns4TPTC8whw)
* [Insertion Sort](https://www.youtube.com/watch?v=ROalU379l3U)
* [Merge Sort](https://www.youtube.com/watch?v=XaqR3G_NVoo)
* [Quick Sort](https://www.youtube.com/watch?v=ywWBy6J5gz8)
* [Shell Sort](https://www.youtube.com/watch?v=CmPA7zE8mx0)

> 4-3 Take a sequence of 2n real numbers as input. Design an $$O(nlogn)$$ algorithm that partitions the numbers into n pairs, with the property that the partition minimizes the maximum sum of a pair. For example, say we are given the numbers (1,3,5,9). The possible partitions are ((1,3),(5,9)), ((1,5),(3,9)), and ((1,9),(3,5)). The pair sums for these partitions are (4,14), (6,12), and (10,8). Thus the third partition has 10 as its maximum sum, which is the minimum over the three partitions.

这题很绕人……首先保证这个序列是经过排序的。然后我们会发现具有上述特征的解必然包括$$(x_0, x_{2n-1})$$。
下面提供一种[证明方法](http://stackoverflow.com/questions/1817274/partition-a-sequence-of-2n-real-numbers-so-that)：

* 假设这种最优组合不包括$$(x_0,x_{2n-1})$$，那么它必然包括下面两个组合$$(x_0,x_a),(x_b,x_{2n-1})$$，其中 $$x_0 \leqslant x_a,x_b \leqslant x_{2n-1}$$。
* 那么如果我们将这两个组合替换为$$(x_0,x_{2n-1}),(x_a,x_b)$$，这两个组合的和具有以下特征：$$x_0+x_a \leqslant x_0+x_{2n-1} \leqslant x_b+x_{2n-1}, x_0+x_a \leqslant x_a+x_b \leqslant x_b+x_{2n-1}$$。
* 因此替换之后这个组合的和的最大值依然能够在所有的组合中保持最小，因此这依然是最优解。

<pre class = "brush:cpp">
		start = 0;
		end = 2n - 1;
		while (start < end) {
		  pair(S[star], S[end]);
		  start++;
		  end--;
</pre>

> 4-12 Devise an algorithm for finding the k smallest elements of an unsorted set of n integers in $$O(n + klogn)$$.

以$$O(n)$$的复杂度建立最小堆，执行k次`extract_min`操作，每次的时间复杂度为$$O(logn)$$。

<pre class = "brush:cpp">
	using namespace std;
	
	typedef struct s_pq {
	    int* buf;
	    int n;
	} pq;
	
	void swap(pq *q, int i, int j);
	void make_heap(pq *q, int s[]);
	void bubble_down(pq *q, int p);
	int extract_min(pq *q) ;
	
	void make_heap(pq *q, int s[]) {
	    for ( int i = 0; i < q->n; i++) {
	        q->buf[i] = s[i];
	    }
	
	    for ( int i = q->n - 1; i >= 0; i-- ) {
	        bubble_down(q, i);
	    }
	}
	
	void bubble_down(pq *q, int p) {
	    int c, min_index;
	    c = 2 * p + 1;
	    min_index = p;
	
	    for (int i = 0 ; i <= 1; i++) {
	        if(c + i < q->n) {
	            if(q->buf[min_index] > q->buf[c + i]) {
	                min_index = c + i;
	            }
	        }
	    }
	
	    if(min_index != p) {
	        swap(q, p, min_index);
	        bubble_down(q, min_index);
	    }
	}
	
	void swap(pq *q, int i, int j) {
	    int temp = q->buf[i];
	    q->buf[i] = q->buf[j];
	    q->buf[j] = temp;
	}
	
	int extract_min(pq *q) {
	    if(q->n < 1) {
	        return -1;
	    }
	    int min = q->buf[0];
	    q->buf[0] = q->buf[q->n - 1];
	    q->n = q->n - 1;
	    bubble_down(q, 0);
	    return min;
	}
	
	int main() {
	    int a[] = {5, 2, 1, 0, 9, 3, 7};
	    int *buf = (int*)calloc(7, sizeof(int));
	    pq q;
	    q.buf = buf;
	    q.n = 7;
	    make_heap(&q, a);
	    cout << endl;
	    int min;
	    for ( int i = 0; i < 3; i++) {
	        min = extract_min(&q);
			cout << min << endl;
			for ( int j = 0; j < q.n; j++) {
				cout << q.buf[j] << " ";
			}
			cout << endl;
	    }
	    return 0;
	}
</pre>