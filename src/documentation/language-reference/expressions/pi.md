---
title: Pi Types
toc: false
---

A Pi type is a type of (dependent) functions.
If {%ard%} p_1, ... p_n {%endard%} are named or unnamed parameters and {%ard%} B {%endard%} is a type, then {%ard%} \Pi p_1 ... p_n -> B {%endard%} is also a type.
If {%ard%} B {%endard%} does not refer to variables defined in the parameters, you can write {%ard%} A_1 -> ... A_n -> B {%endard%} instead, where
{%ard%} A_i {%endard%} are types of the parameters.
If {%ard%} A_i {%endard%} has type {%ard%} \Type p_i h_i {%endard%} and {%ard%} B {%endard%} has type {%ard%} \Type p h {%endard%}, then the type of the Pi type is {%ard%} \Type p_max h {%endard%},
where {%ard%} p_max {%endard%} is the maximum of {%ard%} p_1, ... p_n {%endard%}, and {%ard%} p {%endard%}.
Note that the homotopy level of the Pi type is simply the homotopy level of the codomain.

An expression of the form {%ard%} \Pi p_1 ... p_n -> B {%endard%} is equivalent to {%ard%} \Pi p_1 -> \Pi p_2 ... p_n -> B {%endard%}.
Moreover, if {%ard%} p_1 {%endard%} equals to {%ard%} (x_1 ... x_k : A) {%endard%}, then it is also equivalent to
{%ard%} \Pi (x_1 : A) -> \Pi (x_2 ... x_k : A) -> \Pi p_2 ... p_n -> B {%endard%}.

A _lambda parameter_ is either a variable or a named parameter.
If {%ard%} p_1, ... p_n {%endard%} is a sequence of lambda parameters and {%ard%} e {%endard%} is an expression of type {%ard%} E {%endard%}, then
{%ard%} \lam p_1 ... p_n => e {%endard%} is an expression which has type {%ard%} \Pi p_1 ... p_n -> E {%endard%}.
If some parameters do not have types, then they will be inferred by the typechecker.

If {%ard%} f {%endard%} is an expression of type {%ard%} \Pi (x : A) -> B {%endard%} and {%ard%} a {%endard%} is an expression of type {%ard%} A {%endard%}, then {%ard%} f a {%endard%} is an expression
of type {%ard%} B[a/x] {%endard%}.

An expression of the form {%ard%} (\lam x => b) a {%endard%} reduces to {%ard%} b[a/x] {%endard%}.

An expression of the form {%ard%} \lam x => f x {%endard%} is equivalent to {%ard%} f {%endard%} if {%ard%} x {%endard%} is not free in {%ard%} f {%endard%} (eta equivalence for Pi types).

## Implicit lambdas

An implicit lambda is written as {%ard%} __ {%endard%}.
When this expression appears inside some other expression, the outer expression is replaced with a lambda and {%ard%} __ {%endard%} is replaced with the argument of this lambda.
The outer expression is the closest ancestor of {%ard%} __ {%endard%} in which "it is possible to insert a lambda".
Lambda is never inserted in case arguments, anonymous class extensions, after {%ard%} \new {%endard%}, on the left of the function application, under {%ard%} \pi {%endard%} or {%ard%} \sigma {%endard%}, and under binary operators.

Examples:
* {%ard%} \case f __ \with { ... } == \lam x => \case f x \with { ... } {%endard%}
* {%ard%} __ a + __ \`f\` g __ (h __ 0) == \lam x y z => x a + y \`f\` g z (\lam t => h t 0) {%endard%}
* {%ard%} + (f __) __ == \lam x => + (\lam y => f y) x {%endard%}
* {%ard%} __ -> \Sigma __ __ == \lam A B C => A -> \Sigma B C {%endard%}
