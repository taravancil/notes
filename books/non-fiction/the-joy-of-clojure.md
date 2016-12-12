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

**time** - the relative moments when events occur
**identity** - the properties associated with an entity
**state** - a snapshot of an entity's properties

In OO, there's no distinction between state and identity (mutable state...which
is an oxymoron).

#### 1.4.2 Imperative Baked In

**imperative programming** - a sequence of statements mutates program state.

Most common model for IP is OO, but by allowing unrestrained mutation via
variabes, the imperative model doesn't directly support concurrency.

#### 1.4.3 Much of What OOP Offers, Clojure Provides

##### Polymorpism and the Expression Problem

**polymorphism** - the ability of a function or method to have different
definitions depending on the type of the target object

Clojure provides polymorphism via both multimethods and protocols

```clojure
(defprotocol Concatenable
  (cat [this other]))

(extend-type String
  Concatenatable
  (cat [this other]
    (.concat this other)))

(cat "House" " of Leaves")
;=> "House of Leaves"
```

This defines a protocol `Concatenable` that groups functions that define the set
of functions provided (in this case `cat`). We then extend the protocol to the
`String` class and define the implementation. We can also extend this to another
type:

```clojure
(extend-type java.util.List
  Concatenable
  (cat [this other]
    (concat this other)))

(cat [1 2 3] [4 5 6])
;=> (1 2 3 4 5 6)
```

**The Expression Problem** - Implementing an existing set of abstract methods for
an existing concrete class requires having to change the code for either. Some
dynamic languages provide a partial solution to the problem via *monkey
patching*.

##### Encapsulation

`defn` macro provides namespace private functions.

## 2. Drinking from the Clojure Firehose

### 2.1 Scalars

#### 2.1.1 Numbers

Can consist of:

* digits 0-9
* a decimal point
* a sign (+/-)
* *e* to represent exponential notation
* *M* which flags arbitrary precision

#### 2.1.2 Integers

Can take an infinitely large vaule, limited by memory ofc.

```clojure
42
+9
-107
```

```clojure
; 127 represented with decimal, hexadecimal, octal, radix-32, and binary
literals
[127 0x7F 0177 32r3V 2r01111111]
```

#### 2.1.3 Floating-point Numbers

Like integers, are arbitrarily precise. Can be represented in traditional form
(`1.3`) or with exponential form (`1.3e3`).

#### 2.1.4 Rationals

Represented by an integer numerator and denominator. Are more compact and
precise representation of a given value over floating-point.

`25/5` is resolved to the integer 5.

```
22/7
7/22
-103/4
```

#### 2.1.5 Symbols

Often used to represent another value. When a symbol is evaluated, you get
whatever that symbol refers to in the current context.

#### 2.1.6 Keywords

Similar to symbols, except they always evaluate to themselves. Used more often
than symbols in Clojure.

```clojure
:a
:2
:KeywordName
```

The `:` is part of the literal syntax, not part of the keyword name.

#### 2.1.7 Strings

Pretty much the same as other PLs.

#### 2.1.8 Characters

Prefixed with a backslash and stored as Java Character objects

```clojure
\a      ; lowercase a
\u0042  ; unicode character uppercase B
\\      ; backslash character
```

### 2.2 Collections

#### 2.2.1 Lists

Written with parentheses like in other Lisps:

```clojure
(yankee hotel foxtrot)
```

Like in other lists, a list will be evaluated if not `quote`d.

`()` != `nil`

#### 2.2.2 Vectors

Similar to lists, but unlike lists, no function or macro call is performed on
the elements. Instead each item is evaluated in order.

Vector types are heterogeneous. `[]` != `nil`.

#### 2.2.3 Maps

```clojure
{1 "one", 2 "two", 3 "three"}
```

Commas are optional, just represent whitespace. Like vectors, every item in the
map is evaluated before the result is stored in the map. Unlike vectors, he
order in which they're evaluated isn't guaranteed. Can have items of any type
for both keys and values. `{}` != `nil`.

#### 2.2.4 Sets

```clojure
#{1 2 "three" :four 0x5}
```

`#{}` != `nil`

### 2.3 Functions

Clojure has first-class functions, which means they can be used as any value,
i.e. stored in variables, held in collections, passed as arugments,
returned as the result of a function.

#### 2.3.1 Calling Functions

Uses **prefix notation**:

```clojure
(+ 1 2 3)
;=> 6
```

The advantage of prefix over infix notation is that it allows any number of
operands per operator. Also makes no distinction btwn operator notation and
regular function calls, which provides incredible flexibility.

#### 2.3.2 Defining Functions

**special form** - an expression that's part of the core language, but not
created in terms of functions, types or macros

An anonymous function can be defined as a special form:

```clojure
(fn mk-set [x y] #{x y})
```

^ `mk-set` symbol is optional and is not a globally accessible name for the
function, rather an internal name that the function can use to reference itself.

**arity** - differences in argument count that a function will accept

We can change `mk-set` to take either one or two arguments:

```clojure
(fn
  ([x]  #{x})
  ([x y} #{x y}]))
```

Taking a variable number of arguments:

```clojure
((fn arity [x y & z] [x y z]) 1 2)
;=> [1 2 nil]
((fn arity [x y & z] [x y z]) 1 2 3 4)
;=> [1 2 (3 4)]
((fn arity [x y & z] [x y z]) 1)
;=> wrong number of args error
```

#### 2.3.3 Simplifying Functions with `def` and `defn`

`def` assigns a symbolic name to a piece of Clojure data. Since functions are
data (first-class objects) in Clojure, we can assign them to a name:

```clojure
(def make-set
  (fn
    ([x] #{x})
    ([x y] #{x y})))

(make-set 1 2)
;=> #{1 2}
```
`defn` is a macro and is a much less cumbersome way to define functions than `def`.

```clojure
(defn make-set
  "This is a docstring"
  ([x]    #{x})
  ([x y]  #{x y}))

(make-set 1 2)
;=> #{1 2})
```

#### 2.3.4 In-place Functions with `#()`

**reader feature** - analogous to preprocessor directives in that they signify
that some given form should be replaced with another at read time.

Shorthand notation for creating an anonymous function using the `#()` reader
feature. `#()` is replaced with the special form `fn` and they can be used
interchangably.

`#()` can accept arguments that are implicitly declared through the use of
special symbols prefixed with %:

```clojure
(def make-list_ #(list %))
(def make-list1 #(list %1))
(def make-list2 #(list %1 %2))
(def make-list3 #(list %1 %2 %3))
(def make-list3+ #(list %1 %2 %3 %&))

(make-list_ 1)
;=> (1)

(make-list-3+ 1 2 3 4 5)
;=> (1 2 3 (4 5))
```

### 2.4 Vars

Named by a symbol and holds a single value. Its value can be changed while
program is running. Can be shadowed by a thread local value, but doesn't change
original value or root binding.

#### 2.4.1 Declaring Bindings using `def`

**root binding** - a binding thats' the same across all threads, unless
otherwise rebond relative to specific threads

This binds the value 42 to the symbol `x`:

```clojure
(def x 42)
```

Vars don't require a value. We can declare them then leave binding to individual
threads.

### 2.5 Locals, Loops, and Blocks

#### 2.5.1 Blocks

`do` is for treating a series of expressions as one. All expressions evaluated,
last returned:

```clojure
(do
  6
  (+ 5 4)
  3)
;=> 3
```

#### 2.5.2 Locals

Scope is defined using a `let` form, which starts with a vector that defines the
bindings.

```clojure
(let [r 5
      pi 3.1415
      r-squared (* r r)]
  (println "radius is" r)
  (* pi r-squared))
```

The body of the let is "implicit-do" because it behaves the same way: all
expressions will be evaluated, but only the last one is returned.

#### 2.5.3 Loops

Done with recursive calls.

**tail recursion** - TODO

##### Recur

```clojure
(defn print-down-from [x]
  (when (pos? x)
    (println x)
    (recur (dec x))))

(print-down-from 3)
;=> 3\n2\n1
```

This is similar to how looping is done in imperative PLs, except `recur` does
two things:

1. Rebinds `x` to the new value
2. returns control to the top of print-down-from

If the function has multiple arguments, `recur` call must as well. Just like
with a function call, the expressions in the `recur` call are evaluated in order
first adn only bound to the function args simultaneously.

A recursive accumulator:

```clojure
(defn sum-downn-from [sum x]
  (if (pos? x)
    (recur (+ sum x) (dec x))
    sum))
```

Notice that these two loops used different conditional blocks. Typically, use
`when` when:

* No else-part is associated with the result of the conditional
* You require an implicit `do` in order to perform side effects

##### Loop

Sometimes you want to loop to a point inside a function, not to the top. `loop`
acts like let, except it provides a target for `recur` to jump to:

```clojure
(defn sum-down-from [initial-x]
  (loop [sum 0, x initial-x]
    (if (pos? x)
      (recur (+ sum x) (dec x))
      sum)))
```

The locals `sum` and `x` are initialized, just like they would be with `let`.
The `recur` always loops back to the closest `loop` or `fn`, so here it goes to
`loop`. The loop locals are rebound to the values given in `recur`.

##### Tail Position

**tail position** - a form is in he tail position of an expression when its
value may be the return value of the whole expression.

`recur` can only be used in the tail position of a function or `loop`.

Here the `if` form is in the tail position, because whatever is returned the
whole function will return. The x in the `then` clause is also in tail position, but
the x in the `else` clause is not, because when evauluated it goes to the `-` function, it does not return directly. But the whole `else` clause is in the tail position.

```clojure
(defn absolute-value [x]
  (if (pos? x)
    x
    (- x)))
```

### 2.6 Preventing Things from Happening: Quoting

#### 2.6.1 Evaluation

When collection evaluated, each expression in the collection is evaluated first.
So when you do `(cons 1 [2 3])`, the whole form is evaluated.

Scalar values evaluate to themselves. Vectors evaluate by first evaluating each
item. Because they're literal scalars, that's all well good. But lists are
different, they call functions.

#### 2.6.2 Quoting

`quote` special form prevents its arguments from being evaluated:

```clojure
(def tena 9)
(quote tena)
;=> tena
```

^ Instead of evalutating to the value of `tena`, it evaluates to the symbol
`tena`. This works for vectors, maps, lists, function calls, and special forms.

Like other Lisps, Clojure provides a shorthand for `quote` in the form `'`, but
it's not as commonly used in Clojure in other Lisps.

*Notice that `()` already evaluates to itself. `quote`ing an empty list is not
idiomatic.*

##### Syntax-Quote

Like `quote`, prevents its argument and subforms from being evaluated. Written
with a single back-quote.

**qualified symbols** - begin with a namespace and a forward slash
(`clojure.core/map`)

Syntax-quote automatically qualifies all unqualified symbols:

```clojure
`map
;=> clojure.core/map
`Integer
;=> java.lang.Integer
```

#### 2.6.3 Unquote

Allows you to mark that some forms require evaluation within the body of
syntax-quote:

```clojure
`(+ 10 (* 3 2))
;=> (+ 10 (* 3 2))
`(+ 10 ~(* 3 2))
;=> (clojure.core/+ 10 6)
```

#### 2.6.4 Unquote-Splicing

```clojure
(let [x '(2 3)] `(1 ~x))
;=> (1 (2 3))
(let [x '(2 3)] `(1 ~@x))
;=> (1 2 3)
```

^ The `@` tells Clojure to unpack the sequence `x`, splicing it into the
resulting list rather than inserting it as a nested list.

#### 2.6.5 Auto-gensym

Generates a new unqualified symbol:

```clojure
`potion#
;=> potion__211__auto__
```

### 2.7 Leveraging

Leveraging Java via Interop

Clojure is symbiotic with its host, the JVM.

#### 2.7.1 Accessing Static Class Members

```clojure
java.util.Locale/JAPAN
;=> #<Locale> ja_JP>
```

#### 2.7.2 Creating Java Class Instances

```clojure
(new java.util.HashMap {"foo" 42 "bar" 9 "baz" "quux"})
;=> #<HashMap> {baz=quux, foo=42, bar=9}>
; This is idiomatic:
(java.util.HashMap. {"foo" 42 "bar" 9})
```

#### 2.7.3 Accessing Java Instance Members with the . Operator

Access a property:

```clojure
(.x (java.awt.Point. 10 20))
;=> 10
```

Access a method:

```clojure
(.divide (java.math.BigDecimal. "42") 2M)
;=> 21M
```

#### 2.7.4 Setting Java Instance Properties

Java instance properties can be set with `set!` function:

```clojure
(let [origin (java.awt.Point. 0 0)]
  (set! (.x origin) 15)
  (str (origin))
;=> "java.awt.Point[x=15,y=0]"
```

#### 2.7.5 The .. Macro

To mimic how chaining method calls works in Java, we can use Clojure's . special
form:

```clojure
(.endsWith (.toString) (java.util.Date.)) "2016")
;=> true
```

But this is difficult to read, so Clojure has .. macro:

```clojure
(.. (java.util.Date.) toString (endsWith "2016"))
;=> true
```

#### 2.7.6

The `doto` Macro

Sometimes need to do a bunch of mutations on a fresh instance:

```clojure
(doto (java.util.HashMap.)
  (.put "HOME" "/home/me")
  (.put "SRC" "src"))
```

### 2.8 Exceptional Circumstances

#### 2.8.1 A Little Pitch and Catch

```clojure
(throw (Exception. "I done throwed"))
```

Catching exceptions:

```clojure
(defn throw-catch [f]
  [(try
    (f)
    (catch ArithmeticException e "No dividing by zero!")
    (catch Exception e (str "You are so bad " (.getMessage e)))
    (finally (println "Returning... ")))])
(throw-catch #(/ 10 5))
;=> [2]
(throw-catch / 10 0)
;=> ["No dividing by zero"]
```

### 2.9 Namespaces

#### 2.9.1 Creating Namespaces using `ns`

Straightforward:

```clojure
(ns tara)
```

#### 2.9.2 Loading Namespaces with `:require` Directive

```clojure
(ns joy.req
  (:require [clojure.set :as s]))

(s/intersection #{1 2 3} #{3 4 5})
;=> #{3}
```

#### 2.9.3 Loading and Creating Mappings with `:use`

TODO

#### 2.9.4 Creating Mappings with `:refer`

TODO

#### 2.9.5 Loading Java Classes with `:import`

TODO

## 3. Dipping Our Toes in the Pool

### 3.1 Truthiness

#### 3.1.1 Truthiness in Clojure

Everything except `false` and `nil` are `true` in Clojure. Clojure opts for this
because branches in a program's logic are already ripe for complexity and bugs.

#### 3.1.2 Don't Create Boolean Objects

```clojure
(def evil-false (Boolean. "false"))
```

^ Creates new instance of Boolean, which is totally unnecessary.

#### 3.1.3 `nil` vs. `false`

It's rare that this is necessary, but you can do it with `nil?` and `false?`

### 3.2 Nil Pun with Care

Since empty collections evaluate to `true`, we need to be able to tell if we're
working with an empty collection.

```clojure
(seq [1 2 3])
;=> (1 2 3)

(seq [])
;=> nil
```

In Common Lisp, an empty list acts as a `false` value and can be used as a
*pun* in terminating loop. In Clojure `seq` is used as a terminating condition:

```clojure
(defn print-seq [s]
  (when (seq s)
    (prn (first s))
    (recur (rest s))))

(print-eq [1 2])
; 1
; 2
;=> nil
```

### 3.3 Destructuring

#### 3.3.1 Your Assignment, Should You Choose to Accept It

Let's write a Rolodex. Take a vector of length 3 that represents a person's
first, middle, and last names and return a string that will sort in the normal
way, like "Steele, Guy Lewis".

The annoying way:

```
(def guys-whole-name ["Guy" "Lewis" "Steele"])

(str (nth guys-whole-name 2) ", "
     (nth guys-whole-name 0) " "
     (nth guys-whole-name 1)))
;=> "Steele, Guy Lewis"
```

#### 3.3.2 Destructuring with a Vector

```clojure
(let [[f-name m-name l-name] guys-whole-name]
  (str l-name ", " f-name " " m-name))
```

We can also use `&` to collect the rest of the values into a sequence.

```clojure
(let [[a b c  more] (range 10)]
  (prn "a b c are: " a b c)
  (prn "more is: " more))

;=> a b c are: 0 1 2
;=> more is: (3 4 5 6 7 8 9)
;=> nil
```

Finally, the `:as` directive is used to bind the value of the whole vector to a
symbol.

```clojure
(let [range-vec (vec (range 10))
      [a b c & more :as all] range-vec]
  (println "all is: " all))
;=> all is: [0 1 2 3 4 5 6 7 8 9]
```

#### 3.3.3 Destructuring with a Map

Maybe three part vector wasn't the appropriate data structure. Let's try a map:

```clojure
(def guys-name-map
  {:f-name "Guy" :m-name "Lewis" :l-name "Steele"})

(let [{f-name :f-name m-name :m-name l-name :l-name} guy-name-map]
  (str l-name ", " f-name " " m-name))
```

Rewritten with `:keys`:

```clojure
(let [{:keys [f-name m-name l-name]} guys-name-map]
  (str l-name ", " f-name " " m-name))
```
#### 3.3.4 Destructuring in Function Parameters

Each function paramater can destructure a map or sequence:

```clojure
(defn print-last-name [{:keys [l-name]}]
  (println l-name))

(print-last-name guys-name-map)
; Steele
;=> nil
```

### 3.4 Using the REPL to Experiment

This is a *very good* section and worth revisiting. -tbv

#### 3.4.1 Experimenting with `seqs`

Color every pixel of a canvas with the xor of its x and y coords.

Let's get the coordinates:

```clojure
(for [x (range 2)] y (range 2)] [x y])
;=> ([0 0] [0 1] [1 0] [1 1])
```

**Clojure provides `find-doc` for searches functions and doc strings**

```clojure
(defn xors [max-x maxy-y]
  (for [x (range max-x) y (range max-y)]
    [x y (bit-xor x y)]))
```

#### 3.4.2 Experimenting with Graphics

```clojure
(def frame (java.awt.Frame.))
```

We have a frame, but it's hidden. Let's find out what methods are available to
make it visible:

```clojure
(for [method (seq (.getMethods java.awt.Frame))
        :let [method-name (.getName method)]
        :when (re-find #"Vis" method-name)]
  method-name)
;=> ("setVisible" "isVisible")
```

Let's set the frame to visible:

```clojure
(.setVisible frame true)

; then udate its size
(.setSize frame (java.awt.Dimension. 200 200))
;=> nil

;get the graphics context
(def gfx (.getGraphics frame))

; draw a rectangle
(.fillRect gfx 100 100 50 75)

; colorize
(.setColor gfx (java.awt.Color. 255 128 0))
(.fillRect gfx 100 150 75 50)
```

Now let's combine the `xors` macro we wrote earlier to use it to draw colors:

```clojure
(doseq [[x y xor] (xors 200 200)]
  (.setColor gfx (java.awt.Color. xor xor xor))
  (.fillRect gfx x y 1 1))
```

Ok, now try the same `doseq` but with (500 500) as the args ffor `xors`. You get
an error about the Color parameter being outside of the expected range. When an
error is thrown, the result is stored in a Var named `*e`.

```clojure
(.printStackTrace *e)
; ...
```

Update to keep values under 256

```clojure
(defn xors [xs ys]
  (for [x (range xs) y (range ys)]
    [x y (rem (bit-xor x y) 256)]))
```

