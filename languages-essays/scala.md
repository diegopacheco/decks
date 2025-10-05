## Tiny Scala Essay

created: 15.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Scala

* JVM Based
* Functional Programing
* Highly inspired by Haskell
* REPL
* Great Language

### My Feelings (15.FEB.2024 Scala-lang version 3.3.1)

* OOP and FP
* Great for Big Data
* Compilation is a bit slower
* Great language but a bit complex, like c++ has many options
* I like scala, but also have lots of issue with as a book author (yes I wrote a book about scala 2x)
* My first scala since in prod was deployed in 2010 which was scala 2.x.
* Scala 3 has lots of influence from Python(syntax) and TypeScript on the typesystems.
* For sure make more natural for data engineers to use scala(if is close to python).
* Scala 3 is promissing, but the big issue with scala is the libraries about build targetting a scala runtime i.e 2.10, 2.11, 2.12, etc... Until Java 8, there was no such problem in the java land, since has always had great backward compability up to 8. Other languages have the same story like Zig. 
* Scala deserves respect and wasd trully innovative language(Scala 2x) back on the days. 
* To my suprise - I tought Scala 3 would be a copy of python, yes there is some resanblance with python identation but is not as bad as python is and the syntax is pretty clean and concise(perhaps more haskell than python) wheere {} are optional in most of cases. I live it. 
* Scaka is amazing, this might sound suspecious from a Scala book author but IMHO scala is the best programing language. The issue with scala is when we good too much deep in to the weeds of mathematics, formalism but if you keep in pratical is very very good language. I wish scala was running on Top of Zig it would make it perfect(almost).
* Scala 3 kicks Ass.

### Show me the code

My POCs with Scala: https://github.com/diegopacheco/scala-playground <br/>

#### 1 - Simple Scala 3 application
```Scala
import scala.io.StdIn.readLine
@main def hello(): Unit =
  println("What's up - who are you: ")
  val name = readLine()
  println("Greetings from Scala3, " + name + "! Have a good day.")
```
Scala favors immutability as a functional programing language.
For instance `var` is for mutable state. So value can change.
Hoiwever `val` is immutable, therefore the value cannot change.
As you can see the manin function is shorter than Python now a days.
#### 2 - Named parameters and default values
Scala has a very short way to define functions, which also support default values and named parameters.
```Scala
@main def hello(): Unit =
  val a = 10
  val b = 20
  println(s"${a} + ${b} == ${sum(a,b)}")       // 10 + 20 == 30
  println(s"${a} + 0 == ${sum(a)}")            // 10 + 0 == 10
  println(s"5 + 5 == ${sum(b=5,a=5)}")         // 5 + 5 == 10

def sum(a: Int, b: Int = 0): Int = a + b
```
As you can see Scala also suppprt expression languages(el) in strings. We can evaluate any variable just by doing `${}`
we also can call functions. In order to use this feature you need a `s` in the begining of the string. Scala has explicit typing like in the function `sum` that recives 2 Ints. However Scala also has inplicit typing like in `val a = 10` will be Int but as you see you dont need to pass the type.
#### 3 - Decent Multiline String
A bunch of languages have multiline Strings, Java finnaly is getting that, but scala has it for a long time and is very good.
```Scala
@main def hello(): Unit =
  val color:String = "blue"
  val html: String = 
  s"""
    <html>
    <body>
      <div style="bg-color: ${color}" />
    </body>
    </html>
  """
  println(html)
```
Here not only we are using multiline string to create an HTML output but we also are binding variable using expression language. Such feature is great for doing templating, dealing with serialization like json, yaml or xml.
#### 4 - Control flow assigment and return
```Scala
@main def hello(): Unit =
   var num:Int = 1
   val even = if num%2==0 then true else false
   val odd = 
    if num%2!=0 then
      true
    else
      false
   println(even)
   println(odd)
   println(isEven(2))

def isEven(num: Int) = if num%2==0 then true else false   
```
We can assign ifs to variables either in one line or multiline. It's possible to return directly to the function as well.
Scala allow us todo the same with switch, while and for like this:
```Scala
def print5(): Unit = for i <- List(1, 2, 3, 4, 5) do println(i)
```
Scala loops also support guards (like in Haskell).
```Scala
for
  i <- List(1,2,3,4)
  if i > 2
do
  println(i)
```
Like I was saying before we can assign for loops to var or return but if you want to add them to a collection there is 
a neat operator called `yield` like in Ruby. 
```Scala
def forExpressions():IndexedSeq[Int] =
  for i <- (1 to 10) yield i * 2
```
#### 5 - Pattern Matcher
Like any decent functional programing language having pattern matcher is a must, Scala has a very good one. 
Other languages like Haskell, Clojure, Rust, even Java now a days have it.
```Scala
@main def hello(): Unit =
  pay("NJ")

def pay(selection: Any): Double =
  val tax = selection match
    case 1    => 1.10
    case 2    => 3.45
    case "CA" => 11.78
    case "NJ" => 7.89
    case true => 12.00
    case _    => 15.00
  println(s"You need to pay tax ${tax}")
  tax
```
There is a couple of interesting things here. Last line of a function will be the return, you dont need use the keyword `return`. In order to do pattern matching we need to use the `match` keyword, see that `selection` variable is `Any` which means any type. We can look for any spesific value like integers 1 or 2 but we also can check for booleans or spesific strings, we can do ifs and do very complex rules for matching. Pattern Matcher is a swiss-knife every engineer need to master.
#### 6 - OOP, Traits, Mixins
Scala is not only a functional programing language but also has Object Oriented Programing. Scala mix the 2 paradigms in a very interestint way. Scala has interfaces which are called traits, where we can have signatures or code. 
```Scala
@main def hello(): Unit =
  val max = new TRex("Max")
  println(max.speak())
  println(max.noise())

trait Animal
trait TailAnimal extends Animal
trait Dino extends TailAnimal:
  def speak(): String = "roar"

trait Toy extends Animal:
  def noise(): String = "fluflyuiiiiii "

class TRex(name: String) extends Dino with Toy:
  override def speak(): String = "Roarrrrrrrrrrr! Rawrrrrrrrrr! "
```
Here we have we call a `Type System` or `Relational Algebra` where we define a top level type for demarcation called Animal, them we have traits extending from other traits like TailAnimal. Dino we extends from TailAnimal but we actually have a generic function here. Things get interesting after this. We have Toy which is a differenty tree on the type system. Scala allow multiple inheritance using mixins like in Groovy or C++ using the keyword `with`. Now look the class TRex, where we give it a name string by default immutable, and we extends from Dino but also from toy. 
#### 7 - Case Classes
Case classes are awesome! Specially if you need to deal with DTO/VO objects. Kotlin copied this from Scala and now Java copied from Scala as well. Scala did it first.
```Scala
case class Person(name: String, var profession: String)

@main def hello(): Unit =
  val john  = Person("John","Engineer")
  val marry = Person("Marry","Analyst")

  println(john.name)
  println(john)
  println(john.equals(Person("John","Engineer")))
  println(john == Person("John","Engineer"))

  // john.name = "Petter" //  Reassignment to val name - dont compile
  john.profession = "Prompt Engineer"
  println(john)
```
By default properties in case classes are immutable, but if you put `var` in the front they become mutable. Case classes already provide getters/setters, equals, toString, hashCode and can be present on pattern matcher(`match`).

#### 8 - Union Types
First type i learn TypeScript I've found very cool, specially type utilities and unions. C/Zig has unions but is a different kind of feature. Scala 3 now has some things we have in TypeScript which is pretty awesome.
```Scala
// literal type = 2 <- Int
val two:2 = 2
val dois = two
println(dois)
```
First as you can see here there are type liteals, so actually value `2` is a type and extends Int. This is pretty awesome because now values can make into the type system. But it does not stop here.
```Scala
type Gremio = "Gremio"
type Inter  = "Inter"
def whatsYourTeam(team: (Gremio | Inter)): Unit =
  println(s"your team is ${team}")
```
Here we are defining two types, `Gremio` and `Inter` as you see they are type literals. However look the method whatsYourTeam which recives `(Gremio | Inter)` this `|` is the union operator type it means either `Gremio` or `Inter`. We can do the opposite as well.
```Scala
trait Best:
  def sayIt(): String = "Scala 3"

trait Lang:
  def sayIt(): String = "Language"

class BestLanguage extends Best with Lang:
  override def sayIt(): String = "Scala 3 Language"

def sayTheTruth(theBest: (Best & Lang)): Unit =
  println(s"${theBest.sayIt()}")
```
Here we say there is 2 different traits `Best` and `Lang` we combine them together with `BestLanguage` using mixins. Now look the method `sayTheTruth` we are reciving something that has Best and Lang. 

#### 9 - Collections
Let's see scala collections in action, some lists, sets and maps.
```Scala
 val resList = (1 to 10 by 2).toList 
println(resList)
```
Here we are creating a list based on a range, them we transform the range into a list. Once you have a list here are many methods avaiable like: drop, dropWhile, filter, slice, tail, take, takeWhile and more.

```Scala
val nums = Set(1, 2, 2, 3, 3)           // Set(1, 2, 3)
val letters = Set('a', 'b', 'c', 'a')   // Set('a', 'b', 'c')
val bigger = Seq(4,5,6) ++ nums         // merge List(4, 5, 6, 1, 2, 3) 
println(('A' to 'Z').toList)
```
Here we can see how easy is to deal with Sets, it's also possible to merge Sets with other collections using `++`.
Let's take a look in Map collection in scala.
```Scala
 val states = Map(
   "RS" -> "Porto Alegre",
   "SC" -> "Santa Catarina",
   "SP" -> "Sao Paulo",
   "RJ" -> "Rio de Janeiro",
  )
  for (k, v) <- states do println(s"key: $k, value: $v")

  println(states("RS"))

  val res = states.get("AM") match {
      case Some("Porto Alegre")   => "Bah"
      case Some("Santa Catarina") => "Segue sempre toda vida"
      case Some("Sao Paulo")      => "Entao mano"
      case Some("Rio de Janeiro") => "Caraca meu irmao"
      case _                      => "Eita"
  }
  println(res)

  val bigger = states + ("AM" -> "Manaus") // add to map
  println(bigger)

  val smaller = states - "RJ"              // remove from map
  println(smaller)
```
Map in Scala are awesome, we have a lot of good syntax sugar and is a very nice api, we can do proper pattern matcher with `match`. 

#### 10 - Java Interop
Java can call Scala code. Scala can call Java easily! 
```Scala
@main def hello(): Unit =
  import java.util.StringTokenizer

  val st = StringTokenizer("this is a test Scala3 calling Java 21 api.")
  while (st.hasMoreTokens) do
    println(st.nextToken())
```

#### 11 - Functional Programing
Scala is hybrid, OOP + FP, but remeber is a FP lang, so we have all good things FP langs have like: immutability, concept of fure functions, high order functions, pattern matcher, lambdas, currying, monads, monoids, functors, applicatives etc... Let's see some code example with high order functions - IF you come from java this is called Streams, except Scala had it before java. 
```Scala
@main def hello(): Unit =
  val res = List(1,2,3,4,5,6,7,8,9,10)
            .filter(_ > 3)
            .filter(_ < 7)
            .map(_ * 10)
            .product
  println(res) // 120000
```
Here we are doing function composition, first creating a list from a range 1 to 10, them we are filter only numbers that are bigger than 3, the `_` means each value of the collection being iterated over. There is a second filter now looking for the values that are smaller than 7. Finally the map function each will get each number and multiply by 10. The last two things that happens is, we are calling product (which will reduce and sum all values) and return the result.  

#### 12 - Singleton
Singleton is a big deal and very common pattern in Java. Frameworks like Spring provide a way to get singleton objects but at the runtime. Scala always had singletons as part of the language, just use the keyword `object` thats it you have a singleton. Kotlin copied this from Scala.
```Scala
@main def hello(): Unit =
  import StringUtils._
  println(isNullOrEmpty(null))           // true
  println(isNullOrEmpty("Scala3"))       // false
  println(leftTrim("   Scala3"))         // Scala3
  println(rightTrim("Scala3   "))        // Scala3  

object StringUtils:
  def isNullOrEmpty(s: String): Boolean = s == null || s.trim.isEmpty
  def leftTrim(s: String): String = s.replaceAll("^\\s+", "")
  def rightTrim(s: String): String = s.replaceAll("\\s+$", "")
```
Another cool thing in scala, is that we can do imports inside objects classes or even methods, we can have methods inside methods. Pretty neat!

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
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