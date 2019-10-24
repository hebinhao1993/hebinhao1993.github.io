---
title: elements of set theory
date: 2019-05-21 14:40:54
tags:
mathjax: true
---

## 介绍

集合论的基础是数理逻辑，其次集合论要承认集合的存在以及集合之间$\in$和$\notin$的关系的存在。

集合之间的其他关系都从$\in$关系中推导出来，例如$x \subseteq a$即$\forall t (t\in x \Rightarrow t \in a)$

<!-- more -->

## 部分公理和一些运算符

### Extensionality Axiom

If two sets have exactly the same members, then they are equal.

$$
\forall A \forall B [\forall x (x \in A \Leftrightarrow x \in B) \Rightarrow A = B]
$$

### Empty Set Axiom

There exist a set having no members.

$$
\exists B \forall x \ x \notin B
$$

### Pairing Axiom

For any sets $u$ and $v$, there is a set having as members just $u$ and $v$.

$$
\forall u, \forall v, \exists B, \forall x ( x \in B \Leftrightarrow x = u\ or\ x = v)
$$

### Union Axiom

For any set $A$, there exists a set $B$ whose elementts are exactly the members of the members of A:

$$
\forall x [x \in B \Leftrightarrow (\exists b \in A) x \in b]
$$

### Power Set Axiom

For any set $a$, there is a set whose members are exactly the subsets of $a$.

$$
\forall a, \exists B, \forall x (x \in B \Leftrightarrow x \subseteq a).
$$

### Subset Axioms

For each formula __ not containing B, the following is an axiom:

$$
\forall t_1 \cdots \forall t_k \forall c \exists B \forall x (x \in B \Leftrightarrow x \in c \And \text{__})
$$

这里的formula不是任意的语句，必须是可以用逻辑语言描述的语句。

从Subset Axioms可以推导出，对任意一个非空集合$A$，存在一个集合它的成员刚好是$A$的每个成员的成员。

### 运算符

从上述这些公理我们可以定义一些运算$\subseteq$,$\bigcup$,$\bigcap$ $-$。这四个运算共同组成了the algebra of sets。

## 关系和函数 (relations and functions)

### definition of ordered pairs

$$
<x, y> \text{is defined to be} \{\{x\}, \{x,y\}\}
$$

### cartesian product

$$
A \times B  = {<x,y> | x \in A \And y \in B}
$$

### definition of relation (binary relation)

$$
\text{A relation is a set of ordered pairs}
$$

### definition of domain, range and field

$$
\begin{equation*}
\begin{split}
x \in \text{dom} R \Leftrightarrow& \exists y, <x, y> \in R,\\
x \in \text{ran} R \Leftrightarrow& \exists x, <x, y> \in R,\\
\text{fld} R =& \text{dom} R \bigcup \text{ran} R
\end{split}
\end{equation*}
$$

### definition of n-ARY relations

$$
<x_1, x2, x3, x4> = <<x_1,x_2,x_3>, x_4> = <<<x_1, x_2>, x_3>, x_4>
$$

### definition of function

A function is a relation $F$ such that for each $x$ in $\text{dom} F$ there is only one $y$ such that $xFy$

### definition of single-rooted

A set $R$ is single-rooted iff for each $y \in \text{ran} R$ there is only one $x$ such that $xRy$

### definitions

The inverse of a set $F$:

$$
F^{-1} = \{<u,v> | vFu\}
$$

The composition of set $F$ and set $G$:

$$
F \circ G = \{<u,v> | \exists t (uGt \And tFv)\}
$$

The restriction of $F$ to $A$:

$$
F \restriction A = \{<u,v>| uFv \And u \in A\}
$$

The image of $A$ under $F$:

$$
\begin{equation*}
\begin{split}
F [\![ A ]\!] =& \text{ran} F (\restriction A) \\
=& \{v| (\exists u \in A) uFv \}
\end{split}
\end{equation*}
$$

### Axiom of Choice (first form)

For any relation $R$ there is a function $H \subseteq R$ with $\text{dom} H = \text{dom} R$

### some definitions

For sets $A$ and $B$ we can form the collection of functions $F$ from $A$ into $B$

$$
\sideset{^A}{}B = \{F | \text{F is a function from A into B}\}
$$

### infinite cartesian product

...

### equavalence relations

$R$ is a *equavalence relation* on $A$ iff $R$ is a binary relation on $A$ that is reflexive on A, sysmmetric, and transitive.

### definition

The set $[x]_R$ is defined by:

$$
[x]_R = \{t | xRt\}
$$

A partition $\prod$ of a set $A$ is a set of nonempyt subsets of $A$ that is disjoint and exhuastive:

no two different sets in $\prod$ have any common elements

each element of A is in some set in $\prod$

if $R$ is an equivalence relation on $A$, then we can define the $quotinent set$
$$
A/R = \{ [x]_R | x \in A \}
$$

### ordering relations

Let $A$ be any set. A linear ordering on $A$ (also called a total ordering on $A$) is a binary relation $R$ on $A$ (i.e. $R \subseteq A \times A$) meeting the following two conditions:

$R$ is a transitive relation

$R$ satisfies trichotomy on $A$, by which we mean that for any $x$ and $y$ in $A$, exactly one of the three alternatives

$$
xRy,\quad x = y, \quad yRx
$$

## Natural Numbers

For any set $a$, its successor $a^+$ id defined by

$$
a^+ = a \bigcup {a}
$$

A set $A$ is said to be *inductive* iff $\emptyset \in A$ and it is "closed iunder successor", i.e. $\forall a \in A, a^+ \in A$

### infinity Axiom

There exists an inductive set:

$$
\exists A [ \emptyset \in A \ \And\ (\forall a \in A) a^+ \in A]
$$

### definition of natural number

A nutrual number is a set that belongs to every inductive set

## construction of the real numbers

### integers

define $\sim$ to be the relation on $\omega \times \omega$ for which
$$
<m,n> \sim <p,q>\iff m+q=p+n
$$

The set $\mathbb{Z}$ of integers is the set $(\omega \times \omega)/\sim$ of all equaivalence classes of differences

### rational numbers

define $\sim$ to be the relation on $\mathbb{Z} \times \mathbb{Z}$ for which
$$
<a,b> \sim <c,d>\iff a\cdot d = c\cdot b
$$

The set $\mathbb{Q}$ of rational numbers is the set $\mathbb{Z} \times \mathbb{Z} /\sim$ of all equavalence classes of fractions.

### real numbers

dedekind cut

...

## cardinal numbers and the axiom of choice

A set $A$ is equinumerous to a set $B$ (written $A \approx B$) ifff there is a one-to-one function from A onto B.

A set is *finite* iff it is equinumerous to some natural numebr. Otherwise it is infinite.
