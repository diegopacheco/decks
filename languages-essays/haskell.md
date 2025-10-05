## Tiny Haskell Essay

created: 01.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Haskell

* Created in 1990 (30+ years)
* Designed by many people
* PURELY Functional Programing
* Statically Typed
* Compiled
* Fun after some digestion time
* Concise
* Much more with less
* REPL (ghci) like all good FP langs
* Solid but yeat not everywhere - very niche 
* Lots of libs with package management (cabal)
* Scala and F# was heavily inspired by Haskell
* There are some use cases on Facebook, Github, Target and other companies but mostley academia. 
* List of some companies That Use Haskell in Production https://serokell.io/blog/top-software-written-in-haskell
* Nothing weired around strings like in rust and zig.
* There is not loop in haskell you need to use functions and function composition with map, filter, foldl.
* Syntax is terse, meaning spaces are separators between function parameters
* Types Philosophy: Don't think about types belonging to a big hierarchy of types. Instead, we think about what the types can act like and then connect them with the appropriate typeclasses

### My Feelings (01.FEV.2024 )

* First time I saw haskell was in 2006 and I was not impressed neither scared.
* Them because Scala in 2010, I went to haskell again, that time was a bit scarry
* Over the year I revised haskell time to time, now collecting my last review in 2024
* No fear, no issues jun fun :-) 
* Haskell is not weidely adopted in the industry besides some big companies using it
* People complain in too much academia thing
* But haskell diserves respect is a base for so many modern langs
* Syntax and ideas are amazing.
* But Diego - I dont get Functors, Applicatives, Monoids and Monads - Read this
https://www.adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html
![functor-applicatives-monodas](https://gist.github.com/assets/121278/9cbc6add-380b-401e-abca-c87a6c00a384)

### Show me the code

My POCs with Haskell: https://github.com/diegopacheco/haskell-playground <BR/>
Try haskell: https://tryhaskell.org/

#### 1 - Here is a simple function
```haskell
factorial 0 = 1
factorial n = n * factorial (n - 1)
```
#### 2 - IFs can be returned or assined to variables

To assign anything to variable use let or just return in the function boddy if you want
```haskell
doubleSmallNumber x = if x > 100
                        then x
                        else x*2 
```

#### 3 - Easy to work with collections
```haskell
let lostNumbers = [4,8,15,16,23,42]     -- creates a List
let result = [1,2,3,4] ++ [9,10,11,12]  -- concat 2 lists 
let out = "Steve Buscemi" !! 6          -- gets element out by index
let rev = reverse [5,4,3,2,1]           -- reverse the list
let thr = take 3 [5,4,3,2,1]            -- take 3 elements from the list
let h   = head [5,4,3,2,1]              -- get the first element
let l   = last [5,4,3,2,1]              -- get the last element
let gotit = 4 `elem` [3,4,5,6]          -- is 4 in the list? yes
```
plenty of useful functions like: product, minimum, sum, drop, etc...

#### 4 - Ranges

This are utilities that make working with lists even better. Might "look" like Slices in Rust, Go and Zig but it's not.
```haskell
let lista = [1..20]                      -- create a list from 1 to 20 of Integers
```

#### 5 - List/For/Loop Comprehensions

Copied to Python and Scala. Killer feature, give a for-like super power expressions.
```haskell
let result = [x*2 | x <- [1..10]]              -- creates a list because [] also numbers from 1 to 10 and multiply each by 2

boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x] -- here is a function we can use ifs too.

remove st = [ c | c <- st, c `elem` ['A'..'Z']]  -- here is a function to remove non upper case chars so we can do multiple computations there [] as you can see.

```

#### 6 - We can do pattern matching with guards

Haskell has one of the best if the best pattern matcher in FP landscape.
```haskell
factorial n
   | n < 2     = 1
   | otherwise = n * factorial (n - 1)
```
#### 7 - Typeclasses 

Haskell has relational algebra and is very veyr easy to create your own
types and a very powerful type system for instace.
```haskell
data SimplePerson = SimplePerson String String String Int String
    deriving Show

firstPerson :: SimplePerson
firstPerson = SimplePerson "John" "Doe" "jd@jd.com" 60 "Engineer"
```
Because we are diriving(extending) Show we can do print(since print calls show).
Similarly like you would do with a Trait in Scala or Trait in Rust (all copied from here).

#### 8 - Records
```haskell
data PersonRecord = PersonRecord { first :: String
                     , last :: String
                     , age :: Int
                     , height :: Float
                     , phone :: String
                     , flavor :: String
                     } deriving (Show)
```
Haskell creates the functions to access the data for us here. Many languages have Recods like Scala, Java, Kotlin all copied from Haskell.

#### 9 - Deriving

You can use deriving (Rust copy and called #[derive(Debug)]) and get a lot of capabilities like this.
Eq: allows comparisons with (==), Read: Allow unmarshall from string, Bounded: can get mix, max bounds, Show: let you print.
See we are not coding anything just via declarative forms telling what we want, yes we can orrivede if we need. 
This is like interfaces/iheritance in most of langs, but without sucking. 

```haskell
module Main where

data Day = Sunday | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday 
           deriving (Eq, Ord, Show, Read, Bounded, Enum)

enumCanDoCoolStuff :: [Day]
enumCanDoCoolStuff = [Thursday .. Saturday]

main :: IO ()
main = do
    putStrLn "First day is: "
    print (minBound :: Day)
    putStrLn "Last day is: "
    print (maxBound :: Day)

    print enumCanDoCoolStuff
```

#### 10. Datastructures - Stack

I know haskell can be scary: functors, applicatives, monoids, monads, reader monad, writter monad, state momand, etc...
But let's look how hard is to create a stack datastructure in haskell, check this out.
```haskell
newtype Stack a = Stack [a] deriving Show

empty = Stack []

push x (Stack xs)= Stack (x:xs)

pop (Stack []) = (Nothing, Stack [])
pop (Stack (x:xs)) = (Just x, Stack xs)
```
5 lines not bad right ? :-) OH but to use it must be very hard, check this out. Usage of the Stack:
```haskell
main = do
    let stack = push 1 $ push 2 $ push 3 empty
    print stack    
```

#### Haskell Resources

* Try Haskell, online REPL https://tryhaskell.org/
* Awesome-Haskell https://github.com/uhub/awesome-haskell
* Learn you a haskell for a Great Good (book - free online) https://learnyouahaskell.com/ (I read this book 6-7 times across 10+ years). Amazing book! Everytime I read it I learn something new! 

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0
* Kotlin: https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Clojure Lang: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Nim Lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/