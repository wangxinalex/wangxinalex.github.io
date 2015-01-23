---
layout: post
title:  "Algorithm Design Manual (Chapter 3)"
date:   2014-09-16 20:39:41
categories: Algorithm Booknote
tags: Algorithm
---
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
