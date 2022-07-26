---
title: "Language Reference"
description: "BAM! is a language based on the concept of streams and machines that transform streams."
lead: "BAM! is a language based on the concept of streams and machines that transform streams."
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 110
toc: true
---

BAM! files consist of a list of machine definitions. 

Machines are named stream transformers, that transform streams using other machines.

Every machine has exactly one (implicit) input stream called `input` and produces exactly one output stream.

## Basic stream manipulation

The simplest possible machine is one that simply passes its input stream on without performing any transformations.
```
machine IdentityMachine {
    input
}
```

To get more useful machines, streams can be transformed by piping them into other machines. For example, a machine that produces the square roots of the elements in its input stream might be written like this.

```
machine CustomSquareRoot {
  input -> Sqrt
}
```

`CustomSquareRoot` is now equivalent to `Sqrt`.

Of course, the output of such a pipe expression is still a stream, so pipes can be chained

```
machine FourthRoot {
  input -> Sqrt -> Sqrt
} 
```

Numeric and string literals evaluate to a constant, but infinite stream of their values.

```
machine ConstSqrt2 {
  2 -> Sqrt
}
```
`ConstSqrt2` ignores its input and always returns an infinite stream of `1.4142135623730951`.

## Combining streams
Some machines, e.g. `Add`, take streams of pairs. To create these, streams can be paired up, by joining them with a comma.

```
machine PlusFive {
  input, 5 -> Add
}
```

The combined streams can also be more complex stream expressions and parentheses can be used to specify the order of operations. `,` always has a higher precedence than `->`.

```
machine ComplexMaths {
  (input, 5 -> Add), (4, 1 -> Sub) -> Mul -> Sqrt
}
```

## Stream variables
Piping into machines and building pairs can already compute a lot, but this can quickly get unwieldy. 
Intermediate streams can be bound to variables via `let` statements. This makes it possible to split up code and give streams meaningful names.

```
machine LessComplexMaths {
  let x = (input, 5 -> Add);
  let y = (4, 1 -> Sub);
  x, y -> Mul -> Sqrt
}
```

Let expressions are able to bind more than a single variable. In this case, they expect the stream to return a tuple and destructure the stream, by splitting it up into distinct streams for input elements. 

This is comparable to tuple destructuring in other languages, though an important difference is that the destructured streams might be drained at different times.

```
machine Destructuring {
  let input1, input2 = input -> Dup2; // Dup2 : ∀a. a -> (a, a)
  (input1, 1 -> Add), (input2, 1 -> Sub) -> Add
}
```

Duplicating values with `Dup2` and then destructuring them might seem pointless, but the reason this is necessary is that streams must be used linearly, i.e. the same stream can never be used twice. (This is currently **not** checked!)

In order to use a stream twice, you have to pass it to `Dup2` or `Dup3` and destructure the resulting stream.

## Conditionals
Of course, no useful programming language is complete without branching.

Conditionals are achieved via `stream ? stream : stream` expressions, similar to the ternary conditional operator in many C-inspired languages. 

```
machine TotallyNonOverusedExample {
  let age = input;
  (age, 18 -> Le) 
    ? "Too young" 
    : "Ok"
}
```

Importantly, this operator first drains an element from the first stream and, depending on this, drains an element from **only one** of the other streams. This distinction is crucial if one of the stream produces side effects (e.g. if it uses `Print` or `Read`).

## Recursion
Machines can be used in their own definition. You can imagine this as a machine feeding one of its output conveyor belts back into its input.

```
machine Fib {
  let input_check, input_rec1, input_rec2 = input -> Dup3;

  (input_check, 2 -> Lt)
      ? 1
      : ((input_rec1, 1 -> Sub -> Fib), (input_rec2, 2 -> Sub -> Fib)) -> Add

}
```
