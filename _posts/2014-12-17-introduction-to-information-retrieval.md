---
layout: post
title: "Introduction to Information Retrieval"
description: ""
category: 
tags: []
---
{% include JB/setup %}

##2.3 Faster postings list intersection via skip pointers

![](http://i.imgur.com/VXJtwNx.png)

A simple heuristic for placing skips, which has been found to work well in practice, is that for a postings list of length P, use $$\sqrt{P}$$ evenly-spaced skip pointers.
	
	IntersetWithSkips(p1, p2)
	answer = <>
	while p1 != NIL and p2 != NIL
	    if docID(p1) == docID(p2)
	        add(answer, docID(p1))
	            p1 = next(p1)
	            p2 = next(p2)
	    elseif docID(p1) < docID(p2)
	        if hasSkip(p1) and (docID(skip(p1)) <= docID(p2))
	            while hasSkip(p1) and (docID(skip(p1)) <= docID(p2))
	                p1 = skip(p1)
	        else 
	            p1 = next(p1)
	    else
	        if hasSkip(p2) and (docID(skip(p2)) <= docID(p1))
	            while hasSkip(p2) and (docID(skip(p2)) <= docID(p1))
	                p2 = skip(p2)
	        else 
	            p2 = next(p2)
	return answer

##2.4 Positional postings and phrase queries

###2.4.1 Biword indexes
One approach to handling phrases is to consider every pair of consecutive terms in a document as a *phrase*

1. tokenize the text and perform part-of-speech-tagging
2. group terms into nouns, including proper nouns, (N) and function words, including articles and prepositions, (X), among other classes
3. deem any string of terms of the form NX*N to be an extended biword  

The concept of a biword index can be extended to longer sequences of words, and if the index includes variable length word sequences, it is generally referred to as a *phrase index*.

**Drawbacks**:  

1. High False Positive  
2. Expanding the size of vocabulary  

For the reasons given, a biword index is not the standard solution

###2.4.2 Positional Indexes 
![](http://i.imgur.com/hVZxZp4.png)

	positional_intersect(p1, p2, k){
	    answer = <>
	    while(p1 != NIL and p2 != NIL){
	        if(docID(p1) == docID(p2)){
	            l = <>
	            pp1 = positions(p1)
	            pp2 = positions(p2)
	            while(pp1 != NIL){
	                while(pp2 != NIL){
	                    if(|pos(pp1) - pos(pp2)| <= k){
	                        add(l, pos(pp2))
	                    }else if(pos(pp2)>pos(pp1)){
	                        break
	                    }
	                    pp2 = next(pp2)
	                }
	                while(l!=<> and |l[0] - pos(pp1)| > k){
	                    delete(l[0])
	                }
	                for each(ps in l){
	                    add(answer, <docID(p1), pos(pp1), ps>)
	                }
	                pp1 = next(pp1)
	            }
	            p1 = next(p1)
	            p2 = next(p2)
	        }else if(docID(p1)<docID(p2)){
	            p1 = next(p1)
	        }else{
	            p2 = next(p2)
	        }
	    }
	    return answer
	}

###2.4.3 Combination schemes