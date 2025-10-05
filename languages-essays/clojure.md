## Tiny Clojure Essay

created: 10.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Clojure

* Modern Lisp
* JVM (full interop with Java)
* Also runs on JS (Clojure-Script https://clojurescript.org/)
* Concisve
* Fun
* Functional Programing
* Companies using Clojure: Akamai, Amazon, Apple, Capital One, Deutsche Bank, eBay, Intuit, Walmart, Zalando, Nubank and many more(https://clojure.org/community/companies) 
* Here is amazing sucess stories from mant big companies: https://clojure.org/community/success_stories
* Here is a great rationale https://clojure.org/about/rationale
* REPL

### My Feelings (10.FEB.2024 1.11.1)

* Clojure is amazing, IMHO the best lisp.
* The main issues is that is slow and error handling sucks, tooling is not quite there.
* However, is amazing language, great for DSLs, great for rules and transformations.
* You can use all java build systems and tools like ant, maven, gradle or clojure spesific Leiningen(https://leiningen.org/)

### Show me the code

My POCs with Clojure: https://github.com/diegopacheco/clojure-playground <br/>

#### 1 - Java Interop
Clojure don't have complications around strings, clojure strings are java strings. You can easilly call anythinb in java just add a "." and method name. IF you need call static method can do "Character/isWhitespace". 
```Clojure
(.toUpperCase "clojure")
```
#### 2 - There are loops
Haskell dont have loops (besides recursion) but clojure has loops.
```Clojure
(for [i [1 2 3]] 
  (prn (str "Hello " i))
)
```
#### 3 - Records
Like Haskell, Scala(case class), Kotlin(data class), Java 21 - clojure has records.
```Clojure
(defrecord Person [first last email])
(def diego (Person. "Diego" "Pacheco" "my-email@email.com"))
(prn (:first diego))
(prn (:last diego))
(prn (assoc diego :email "email@email.mail"))
```
#### 4 - Atoms
Atoms are a nice way to handle shared sync state. Intended use of atom is to hold one of Clojure’s immutable data structures.
```Clojure
(def a (atom 1))              ;; creates an atom type int - sets value to 1
(compare-and-set! a 1 99)     ;; sets the atom to 99 using (CAS) if value is 1
(swap! a inc)                 ;; increment the atom value to +1
(println @a))                 ;; de-ref and print the atom
```
#### 5 - Functional Programing
Powered by List, backed by Java JVM, Immutability, high order functions, recursion and much more...
```Clojure
(reduce + [1,2,3,4,5,6,7,8,9,10])                ;; 55
(map + '(1, 2, 3, 4, 5, 6, 7))                   ;; creates a LazySeq (1, 2, 3, 4, 5, 6, 7)
(filter #(even? %1) (map inc [1,2,3,4,5,6]))     ;; (2, 4, 6)
```
#### 6 - Function/Method polymorphism with defmulti
Creates a new multimethod with the associated dispatch function.
```Clojure
(defmulti greeting
  (fn[x] (get x "language")))                        ;; recives a hasmap called x and look for the key "language"
  
(defmethod greeting "English" [params] "Hello!")     ;; first implementation for english
(defmethod greeting "French" [params] "Bonjour!")    ;; second implementation for frnech
(def english-map {"id" "1", "language" "English"})   ;; define the english hashmap 
(def french-map  {"id" "2", "language" "French"})    ;; define the french hashmap
(prn (greeting english-map))
(prn (greeting french-map))
```
We can use multimetohods to do pattern matching across multiple functions (like in Haskell) or Java.
```Clojure
(defmulti factorial identity)

(defmethod factorial 0 [_]  1)
(defmethod factorial :default [num] 
    (* num (factorial (dec num))))

(factorial 0)                       ;; => 1
(factorial 3)                       ;; => 6
```
#### 7 - Protocols
Clojure's alternative to interfaces is called Protocols. 
```Clojure
(defprotocol tax-service 
	(toll-one-way[this] 0 ) )                          ;; protocol definition

(defrecord brazillian-service [car]                        ;; define a record that implement the protocol
  tax-service
    (toll-one-way [this] 6.95))

(defrecord argentinian-service [car]                      ;; define a record that implement the protocol
  tax-service
    (toll-one-way [this] 16.95))

(extend-protocol tax-service                              ;; extend the protocol and override behavior
  java.lang.String
    (toll-one-way [this] (toll-one-way (brazillian-service. this)) ))

(toll-one-way "uno")
```
#### 8 - Agents
Agents are similar to atoms(shared access to mutable state), but they are async, this is similar to JS async/await.
Agents provide independent, asynchronous change of individual locations. Clojure’s Agents are reactive, not autonomous, async, the actions of all Agents get interleaved amongst threads in a thread pool.
```Clojure
(defn increment [curr num] (+ curr num))
(def smith (agent 1))
(send smith increment 5)
(print @smith)
```
#### 9 - Seq and Collections
Clojure defines many algorithms in terms of sequences (seqs). Most of the sequence library functions are lazy.
When seq is used on objects that implement Iterable, the resulting sequence is still immutable and persistent. Seq are clojure bread and butter, a lot of functions return and expect sequences.
```Clojure
(print (seq '(1)))   ;; (1)        - clojure.lang.PersistentList
(print (seq [1 2]))  ;; (1 2)      - clojure.lang.PersistentVector$ChunkedSeq
(print (seq "abc"))  ;; (\a \b \c) - clojure.lang.StringSeq
```
IF you wondering how clojue can have immutable data structures and still be usable? should not be completely slughish to the point where is in-pratical? The trick is Persistent Data Structures (https://www.youtube.com/watch?v=toD45DtVCFM&ab_channel=ZhangJian) this is a great video by Rich.

TL;DR; A persistent data structure is a data structure that always preserves the previous version of itself when it is modified. Considered as 'immutable' as updates are not in-place. A data structure is partially persistent if all versions can be accessed but only the newest version can be modified. The fear is that you get slow modification operations because you’ll have to copy the whole array all the time, and it will use a lot of memory. It would be ideal to somehow avoid redundancy as much as possible without losing performance when looking up values, along with fast operations. That is exactly what Clojure’s persistent vector does, and it is done through balanced, ordered trees.Here is a great post on the matter: https://hypirion.com/musings/understanding-persistent-vector-pt-1

#### Resources

* Clojure by Example https://kimh.github.io/clojure-by-example/

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0
* Kotlin https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Haskell: https://gist.github.com/diegopacheco/057087dc7ae236bdd0700014a31c88ef
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Nim lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/