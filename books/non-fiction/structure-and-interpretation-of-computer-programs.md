# Structure and Interpretation of Computer Programs

Harold Abelson, Gerald Jay Sussman, Julie Sussman

## 1. Building Abstractions with Procedures

### 1.1 The Elements of Programming

Goal of PL is to combine simple ideas to represent more complex ideas. 3
mechanisms for doing so:

* *primitive expressions* represent most simple entities of the lang
* *means of combination* by which compound elements are built from simpler ones
* *means of abstraction* by which compound elements can be named and manipulated
  as units

In programming, 2 types of elements: procedures and data (but actually they're
not so distinct as we might immediately think).

#### 1.1.1 Expressions

Expressions like `(+ 1 2)` and `(/ 10 5)` are *combinations*. Leftmost element
is called the *operator* and the other elements are called the *operands*.

*Prefix notation* has a number of benefits, including the fact that it can take
an arbitrary number of arguments. Second because it allows for nested
combinations:

```scheme
(+ (3 * 5) (- 10 6))
19
```

The interpreter always behaves the same way, even for complex expressions: read
expression from terminal, evaluate the expression, print the result (Read
Evaluate Print Loop).

#### 1.1.2 Naming and the Environemnt

Critical part of PL is how to refer to computational objects. Scheme uses
`define`:

```scheme
(define my-name "tara")
```

This is the simplest abstraction in a lang. It provides a way to refer to the
results of compound operations.

#### 1.1.3 Evaluating Combinations

Evaluating combinations is in itself a procedure. So to evaluate a combination,
the interpreter must:

1. Evaluate the subexpressions
2. Apply the procedure that is the value of the leftmost subexpression (the
   operator) to the args that are the values of the other subexpressions (the operands)

Notice that these two steps are recursive. To evaluate a comibnations requires
invoking this process to evaluate subexpressions.

When evalutating nested subexpressions, one eventually ends up with primitive
expressions like numerals, operators, etc. We handle those by saying:

* numerical values are the number that they name
* values of built-in operators are machine instruction sequences that carry out
  corresponding operations
* values of names are objects associated with the name in that environment

*Special forms* (like `define`) do not follow these evaluation rules.

#### 1.1.4 Compound Procedures


