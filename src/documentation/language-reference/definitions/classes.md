---
title: Classes
toc: false
---

A _class_ is a record that has several useful properties.
A class is defined in the same way as a record, but it begins with the keyword {%ard%} \class {%endard%} instead of {%ard%} \record {%endard%}.
Classes can extend other classes and records and vice versa.
The first explicit parameter of a class is called its _classifying field_.
Classes are implicitly coerced to the type of the classifying field.
Let us consider an example:

{% arend %}
\class Semigroup (E : \Set)
  | \infixl 7 * : E -> E -> E
  | *-assoc (x y z : E) : (x * y) * z = x * (y * z)

\func square {S : Semigroup} (x : S) => x * x
{% endarend %}

It is allowed to write {%ard%} x : S {%endard%} instead of {%ard%} x : S.E {%endard%} since {%ard%} S {%endard%} is implicitly coerced to an element of type {%ard%} \Set {%endard%}, that is {%ard%} S.E {%endard%}.

In case a parameter of a definition has type, which is an [extension](../expressions/class-ext) {%ard%} C { ... } {%endard%} of {%ard%} C {%endard%},
it is marked as a _local instance_ of class {%ard%} C {%endard%}. Implicit parameter {%ard%} \this {%endard%} of a field of class {%ard%} C {%endard%} is also a local instance. 

If a parameter {%ard%} p : C { ... } {%endard%} of a definition {%ard%} f {%endard%} is a local instance, then the corresponding implicit argument of {%ard%} f {%endard%} can be inferred by the instance inference algorithm.
It looks for the first available instance {%ard%} v : C { ... } {%endard%} such that the expected value of the classifying field coincides with the value of the classifying field in {%ard%} v {%endard%}. 
For instance, the function {%ard%} square {%endard%} in the example above has one local instance {%ard%} S {%endard%} of the class {%ard%} Semigroup {%endard%}.
The field {%ard%} * {%endard%} of the class {%ard%} Semigroup {%endard%} is used in the body of {%ard%} square {%endard%} and the expected type of its call in {%ard%} x * x {%endard%} is 
{%ard%} S.E -> S.E -> {?} {%endard%}. This implies that the expected value of the classifying field is {%ard%} S.E {%endard%},
which coincides with the value of the classifying field in {%ard%} S {%endard%}, so this instance is inferred as the implicit argument of {%ard%} * {%endard%}.

## Classifying fields

The classifying field can be specified explicitly with keyword {%ard%} \classifying {%endard%} as follows:

{% arend %}
\class C (A : \Set) (\classifying B : \Set)
  | f : B -> A

\func test {c : C} (x : c) => f x
{% endarend %}

Since field {%ard%} B {%endard%} is marked as classifying, the type of {%ard%} x {%endard%} is {%ard%} c.B {%endard%} and the instance for {%ard%} f {%endard%} will be successfully inferred.

It is also possible to define a class without classifying fields.
If the class has some explicit parameters, then this can be done using keyword {%ard%} \noclassifying {%endard%}, which is written after the name of the class.

## Instances

It is also possible to define _global instances_.
To do this, one needs to use the keyword {%ard%} \instance {%endard%}:

{% arend %}
\instance NatSemigroup : Semigroup Nat
  | * => Nat.+
  | *-assoc (x y z : Nat) : (x Nat.+ y) Nat.+ z = x Nat.+ (y Nat.+ z) \elim z {
    | 0 => idp
    | suc z => pmap suc (*-assoc x y z)
  }
{% endarend %}

An implementation of a field can be defined by pattern matching as demonstrated above.
In this case, a separate function with the same name as the field is created in the namespace of the instance.

A global instance is just a function, defined by [copattern matching](functions#copattern-matching).
It can be used as an ordinary function.
The only difference with an ordinary function is that it can only be defined by copattern matching and must define an
instance of a class.
Also, the classifying field of an instance must be a data or a record applied to some arguments and if some parameters
of the instance have a class as a type, its classifying field must be an argument of the classifying field of the 
resulting instance.

If there is no local instance with the expected classifying field, then such an instance will be searched among
global instances.
There is no backtracking, so the first appropriate instance will be chosen.
A global instance is appropriate in a usage if the expected classifying field is the same data or record
as the data or the record in the classifying field of the instance. If this holds, the global instance
is appropriate even if the data or the record are applied to different arguments.
