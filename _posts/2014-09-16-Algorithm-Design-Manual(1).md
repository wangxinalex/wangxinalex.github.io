---
layout: post
title:  "Algorithm Design Manual (Chapter 1)"
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
