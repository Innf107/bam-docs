---
title: "Builtin Reference"
description: "There are a few builtin machines that are defined inside the interpreter."
lead: "There are a few builtin machines that are defined inside the interpreter."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 130
toc: true
---

## Add : (num, num) -> num
Adds pairs of numbers from the input stream

## Sub : (num, num) -> num
Subtracts pairs of numbers in the input stream

## Mul : (num, num) -> num
Multiplies pairs of numbers in the input stream

## Div : (num, num) -> num
Divides pairs of numbers in the input stream

## Mod : (num, num) -> num
Computes the modulus of pairs of numbers in the input stream

## Pow : (num, num) -> num
Computes powers of pairs of numbers in the input stream

## Sqrt : num -> num
Computes the square roots of numbers in the input stream

## Gt : (num, num) -> bool
Produces 'true' in the output stream if the first element of a pair in the input stream is larger than the second element.

## Lt : (num, num) -> bool
Produces 'true' in the output stream if the first element of a pair in the input stream is smaller than the second element.

## Eq : ∀a. (a, a) -> bool
Produces 'true' in the output stream if both elements of a pair in the input stream are equal.

## And : (bool, bool) -> bool
Computes the logical 'and' of boolean pairs in the input stream

## Or : (bool, bool) -> bool
Computes the logical 'or' of boolean pairs in the input stream

## Not : bool -> bool
Negates booleans in the input stream

## Dup2 : ∀a. a -> (a, a)
Duplicate each element in the input stream.
This is usually meant to be used in combination with `let` destructuring. E.g.

```
let input_copy1, input_copy2 = input -> Dup2
```

## Dup3 : ∀a. a -> (a, a, a)
Similar to `Dup2`, but produces tuples of three elements.

## Print : ∀a. a -> a
Prints input values to `stdout` and passes them on unchanged

## Read : ∀a. a -> String
Ignores its input and asks for user input on the console.
