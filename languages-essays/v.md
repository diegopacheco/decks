## Tiny V Lang Essay

created: 12.MAR.2023

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why V

* Simple: Very similar to Go (V compiler uses LLVM but originally written in Go)
* Statically typed and compiled programming language
* Fast: C interop with no cost, compiles to native binaries, as fast as C
* Compiled: V backend compile to readable C. Web Server binary its only 250kb. 
V in writen in V and compile it self under a second, compiler is single thread, will 2-3x faster in the future. Compiler is less than 1MB, it is able to compile in less than a second about 1 million lines
* Open Sourced in June 2019 by Alex Medvedniko
* You can play with it here: https://play.vlang.io/

### My Feelings (12.MAR.2023 v-lang version 0.2.4)

* Has a REPL, all langs should have
* Generates C so is like Scala, Clojure, Kotlin to Java or CoffeeScript,TypeScript to JavaScript.
* Very cool and fun lang to code. Really like coding with it, more than Go.
* Imuutable by default, can make it mutable iwith *mut* (great choise, like Scala)
* Really cool that has tests, package manaer(mpm) inside the box.
* Like Go has: Channels and JSON support also neat.
* Memory Mgmt: V has an auto-free engine(compiler insert free call), remaning is handle by reference counting.
* If you know Go, is very fast learning curve
* Eco-system is very new, weak point but there is some libs: https://github.com/vlang/awesome-v. New langs have this issue. 
* Documentation is very raw, missed exmamples and deeper explanations - New langs have this issue.
* C to V translatiom is pretty cool(https://github.com/vlang/c2v) - there is doom ported to V: https://github.com/vlang/doom
* IMHO this is not good(not a big fan on ORM): V has a built-in ORM supports SQLite, MySQL and Postgres. 
* Like Rust has: unsafe
* V has __global ( ) the good thing is that is not default. You need to turn it on.
* I hope the lang grows, have more companies using. 

### Show me the code

My POCs with V: https://github.com/diegopacheco/v-playground <br/>

#### 1 - Similar to Rust, precise memory types
i8, i16, i64, int, u8, u16, u32, u64
```V
i := u8(1)
mut x := i64(1)
```

#### 2 - Powerfull Array options 
len: Number of pre-alocated elements in memory. <BR>
cap: ammount of memory space reserved for elements. Array can grow to this size without being re-allocated. 
```V
mut a := []int{len: 1000, cap: 10000, init: 0}
```
Arrays has several other bult-in functions like: repeat, insert, prepend, trim, clear, first, last, pop, reverse.
Like Go, Zig and Rust you can get a Slice of an array
```V
nums := [0, 10, 20, 30, 40]
println(nums[1..4]) 
```

#### 3 - Type System
Similar to TypeScript in some way like this:
```V
struct Point {
	x int
	y int
}
struct Line {
	p1 Point
	p2 Point
}

type ObjectSumType = Line | Point
```
V has Union as well. 

#### 4 - Strings, not as complicated as Rust and Zig

In V, a string is a read-only array of bytes. All Unicode characters are encoded using UTF-8.
String values are immutable. You cannot mutate elements. But you can concated with other strings(if *mut*).
```V
mut s := 'hello 🌎' // emoji takes 4 bytes
assert s.len == 10
s += " ok"
println("s value is ${s}")

state := 'akaska'
println(state[0])              // Output: 97
println(state[0].ascii_str())  // Output: a

println("I can cast to int like this: ${"42".int()}")
```

#### 5 - runes
Cool name, have to admit. Single unicode chat, alias for *u32* <br/>
U use runes by using (backticks) ``
```
rocket := `🚀`
println(rocket)
```
V has several other cool features like (in operator like Python), enums like most of langs, Pattern Matcher(match), <br/>
Support for Collections on std lib(vlib) for Maps, Sets, LinkedLists, Trees, etc... 

#### 6 - Functional Programing, High Order Functions, Immutability, Lambdas

Like any good modern lang, you see functional programing elements here.
```V
println([1, 2, 3, 4, 5, 6, 7, 8, 9, 10].filter(it%2==0).map(fn (i int) int { return i+1 } )) 
// prints: [3, 5, 7, 9, 11]
```
You can also do .any() or .all()

#### 7 - For Loops like Go
```V
names := ['John', 'Peter']
for i, name in names {
   println('${i}) ${name}')
  // Output: 0) John
  //         1) Peter
}
```
Can also use "_" to ignore a variable, just like Go. <br/>
We can use Ranges in for( i in 1 .. 5), conditional fors, C-style for. 

#### 7 - Heap Struct
Structs by default are allocated on the Stack but you can allocate on the heap by insing: "&". <br/>
```V
struct Point {
	x int
	y int
}
p := &Point{10, 10}
println(p.x)
```
We can make files required with "[required]". Anonnimous structs are also avaliable. 

Similar to Rust, we can add methods/functions in Structs
```V
struct MarryableUser {
   age int
}
fn (u User) can_register() bool {
	return u.age > 21
}
```

We also have Interfaces, that can define fields and methods.
```V
interface Animal {
  name string
  makeSound() string
}
```

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0
* Kotlin: https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Clojure: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Haskell: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* Nim Lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97	
	
#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/