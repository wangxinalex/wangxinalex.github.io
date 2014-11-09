---
layout: post
title: "Dynamic Programming"
description: ""
category: Algorithm
tags: Algorithm Dynamic_Programming
---
{% include JB/setup %}
#Rod-cutting Question

##Recursive top-down implementation

	function CUT-ROD(p,n)
		if n == 0
			return 0
		q = - Inf
		for i = 1 to n
			q = max(q, p[i] + CUT-ROD(p, n-i))
		return q

To analyze the running time of CUT-ROD, let `T(n)` denote the total number of calls made to CUT-ROD when called with its second parameter equal to n.

![](http://i.imgur.com/ClbPe7G.png)

$$T(n) = 1+\sum^{n-1}_{j=0}{T(j)}$$

##Using dynamic programming for optimal rod cutting

> Having observed that a
naive recursive solution is inefficient because it solves the same subproblems repeatedly, we arrange for each subproblem to be solved only once, saving its solution. If we need to refer to this subproblem’s solution again later, we can just look it up, rather than recompute it. Dynamic programming thus uses additional memory to save computation time; it serves an example of a *time-memory trade-off*.

###Top-down with memorization

* write the procedure recursively in a natural manner, but modified to save the result of each subproblem (usually in an array or hash table).  
* The procedure now first checks
to see whether it has previously solved this subproblem. If so, it returns the saved
value, saving further computation at this level; if not, the procedure computes the
value in the usual manner. 

We say that the recursive procedure has been **memoized**; it “remembers” what results it has computed previously.

	Memoized-Cut-Rod(p,n)
		let r[0..n] be a new array
		for i = 0 to n
			r[i] = -Inf
		return Memoized-Cut-Rod_Aux(p, n, r)

	Memoized-Cut-Rod-Aux(p,n,r)
		if r[n] >= 0
			return r[n]
		if n == 0
			q = 0
		else q = -Inf
			for i = 1 to n
				q = max(q, p[i] + Memoized-Cut-Rod-Aux(p, n-i, r))
		r[n] = q
		return q

###Bottom-up method

* We sort the subproblems by size and solve them in size order, smallest first. 
* When solving a particular subproblem, we have already solved all of the smaller subproblems its solution depends upon, and we have saved their solutions.
* We solve each subproblem only once, and when we first see it, we have already solved all of its prerequisite subproblems.

The bottom-up version is even simpler

	Bottom-Up-Cut-Rod(p,n)
		let r[0..n] be a new array
		r[0] = 0
		for j = 1 to n
			q = -Inf
			for i = 1 to j
				q = max(q, p[i] + r[j-i])
			r[j] = q
		return r[n]

###Subproblem graphs

We can think of the subproblem graph as a **reduced** or **collapsed** version of the recursion tree for the top-down recursive method, in which we coalesce all nodes for the same subproblem into a single vertex and direct all edges from parent to child.

![](http://i.imgur.com/ogG9QRW.png)

The bottom-up method for dynamic programming considers the vertices of the subproblem graph in such an order that we solve the subproblems y adjacent to a given subproblem x before we solve subproblem x.

###Reconstructing a solution

	Extended-Bottom-Up-Cut-Rod(p,n)
		let r[0..n] and s[0..n] be new arrays
		r[0] = 0
		for j = 1 to n
			q = -Inf
			for i = 1 to j
				if q < p[i] + r[j-i]
					q = p[i] + r[j-1]
					s[j] = i
			r[j] = q
		return r and s

	Print-Cut-Rod-Solution(p,n)
		(r,s) = Extended-Bottom-Up-Cut-Rod(p,n)
		while n > 0
			print s[n]
			n = n - s[n]