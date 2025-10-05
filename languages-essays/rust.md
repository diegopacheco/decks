## Tiny Rust Essay

created: 20.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Rust

* Secure
* Fast, really fast
* Better C++
* Research in 2006, for Mozilla browser engine(Servo). Open Sources by Mozilla in 2015

### My Feelings (20.FEB.2024 Rust version 1.78.0-nightly)

* Fast, really fast, much more than Scala, Go, Kotlin, Clojure. This is a real deal!
* Learning Curve is a problem
* Learning takes time (Borrow and Onership), compailing in not easy in the beginning. You will fight the compiler !!!
* Lots of FANG / Sillicon Valley companies are using it
* Once you have +300 crates as dependencies things get slow to build
* Once you need to use `unsafe` is here rust let you down, where zig might shine.
* Great Eco-system
* Rust Foundation Drama
* Tooling and documentation improved a lot since (2019)

### Show me the code

My POCs with Rust: https://github.com/diegopacheco/rust-playground <br/>

#### 1 - Immutable vs Mutable
Like any decent modern language there is a clear difference between mutability and immutability.
Let's take a look in this code in Rust.
```Rust
fn main() {
    let cant_change:f64 = 3.14;
    println!("{}", cant_change);  // 3.14

    //  compiler error
    // cant_change = 4.5;
    // error[E0384]: cannot assign twice to immutable variable `cant_change`

    let mut can_change;    // i32
    can_change = 42;
    can_change += 1;
    println!("{}", can_change); // 42
}
```
Rust has immutable variables by default like Scala. IF you want be able to change a value of a variable you need the keyword `mut` after `let`. Rust is flexible with types, you can either let Rust figure it out or you can be spesific(just like Scala).

#### 2 - Pattern Matcher
Like any post-functional programing language (Scala, Kotlin, Clojure) Rust also has Pattern Matcher, which is very very similar with the one in Scala.
```Rust
 match words {
        // Ignore everything but the last element, which must be "!".
        [.., "!"] => println!("!!!"),
        // `start` is a slice of everything except the last element, which must be "z".
        [start @ .., "z"] => println!("starts with: {:?}", start),
        // `end` is a slice of everything but the first element, which must be "a".
        ["a", end @ ..] => println!("ends with: {:?}", end),
        rest => println!("{:?}", rest),
    }
```
First expression ignores everything but wants to end with `!`. Second expression could be anytthig where the last element is `z` and the last one we see the opposite it needs to start with `a` but we dont care the end. Pattern matcher words for varios types, Vectors, and much more.

#### 3 - Borrow & Onwership
This is the big deal. It's both why rust shines and also why is difficult language. Most of the problems with c and c++ are related to memory magement, null pointers, dandle pointers, overflows etc... Rust has a very different take on this problem, where you have a more disciplined code (Safe) and the compiler helps you a great deal to write memory safe programs. However if you need to resort to `Unsafe` is where you are in c++ land again and rust loses value.

There is a set of rules know as Borrower and Ownership that shift the law's of physics to engineers. Have you ever imagine to call an function and them call it twice and stop working? :-) This code does not compile!
```Rust
fn main() {
    moved_values_borrowing_to_fix()
}

fn moved_values_borrowing_to_fix(){
    let a = String::from("Hello World");
    let b = a;
    
    // dont compile
    println!("{:?}",a);  // error[E0382]: borrow of moved value: `a`
    println!("{:?}",b);
}
```
Luckly the compiler errors in Rust imporved a great deal, you will get this:
```bash
error[E0382]: borrow of moved value: `a`
  --> src/main.rs:10:21
   |
6  |     let a = String::from("Hello World");
   |         - move occurs because `a` has type `String`, which does not implement the `Copy` trait
7  |     let b = a;
   |             - value moved here
...
10 |     println!("{:?}",a);  // error[E0382]: borrow of moved value: `a`
   |                     ^ value borrowed here after move
   |
   = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
   |
7  |     let b = a.clone();
   |              ++++++++

For more information about this error, try `rustc --explain E0382`.
```
There are several fixes we could do like:
* (A) Comment out the first print `println!("{:?}",a);` // ok solution if you dont need it - but not really good of all cases)
* (B) Clone the data `let b = a.clone();`               // works in some cases but uses more memory
* (C) Borrower a copy `let b = &a;` using the `&`       // better

Some problem will happen with different methods, or methods that dont return anything and do it all by reference, with methods we have the previous options plus (you could make the mothod return) and there is the `move` keyword if you want to explicit move the ownership by converting any variables captured by reference or mutable reference to variables captured by value.
Such code here, dont compile
```Rust
fn main() {
     let str = String::from("Hello Rust?");
     print_string(str.clone());
     print_string(str); // error[E0382]: use of moved value: `str`
}
fn print_string(s:String){
    println!("{:?}",s);
}
```
To fix, we can use clone or we can make print_string return the String for instance. 

#### 4 - Control Flow
Control flow is the bread and butter of any language. Like Scala and Zig, Rust control flow allow us to store the result of ifs, pattern matcher to variables or reutrn on the function. Rust has something really killer called if let.
```Rust
fn main() {
    let x = Some(42);
    if let Some(i) = x {
        println!("if_let working {}",i);
    }else{
        println!("oops!");
    }
}
```
IF let(is the default if and default while in Zig), allow to some code run only if is sound, meaning valid, them only the variable will be bindinged, this is perfect to work with errors/failures.

Rust also allow us to have IFs in a short form(1 liners) like Scala.
```Rust
use rand::Rng;

fn main() {
  let mut rng = rand::thread_rng();
  let temperature:i16 = rng.gen_range(20, 100);
  let result = if temperature<=30 {"cold"} else {"hot"};
  println!("{}",result);
}
```

Rust, like Scala, Kotlin, Go, Python, has range in loops.
```Rust
for x in 0..10 {
  print!("for {}", x);
}
```

#### 5 - Enums and Traits
Rust has enums, like most of langs.
```Rust
enum Status {
    Single,
    Married,
}

fn main(){
  let mut status:Status = Married;
  match s {
        Single => println!("Go do some party!"),
        Married => println!("Go buy some diapers!"),
    }
}
```
Enums can be part of pattern matcher(like in Scala).

Rust has interfaces in form of Traits (like Scala). Rust has Structs(that can behave like classes) in rust a struct can implement one or many traits. 
```Rust
    trait Animal {
        fn name(&self) -> &'static str; 
        fn talk(&self){
            print!("{} cannot talk",self.name());
        }
    }
    
    struct Human{
        name: &'static str
    }

    impl Animal for Human {
        fn name(&self) -> &'static str {
            return self.name;
        }
        fn talk(&self){
            print!("{} says hello",self.name());
        }
    }
```
Here we have an `Animal` treait and a `Human` Struct. Wich `impl Animal for Human` we make Human an animal and therefore we need suplement the function talk. Here you can see `self` like you would see in Python or Zig. IF you wonder like you really need OOP there is this interesting point of view from Erlang creator(https://harmful.cat-v.org/software/OO_programming/why_oo_sucks) besides some controvesial points there is some interesting points of view on post. IMHO OOP can be good is this is a matter schools of tought and how you do things. 

#### 6 - Concurrency
Rust has support for OS Threads in the language. There is support of Green Threads but mostely via libs like Tokyo(https://tokio.rs/).
```Rust
use tokio::net::TcpStream;

async fn my_async_fn() {
    println!("hello from async");
    let _socket = TcpStream::connect("127.0.0.1:3000").await.unwrap();
    println!("async TCP operation complete");
}

#[tokio::main]
async fn main() {
    let what_is_this = my_async_fn();
    // Nothing has been printed yet.
    what_is_this.await;
    // Text has been printed and socket has been
    // established and closed.
}

```
Rust has support for Mutex and lockers and also Channels. Tokyo has other concurrency possibilities like Futures and Channels(like Go and Kotlin). Here is a good comparison between OS/Green Threads for the JVM: https://gist.github.com/diegopacheco/a81e6dc67de5548c670f6d360304ed80

#### 7 - Functional Programing & OOP-Ish
Functional Programing with Higher Order Functions.
```Rust
   (1..1000)
    .filter(|&x| x % 2 == 0)
    .filter(|&x| x % 3 == 0)
    .take(5)
    .map(|x| println!("{}",x));
```
I would say Rust is more functional than OOP-ish. There is some level of OOP but not much.
You can have something similar to classes with Struct, in rust Struct support generics, so you can do this:
```Rust
struct Point<T>{
    x: T,
    y: T
}
```
Like I mention before, you can use Traits(interfaces) and implement traits for Structs. 

Rust has a lot of FP unfluence on it, like in Haskell or Scala, in Rust we can have functions to depend on parameter traits, this allow us to create powerful, generic and yet type safe functions. Check this out:
```Rust
fn print_me<T> (value: &T) where T: std::fmt::Debug {
    println!("value is {:?}",value);
}
```
`print_me` function expected a generic value `T` but there is a requirement(like in Haskell or Scala) T needs to implement the Trait Debug, which could be directely implemented or derived in case of structs. Now look how we can this function.
```Rust
fn main(){

    #[derive(Debug)]
    struct IntPoint{
        x: i32,
        y: i32
    }

    let i = 42;
    print_me(&i);
    print_me(&"hello".to_string());
    print_me(&true);
    print_me(&IntPoint{ x:1, y:2});

    struct CantDebug{}
    // compilation error - error[E0277]: `CantDebug` doesn't implement `Debug`
    // print_me(&CantDebug{}); // `CantDebug` cannot be formatted using `{:?}`
}
```
It's possible to require multiple traits combined. As you can see the struct `IntPoint` is deriving Debug trait thats why we can pass it along to `print_me`. Structs can have data + functions and implement interfaces(traits) in the end of the day it's not super differnt from a class. 

Another killer thing in Rust is that by default things are private, specially in struct, versus in Zig(0.11 at least structs fileds are all public and you cant make it private, I understand the reason is performance, but I prefer Rust approach on this one).

We can easily turn a Struct into Json or Json intro Struct using serder(https://crates.io/crates/serde)
person.rs
```Rust
#[derive(Serialize, Deserialize, Debug)]
pub struct Person {
    pub name: String,
    pub age: u8,
    pub address: Address,
    pub phones: Vec<String>,
}

#[derive(Serialize, Deserialize, Debug)]
pub struct Address {
    pub street: String,
    pub city: String,
}
```
main.rs
```Rust
fn main() {
    // Json to Struct
    let data = r#" {
        "name": "John Doe", "age": 43,
        "address": {"street": "main", "city":"Downtown"},
        "phones":["27726550023"]
       } "#;
    let p: Person = serde_json::from_str(data).expect("deserialize error");
    println!("{:#?}", p);

    // Struct to Json   
    let jd = Person{
        name: "John Doe".into(),
        age: 62,
        phones: vec!("12354334534".into()),
        address: Address{
            street: "123 popular st".into(),
            city: "Wonderland".into(),
        }
    };
    println!("{:#?}", serde_json::to_string(&jd));
}

```

#### Good Rust Resources

* A Gentle introduction to Rust https://stevedonovan.github.io/rust-gentle-intro/readme.html
* The Rust programing language reference book https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html
* Rust by Example https://doc.rust-lang.org/rust-by-example/index.html

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
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