---
layout: post
title: "Backtracking"
description: ""
category: Algorithm
tags: Algorithm Backtracking
---
{% include JB/setup %}

#Chapter 1 Backtracking
> Backtracking is a **systematic** way to iterate through all the possible configurations of a search space.
> These configurations may represent all possible arrangemants if objects (permutations) or all possible 
> ways of building a collections of them (subsets).

<pre class = "brush:cpp">
bool finished = FALSE;
backtrack(int a[], int k, data input){
	int c[MAXCANDIDATES];
	int ncandidates;
	int i;
	
	if(is_a_solution(a, k, input)){
		process_solution(a, k, input);
	}else{
		k = k + 1;
		construct_candidates(a, k, input, c, &ncandidates);
		for(i = 0; i < ncandidates; i++){
			a[k] = c[i];
			make_move(a, k, input);
			backtrack(a, k, input);
			unmake_move(a, k, input);
			if(finished) return;
		}
	}
}
</pre>

##Chapter 1.1 Constructing all subsets

<pre class = "brush:cpp">    
#include &lt;stdio.h&gt;
#include &lt;bitset&gt;
#include &lt;iostream&gt;

using namespace std;

#define NMAX 4
#define SUBSET_NUM 16

bool finished = false;
int solutions = 0;

bool is_a_solution(bitset&lt;NMAX&gt; &a, int k, int n) {
	return k == n;
}

void construct_candidates(bitset&lt;2&gt;& c, int& ncandidates) {
	c.set(0);
	c.reset(1);
	ncandidates = 2;
}

void process_solution(bitset&gt;NMAX&gt;& a, int k) {
	cout << a << endl;
	int i;
	printf("{");
	for(i = 0; i < k; i++) {
		if(a.test(i)) {
			printf(" %d", i);
		}
	}
	printf(" }\n");
	if(++solutions == SUBSET_NUM) {
		finished = true;
	}
}

void backtrack(bitset&lt;NMAX&gt; &a, int k, int n) {
	bitset&lt;2&gt; c;
	int ncandidates;
	int i;
	if(is_a_solution(a, k, n)) {
		process_solution(a, k);
	} else {
		construct_candidates(c, ncandidates);
		for(i = 0; i < ncandidates; i++) {
			a[k] = c[i];
			backtrack(a, k + 1, n);
			if(finished) {
				return ;
			}
		}
	}
}

void generate_subsets(int n) {
	bitset&lt;NMAX&gt; a;
	backtrack(a, 0, n);
}

int main() {
	generate_subsets(4);
	return 0;
}
</pre>

##Chapter 1.2 Constructing all permutations

<pre class = "brush:cpp">    

#include &lt;stdio.h&gt;
#include &lt;iostream&gt;
#include &lt;bitset&gt;

using namespace std;

#define NMAX 4
#define PERM_NUM 24

bool finished = false;
int solutions = 0;

bool is_a_solution(int a[], int k, int n) {
	return k == n;
}

void construct_candidates(int a[], int k, int n, int c[], int &ncands) {
	bitset&lt;NMAX&gt; in_perm;
	for(int i = 0; i < k; i++) {
		in_perm.set(a[i]);
	}
	ncands = 0;
	for(int i = 0; i < n; i++) {
		if(!in_perm.test(i)) {
			c[ncands++] = i;
		}
	}
}

void process_solution(int a[], int k) {
	printf("{");
	for(int i = 0; i < k; i++) {
		printf(" %d", a[i]);
	}
	printf(" }\n");
	if(++solutions == PERM_NUM) {
		finished = true;
	}
}

void backtrack(int a[], int k, int n) {
	int ncands = 0;
	int c[NMAX];
	if(is_a_solution(a, k, n)) {
		process_solution(a, k);
	} else {
		construct_candidates(a, k, n, c, ncands);
		for(int i = 0; i < ncands; i++) {
			a[k] = c[i];
			backtrack(a, k + 1, n);
			if(finished) {
				return ;
			}
		}
	}
}

void generate_perms(int n) {
	int a[NMAX];
	backtrack(a, 0, n);
}

int main() {
	generate_perms(4);
	return 0;
}
</pre>
