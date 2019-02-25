# The Little Schemer

[![Build Status](https://travis-ci.org/willprice/little-schemer.svg?branch=master)](https://travis-ci.org/willprice/little-schemer)
[![codecov](https://codecov.io/gh/willprice/little-schemer/branch/master/graph/badge.svg)](https://codecov.io/gh/willprice/little-schemer)

[![The Little Schemer](https://mitpress.mit.edu/sites/default/files/imagecache/booklist_node/9780262560993.jpg)](https://mitpress.mit.edu/books/little-schemer-fourth-edition)

Many little exercises with little test suites written in [racket](https://racket-lang.org/).

## Types

| Type                    | Parameter name | Description                                                                                                      |
|-------------------------|----------------|------------------------------------------------------------------------------------------------------------------|
| *Atom*                  | `a`            | A string of characters (not containing parenthesis or starting with numerics)                                    |
| *List*                  | `l`            | A series of S expressions separated by spaces enclosed in parenthesis                                            |
| *Number*                | `n`            | A series of numeric characters (excluding floating point numbers)                                                |
| *S Expression*          | `sexp`         | Any of the above                                                                                                 |
| *Flat list*             | `lat`          | A list containing no lists as children                                                                           |
| *Tuple*                 | `tup`          | A list containing only numbers                                                                                   |
| *Arithmetic Expression* | `nexp`         | A 3 item list, the 1st and 3rd entries are also `nexp`, the 2nd is an atom representing an operation or a number |
| *Set*                   | `set`          | A list of atoms containing no duplicates                                                                         |
| *Pair*                  | `p`            | A list of length 2                                                                                               |
| *Relation*              | `rel`          | A set of pairs                                                                                                   |
| *Function*              | `fun`          | A relation where the first element of each pair forms a set                                                      |
| *Test*                  | `test?`        | A lambda which compares two s expressions for equality                                                           |
| *Expression*            | `e`            | A S expression representing a computation                                                                        |

### Compound types
Sometimes we write functions with parameters that take multiple types, there are
some conventions for these types.

| Paramter naming convention   | Implicit types   |
| ---------------------------- | ---------------- |
| `sorn`                       | Symbol, Number   |
| `pora`                       | Pair, Atom       |


## Lambdas
### Naming conventions
* `*`-lambdas: Recurse over a list.
* `multi`-lambdas: Recurse over a list performing an action multiple times.
* `&co`-lambdas: Recurse over a list performing the collector lambda each time.

### Total vs Partial
(Relevant from chapter 9 onwards)

Total functions are guaranteed to terminate however partial functions may not
terminate. Total functions are referred to as `natural` and partial as
`unnatural`.

### Primitives
We assume the existence of the following functions:

| Function  | Parameters           |
|-----------|----------------------|
| `define`  | `(a l)`              |
| `lambda`  | `(args body)`        |
| `cond`    | `(((bool) (sexp))+)` |
| `atom?`   | `(sexp)`             |
| `eq?`     | `(a1 a2)`            |
| `null?`   | `(l)`                |
| `quote`   | `(sexp)`             |
| `car`     | `(l)`                |
| `cdr`     | `(l)`                |
| `cons`    | `(sexp l)`           |
| `not`     | `(bool)`             |
| `and`     | `(sexp+)`            |
| `or`      | `(sexp+)`            |
| `add1`    | `(n)`                |
| `sub1`    | `(n)`                |
| `zero?`   | `(n)`                |
| `number?` | `(sexp)`             |


### Derived
We write a lot of functions using the primitive functions we've been given.

#### [Equality lambdas](equality.rkt)
* `(eq? a1 a2)` - Checks equality of non-numeric atoms. (builtin)
* `(eqan? a1 a2)` - Checks equality of atoms and numbers.
* `(eqlist? l1 l2)` - Checks equality of two lists.
* `(equal? s1 s2)` - Checks equality of two S-expressions. (builtin)

#### [List lambdas](lists.rkt)
* `(firsts l)`
* `(length l)`
* `(rember* a l)`
* `(occur* a l)`
* `(insertR* new old l)`
* `(insertL* new old l)`
* `(subst* new old l)`
* `(member* a l)`
* `(leftmost l)`
* `(eqlist? l1 l2)`
* `(third l)`
* `(evens-only* l)`

#### [Flat list lambdas](lats.rkt)
* `(lat? l)`
* `(member? a lat)`
* `(rember a lat)`
* `(insertR new old lat)`
* `(insertL new old lat)`
* `(subst new old lat)`
* `(subst2 new o1 o2 lat)`
* `(multirember new old lat)`
* `(multiinsertR new old lat)`
* `(multiinsertL new old lat)`
* `(multisubst new old lat)`
* `(pick n lat)`
* `(rempick n lat)`
* `(no-nums lat)`
* `(all-nums lat)`
* `(occur a lat)`
* `(multiinsertLR new oldL oldR lat)`
* `(looking a lat)`
* `(keep-looking a sorn lat)`

#### [Number lambdas](numbers.rkt)
* `(+ n m)`
* `(- n m)`
* `(× n m)` (note this is a unicode multiplication sign and not the letter x)
* `(> n m)`
* `(< n m)`
* `(= n m)`
* `(↑ n m)`
* `(÷ n m)`
* `(one? n)`

#### [Arithmetic expression lambdas](expressions.rkt)
* `(numbered? aexp)`
* `(value nexp)`
* `(operator nexp)`
* `(1st-sub-exp nexp)`
* `(2nd-sub-exp nexp)`
* `(atom-to-function x)`

#### [Tuple lambdas](tups.rkt)
* `(tup? l)`
* `(addtup tup)`
* `(tup+ tup1 tup2)`


#### [Set lambdas](sets.rkt)
* `(set? lat)`
* `(makeset lat)`
* `(subset? set1 set2)`
* `(eqset? set1 set2)`
* `(intersect? set1 set2)`
* `(intersect set1 set2)`
* `(union set1 set2)`
* `(intersectall l-set)`

#### [Pair lambdas](pairs.rkt)
* `(a-pair? x)`
* `(first p)`
* `(second p)`
* `(build s1 s2)`
* `(revpair p)`
* `(shift p)`
* `(length* pora)`
* `(weight* pora)`
* `(shuffle pora)` (partial)


#### [Relation lambdas](rels.rkt)
* `(fun? rel)`
* `(revrel rel)`
* `(fullfun? rel)` (aka `one-to-one?`)

#### [Higher order lambdas](higher-order.rkt)
Where a function returns a function, I have noted the parameters of the
returned function after a `->` followed by a `_` to represent the anonymous
function.

* `(rember-f test?) -> (_ a l)`
* `(eq-c? a) -> (_ x)`
* `(insertL-f test?) -> (_ new old l)`
* `(insertR-f test?) -> (_ new old l)`
* `(insert-g seq) -> (_ new old l)`

#### [Collector lambdas](collectors.rkt)
* `(multirember&co a lat col)`
    * `(a-friend x y)`
    * `(new-friend x y)`
    * `(latest-friend x y)`
    * `(last-friend x y)`
* `(multiinsertLR&co new oldL oldR lat col)`
* `(evens-only*&co l col)`

#### [Misc](misc.rkt)
* `(sero? n)`
* `(edd1 n)`
* `(zub1 n)`

#### [Table lambdas](tables.rkt)
*Tables* are composed of *entries* which are *pairs* of lists (of the
same length), the first list is *names*, the second is *values*
thus representing the binding of names to values.

Lookup lambdas take a `-f` lambda that runs in the case the `name` is
not found in the entry/table. The lambda is passed the `name` that was
not found.

* `(new-entry names values)`
* `(look-up-in-entry name entry entry-f)`
* `(extend-table entry table)`
* `(look-in-table name table)`


#### [Interpreter lambdas](interpreter.rkt)
`e` stands for expression.

![`value` dependencies](./media/value-deps.svg)

* `(value e)`
* `(expression-to-action e)`
* `(atom-to-action e)`
* `(list-to-action e)`
* `(meaning e table)`
* `(*const e table)`
* `(*quote e table)`
* `(*identifier e table)`
* `(*lambda e table)`
    * `(table-of non-primitive-lambda)`
    * `(formals-of non-primitive-lambda)`
    * `(body-of non-primitive-lambda)`
* `(*cond e table)`
* `(*application e table)`
    * `(function-of e)`
    * `(arguments-of e)`
* `(primitive? lambda-meaning)`
* `(non-primitive? lambda-meaning)`
* `(initial-table name)`
* `(evcon lines table)`
* `(evlis args table)`
* `(apply-primitive name vals)`
* `(apply-closure closure vals)`
* `(apply fun vals)`

## Contributing
* Fork the repository.
* Add function(s) with associated test suite(s).
* Run tests with
```
$ ./test.sh
```
* Add an entry to the README with the function, it's signature and purpose.
  Hyperlink the function name to the source of the function.
* Commit with a description of the function added.
* Rebase onto the main branch.

