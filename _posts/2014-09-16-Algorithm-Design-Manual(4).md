---
layout: post
title:  "Algorithm Design Manual (Chapter 4)"
date:   2014-09-17 20:39:43
categories: Algorithm Booknote
tags: Algorithm
---
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
	* stable or unstable
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

----------

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
