## Steel Lang Tiny Essay

created: 23.MAR.2025

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Steel

* Scheme
* Embeded Scheme for Rust
* Inspired By Racket
* Good Performance

### My Feelings (20-Jan-2025 steel 0.6.0)

* Scheme
* Fast because of Rust
* Cool
* Fun

### Show me the code

My POCs with Steel: https://github.com/diegopacheco/steel-playground <br/>

#### 1 - Filter

Classical high-order functions applying to collections to perform processing. Same function exist in Scala, Haskell, Rust, Java, Clojure and many other functional or post-functional languages.

```scheme
(define result (filter even? '(1 2 3 4 5)))
(display result)  ; Output: (2 4)
```

#### 2 - Map

Classical high-order functions applying to collections to perform processing. Same function exist in Scala, Haskell, Rust, Java, Clojure and many other functional or post-functional languages.

```scheme
(define result (map (λ (x) (+ x 1)) (list 1 2 3)))
(display result)
```

#### 3 - Reduce

Like in Clojure, Scala, Haskell, Kotlin. It's like a for but focused into geting just 1 value at the end. Some langs like scala call this Product.

```scheme
(define result (reduce + 0 (list 1 2 3 4 5 6)))
(display result)
```

#### 4 - Transducers

Transducers (just like Clojure) are composable, high-order transformations for processing data that:
* Are independent of the input/output collection types
* Can be combined and reused
* Process data efficiently with a single traversal

```scheme
;; Accepts lists
(display (transduce (list 1 2 3 4 5) (mapping (lambda (x) (+ x 1))) (into-list))) ;; => '(2 3 4 5 6)

;; Accepts vectors
(display (transduce (vector 1 2 3 4 5) (mapping (lambda (x) (+ x 1))) (into-vector))) ;; '#(2 3 4 5 6)

;; Even accepts streams!
(define (integers n)
    (stream-cons n (lambda () (integers (+ 1 n)))))

(display (transduce (integers 0) (taking 5) (into-list))) ;; => '(0 1 2 3 4)
```

#### 5 - Contracts

Contracts
* Contracts in Steel (similar to Racket) are a way to
* Specify what inputs a function accepts
* Specify what outputs a function provides
* Automatically check these conditions at runtime
* Report meaningful errors when contracts are violated

Contracts allow you to have pre-conditions and post-conditions (like DBC (Design by Contract)).
```scheme
(define/contract (test x y)
    (->/c even? even? odd?)
    (+ x y 1))

(test 2 2) ;; => 5
```
Requirements here are:
* First even?: Requires first parameter (x) to be even
* Second even?: Requires second parameter (y) to be even
* odd?: Requires the function's return value to be odd

#### 6 - Data Structure: Stack

Here, I'm implementing a stack data structure in Steel. Steel is pretty nice we can do that in 10 LoC.

```scheme
(define (make-stack)
  (let ((items '()))
    (lambda (op . args)
      (case op
        ((push) (set! items (cons (car args) items)))
        ((pop) (let ((top (car items)))
                 (set! items (cdr items))
                 top))
        ((top) (car items))
        ((empty?) (null? items))))))

(define stack (make-stack))
(stack 'push 1)
(stack 'push 2)
(stack 'push 3)
(display (stack 'top)) (newline)
(display (stack 'pop)) (newline)
(display (stack 'pop)) (newline)
(display (stack 'pop)) (newline)
```

## Other Tiny Essays 

* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0
* Kotlin: https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Clojure: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Haskell: https://gist.github.com/diegopacheco/057087dc7ae236bdd0700014a31c88ef
* Nim Lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75  
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/