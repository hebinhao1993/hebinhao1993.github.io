---
title: haskell
date: 2019-07-11 21:06:25
tags:
---

<!-- more -->

function

function without parameter, definition or name

list: a homogenous data structure。一个list的类型取决于成员的类型。

string 是一种list。 "hello"仅仅是['h','e','l','l','o']的语法糖。
[1,2,3]是1:2:3:[]的语法糖。

tuple： tuple 和 list 类似。但是tuple不必homogenous。tuple可以包含多种类型。tuple不能包含无限的成员。一个tuple的类型，取决于它有几个成员以及成员的类型。

haskell有一个static type system。任何表达式的type都在编译时获知。在haskell中任何东西都有类型。haskell有type inference。

functions和 variables必须以小写字母开头。module name必须以大写字母开头

Operators are functions which can be used in infix style.

可以使用\`\`和()来改变

```haskell
10 `div` 4
(+) 100 100
```

如果函数名是由字母和数字组成，那么默认是前缀，如果是符号，那么默认是中缀。

空格是函数调用的标志。

Types are a way of categorizing values.

`::` is read as has the type

Expressions, when evaluated, reduce to values. Every value has a
type. Types are how we group a set of values together that share
something in common.Sometimes that “something in common”
is abstract, sometimes it’s a specific model of a particular concept
or domain.

data declaration:

1. keyword data
2. type constructor
3. data constructor

pattern matching. We can define a function by matching on a data constructor, or value, and descrbing the behavior the function should have based on which value it matches.

Typeclasses are a way of adding functionality to types that is reusable across all the types that have instances of that typeclass.

As we have seen, a datatype declaration defines a type constructor
and data constructors. Data constructors are the values of a particular
type; they are also functions that let us create data, or values, of a
particular type.

typed lambda calculas. System F. Hindley-Milner system.

The arrow, (->), is the type constructor for functions in Haskell.

Functions are values.

The term sectioning specifically refers
to partial application of infix operators, which has a special syntax
and allows you to choose whether the argument you’re partially
applying the operator to is the first or second argument

In Haskell, polymorphism divides into two categories: parametric
polymorphism and constrained polymorphism.
