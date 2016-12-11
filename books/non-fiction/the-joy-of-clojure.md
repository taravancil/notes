# The Joy of Clojure - Michael Fogus, Chris House

## 1. Clojure Philosophy

### 1.1 The Clojure Way

Rich Hickey had a few goals in mind when designing Clojure:

#### 1.1.1 Simplicity

Not only does Clojure provide tools for stripping away complexity, but since
it's not object-oriented, it avoids the built in complexity of OO languages
requiring layering of class defs, inheritance, and type defs. Clojure cuts
through this by using pure functions, which simply take arguments and return a
value.

#### 1.1.2 Freedom to Focus

If a lang requires you to think a lot about syntax, operator precedence, or
inheritance, that is a distraction.

Clojure allows you to redefine your code on the fly, even when your program is
running, which provides an interesting and rapid model for programming.

#### 1.1.3 Empowerment

Clojure wasn't designed to be didactic or just to explore a corner of computing
theory. It was meant to be practical.

The decision to use JVM reflects the design goal of being a practical language.
It does have a slow bootup time, but it's fast, mature, and widely supported.

#### 1.1.4 Clarity

Clojure aggressively separates concerns:

Other Languages | Clojure
--- | ---
Object with mutable fields | Values from identities
Class acts as namespace for methods | Function namespaces from type namespaces
Inheritance hierarchy of classes | Hierarchy of names from data and functions
Data and methods bound lexically | Data objects from functions
Method impls embedded through class inheritance | Interface declarations from
function declarations

#### 1.1.5 Consistency

Aims to provide consistency of syntax and data structures. E.g. `doseq` and
`for` macros share syntax even though they do different things.

### 1.2 Why Another Lisp?

#### 1.2.1 Beauty

All of computation can be defined with only seven functions and two special
forms: `atom`, `car`, `cdn`, `cond`, `cons`, `eq`, `quote`, `lambda`, and
`label` (thanks, [John
McCarthy](http://www-formal.stanford.edu/jmc/recursive/recursive.html)).

#### 1.2.2 Extreme Flexibility

Lisps make writing macros trivial.

#### 1.2.3 Code is Data

When a language is represented as its own inherent data structures, the language
itself can manipulate the its own structure and behavior (Graham 1995).

### 1.3 Functional Programming

#### 1.3.1 A Workable Definition of Functional Programming

For a language to be functional, its functions must be first class, that is they
must be able to be stored, passed, and returned like any other piece of data.

#### 1.3.2 The Implications of Functional Programming

OO programmers often approach a problem by defining a set of nouns (classes),
functional programmers sees the solution as a composition of verbs (functions).
Functional solution will be more succinct, understandable, and reusable.

### 1.4 Why Clojure Isn't Especially Object-Oriented

It was born largely out of frustration of concurrent programming, complicated by
weaknesses of OO in facilitating it.

#### 1.4.1 Defining Terms

*time* - the relative moments when events occur
*identity* - the properties associated with an entity
*state* - a snapshot of an entity's properties

In OO, there's no distinction between state and identity.


