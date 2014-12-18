---
layout: post
title: "Introduction to Information Retrieval"
description: ""
category: booknote
tags: [Information_Retrieval, Machine_Learning]
---
{% include JB/setup %}
##2.2 Determining the vocabulary of terms

###2.2.1 Tokenization

###2.2.2 Dropping common terms: stop words

![](http://i.imgur.com/pKniQJn.png)

###2.2.3 Normalization

![](http://i.imgur.com/7vpAufs.png)

###2.2.4 Stemming and lemmatization

am, are, is => be
car, cars, car's, cars' => car

the boy's cars are different colors => the boy car be differ color

**Stemming**: refers to
a crude heuristic process that chops off the ends of words in the hope of
achieving this goal correctly most of the time, and often includes the removal of derivational affixes.

**Lemmatization**:  refers to doing things
properly with the use of a vocabulary and morphological analysis of words,
normally aiming to remove inflectional endings only and to return the base or dictionary form of a word, which is known as the *lemma*.

![](http://i.imgur.com/KJtdHq1.png)

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

1. tokenize the text and perform part-of-speech-tagging, which classifies words as nouns, verbs, etc.
2. group terms into nouns, including proper nouns, (N) and function words, including articles and prepositions, (X), among other classes
3. deem any string of terms of the form NX*N to be an extended biword  

![](http://i.imgur.com/ZipRFmo.png)

The concept of a biword index can be extended to longer sequences of words, and if the index includes variable length word sequences, it is generally referred to as a *phrase index*.

**Drawbacks**:  1. High False Positive 2. Expanding the size of vocabulary  

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
	                        add(l, pos(pp2)) // add all adjacent tokens of pp1 in p2 
	                    }else if(pos(pp2)>pos(pp1)){
	                        break
	                    }
	                    pp2 = next(pp2)
	                }
	                while(l!=<> and |l[0] - pos(pp1)| > k){
	                    delete(l[0])//delete all tokens remaining in l, 
						//which are adjacent to the previous pp1, but not adjacent to the current pp1
	                }
	                for each(ps in l){
	                    add(answer, <docID(p1), pos(pp1), ps>) //add all adjacent pairs 
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

A combination strategy uses a phrase index, or just a biword index, for certain
queries and uses a positional index for other phrase queries.

#Chapter 3 Dictionaries and tolerant retrieval

