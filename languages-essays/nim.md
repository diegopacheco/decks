## Tiny Nim Essay

created: 24.MAR.2023

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Nim

* Simple: Simple language, dont have lots of ways of doing things.
* Elagant: very low verbosity, very consise.
* Multi-paradim(FP and OOP and Procedural), staticaly typed, type inference
* Created in 2008 by Andreas Rumpf
* You can play with it here: https://play.nim-lang.org/
* Companies using Nim Lang: https://github.com/nim-lang/Nim/wiki/Organizations-using-Nim

### My Feelings (24.MAR.2023 Nim-lang version 1.6.10)

* Fast compilation
* Fun to code with
* Unit tests, json built-in
* No complex types, very simple constructs, but can get complex with macros, oop.
* Dolcumentation is too basic, far from ideal. Not widely adopted in the industry.
* As fast as C, expressive as python, but less confusing them python
* OOP is a bit verbose (like rust and go)
* make the function(procedure) public requires "*" not obvious.
* Feels like a better Python

### Show me the code

My POCs with Nim: https://github.com/diegopacheco/nim-playground <br/>

#### 1 - Nice Slicing like Go
```Nim
let abc = ['a', 'b', 'c', 'd', 'e']
echo fmt"first {abc[0]}"
echo fmt"last {abc[^1]}"
echo fmt"slice {abc[1..3]}"
```
#### 2 - For Loops like Go 
```Nim
let word2 = "cool"
for i, c in word2:
    echo fmt"{i} index char is: {c}"
```
#### 3 - Implicit Result var
```Nim
proc getAlphabet(): string =
  for letter in 'a'..'z':
    result.add(letter)
```
#### 4 - Json support
```Nim
import json

type
  Element = object
    name: string
    atomicNumber: int

let jsonObject = parseJson("""{"name": "Carbon", "atomicNumber": 6}""")
let element = to(jsonObject, Element)
echo element.name
echo element.atomicNumber
```
#### 5 - Channels
```Nim
import os
import threadpool

var ch: Channel[string]
ch.open()

proc orderDish() =
  sleep(1000)
  ch.send("[French Fries]")

proc reciveKitchen() =
  let msg = ch.recv()
  echo "Received dish - working on it: " & msg

spawn orderDish()
spawn reciveKitchen()
sync()
```

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0
* Kotlin: https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Clojure: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Haskell: https://gist.github.com/diegopacheco/057087dc7ae236bdd0700014a31c88ef
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/