## Tiny Kotlin Essay

created: 12.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Kotlin

* JVM based - great interop with Java both ways
* Created in 2011 by JetBrains(InteliJ company)
* Concisve
* Fun
* OOP and Functional Programing
* Companies using Kotlin: Amazon, Doordash, Expedia, ING, Intuit, N26(bank), Memo Bank, CapitalOne, Netflix, Uber, Pinterest, Google and many more.

### My Feelings (12.FEB.2024 1.9.10)

* Kotlin has a mobile comunity, if is being hit because of Dart/Flutter but is a a has market share.
* Many languages tried to be a better java but i think kotling did it.
* You can see influence from other langs like Scala, Typescript and Go, but with far less complexity.
* Kotlin is not perfect has a bit slower compilation compared with java
* A decent Java engineers can easily learn kotlin in a week.
* IF you are a java shop, the tooling is the same, same way to build, deploy and operate.
* Because of the JVM, use any lib in java open source or that your company might have.
* Java classes are open by default, in kotlin they are close by default, which means they are final, you can open them using the "open" keyword or there is a maven/gradle plugin that allow you todo that at build time. IMHO this is a good idea in principle but in pratice it's not how Java works since everything is proxies.
* IMHO Extensions is the worst thing in Kotlin, looks like the copy from Ruby(methodMissing) - this breaks encapsulation and information hidding - IMHO the most dangerous thing on the language.

### Show me the code

My POCs with Kotlin: https://github.com/diegopacheco/kotlin-playground <br/>

#### 0 - Null the Billion dollars mistake
Nulls are dangerous because represent null and empty sometimes and this is a source of many bugs. Consider a mistake in all languages: 
Kotlin is non-nullable by default, ofcourse we might need nulls to interop with java, just add "?". 
```Kotlin
// return null
fun parseInt(str: String): Int? {
    // ...
}
// cannot return null
fun parseInt(str: String): Int {
    // ...
}

// ?: avoid doing ifs, pretty powerfull.
val list = callingSomethingMightBeNull() ?: listOf("apple")

// another example of Elvis(?) in action
val len = myStr?.length ?: -1 
```

#### 1 - Java Interop
```kotlin
import java.util.*

fun callJava(source: List<Int>) {
    val list = ArrayList<Int>()
    // java foreach here
    for (item in source) {
        list.add(item)
    }
    // range kotlin works on java objects too
    for (i in 0..source.size - 1) {
        list[i] = source[i]             // get and set are called
    }
}
```
We easily can call Kotlin from Java. Let's look some Kotlin code.
```kotlin
// app.kt
package com.diegopacheco.core

class KotlinProcessingService /*...*/ 
fun getOptimalDay(): Int { 
    /*...*/ 
}
```
Now we call kotlin from Java.
```Java
import com.diegopacheco.core.KotlinProcessingService;

KotlinProcessingService service = new KotlinProcessingService();
System.out.println(com.diegopacheco.core.AppKt.getOptimalDay());
```
Same project can have kotlin and Java. 

#### 2 - Range
Like Haskell, Python, Go and Rust, there are range loops.
```kotlin
for (x in 1..5) {
  print(x)
}

for (x in 1..10 step 2) {
  print(x)
}

for (fruit in listOf("apple", "banana", "kiwifruit")) {
  println(fruit)
}
```

#### 3 - Default Parameters and Named Parameters
Like Scala and Python, there are named parameters and defualt values.
```Kotlin
fun print(a:Int = 42, b:String = "ok", flag:Boolean = true) {
    println("a=${a} b=${b} flag=${flag}")
}

print();
print(a=20,flag=false);
```
Such feature might sound silly but actually is quite useful and make the code more readable.
Default values simplyfy api calls, making the code simple to use, of course if they change is a backward compatibility issuse.
Named parameters allow us to understand the method/function without goind to the definition, very useful for complex parameters. 

#### 4 - Lazy vars
Sometimes we need to do lazy initialization to avoid performance issues.
Kotlin has that in the language with the keyword `lazy`.
```Kotlin
val myUser:User by lazy {
    print("Lazy initialization")
    // Heavy DB logic doing tones of aggregation...
    User("Diego", "Pacheco")
}
```

#### 5 - When Expressions
It's a powerful pattern matcher like in Haskell, Scala, Clojure, Rust, Zig.
```Kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
```
See that we can return the `when` and also assign to a variable.

#### 6 - Data Classes
Like Case classes in Scala or Records in Haskell and Clojure.
```Kotlin
data class User(val name: String, val age: Int)
```
Kotlin will give us:
* .equals()
* .hashCode()
* .toString()
* .componentN() functions
* .copy()

#### 7 - Functional Programing - High Order Functions
IF you are use to java Streams, will recognize this, good old functions as parameters and results at the hear of the FP like in Haskell, Scala, Clojure.
```Kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.uppercase() }
    .forEach { println(it) }
```

#### 8 - Singletons in the language
Java programmers use Singletons a lot. Like in Scala in kotlin this is part of the language.
```Kotlin
object Singleton {
   // Code to be executed
}
```

#### 9 - Spring Boot Application
We can use Spring Boot and Kotlin, Spring has great support for Kotlin.
```Kotlin
@Controller
class HtmlController {
  @GetMapping("/") fun blog(model: Model): String {
    model["title"] = "Blog"
    return "blog"
  }
}

@Entity
class User(var login: String,var firstname: String, var lastname: String, var description: String? = null,
           @Id @GeneratedValue var id: Long? = null)

interface UserRepository : CrudRepository<User, Long> {
  fun findByLogin(login: String): User?
}
```
The code is some small we can fit in one file :-) 

#### 10 - Concurrency and Channels
Kotlin has Green Thread(called coroutines) it's a library but part of kotlin.
Like Go channels. Green Threads are great for `waiting` and IO Bound workloads.
```Kotlin
val channel = Channel<Int>()
launch {
    // this might be heavy CPU-consuming computation or async logic, we'll just send five squares
    for (x in 1..5) channel.send(x * x)
}
// here we print five received integers:
repeat(5) { println(channel.receive()) }
println("Done!")
```

#### Useful Links

* https://kotlinlang.org/docs/basic-syntax.html
* https://spring.io/guides/tutorials/spring-boot-kotlin

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Zig: https://gist.github.com/diegopacheco/7d7c8110db68352d58a18b0e3e3c2bb0 
* Clojure: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Haskell: https://gist.github.com/diegopacheco/057087dc7ae236bdd0700014a31c88ef
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Nim lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/