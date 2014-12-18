---
layout: post
title:  "The Science of Programming"
date:   2014-09-16 20:39:39
categories: Booknote
tags: Mathematics
---
#Chapter 1 Propositions

##1.1 Fully Parenthesized Propositions

![](http://i.imgur.com/C8OZa24.png)

##1.2 Evaluation of Constant Propositions
Truth Table
![](http://i.imgur.com/6igfo2K.png)

##1.3 Evaluation of Proposition in a State
Definition 1.3.1: A _state_ s is a _function_ from a set of identifiers to the set of values T and F.  
Definition 1.3.2: Proposition e is _well-defined_ in state s if each identifier in e is associated with either T or F in state s.

In state $$s = {(b,T),(c,F)}$$, proposition $$(b \vee c)$$ is well-definition while proposition $$(b \wedge d)$$ is not.

Definition 1.3.3: Let proposition e be well-defined in state s. Then s(e), the value of e in state s, is the value obtained by replacing all occurrences of identifiers b in e by their values s(b) and evaluating the resulting constant proposition.

$$s(((\neg b)\vee c))$$ is evaluated in state $$s = {(b,T),(c,F)}$$

$$s(((\neg b)\vee c))$$
$$=((\neg T)\vee F )$$
$$=(F \vee F)$$
$$=F   \square$$

##1.4 Precedence Rules for Operators
1. Sequences of the same operator are evaluated from left to right, e.g. $$b \wedge c \wedge d \equiv ((b \wedge c)\wedge d)$$.
2. The order of evaluation of different, adjacent operators is given by the list: __not, and, or, imp, equals__.

$$\neg b = b \wedge c \equiv (\neg b) = (b \wedge c)$$

$$b \vee \neg c \Rightarrow d \equiv (b\vee(\neg c)) \Rightarrow d$$

$$b \Rightarrow c \Rightarrow d \vee e \equiv (b \Rightarrow c)\Rightarrow (d \wedge e) \square$$

![](http://i.imgur.com/Up19xUP.png)

![](http://i.imgur.com/hvyMXV3.png)

![](http://i.imgur.com/FINlzK5.png)

##1.5 Tautologies

The basic way to show that proposition is a tautology is to show that its evaluation yields T in every possible state.

> To prove a conjecture, it is necessary to prove that it is true in all cases; to disprove a conjecture, it is sufficient to find a single case where it is false.

##1.6 Propositions as Sets of States
Proposition b is _weaker_ than c if $$c \Rightarrow b$$. Correspondingly, c is said to be _stronger_ than b. A strong proposition makes more restrictions on the __combinations of values its identifiers can be associated with__, a weaker proposition makes fewer. The  weakest proposition is T (or any tautology), because it represents the set of all states; the strongest is F, because it represents the set of no states.

##1.7 Transforming English to Proposition Form

#Chapter 2 Reasoning using Equivalence Transformations

##2.1 The Laws of Equivalence
Definition 2.1.1: Propositions E1 and E2 are _equivalent_ iff E1=E2 is a tautology. In this case, E1=E2 is an equivalence.

1. Commutative Laws
2. Associative Laws
3. Distributive Laws
4. De Morgan's Laws
	1. $$\neg(E1 \wedge E2) = \neg E1 \vee \neg E2$$
	2. $$\neg(E1 \vee E2) = \neg E1 \wedge \neg E2$$
5. Law of Negation
6. Law of the Excluded Middle: $$E1 \vee \neg E1 = T$$
7. Law of Contradiction
8. Law of Implication: $$E1\Rightarrow E2 = \neg E1 \vee E2$$
9. Law of Equality: $$(E1 = E2) = (E1\Rightarrow E2)\wedge(E2\Rightarrow E1)$$
10. Laws of or-simplification:
	1. $$E1\vee E1 = E1$$
	2. $$E1\vee T = T$$
	3. $$E1\vee F = E1$$
	4. $$E1\vee(E1\wedge E2) = E1$$
11. Laws of and-simplification:
	1. $$E1\wedge E1 = E1$$
	2. $$E1\wedge T = E1$$
	3. $$E1\wedge F = F$$
	4. $$E1\wedge(E1\vee E2) = E1$$
12. Law of Idnetity: $$E1=E1$$

The laws of Equality and Implication deserve special mention. Together, they define __equality__ and __impl__ in terms of other operators: $$b=c$$ can always be replaced by $$(b\Rightarrow c)\wedge (c\Rightarrow b)$$ and $$\neg b\Rightarrow c$$ by $$b\vee c$$. 

##2.2 The Rules of Substitution and Transitivity

> __Rule of Substitution__: Let e1=e2 be an equivalence and E(p) be a proposition, written as a function of one of its identifiers p. Then E(e1)=E(e2) and E(e2)=E(e1) are also equivalent.

> __Rule of Transitivity__: If e1=e2 and e2=e3 are equivalences, then so is e1=e3(and hence e1 is equivalent to e3).

##2.3 A Formal System of Axioms and Interfaces Rules
> Axioms. Any proposition that arises by substituting propositions for E1, E2 and E3 in one of the Laws 1-12 is called a _theorem_.

1. __Rule of Substitution__: $$\frac{e1=e2}{E(e1)=E(e2), E(e2)=E(e1)}$$
2. __Rule of Transitivity__: $$\frac{e1=e2,e2=e3}{e1=e3}$$

#Chapter 3 A Natural Deduction System

##3.1 Introduction to Deductive Proofs

##3.2 Inference Rules

$$\wedge-I: \frac{E_1, \cdots, E_n}{E_1 \wedge \cdots \wedge E_n}$$

$$\wedge-E: \frac{E_1\wedge \cdots \wedge E_n}{E_i}$$

$$\vee-I:\frac{E_i}{E_1\vee \cdots \vee E_n}$$

![](http://i.imgur.com/FgMvauq.png)

$$\vee-E:\frac{E_1\vee \cdots \vee E_n, E_1\Rightarrow E, \cdots , E_n\Rightarrow E}{E}$$

![](http://i.imgur.com/Z2XLjvl.png)

$$\Rightarrow-E: \frac{E_1\Rightarrow E_2, E_1}{E_2}$$

Rule $\Rightarrow-E$ is called _modus ponens_(演绎推理).

$$=-I: \frac{E_1\Rightarrow E_2, E_2\Rightarrow E_1}{E_1=E_2}$$

$$=-E: \frac{E_1=E_2}{E_1\Rightarrow E_2,E_2\Rightarrow E_1}$$

##3.3 Proofs and Subproofs

$$\Rightarrow -I:\frac{From E_1, \cdots, E_n infer E}{(E_1\wedge \cdots \wedge E_n)\Rightarrow E}$$

A theorem of the form "From $$e_1,\cdots, e_n$$ infer $$e$$" is interpreted as: if $$e_1,\ldots,e_n$$ are true in a state, then so is $$e$$.

###Subproofs
![](http://i.imgur.com/zzQCWfF.png)

###Scope Rules
![](http://i.imgur.com/AnUdxFw.png)

###Proof by Contradiction

$$\neg-I: \frac{From\  E\ infer\ E_1\wedge \neg E_1}{\neg E}$$

$$\neg-E: \frac{From\ \neg E\ infer\ E_1\wedge \neg E_1}{E}$$

![](http://i.imgur.com/5JrMD05.png)

##3.4 Adding flexibility to the Natural Deduction System

###Using theorems as schemata

###Informal proof

$$\frac{From\ E_1(p),\cdots,E_n(p)\ infer\ E(p)}{From\ E_1(G), \cdots, E_n(G)\ infer\ E(G)}$$

###The Rule of Substitution of equals for equals

> Let proposition E be thought of as a function of one of its identifiers, p, so that we write it as E(p). Then  if e1=e2 and E(e1) appear on previous lines, then we may write E(e2) on a line.

$$\frac{e_1=e_2, E(e_1)}{E(e_2)}$$

###Some basic theorems

1. Commutative laws.
2. Associative laws.
3. Distributive laws.
4. De Morgan's laws.
	1. ![](http://i.imgur.com/s2LQcg9.png)
	2. ![](http://i.imgur.com/kg1E6q9.png)
5. Law of Negation. 
6. Law of the Excluded Middle
7. Law of Contradiction
8. Law of Implication
9. Law of Equality
10. Laws of or-simplication
11. Laws of and-implication

##3.5 Developing Natural Deduction System Proofs

###Some general hints on developing proofs

###The Development of a proof
![](http://i.imgur.com/QR31IX0.png)

###The development of a second proof
![](http://i.imgur.com/H4SFmEW.png)

###The Tardy Bus Problem

#Chapter 4 Predicates

##4.1 Extending the Range of s State

###Evaluating Predicates

###Reasoning about Atomic Expressions

###The operators _cand_ and _cor_
short-circuit __and__ and __or__

![](http://i.imgur.com/EggfRKO.png)

b __cand__ c: __if__ b __then__ c __else__ F  
b __cor__ c: __if__ b __then__ T __else__ c

2. Associativity
3. Distributivity
4. De Morgan: $$\neg(E1\ cand\ E2) = \neg E1\ cor\ \neg E2$$
5. Excluded Middle
6. Contradiction
7. cor-simplification: E1 cor (E1 cand E2) = E1
8. cand-simplification: E1 cand (E1 cor E2) = E1
 
##4.2 Quantification

###Existential Quantification

![](http://i.imgur.com/vhaxr89.png)

Definition of E:

$$(E\ i: m\leqslant i<m:E_i)=F$$, and, for $$f\geqslant m$$

$$(E\ i: m\leqslant i<k+1:E_i)=(E\ i:m\leqslant i < k:E_i)\vee E_k \square$$

###Universal Quantification
Definition. $$(A\ i:m\leqslant i < n:E_i)=\neg(E\ i:m\leqslant i < n: \neg E_i).\ \square$$

###Numerical Quantification
Definition. $$(N\ i:m\leqslant i < n: E_i)$$ denotes the number of different values i in range $$m \leqslant i < n$$ for which $$E_i$$ is true. N is called the _counting_ quantifier.

$$(E\ i:m\leqslant i < n:E_i) = (N\ i:m\leqslant i < n: E_i)\geqslant 1$$

$$(A\ i: m\leqslant i < n:E_i) = (N\ i:m\leqslant i < n:E_i)=n-m$$

Now it is easy to assert than k is the third smallest integer such that $$E_k$$ holds: 

$$((N\ i: 0\leqslant i < k:E_i)=2)\wedge E_k$$

###A Note on Ranges
![](http://i.imgur.com/lYTjCXX.png)

##4.3 Free and Bound Identifiers
__Restriction on identifiers__: In an expression, an identifier may not be both bound and free, and an identifier may not be bound to two different quantifiers.

![](http://i.imgur.com/b5sHNdi.png)

##4.4 Textual Substitution
![](http://i.imgur.com/OuvpKJG.png)

![](http://i.imgur.com/O0OUMDG.png)

###Simultaneous Substitution

##4.5 Quantification Over Other Ranges

###The Type of a Quantified Identifier

###Tautologies and Implicit Quantification

###Inference Rules for A and E

$$A-I:\frac{R\Rightarrow E}{(A\ i:R:E)}$$ where _i_ is a fresh identifier.

$$A-E:\frac{(A\ i:R:E)}{R^i_e\Rightarrow E^i_e}$$ for any predicate _e_

$$E-I:\frac{(A\ i:R:E)}{\neg(E\ i:R:\neg E)}$$

$$E-E:\frac{(E\ i:R:E)}{\neg(A\ i:R:\neg E)}$$

**bound-variable substitution:**$$\frac{E\ i:R:E}{E\ k:R^i_k:E^i_k}$$(provided _k_ does not appear free in R and E)

##4.6 Some Theorems about Textual Substitution and States

$$s(E^x_e) = s(E^x_{s(e)})$$

Substituting an expression _e_ for _x_ in E and then evaluating in _s_ yields the same result as substituting the _value_ of _e_ in _s_ for _x_ and then evaluating.

Let $$s'=(s;x:s(e))$$. Then $$s'(E)=s(E^x_e)$$

For a list of distinct identifiers $$\bar{x}$$, expression E and a list $$\bar{u}$$ (of the same length as $$\bar{x}$$) of fresh, distinct identifiers, we have 

$$(E_{\bar{u}}^{\bar{x}})_{\bar{x}}^{\bar{u}} = E \square$$

#Chapter 5 Notations and Conventions for Arrays

##5.1 One-dimensional Arrays as Functions
![](http://i.imgur.com/BayG44W.png)

##5.2 Array Sections and Pictures

###(!)Array Pictures
![](http://i.imgur.com/YbKjwnT.png)

![](http://i.imgur.com/XPft6u5.png)

![](http://i.imgur.com/wdO73rl.png)

##5.3 Handling Arrays of Arrays of...
![](http://i.imgur.com/58VfVhz.png)

#Chapter 6 Using Assertions To Document Programs

##6.1 Program Specifications

> If execution of S in begun in a state satisfying Q, then it is guaranteed to terminate in a finite amount of time in a state satisfying R.

Q is called the _precondition_ or _input assertion_ of S; R is the _postcondition, output assertion or result assertion_.

##6.2 Representing Initial and Final Values of Variables

##6.3 Proof Outlines
![](http://i.imgur.com/LMDApkJ.png)

![](http://i.imgur.com/Oakyc3S.png)

#Part II The Semantics of a Small Language

#Chapter 7 The Predicate Transformer wp
> For any command S and predicate R, which describes the desired result of executing S, we will define another predicate, denoted by _wp(S,R)_, that represents: the set of __all__ states such that execution of S begun in any one of them is guaranteed to terminate in a finite amount of time in a state satisfying R.

> **The notation {Q}S{R} is simply another notation for $$Q\Rightarrow wp(S,R)$$.**

###Some properties of wp

1. **Law of the Excluded Miracle**:$$wp(S,F) = F$$
2. **Distributivity of Conjunction**: $$wp(S,Q)\wedge wp(S,R) = wp(S,Q\wedge R)$$
3. **Law of Monotonicity**: if $$Q\Rightarrow R$$ then $$wp(S,Q)\Rightarrow wp(S,R)$$
4. **Distributivity of Disjunction**:$$wp(S,Q)\vee wp(S,R)\Rightarrow wp(S, Q\vee R)$$

#Chapter 8 The Commands skip, abort and Composition
**Definition**
1. $$wp(skip, R) = R$$
2. $$wp(abort, R) = F$$
3. $$wp("S1;S2", R) = wp(S1,wp(S2,R))$$(sequential composition)

#Chapter 9 The Assignment Command

##9.1 Assignment to Simple Variables
**(!)Definition** $$wp("x:= e", R) = domain(e)$$ **cand** $$R_e^x$$
Since x will contain the value of e after execution, then R will be true after execution *iff* R, with the value of x replaced by e, is true before execution. 

##9.2 Multiple Assignment to Simple Variables

**Definition** $$wp("\bar{x}:= \bar{e}", R) = domain(\bar{e})$$ **cand**$$R_{\bar{e}}^{\bar{x}}$$

**Caution**: $$x,y:=e1,e2$$ is semantically equivalent to "$$x:=e1;y:=e2$$" and also to "$$y:=e2;x:=e1$$", **provided that x does not occur in e2 and y does not occur in e1**.

![](http://i.imgur.com/1nKsxyl.png)

##9.3 Assignment to an Array Element
**Definition** $$wp("b[i]:=e", R) = inrange(b,i)$$ **cand** $$domain(e)$$ **cand** $$R_{(r, i:e)}^{b}$$ or simply $$wp("b[i]:= e", R) = R_{(b,i:e)}^{b}$$
![](http://i.imgur.com/hDManYT.png)

![](http://i.imgur.com/TvbfCwd.png)

##9.4 The General Multiple Assignment Command
**Execution of a multiple assignment.** First, determine the variables specified by the $$x_i \circ s_i$$ and evaluate the expressions $$e_i$$ to yield values $$v_i$$. Then assign $$v_1$$ to $$x_1 \circ s_1$$, $$v_2$$ to $$x_2 \circ s_2, \ldots,$$ and $$v_n$$ to $$x_n \circ s_n$$. The order to assignment must be from left to right.
**Definition.** $$wp("\bar{x\circ s}:=\bar{e}", R) = R_{\bar{e}}^{\bar{x\circ s}}$$
![](http://i.imgur.com/4YOmHHX.png)

![](http://i.imgur.com/KkCp6fe.png)

#Chapter 10 The Alternative Command
**Definition** $$wp(IF,R) = domain(BB)\wedge BB \wedge B_1\Rightarrow wp(S_1,R)\wedge \cdots \wedge B_n\Rightarrow wp(S_n, R) \square$$

**Definition** $$wp(IF,R) = (E\ i:1\leqslant i \leqslant n: B_i)\wedge (A\ i:1\leqslant i \leqslant n:B_i\Rightarrow wp(S_i,R))\square$$
![](http://i.imgur.com/ipSSgaM.png)

###Some Comments about the Alternative Command

###A Theorem about the Alternative Command
![](http://i.imgur.com/pjsjVSF.png)

#Chapter 11 The Iterative Command

###11.1The conventional while-loop and the iterative command
![](http://i.imgur.com/beTKuwJ.png)

###The Formal Definition of DO

$$H_k(R) = H_0(R)\vee wp(IF, H_{k-1}(R)), for\ k>0$$

**Definition**. $$wp(DO,R) = (E\ k:0\leqslant k:H_k(R))\square$$

* Predicate $$H_k(R)$$, for k > 0, to represent the set of all states in which execution of DO terminates in k or *fewer* iterations, with R true.
* A predicate P that is true before and after each iteration of a loop is called an *invariant relation* or *invariant* of the loop.
* We introduce an integer function, t, of the program variables that is an upper bound on the number of iterations still to be performed. We call t the *bound function*.

![](http://i.imgur.com/hRyYFd8.png)
**General Procedure:**
    1. Introduce the *invariant* P
    2. Determine that P is true just after the initialization
    3. Any iteration of the loop beginning with P true terminates with P true, so that P is an invariant of the loop.
    4. P must be true upon the termination.

###A theorem concerning a loop, an invariant and a bound function

![](http://i.imgur.com/gX90qwV.png)

###Annotation a loop and understanding the annotation

![](http://i.imgur.com/pFy5NJP.png)

**Checklist for understanding a loop:**

![](http://i.imgur.com/YQnIJJH.png)

#Part III The Development of Programs

#Chapter 13 Introduction
> A program and its proof should be developed hand-in-hand, with the proof usually leading the way. 

> Use theory to provide insight; use common sense and intuition where it is suitable, but fall back on the formal theory for support when difficulties and complexities arise.

> Know the properties of the objects that are to be manipulated by a program.

> Never dismiss as obvious any fundamental principal, for it is only through conscious application of such principals that success will be achieved.


#Chapter 14 Programming as a Goal-Oriented Activity

> Programming is a goal-oriented activity.

> Before attempting to solve a problem, make absolutely sure you know what the problem is.

> Before developing a program, make precise and refine the pre- and postconditions.

**Strategy for developing an alternative command:** To invent a guarded command, find a command C whose execution will establish postcondition R in at least some cases; find a Boolean B satisfying $$B\Rightarrow wp(C,R)$$; and put them together to form $$B\rightarrow C$$. Continue to invent guarded commands until the precondition of the construct implies that at least one guard is true.


> All other thins being equal, make the guards of an alternative command as strong as possible, so that some errors will cause abortion.

#Chapter 15 Developing Loops from Invariants and Bounds

> All other things being equal, make the guards of a loop as weak as possible, so that an error may cause an infinite loop.

**Strategy for developing a loop:** First develop the guard B so that $$P\wedge \neg B \Rightarrow R$$; then develop the body so that it decreases the bound function while reestablishing the loop invariant.

$$R:(0\leqslant i < m \wedge 0\leqslant j < n \wedge x  = b[i,j])\vee (i = m \wedge x \notin b)$$

![](http://i.imgur.com/FhRkBJu.png)

$$\neg B: i = m\vee (i < m\ cand\ x=b[i,j])$$

$$B: i \neq m\ cand\ x\neq b[i,j]$$

$$if\ j < n - 1 \rightarrow j := j + 1\ else\ j = n - 1\rightarrow i,j := i + 1, 0\ fi$$

![](http://i.imgur.com/qsBGgbv.png)

##15.2 Making Progress Towards Termination

> Write a program that sorts the four integer variables q0,q1,q2,q3. That is, upon termination the following should be true: $$q0\leqslant q1 \leqslant q2\leqslant q3$$

$$Q:q0=Q1\wedge q1=Q1 \wedge q2=Q2 \wedge q3=Q3$$

$$R:q0\leqslant q1\leqslant q2\leqslant q3 \wedge perm((q0,q1,q2,q3),(Q0,Q1,Q2,Q3))$$

$$P:perm((q0,q1,q2,q3),(Q0,Q1,Q2,Q3))$$

$$t:(N\ i,j:0\leqslant i<j<4;qi>qj)$$

![](http://i.imgur.com/v3pi46C.png)

#Chapter 16 Developing Invariants

##16.1 The Balloon Theory

![](http://i.imgur.com/nvWChRB.png)

###16.1.1 Weakening a Predicate
1. **Delete a conjunct**. Predicate $$A\wedge B\wedge C$$ can be weakened to $$A\wedge C$$
2. **Replace a constant by a variable**. Predicate $$x\leqslant b[1:10]$$, where x is a simple variable, can be weakened to $$x\leqslant b[1:i]\wedge 1\leqslant i\leqslant 10$$, where i is a fresh variable.
3. **Enlarge the range of a variable.** Predicate $$5\leqslant i<10$$ can be weakened to $$0\leqslant i < 10$$.
4. **Add a disjunct**. Predicate A can be weakened to $$A\vee B$$, for some other predicate B.

##16.2 Deleting a Conjunct
For the guard of the loop, use the *complement* of the deleted conjunct, so that when the loop terminates because the guard is false, the deleted conjunct is true.

**Strategy**: When deleting a conjunct from R to produce an invariant P, try using complement of the deleted conjunct for the guard B of the loop.

**The Linear Search Principle**: to find a minimum value(at least equal to some lower bound) with a property, investigate values starting at that lower bound in *increasing* order. Similarly, when looking for a maximum value investigate values in decreasing order.


##16.3 Replacing a Constant By a Variable

$$R: s = (\sum j:0\leqslant i < n:b[j])$$

R can be weakened by replacing n by a fresh variable i, yielding

$$s = (\sum j: 0\leqslant j < i:b[j])$$

$$P: 0\leqslant i < n\wedge s = (\sum j:0\leqslant j < i:b[j])$$

$$i,s := 0,0; do\ i\neq n\rightarrow i,s:=i+1,s+b[i]\ od$$

> Introduce a variable only when there is a good reason for doing so.

> Put suitable bounds on each variable introduced.

##16.4 Enlarging the Range of a Variable

