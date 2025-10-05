## Tiny Gleam Essay

created: 02.JAN.2025

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - 2023 I learned Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 2024: I learned C3.

### Why Gleam

* Created in 2016 by Louis Pilfold
* Based on Erlang
* Fun
* Modern
* Erlang/Javascript Targets
* Friendly with Typesystem 

### My Feelings (02.JAN.2025 Gleam 1.6.3 )

* Fun
* I like it quite a lot.
* Makes earlang easy and cool.
* Has a lot of Functional programing influence: Type system, algebric data types, pattern matcher, immutable objects.
* Documentation is amazing, for learning.
* Has some libraries and they up to date.
* It's easy to create new projects: `gleam new` and also run `gleam run` or run tests `gleam test`
* The syntax feels a lot like Rust

### Show me the code

My POCs with Gleam: https://github.com/diegopacheco/gleam-playground <br/>

#### 1 - High Order Functions

You can't do functional programing without proper high order functions.

```Gleam
import gleam/io

pub fn main() {
  // Call a function with another function
  io.debug(twice(1, add_one))

  // Functions can be assigned to variables
  let my_function = add_one
  io.debug(my_function(100))
}

fn twice(argument: Int, passed_function: fn(Int) -> Int) -> Int {
  passed_function(passed_function(argument))
}

fn add_one(argument: Int) -> Int {
  argument + 1
}

```

#### 2 - Pipelines

Allow chaining several functions in a coheseve chain. Similar to Streams in Java or Seq in Scala.

```Gleam
import gleam/io
import gleam/string

pub fn main() {
  // Without the pipe operator
  io.debug(string.drop_start(string.drop_end("Hello, Joe!", 1), 7))

  // With the pipe operator
  "Hello, Mike!"
  |> string.drop_end(1)
  |> string.drop_start(7)
  |> io.debug

  // Changing order with function capturing
  "1"
  |> string.append("2")
  |> string.append("3", _)
  |> io.debug
}
```

#### 3 - Labeled Arguments

I like this feature a lot. It make very easy to get the wrong order or arguments, this fix the problem.

```Gleam
import gleam/io

pub fn main() {
  // Without using labels
  io.debug(calculate(1, 2, 3))

  // Using the labels
  io.debug(calculate(1, add: 2, multiply: 3))

  // Using the labels in a different order
  io.debug(calculate(1, multiply: 3, add: 2))
}

fn calculate(value: Int, add addend: Int, multiply multiplier: Int) {
  value * multiplier + addend
}
```

Similar thing as Scala 3, Python have. Such a nice feature.

#### 4 - Pattern Matcher

Like any food modern language (influenced by FP).

```Gleam
import gleam/int
import gleam/io

pub fn main() {
  let result = case int.random(5) {
    // Match specific values
    0 -> "Zero"
    1 -> "One"
    // Match any other value and assign it to a variable
    other -> "It is " <> int.to_string(other)
  }
  io.debug(result)
}
```

#### 5 - Custom Types

Gleam allow us crete custom types, gleam also supports tuples natively. Here is an example, this is like an enum in Scala 3, java, kotlin.

```Gleam
import gleam/io

pub type Season {
  Spring
  Summer
  Autumn
  Winter
}

pub fn main() {
  io.debug(weather(Spring))
  io.debug(weather(Autumn))
}

fn weather(season: Season) -> String {
  case season {
    Spring -> "Mild"
    Summer -> "Hot"
    Autumn -> "Windy"
    Winter -> "Cold"
  }
}

```

We can use custom types not only to have enums but also Case Classes like we do have in scala, our Records for java.

```Gleam
import gleam/io

pub type SchoolPerson {
  Teacher(name: String, subject: String)
  Student(String)
}

pub fn main() {
  let teacher1 = Teacher("Mr Schofield", "Physics")
  let teacher2 = Teacher(name: "Miss Percy", subject: "Physics")
  let student1 = Student("Koushiar")
  let student2 = Student("Naomi")
  let student3 = Student("Shaheer")

  let school = [teacher1, teacher2, student1, student2, student3]
  io.debug(school)
}
```

Want some Monads? We can use custom types to create generics and advanced types like the Monad Maybe(Option).

```Gleam
pub type Option(inner) {
  Some(inner)
  None
}

// An option of string
pub const name: Option(String) = Some("Annah")

// An option of int
pub const level: Option(Int) = Some(10)

```

#### 6 - Lists

Like any decent modern language, gleam has support for lists.

```Gleam
import gleam/io
import gleam/list

pub fn main() {
  let ints = [0, 1, 2, 3, 4, 5]

  io.println("=== map ===")
  io.debug(list.map(ints, fn(x) { x * 2 }))

  io.println("=== filter ===")
  io.debug(list.filter(ints, fn(x) { x % 2 == 0 }))

  io.println("=== fold ===")
  io.debug(list.fold(ints, 0, fn(count, e) { count + e }))

  io.println("=== find ===")
  let _ = io.debug(list.find(ints, fn(x) { x > 3 }))
  io.debug(list.find(ints, fn(x) { x > 13 }))
}

```

#### 7 - Result

Like I said, Gleam give me Rust feelings, we can unwrap here too!

```Gleam
import gleam/int
import gleam/io
import gleam/result

pub fn main() {
  io.println("=== map ===")
  let _ = io.debug(result.map(Ok(1), fn(x) { x * 2 }))
  let _ = io.debug(result.map(Error(1), fn(x) { x * 2 }))

  io.println("=== try ===")
  let _ = io.debug(result.try(Ok("1"), int.parse))
  let _ = io.debug(result.try(Ok("no"), int.parse))
  let _ = io.debug(result.try(Error(Nil), int.parse))

  io.println("=== unwrap ===")
  io.debug(result.unwrap(Ok("1234"), "default"))
  io.debug(result.unwrap(Error(Nil), "default"))

  io.println("=== pipeline ===")
  int.parse("-1234")
  |> result.map(int.absolute_value)
  |> result.try(int.remainder(_, 42))
  |> io.debug
}

```

#### 8 - Use

Gleam does not have exception and macros. So we can rely on Result and also `use` in order to have fallbacks.

```Gleam
import gleam/io
import gleam/result

pub fn main() {
  let _ = io.debug(without_use())
  let _ = io.debug(with_use())
}

pub fn without_use() -> Result(String, Nil) {
  result.try(get_username(), fn(username) {
    result.try(get_password(), fn(password) {
      result.map(log_in(username, password), fn(greeting) {
        greeting <> ", " <> username
      })
    })
  })
}

pub fn with_use() -> Result(String, Nil) {
  use username <- result.try(get_username())
  use password <- result.try(get_password())
  use greeting <- result.map(log_in(username, password))
  greeting <> ", " <> username
}

// Here are some pretend functions for this example:

fn get_username() -> Result(String, Nil) {
  Ok("alice")
}

fn get_password() -> Result(String, Nil) {
  Ok("hunter2")
}

fn log_in(_username: String, _password: String) -> Result(String, Nil) {
  Ok("Welcome")
}

```

#### 9 - Externals

Aloow us to import and use non-Gleam code: Erlang/Exlir or JavaScript.

```Gleam
import gleam/io

// A type with no Gleam constructors
pub type DateTime

// An external function that creates an instance of the type
@external(javascript, "./date.js", "now")
pub fn now() -> DateTime

// The `now` function in `./date.js` looks like this:
// export function now() {
//   return new Date();
// }

pub fn main() {
  io.debug(now())
}

```

#### 10 - Tail Calls

Gleam has default tail recursion optimization.

```Gleam
import gleam/io

pub fn main() {
  io.debug(factorial(5))
  io.debug(factorial(7))
}

pub fn factorial(x: Int) -> Int {
  // The public function calls the private tail recursive function
  factorial_loop(x, 1)
}

fn factorial_loop(x: Int, accumulator: Int) -> Int {
  case x {
    0 -> accumulator
    1 -> accumulator
    // The last thing this function does is call itself
    // In the previous lesson the last thing it did was multiply two ints
    _ -> factorial_loop(x - 1, accumulator * x)
  }
}
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
* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/