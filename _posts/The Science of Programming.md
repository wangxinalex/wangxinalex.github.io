#Chapter 1 Propositions
##1.1 Fully Parenthesized Propositions

![](http://i.imgur.com/C8OZa24.png)

#1.2 Evaluation of Constant Propositions
Truth Table
![](http://i.imgur.com/6igfo2K.png)

#1.3 Evaluation of Proposition in a State
Definition 1.3.1: A _state_ s is a _function_ from a set of identifiers to the set of values T and F.
Definition 1.3.2: Proposition e is _well-defined_ in state s if each identifier in e is associated with either T or F in state s.

In state $$s = {(b,T),(c,F)}$$, proposition $$(b \vee c)$$ is well-definition while proposition $$(b \wedge d)$$ is not.

Definition 1.3.3: Let proposition e be well-defined in state s. Then s(e), the value of e in state s, is the value obtained by replacing all occurrences of identifiers b in e by their values s(b) and evaluating the resulting constant proposition.

$$s(((\neg b)\vee c))$$ is evaluated in state $$s = {(b,T),(c,F)}$$

$$s(((\neg b)\vee c))$$
$$=((\neg T)\vee F )$$
$$=(F \vee F)$$
$$F$$ \square

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

Rule $$\Rightarrow-E$$ is called _modus ponens_(演绎推理).

$$=-I: \frac{E_1\Rightarrow E_2, E_2\Rightarrow E_1}{E_1=E_2}$$

$$=-E: \frac{E_1=E_2}{E_1\Rightarrow E_2,E_2\Rightarrow E_1}$$
 
##3.3 Proofs and Subproofs

$$\Rightarrow -I:\frac{From E_1, \cdots, \E_n infer E}{(E_1\wedge \cdots \wedge E_n)\Rightarrow E}$$

A theorem of the form "From $$e_1,\cdots, e_n$$ infer $$e$$" is interpreted as: if $$e_1,\ldots,e_n$$ are true in a state, then so is $$e$$.

###Subproofs
![](http://i.imgur.com/zzQCWfF.png)

###Scope Rules

