# The Little Typer

## References
1. [The Pie language](https://github.com/the-little-typer/pie)
1. [Buy a book](http://product.dangdang.com/1438225688.html)

## The More Things Change, The More They Stay The Same
1. The Law of Tick Marks
    1. A tick mark directly followed by one or more letters and hyphens is an `Atom`.
        * 'ratatouille is an `Atom`
    1. A form of judgement is an observation with blank spaces in it, such as *___ is a ___*
1. The Commandment of Tick Marks
    1. Two expressions are the same `Atom` if their values are tick marks followed by identical letters and hyphens.
        * 'citron is the same `Atom` as 'citron
    1. The second form of judgement is that *___ is the same ___  as ___*
    1. Expressions that describe other expressions, such as `Atom`, are called *types*
    1. The third from of judgement is *___ is a type*
1. The Law of `Atom`
    1. `Atom` is a *type*
    1. The fourth form of judgement is *___ and ___ are the same type*
        * `Atom` and `Atom` are the same *type*
1. The Four Forms of Judgement
    1. ___ is a ___
    1. ___ is the same ___  as ___
    1. ___ is a type
    1. ___ and ___ are the same type
1. Normal Forms
    1. Given a type, every expression described by that type has a normal form, which is the most direct way of writing it. If two expressions are the same , then they have identical normal forms, and if they have identical normal forms, the they are the same.
1. Normal Forms and Types
    1. Sameness is always according to a type, so normal forms are also determined by as type.
1. The First Commandment of cons
    1. Two `cons`-expression are the same `(Pair A D)` if their `car`s are the same · and their `cdr`s are the same `D`. Here, `A` and `D` stand for any type.
1. Normal Forms of Types
    1. Every expression that is a type has a normal form, which is the most direct way of writing that type. If two expressions are the same type, then they have identical normal forms, and if two types have identical normal forms, they are the same type.
1. Claims before Definitions
    1. Using define to associate a name with an expression requires that the expression's type has previously been associated with the name using claim.
        ```lisp
        (claim one
          Nat)
        (define one
          (add1 zero))
        ```
1. Values
    1. An expression with a constructor at the top is called a *value*
1. Values and Normal Forms
    1. Not every value is in normal form. This is because the arguments to a constructor need not be normal. Each expression has only one normal form, but it is sometimes possible to write it as a value in more than one way.
    1. Finding a value that is the same as some starting expression is called *evaluation*
1. Everything Is a Expression
    1. values are also expressions, Evaluation finds an expression, not some other kind of thing.
1. The Commandment of `zero`
    1. `zero` is the same `Nat` as `zero`
1. The Commandment of `add1`
    1. If `n` is teh same `Nat` as `k`, then `(add1 n)` is the same `Nat` as `(add1 k)`
1. Definitions Are Forever
    1. Once a name has been claimed, it cannot be reclaimed,
    and once a name has been defined, it cannot be redefined.
    1. `(cons 'zero 'onion)` is a `(Pair Atom Atom)`
    1. `zero` and `add1` are *constructors* that create *data*, `Pair` is a *type constructor* because it constructs a *type*

## Doin' What Comes Naturally
1. Constructors and Eliminators
    1. *Constructors* build values, and *eliminators* take apart values built by constructors
        * `car` is an *eliminator*
    1. `λ` builds functions
1. Eliminating Functions
    1. Applying a function to arguments is the eliminator for functions
1. The Initial Law of Application
    1. If `f` is an `(-> Y X)` and `arg` is a `Y`, then `(f arg)` is an `X`
1. The Initial Commandments of `λ`
    1. Two `λ`-expressions that expect the same number of arguments are the same if their bodies are the same after consistently renaming their variables
    1. If `f` is an `(-> Y X)`, then `f` is the same `(-> Y X)` as `(λ (y) (f y))` as long as `y` does not occur in `f`.
1. The Law of Renaming Variables
    1. Consistently renaming variables can't change the meaning of anything
    1. Expressions that are not values and cannot yet be evaluated due to variable are called *neutral*
    1. If two expressions have identical eliminators at the top and all arguments to the eliminators are the same, then the expressions are the same.
1. The Commandment of Neutral Expressions
    1. Neutral expressions that are written identically are the same, no matter their type.
1. The Law and Commandment of `define`
    1. Following  
    `(claim name X)` and `(define name expr)`  
    if `expr` is an `X`, than `name` is `X`  
    and `name` is the same `X` as `expr`.
1. The Second Commandment of `cons`
    1. If `p` is a `(Pair A D)`, then it is the same `(Pair A D)` as `(cons (car p) (cdr p))`
1. Names in Definitions
    1. Only names that are not already used, whether for constructors, eliminators, or previous definitions, can be used with `claim` or `define`.
1. Dim Names
    1. Unused names are written dimly, but they do need to be there.
1. The Law of `which-Nat`
    1. If `target` is a `Nat`, base is an `X`, and step is an `(-> Nat X)`,  
    then `(which-Nat target base step)` is an `X`.
1. The Commandments of `which-Nat`
    1. If `(which-nat zero base step)` is an `X`, then it is the same `X` as `base`
    1. If `(which-Nat (add1 n) base step)` is an `X`, then it is the same `X` as `(step n)`
    1. Explicit recursion is not an option because it possible write a no value expression (dead loop)
    ```lisp
    (claim forever (-> Nat Atom))
    (define forever (λ (and-ever) (forever and-ever)))
    (define gauss
        (λ (n) (which-Nat n
                0
                (λ (n-1)
                    (+ (add1 n-1) (gauss n-1)))))) ; NG
    ```
1. Type Values
    1. An expression that is described by a type is a value when it has a `constructor` at its top. Similarly, an expression that is a type is a value when it has a `type constructor` at its top.
    1. `U` describes all the types except for itself.
1. Every `U` Is a Type
    1. Every expression described by `U` is a type, but not every type is described by `U`.
    ```lisp
    (claim Pear U)
    (define Pear (Pair Nat Nat))
    ```
1. Definitions Are Unnecessary
    1. Everything can be done without definitions, but they do improve understanding.
    ```lisp
    (claim Pear-maker U)
    (define Pear-maker (-> Nat Nat Pear))
    (claim elim-Pear (-> Pear Pear-maker Pear))
    (define elim-Pear (λ (pear maker) (maker (car pear) (cdr pear))))
    ; equals
    (claim elim-Pear
        (-> (Pair Nat Nat)
            (-> Nat Nat
                (Pair Nat Nat))
            (Pair Nat Nat)))
    ```

## A forkful of Pie
1. The Law of `the`
    1. If `X` is a type and `e` is an `X`, then `(the X e)` is an `X`.
1. The Commandment of `the`
    1. If `X` is a type and `e` is an `X`, then `(the X e)` is the same `X` as `e`.

## Eliminate All Natural Numbers!
1. Sameness
    1. If a "same as" chart could show tha two expressions are the same, then this fact can be used anywhere without further justification.
    "Same As" charts are only to help build understanding.
1. Total Function
    1. A function that always assigns a value to every possible argument is called a total function.
    1. All functions are total.
1. The Law of `iter-Nat`
    1. If `target` is a `Nat`, `base` is an `X`, and `step` is an `(-> X X)`,
    then `(iter-Nat target base step)` is an `X`
1. The Commandments of `iter-Nat`
    1. If `(iter-Nat zero base step)` is an `X`,
    then it is the same `X` as `base`
    1. If `(iter-Nat (add1 n) base step)` is an `X`,
    then it is the same `X` as `(step (iter-Nat n base step))`
    ```lisp
    (claim step-+
        (-> Nat
            Nat))
    (define step-+
        (λ (+n-1)
            (add1 +n-1)))
    (define +
        (λ (n j)
            (iter-Nat n
                j
                step-+)))
    ```
1. The Law of `rec-Nat`
    1. If `target` is a `Nat`, `base` is an `X`, and `step` is an `(-> Nat X X)`,
    then `(rec-Nat target base step)` is an `X`
1. The Commandments of `rec-Nat`
    1. If `(rec-Nat zero base step)` is an `X`,
    then it is the same `X` as `base`
    1. If `(rec-Nat (add1 n) base step)` is an `X`,
    then it is the same `X` as `(step n (rec-Nat n base step))`
    ```lisp
    (claim step-gauss
        (-> Nat Nat
            Nat))
    (define step-gauss
        (λ (n-1 gauss-n-1)
            (+ (add 1 n-1) gauss-n-1)))
    (define gauss
        (λ (n)
            (rec-Nat n
                0
                step-gauss); implicit recursion is safe
    ```


## Easy as Pie
1. The Intermediate Law of Application
    1. If `f` is a `(Π ((Y U)) X)` and `Z` is a `U`, then `(f Z)` is an `X` where every `Y` has been consistently replaced by `Z`
    ```lisp
    (claim twin
        (Π ((Y U))
            (-> Y
                (Pair Y Y))))
    (define twin
        (λ (Y)
            λ (x)
                (cons x x)))
    (define twin-Atom
        (twin Atom))
    ```

## List, List and More List
1. The Law of `List`
    1. If `E` is a type, then `(List E)` is type.
1. The Law of `nil`
    1. `nil` is a `(List E)`, no matter what type `E` is.
1. The Law of `::`
    1. If `e` is an `E` and `es` is a `(List E)`, then `(::e es)` is a `(List E)`.
1. The Law of `rec-List`
    1. If target is a `(List E)`, base is an `X`, and step is an `(-> E (List E) X X)`,  
    then `(rec-List target base step)` is an `X`.
1. The Commandments of `rec-List`
    1. If `(rec-List nil base step)` is an `X`, then it is the same `X` as `base`.
    1. If `(rec-List (:: e es) base step)` is an `X`,  
    then it is the same `X` as `(step e es (rec-List es base step))`
    ```lisp
    (claim step-length
        (-> Atom (List Atom) Nat Nat))
    (define step-length
        (λ (e es length-es)
            (add1 length-es)))
    (claim length
        (-> (List Atom)
            Nat))
    (define length
        (λ (es)
            (rec-List es)
                0
                step-length))
    ```
    ```lisp
    ; use U
    (claim step-length
        (Π ((E U))
            (-> (List E)
                Nat)))
    (define step-length
        (λ (E)
            (λ (e es length-es)
                (add1 length-es))))
    (claim length
        (Π ((E U))
            (-> (List E)
                Nat)))
    (define length
        (λ (E)
            (λ (es)
                (rec-List es
                    0
                    (step-length E)))))
    ```
1. List Entry Types
    1. All the entries in a list must  have the same type.
    ```lisp
    ; ((append Atom) (:: 'A nil) (:: 'B nil))
    ; ->
    ; (:: 'A (:: 'B nil))
    (claim step-append
        (Π ((E U))
            (-> E (List E) (List E)
                (List E))))
    (define step-append
        (λ (E)
            (λ (e es append-es)
                (:: e append-es))))
    (claim append
        (Π ((E U))
            (-> (List E) (List E)
                (List E))))
    (define append
        (λ (E)
            (λ (start end)
                (rec-List start
                    end
                    (step-append E)))))
    ```
    ```lisp
    ; ((snoc Atom) (:: 'A nil) 'B)
    ; ->
    ; (:: 'A (:: 'B nil))
    (claim snoc
        (Π ((E U))
            (-> (List) E
                (List E))))
    (define snoc
        (λ (E)
            λ (es e)
                (rec-List es
                    (:: e nil) ; Atom -> (List Atom)
                    (step-append E))))
    ```
    ```lisp
    (claim step-reverse
        (Π ((E U))
            (-> E (List E) (List E)
                (List E))))
    (define step-reverse
        (λ (E)
            (λ e es reverse-es
                ((snoc E) reverse-es e))))
    (claim reverse
        (Π ((E U))
            (-> (List E)
                (List E))))
    (define reverse
        (λ (E)
            (λ es
                (rec-List es
                    nil
                    (step-reverse E)))))
    ```

## Precisely How Many?
1. The Law of `Vec`
    1. If `E` is a type and `K` is a `Nat`, then `(Vec E k)` is a type.
1. The Law of `vecnil`
    1. `vecnil` is a `(Vec E zero)`.
1. The Law of `vec::`
    1. If `e` is an `E` and `es` is a `(Vec E k)`,
    then `(vec:: e es)` is a `(Vec E (add1 k))`.
1. The Law of `Π`
    1. The expression `(Π ((y Y)) X)` is a type when `Y` is a type, and `X` is a type if `y` is a `Y`.
1. Use a More Specific Type
    1. Make a function total by using a more specific type to rule out unwanted arguments.
1. `->` and `Π`
    1. The type `(-> Y X)` is a shorter way of writing `(Π ((y Y)) X)` when `y` is not used in `X`.
    ```lisp
    (claim first
        (Π ((E U)
            (l Nat)
            (es (Vec E (add1 l)))) ; es is dim
            E))
    (define first
        (λ (E l es)
            (head es)))
    ```
1. The Final Law of `λ`
    1. If `x` is an `X` when `y` is a `Y`, then `(λ (y) x)` is a `(Π ((y Y)) X)`.
1. The Final Law of Application
    1. If `f` is a `(Π ((y Y)) X)` and `z` is a `Y`, then `(f z)` is an `X`
    where every `y` has been consistently replaced by `z`.
1. The Final Commandments of `λ`
    1. If two `λ`-expression can be made the same `(Π ((y Y)) X)` by consistently renaming their variables, then they are the same.
    1. If `f` is a `(Π ((y Y)) X)` and `y` does not occur in `f`, then `f` is the same as `(λ (y) (f y))`.

## It All Depends On the Motive
1. Dependent Types
    1. A type that is determined by something that is not a type is called a dependent type.
    ```lisp
    (claim peas
        (Π ((how-many-peas Nat))
            (Vec Atom how-many-peas)))
    (define peas
        (λ (how-many-peas)
            (rec-Nat how-many-peas
                vecnil
                (λ (l-1 peas-l-1) ; it against the type, (-> Nat X X), 
                    (vec:: 'pea peas-l-1))))) ; (vec:: 'pea peas-l-1) and peas-l-1 are not the same type
    ```
    1. *Motive* can be any `(-> Nat U)`
1. Use `ind-Nat` for Dependent Types
    1. Use `ind-Nat` instead of `rec-Nat` when the `rec-Nat`- or `ind-Nat`-expression's type depends on the targe Nat, The `ind-Nat`-expression's type is the motive applied to the target
    ```lisp
    (define mot-peas
        (λ (k)
            (Vec Atom k)))
    (claim step-peas
        (Π ((l-1 Nat))
            (-> (mot-peas l-1)
                (mot-peas (add1 l-1)))))
    (define step-peas
        (λ (l-1)
            (λ (peas-l-1)
                (vec:: 'pea peas-l-1))))
    ```
1. The Law of `ind-Nat`
    1. If `target` is a `Nat`, `mot` is an `(-> Nat U)`,
    `base` is a `(mot zero)`,
    and `step` is a
        ```lisp
        (Π ((n-1 Nat)
            (-> (mot n-1)
                (mot (add1 n-1)))))
        ```
    then `(ind-Nat target mot base step)` is a `(mot target)`
1. The Commandments of `ind-Nat`
    1. The `ind-Nat`-expression `(ind-Nat zero mot base step)` is the same `(mot zero)` as `base`
    1. The `ind-Nat`-expression `(indNat (add1 n) mot base step)` and `(step n (ind-Nat n mot base step))` are the same `(mot (add1 n))`.
    ```lisp
    (claim step
        (Π ((n Nat)); almost Nat
            (-> (mot n); almost answer
                (mot (add1 n)))))
    ```
1. Induction on Natural Numbers
    1. Building a value for any natural number by giving a value for `zero` and a way to transform a value for `n` into a value `n + 1` is called *induction on natural numbers*.
    ```lisp
    (define peas
        (λ (how-many-peas)
            (ind-Nat how-many-peas
                mot-peas
                vecnil
                step-peas)))
    ```
    ```lisp
    (claim also-rec-Nat
        (Π ((X U))
            (-> Nat
                X
                (-> Nat X
                    X)
                X)))
    (define alse-rec-Nat
        (λ (X)
            (λ (target base step)
                (ind-Nat target
                    (λ (k) X) ; k is dim
                    base
                    step))))
    ```
1. Define `last`
    ```lisp
    (claim last
        (Π ((E U)
            (l Nat))
            (-> (Vec E (add1 l))
                E)))
    ; consider using ind-Nat to define the last
    (claim base-last
        (Π ((E U))
            (-> (Vec E (add1 zero) ; last is a function, so the base is a function too
                E))))
    (define base-last
        (λ (E)
            λ (es)
                (head es))) ; when l is zero, vector es only contains one entry
    (claim mot-last
        (-> U Nat
            U))
    (define mot-last
        (λ (E k)
            (-> (Vec E (add1 k))
                E)))
    (claim step-last
        (Π ((E U) (l-1 Nat))
            (-> (mot-last E l-1) ; almost answer
                (mot-last E (add1 l-1)))))
    (define step-last
        (λ (E l-1)
            (λ (last-l-1); last-l-1 is a function too
                (λ (es)
                    (last-l-1 (tail es))))))
    （define last
        (λ (E l)
            (ind-Nat l
                (mot-last E)
                (base-last E)
                (step-last E))))
    ```
1. Readable Expressions
    1. Getting the right answer is worthless if we do not know that it is correct. Understanding the answer is at least as important as having the correct answer.

## Pick a Number, Any Number
1. With a new type constructor, type can express a new idea called *equality*
1. The Law of `=`
    1. An expression `(= X from to)` is a type if `X` is a type, `from` is an `X`, and `to` is a3n `X`.
1. Reading `FROM` and `TO` as Nouns
    1. Because `from` and `to` are convenient names, the corresponding parts of an `=`-expression are referred to as the  `FROM` and `TO`
    1. Types can be read as statements  
        * `(= Atom 'apple 'apple)` can be read: *"The expressions 'apple and 'apple are equal Atoms"*  
        * `(Π ((n Nat)) (= Nat (+ 1 n) (add1 n)))` can be read: *"For every Nat n, (+ 1 n) equals (add1 n)"*
    1. The `same` is the only constructor for `=`. here, "values" means "proof". 
1. The Law of `same`
    1. The expression `(same e)` is an `(= X e e)` if `e` is an `X`.
        * `(same 21)` is an `(= Nat (+ 17 4) (+ 11 10))`
        * `(same (incr 3))` is a proof of `(= Nat (+ 2 2) 4)`
    ```lisp
    (claim +1=add1
        (Π ((n Nat))
            (= Nat (+ 1 n) (add1 n))))
    (define +1=add1
        (λ (n)
            (same (add1 n))))
    ```
1. Neutral Expressions
    1. Variables that are not defined are neutral. If the target of an eliminator expression is neutral, then the eliminator expression is neutral.
1. "If" and "Then" as Types
    1. The expression `(-> X Y)` can be read as the statement, *"if X then Y"*
1. Observation about `incr`
    1. No matter which `Nat n` is, `(incr (add1 n))` is the same Nat as `(add1 (incr n))`
    ```lisp
    (claim step-incr=add1
        (Π ((n-1 Nat))
            (-> (= Nat (incr n-1) (add1 n-1))
                (= Nat (add1 (incr n-1)) (add1 (add1 n-1))))
        ))
    ```
    1. There is only one eliminator for `=`, called `cong`.
1. The Law of `cong`
    1. If `f` is an `(-> X Y)` and target is an `(= X from to)`,
    then `(cong target f)` is and `(= Y (f from) (f to))`.
    ```lisp
    (define step-incr=add1
        (λ (n-1)
            (λ (incr=add1-n-1)
                (cong incr=add1-n-1 (+ 1)))))
    (define incr=add1
        (λ (n)
            (ind-Nat n)
                mot-incr=add1  ; (λ (k) (= Nat (incr k) (add1 k)))
                base-incr=add1 ; (same (add1 zero))
                step-incr=add1))
    ```
1. The Commandment of `cong`
    1. If `x` is an `X`, and f is an `(-> X Y)`,  
    then `(cong (same x) f)` is the same `(= Y (f x) (f x))`  
    as `(same (f x))`.
1. Interval
    ```lisp
    (claim sandwich
        (-> Atom Atom))
    (define sandwich
        (λ (which-sandwich)
            'delicious))
    ```
    `(same 'delicious)` is a proof that `(sandwich 'hoagie)`, `(sandwich 'hero)` and `(sandwich 'grinder)` are all equal.

## Double Your Money, Get Twice as Much
1. Some expressions are not the same, but one can be `replace`d with the other
    ```lisp
    (+ n (add1 j)) ; +add1
    (add1 (+ n j)) ; add1+
    ```
1. The Law of `replace`
    1. If `target` is an `(= X from to)`, `mot` is an `(-> X U)`, and `base` is a `(mot from)` then `(replace target mot base)` is a `(mot to)`
    ```lisp
    (claim add1+=+add1
        (Π ((n Nat) (j Nat))
            (= Nat (add1 (+ n j)) (+ n (add1 j)))))
    (define add1+=+add1
        (λ (n j)
            (ind-Nat n
                (mot-add1+=+add1 j
                (same (add1 j)
                (step-add1+=+add1 j)))
            )))
    (define step-add1+=+add1
        (λ (j n-1)
            (λ (add1+=+add1-n-1)
                (cong add1+=+add1-n-1 (+ 1)))))

    (define step-twice=double
        (λ (n-1)
            (λ (twice=double-n-1)
                (replace (add1+=+add1 n-1 n-1)
                    (mot-step-twice=double n-1)
                    (cong twice=double-n-1 (+ 2)))
            )))
    (define twice=double
        (λ (n)
            (ind-Nat n
                mot-twice=double
                (same zero)
                step-twice=double)))
    ```
1. Observation about `+`
    1. No matter which `Nat`s `j` and `k` are, `(+ (add1 j) k)` is the same `Nat` as `(add1 (+ j k))`
1. Solve Easy Problems First
    1. If two functions produce equal results, then use the easier one when defining a dependent function, and then use replace to give it the desired type.
1. The Law of `symm`
    1. If `e` is an `(= X from to)`, then `(symm e)` is an `(= X to from)`.
1. The Commandment of `symm`
    1. If `x` is an `X`, then `(symm (same x))` is the same `(= X x x)` as `(same x)`.
    ```lisp
    (define twice-Vec
        (λ (E l)
            (λ (es)
                (replace (symm (twice=double l))
                    (λ (k) (Vec E k))
                    (double-Vec E l es)
                ))))
    ```

## It Also Depends on the List
1. The Law of `Σ`
    1. The expression `(Σ ((x A)) D)` is a type when `A` is a type, and `D` is a type if `x` is an `A`.
1. The Commandment of `cons`
    1. If `p` is a `(Σ ((x A)) D)`, then `p` is the same as `(cons (car p) (cdr p))`.
    1. If `(cons a d)` is a `(Σ ((x A)) D)` then `a`'s type is 4`A` and `d`'s type is found by consistently replacing every `x` in `D` with `a`.
        * `(cons Nat 4)` is a `(Σ ((A U)) A)`.
    1. `(Pair A D)` is a short way of writing `(Σ ((x A)) D)` where x is not used in `D`.
    1. `(Pair A D)` can be read *"A and D"*
    1. A `Σ`-expression can be read as *"there exists"*
        * `(Σ ((es (List Atom))) (= (List Atom) es (reverse Atom es)))` can be read as *"There exists a list of atoms that is equal to itself reversed"*
1. Use a Specific Type for Correctness
    1. Specific types can rule out foolish definitions.
    ```lisp
    ; not specific enough
    (claim list->vec
        (Π ((E U))
            (-> (List E)
                (Σ ((l Nat))
                    (Vec E l)))))
    ; foolish but permitted
    (define list->vec
        (λ (E es)
            (cons 0 vecnil)))

    ; specific type
    (claim list->vec
        (Π ((E U)
            (es (List E)))
            (Vec E (length E es))))
    ```
1. The Law of `ind-List`
    1. If `target` is a `(List E)`, `mot` is an `(-> (List E) U)`, `base` is a `(mot nil)`, and `step` is a 
        ```lisp
        (Π ((e E)
            (es (List E)))
            (-> (mot es)
                (mot (:: e es))))
        ```
        then `(ind-List target mot base step)` is a `(mot target)`.
1. The Commandments of `ind-List`
    1. The `ind-List`-expression `(ind-List nil mot base step)` is the same `(mot nil)` as base.
    1. The `ind-List`-expression `(ind-List (:: e es) mot base step)` is the same
    `(mot (:: e es))` as `(step e es (ind-List es mot base step))`
    ```lisp
    (define list->vec
        (λ (E es)
            (ind-List es
                (mot-list->vec E) ; (λ (E es) (Vec E (length E es)))
                vecnil
                (step-list->vec E))))
    (define step-list->vec
        (λ (E e es)
            (λ (vec-almost)
                (vec:: e vec-almost))))
    ```

## All Lists Are Created Equal
1. The Law if `ind-Vec`
    1. If `n` is a `Nat`, `target` is a `(Vec E n)`, `mot` is an
        ```lisp
        (Π ((k Nat))
            (-> (Vec E k)
                U))
        ```
        `base` is a `(mot zero vecnil)`, and `step` is a
        ```lisp
        (Π ((k Nat)
            (e E)
            (es (Vec E k)))
            (-> (mot k es)
                (mot (add1 k) (vec:: e es))))
        ```
        then `(ind-Vec n target mot base step)` is a `(mot n target)`.
1. The Commandments of `ind-Vec`
    1. The `ind-Vec`-expression  
    `(ind-Vec zero vecnil mot base step)`  
    is the same `(mot zero vecnil)` as `base`.
    1. The `ind-Vec`-expression  
    `(ind-Vec (add1 n) (vec:: e es) mot base step)`   
    is the same `(mot (add1 n) (vec:: e es))` as  
     `(step n e es (ind-Vec n es mot base step))`.
1. When in Doubt,Evaluate
    1. Gain insight by finding the values of expressions in types and working out examples in "same-as" charts.

## Even Numbers Can Be Odd
1. What is an even number?
    1. A number that can be split into two equal halves.
    1. Two equal halves -- There is some number that, added to itself, yield the original number.
    ```lisp
    (claim Even
        (-> Nat U))
    (define Even
        (λ (n)
            (Σ ((half Nat))
                (= Nat n (double half)))))
    (Even 10) ; value: (cons 5 (same 10))
    ```
1. Carefully Choose Definitions
    1. Carefully-chosen definitions can greatly simplify later proofs.
    ```lisp
    (define Even
        (λ (n)
            (Σ ((half Nat))
                (= Nat n (+ half half)))))
    ```

## Even Haf a Baker's Dozen
1. Is Every natural number either even or odd?
    1. *"Every natural number is either even or odd."*
1. The Law of `Either`
    1. `(Either L R)` is a type if `L` is a type and `R` is a type.
    1. `(left lt)` is an `(Either L R)` if `lt` is an `L`.
    1. `(right rt)` is an `(Either L R)` if `rt` is an `R`.
1. The Law of `ind-Either`
    1. If `target` is an `(Either L R)`,  
    `mot` is an `(-> (Either L R) U)`,  
    `base-left` is a `(Π ((x L)) (mot (left x)))`,  
    and `base-right` is a `(Π ((y R)) (mot (right y)))`  
    then `(ind-Either target mot base-left base-right)` is a `(mot target)`
1. The Commandments of `ind-Either`
    1. `(ind-Either (left x) mot base-left base-right)`  
    is the same `(mot (left x))` as `(base-left x)`
    1. `(ind-Either (right y) mot base-left base-right)`  
    is the same `(mot (right y))` as `(base-right y)`
    ```lisp
    (claim even-or-odd
        (Π ((n Nat))
            (Either (Even n) (Odd n))))
    (define even-or-odd
        (λ (n)
            (ind-Nat n
                mot-even-or-odd     ; (λ (k) (Either (Even k) (Odd k)))
                (left zero-is-even) ; (cons 0 (same 0))
                step-even-or-odd)))

    （define step-even-or-odd
        (λ (n-1)
            (λ (e-or-o-n-1)
                (ind-Either e-or-o-n-1
                    (λ (e-or-o-n-1)
                        (mot-even-or-odd (add1 n-1)))
                    (λ (e-n-1) ; e-n-1 is an (Even n-1)
                        (right ; add1 and become an odd
                            (add1-even->odd n-1 e-n-1)))
                    (λ (o-n-1) ; o-n-1 is a (Odd n-1)
                        (left  ; add1 and become an even
                            (add1-odd->even n-1 o-n-1)))))))

    (even-or-odd 2)
    ; equals
    (left (add1-odd->even
        1 (add1-even->odd
            0 zero-is-even)))
    ; equals
    (left (cons 1 (same 2)))
    ```

## There's Safety in Numbers
1. To represent the case when there is no entry, we need a new type, called `Trivial`
1. The Law of `Trivial`
    1. `Trivial` is a type.
    1. `sole` is a `Trivial` 
    1. If `e` is a `Trivial`, then `e` is the same as `sole`.
```lisp
    (claim Maybe (-> U U))
    ; either an X or a Trivial
    (define Maybe
        (λ (X)
            (Either X Trivial)))

    (claim nothing
        (Π ((E U))
            (Maybe E)))
    (define nothing
        (λ (E)
            (right sole)))

    (claim just
        (Π ((E U))
            (-> E (Maybe E))))
    (define just
        (λ (E e)
            (left e)))
```
```lisp
    (claim maybe-head
        (Π ((E U))
            (-> (List E))
                (Maybe E)))
    (define maybe-head
        (λ (E es)
            (rec-List es
                (nothing E)
                (λ (hd tl head-ti)
                    (just E hd)))))
```
1. The Law of `Absurd`
    1. `Absurd` is a type
    1. There are no `Absurd` values
    1. Every expression of type `Absurd` is neutral, and all of them are the same.
1. The Law of `ind-Absurd`
    1. The expression `(ind-Absurd target mot)` is a `mot` if `target` is an `Absurd` and `mot` is a `U`.
    1. For each `Nat` `n`, `(Fin n)` should be a type with `n` values.
    ```lisp
    (claim Fin (-> Nat U))
    ; (Maybe Absurd) -> (nothing Absurd)
    ; (Maybe (Maybe Absurd)) -> (nothing (Maybe Absurd)), (just (Maybe Absurd) (nothing Absurd))
    ; ...
    (define Fin
        (λ (n)
            (iter-Nat n
                Absurd
                Maybe)))
    ```
1. The first entry in a `Vec` is found using `(fzero n)` when the `Vec` has `(add1 n)` entries.
    ```lisp
    (claim fzero
        (Π ((n Nat))
            (Fin (add1 n)))) ; equals (Maybe (Fin n))
    (define fzero
        (λ (n)
            (nothing (Fin n)))); every (Fin n) can fit to nothing
    ```
1. `fadd1` points somewhere in its tail
    ```lisp
    (claim fadd1
        (Π ((n Nat))
            (-> (Fin n)
                (Fin (add1 n)))))
    (define fadd1
        (λ (n)
            λ (i-1)
                (just (Fin n) i-1)))
    ```

## Imagine That...
1. Imagine That...
    1. Using types, it is possible to assume things that may or may not be true, and then see what can be concluded from these assumptions.
1. Sameness versus Equality
    1. Either two expressions are the same, or they are not. It is impossible to prove that they are the same because sameness is a judgement, not a type, and a proof is an expression with a specific type

## If It's All the Same to You

## My Thoughts
1. Dependent type is a type that determined by a value.
1. Types can be read as statements and values become evidence, as a result, finding values for a given type equals to proving a given statement.