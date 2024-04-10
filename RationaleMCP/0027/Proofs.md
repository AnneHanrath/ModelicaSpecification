# Unit checking proofs and properties

Document is very WIP.



This document is aimed to underly some theoretical properties, which can be applied to the unit checking proposal in the same folder.


It will show a linear algebra way of thinking about unit checking and show properties which a unit checking algorithm should fullfil.


## Consistency

If one wants to talk about algorithm, which determine consistency of a model with regards to units, one first needs to talk what one considers consistent in the context.

(**Definition "Consistency"**

Let `Cons` be a set of unit constraints derived of a Modelica model. Let `UnitVars` be the set of unit variables which are contained within `Cons`.

Let there be an assignment `S^*(Cons)`, such that for every `x.unit \in UnitVars` we can assign an expression which is a product of `well--formed` units to `x.unit`.

`Cons` is called consistent, if every constraint have equivalent (well-formed) units on both sides after application of the assignment.
)

In the context of this document, a different property to determine consistency is used, therefore it is needed to show that those properties are equivalent.

(**Lemma 1**  
 Let `Cons` be a consistent set of contraints. Let `S^*(Cons)` be an assignment according to Definition 1. If we transform the constraints into single--sided form, each contraint `c \in Cons` is equivalent to `"1"`.

Proof

A constraint of not single--side form has a `well--formed` right hand side unit which is identical to the equally `well--formed` left hand side unit. As the transformation of the unit into a single--side form is to divide the right hand side unit by the left hand side unit the units cancel out and a `"1"` remains.
)

(
**Lemma 2**

Let `Cons` be a set of constraints in single--sided form. Let `S^*(Cons)` be an assignment such that each contraint in single--sided Form is equivalent to `"1"`. Then `Cons` is consistent.

Proof

Apply the Assignment to every contraint in `Cons`. Assuming that there would exist one `c \in Cons` for which the constraints evaluates to false, then we transform this contraint into single--sided form and see that it can not be equivalent to `"1"`. Therefore this assumption can not be true.
)

With this we can use the equivalence to `"1"` as definition of consistency, which will come in handy in the next chaper.

## A linear algebra look onto consistency of constraints

Let `Cons = {c_1, ..., c_k} for k \in \N` be a set of unit constraints derived of a Modelica model in single--sided Form (A). To find out, if `Cons` is consistent, one needs to find an assignment `S`, such that using the assignment, each constraint is equivalent to `"1"` (see [Lemma 1](#Lemma 1) and [Lemma 2](#Lemma 2) ).

Excluding the `der(...)`-Operator, all operations within Modelica will lead to multiplication or division of units when creating unit constraints - a constraint in single--sided Form is always a product of unit variables with well--formed units if the set is consistent.

To check if each constraint is equivalent to `"1"`, means to check that the units used within the constraint cancel each other out. In other words, we need to add and subtract rational unit exponents of each unit in the constraint.

Let `UnitVars` be the set of unit variables which are contained within `Cons`. For simplicity reasons, we say `UnitVars` contains `n\in \N` unit variables which are called `x_1, ..., x_n`. (B)

Let `UnitBase` be all modelica base Units, meaning it does not contain units which are equivalent to each other or scaled. Let there be `m \in \N` numbers of units in `UnitBase` which we can count `u1, ...,u_m`. (C)

(Remark: if we only use "true" base units meaning, that we are excluding equivalent, convertible or transformable base units, we do no longer talk about equivalence to `"1"` but about equality to "1".)

Let `p_{i,j} \in \Q` be the exponent of `x_i` in `c_j` (i from 1 to n, j from 1 to k) and `q_{i,j}` be the exponent of `u_i` in `c_j` (i from 1 to m, j from 1 to k). (D)

Each single--sided constraint `c_i` can then be expressed as

`\Prod_{i=1}^n x_i^{p_{i,j}} \cdot \Prod_{i=1}^m u_i^{q_{i,j}}`.

If this is equivalent to `"1"`, this means that

`\Sum_{i=1}^n p_{i,j} + \Sum_{i=1}^m q_{i,j}  = 0` (1)

for all `1 < j < k`.

This is a system of linear equation over rational numbers

### properties of linear equation system with rational numbers

When evoking something like Gauss Elimination to (2) any elimination step will only be using a rational factor for multiplication. Therefore, one never will obtain a factor in the system, which is not rational.

### **Lemma 3**

(
Let (A), (B), (C), (D)

with the resulting equation system

`\Sum_{i=1}^n p_{i,j} + \Sum_{i=1}^m q_{i,j}  = 0      1 < j < k` (2)

1. If (2) has no solution, `Cons` is inconsistent, and it it not possible to construct a valid assignment.
2. If (2) has one solution or an infinite amount of solutions, it is possible to construct a valid assigment
)

#### Proof

First show that we can not change anything about consistency of a system, when we use Gauss elimination in this context.

TODO

scetch of the remaining proof:

Secondly, invoke Gauss elimination. One obtains the following system after complete substitution

| unit vars  | units  |        |
|------------|--------|--------|
|1 0     ...0|        |        |  
|0 1 0   ...0|        |        |
|    ...     |    A   |    0   |
|0  ... 0 1 0|        |        |
|0     ...0 1|        |        |
|------------|--------|--------|
|            |        |        |
|     0      |    B   |    0   |
|            |        |        |


##### Proof of 1.

B contains rows which are not zero rows.

A consistent set of constrains fullfills that each equation is zero with (1). So the set can not be consistent.

And each assignment of any unit variables can not change anything about those lines. Therefore the second part is also not true



##### Proof of 2.

a) the linear equation has one unique solution

B is equal to 0 and the upper left block is of sixe `n x n`. Therefore, each unit variable can be assigned to the product of units with entries in A in the same row. If there are no entries in the row, the empty unit is assigned. Thus, an consistent assignment is constructed

b) the linear equation has infinite solutions

B is either equal to 0 or does not exist and the left block has more columns than rows. In that case, one can add a new line for any missing unit variable and assign it to the empty unit. Then one has a consistent assignment.


