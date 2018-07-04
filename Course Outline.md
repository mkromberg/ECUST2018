# Array-oriented Functional Programming with Dyalog
## Workshop at East China University of Science and Technology
## July 5th, 2018
Materials available at [https://github.com/mkromberg/ECUST](https://github.com/mkromberg/ECUST)

## Morten Kromberg
####(With thanks to Jay Foad, Roger Hui and John Scholes)

The workshop assumes no prior knowledge of APL.

Prerequisites: It would help if attendees installed Dyalog on their
laptops prior to the workshop - though we will spend a little time at
the start of the day making sure everyone is able to use APL.

The format will include many short exercises. Feedback and discussion
will be encouraged throughout.

## Documentation
Supporting documentation and materials are available online:

* [http://tryapl.org](http://tryapl.org) see the "resources" tab
* [http://help.dyalog.com](http://help.dyalog.com) online help
* [http://docs.dyalog.com](http://doc.dyalog.com) manuals etc
* [http://http://www.dyalog.com/mastering-dyalog-apl.htm](http://www.dyalog.com/mastering-dyalog-apl.htm) Introduction to Dyalog APL by Bernard Legrand
* Google: Try e.g. [https://www.google.com/?q=dyalog+apl+power+operator](https://www.google.com/?q=dyalog+apl+power+operator)

# Morning

* Verifying Dyalog and keyboard installations (http://dyalog.com/apl-font-keyboard.htm).
* Entering APL glyphs from the keyboard:
  * Click on language bar (hover for help)
* Configuration: please use `]box on`.
* TryAPL (http://tryapl.org/) has a floating soft keyboard, or you can use the IME (Windows) or Mac & Linux keyboard layouts.

## Introduction

Powerpoint slides 1-12:

* How APL evolved from a mathematical notation.
* Started as "Iverson notation", used on the blackboard for teaching.
* "A Programming Language" book published in 1962, long before it was
implemented on any computer. (*Program* just meant *algorithm*.)
* Used to formally describe the System/360 at IBM.
* First interpreter for the 360 in 1966, developed by Ken, Larry Breed, Adin
Falkoff, Dick Lathwell and Roger Moore.
* Notation had to be linearised.
* Most other languages (e.g. Fortran) grew from the machine up, not from maths down.

## First impressions

    mean←{(+⌿⍵)÷≢⍵}

* The character set: APL uses special symbols.
* Simple regular syntax.
* No reserved words in the language.

* Array programming is a paradigm in its own right.
* There are other APLs (APL2, NARS2000, GNU APL, NGN APL, ELI, etc), generally
not completely compatible despite an ISO standard...
* and other languages in the family (A, A+, Nial, J, K etc).


Array languages tend to share three features:
* terse
* symbolic (not wordy)
* high-order functions

> The only program which stands a chance of being correct is a short
  one --Arthur Whitney

* Primitives are carefully chosen; e.g. you get Grade instead of Sort,
  which enables `names[⍋ages]`.

## Syntax: functions, arguments and naming

* Functions are prefix or infix (*monadic* or *dyadic*) e.g. `-`
stands for negate or subtract respectively.
* Unlike many languages (e.g. C, Java), this goes for user-defined
functions too.
* *All* functions associate right: `3×2+1` is `3×(2+1)`.

**Exercise 00:** As a group:

What do these expressions evaluate to?

      3+2
      3*2   ⍝ * is not multiplication - try F1 help!
      3×2
      9*0.5 ⍝ square root
      6÷2
      5-3-1 ⍝ not 1
      1 2 3
      -1 + 2⍝ not 1
      ¯1 + 2
      1 + 2 3
      1 2 + 3
      1 2 + 3 4
      1 2 × 3 4
      1 2 ÷ 3 4
      1 2 + 3 4 5   ⍝ errors: see later
      1÷2  3÷4
      1 (2 3) + (4 5) 6 ⍝ as if (1 1)(2 3) + (4 5)(6 6)
    
* Conformability rules for scalar functions: vectors must have same length, or
either argument can be a scalar ("scalar extension")
* Assignment uses the left arrow to name an array, `x←3`.

**Exercise 01** (introducing assignment):

What do these expressions do?

      x←3           ⍝ result of assignment isn't printed
      x+x
      y+(y←4)       ⍝ R-to-L execution guaranteed here, pass through result
      (x y z)←1 2 3
      (x y)←y x
      q←x
      x q
      x←4
      x q           ⍝ arrays have value semantics


**Exercise 02** (first sight of a non-scalar function):

Try these expressions:

      5=2+3         ⍝ not "true" but "1"
      A←⍳3
      A=1 2 3
      A≡1 2 3       ⍝ non-scalar function

## Arrays

* In APL "everything is an array", but what’s an array?
  * Rectangular collection of items,
  * arranged along zero or more orthogonal axes,
  * of numbers, characters and arrays
* *All* primitives work on arrays and return arrays.
* Strings are just vectors of characters.

**Exercise 03** (introducing some structural primitives):

Try:

      ]boxing on
      ⍴1 2 3
      ⍴'JAY'
      ⍳4
      ⍴⍳4
      ⌽⍳4
      5↑'SHANGHAI'
      ¯3↑'SHANGHAI'
      ⍴(1 2 3)(4 5 6)
      ⍴'Morten' 'Kromberg'
      5⍴'JAY'       ⍝ introduce Reshape
      A←3 3⍴⍳9
      A             ⍝ display is "row major"
      ⍴A
      ⍴⍴A
      ⌽A
      ⊖A
      ⍉A
      ,A
      ↓A
      ⍴↓A
      ↑(1 2 3)(4 5 6)
      ⍪1 2 3
      ⍴⍪1 2 3
      3 3⍴'APL'
      2 3 4⍴⍳24     ⍝ display is in planes
      B←2 3 4 5 6 7⍴99 ⍝ N.B. don't display this!
      ⍴B
      ⍴⍴B

* Other languages have scalars and arrays, but in APL there is no such
  distinction.
* Every array has a rank. What is the rank of a scalar? Try it! What
  is the shape of a scalar? Compare with `⍳0` and `⍬`.
* How many items in a rank-0 array? Try `×/⍬`.
* APL *recycles* the word "scalar" to mean "rank-zero array".
* Why is a scalar an array? Because *everything* is an array! And it
  helps with consistency: reduction of matrix is vector, of vector is
  scalar.
* (Muse: "there's no access to the mote". See Trenchard More's papers
  on "Nested Rectangular Arrays".)
* Using the array as the fundamental type means that performance can
  be very good, even though it's interpreted.

**Exercise 04** (operators)

Try:

      +/2 3 4       ⍝ "summation over" (KEI)
      ×/2 3 4       ⍝ "product over" (KEI)
      ⌈/2 3 4
      ≠/1 0 1 1     ⍝ parity

* `/` here is an operator called Reduce (or fold)
* (Monadic) operators take a function operand on their *left*.
* So `+/` is "plus reduce", or "sum".
* Reduce inserts its operand function in the spaces: `1+2+3`
* Generalises in ways that are more useful than you might expect! (See
  http://www.dyalog.com/blog/2014/11/musings-on-reduction/)

Inner product:

      1 2 3 +.× 4 5 6
      +/ 1 2 3 × 4 5 6
      A←2 2⍴1 2 3 4
      A +.× A
      ⍴ (2 3 4⍴⍳24) +.× (4 5 6⍴⍳120)
      (3 4 5 +.* 2) * 0.5
      'ABC' ∨.= 'APL'

* *Dyadic* operators also take an operand on the right.
* All dyadic operators are left-associative (cf functions).
* `.` (dot) is a *dyadic* operator called Inner Product:

⍝ Outer product:

      X←⍳4
      X ∘.+ X
      X ∘.× X
      X ∘.⌈ X
      X ∘.= X
      ⍴ (2 3⍴⍳6) ∘.+ (4 5⍴⍳20)

* As a special case, left operand of `∘` gives Outer Product (or table):

## Some thoughts about MatLab vs APL

Powerpoint slides 13-15

## Numbers

* APL presents the illusion that "a number is just a number".
* Users don't have to think about data types.
* Automatic promotion on overflow; occasional demotion on heap
  compaction / garbage collection.

**Exercise 05** (more scalar arithmetic):

Try:

      0 0 1 1 ∨ 0 1 0 1
      0 0 1 1 ∧ 0 1 0 1
      15 ∨ 35
      15 ∧ 35
      3÷4
      *1
      ○1
      2*0.5
      ¯1*0.5
      3*99
      3*999

**Exercise 06** (Random Numbers):

Try:

      ? 6 
      ? 10 ⍴ 6 
      ? 0 0 0
      3 ? 4 ⍝ NB not a scalar function

**Exercise 07** (Booleans)

* Booleans are just numbers 0 and 1 (we saw this with `=`).
* Knuth even called this "Iverson's convention" in his paper "Two
  Notes on Notation"
  (https://www.maa.org/sites/default/files/pdf/upload_library/22/Ford/knuth403-422.pdf).
* This allows useful tricks like `+/A>0` (how many positive numbers
  in A?).
* See Scholes's blog on Data-driven Conditionals
  (http://www.dyalog.com/blog/2014/10/data-driven-conditionals-2/).
* (Morten Kromberg calls it loop-free programming.)
* Coming back into fashion because of GPUs.

Try:

      A←1 ¯2 3 4 ¯5
      A>0
      +/A>0
      (A>0)/A
      2 3 0/'XYZ'
      B←0 1 0 1 1 0 0 1
      B/⍳⍴B
      ⍸B

## Indexing and partial assignment

**Exercise 08** (indexing)

Try:

      vec←'SHANGHAI'
      vec[5]
      mat←3 4⍴⍳12
      mat[2;3]
      mat[2;]
      Cities←'SHANGHAI' 'LONDON'
      Cities[2]
      Cities[2 1]
      Cities[⊂2 1]
      [2]¨Cities
      2⊃¨Cities
      ⊃mat
      '.⌹'[1+1 7 4 2∘.≥⍳10]
      ⍝ Assignment
      mat[2;3]←99
      mat
      A←B←⍳10
      B[2]←99
      A B
      (3↑B)←0
      B

* N.B. bracket indexing can't be used as an operand function :-(
* Using an array *inside* the brackets can be very useful: `'.⌹'[mat]`

* Indexed assignment works intuitively: `vec[3]←99`
* N.B. arrays still have **value** semantics; other namings of the
  same array are *not* affected.
* Selective assignment: `(3↑vec)←4 5 6` *also* works intuitively!
* But N.B. only for "structural" primitive functions; you can't do `(0×A)←1`.

## User Defined Functions

**Exercise 09** (single line dfns):

Try:

     {⍺+⍵}
     3{⍺+⍵}4
     3{⍵+⍵}4
     {⍵+⍵}4
     {⍺+⍺}4
     where←{⍵/⍳⍴⍵}
     where 0 1 0 1 1 0 0 1
     disc←{(a b c)←⍵ ⋄ (b*2)-4×a×c}
     disc 1 ¯1 ¯1  ⍝ (defining equation of the golden ratio)

**Exercise 10** (searching and sorting):

Try:

     'THE CAT IN THE HAT'∊'AEIOU'
     'AT'⍷'THE CAT IN THE HAT'
     'THE CAT IN THE HAT'~'AEIOU'
     3 1 4 1 5 9 ⍳ 1 2 3 4
     (3 3⍴'CATHATSAT')⍳2 3⍴'HATMAT'
     'ABCD'∪'DEFG'
     'ABCD'∩'DEFG'
     ⍋pi←3 1 4 1 5 9
     pi[⍋pi]

**Exercise 11** (more operators):

Try:

     ⍝ Each: ¨

     A←'Jay' 'Foad'
     ⍴A
     ⍴¨A
     ⌽A
     ⌽¨A

     ⍝ Key: ⌸

     x←('zero' 'one' 'two' 'three' 'four')[?20⍴5]
     x
     y←?20⍴10
     y
     x {⍺ ⍵}⌸ y
     x {⍺,≢⍵}⌸ y
     x {⍺,+⌿⍵}⌸ y
     x {⍺,⌈⌿⍵}⌸ y

## Defined functions, Continued

* Dyalog has anonymous (lambda-style) function definitions called
  *dfns*.
* MANY examples: http://dfns.dyalog.com
* Defined in braces, with `⍺` and `⍵` standing for the arguments.
* Functions have at most two arguments, but it's easy to make an
  argument tuple and decompose it with strand assignment.

* There is only one control structure: the guard.
* Dfns consist of a single expression preceded by zero or more local
  definitions and guards.
* At a pinch, you can use diamonds to separate "lines".
* Use the *function editor* to define multi-line dfns. Hit ESC to exit
  the editor.
* The current functions is denoted `∇` to facilitate simple recursion:

**Exercise 09** (multi-line dfns):

Try:

      )ed sign
      sign←{
        ⍵<0:'negative'
        ⍵>0:'positive'
        'zero'
      }
      sign 2
      )ed fact
      fact←{
        ⍵=0:1
        ⍵×∇ ⍵-1
      }
      fact 10
      !10

* User defined functions can Just Work on arrays of arbitrary rank!
  See dfns.easter (http://dfns.dyalog.com/n_easter.htm)

## Reading APL

(Powerpoint 16&17)

## Multi-Line Dfns & Procedures

(Powerpoint, 18-21)

* APL had tradfns long before dfns.
* The only control flow was `→` (goto).
* Control structures (`:If` etc) improved life a little.
* Variables had dynamic scope.

## Note: remaining exercises not described here.