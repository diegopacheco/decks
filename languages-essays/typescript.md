## Tiny Typescript Essay

created: 20.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Typescript

* Created in 2012 by Microsoft
* Type System
* Fun
* Reduce Bugs
* Make JS Better
* Superset of JS
* No Performance Overhead

### My Feelings (02.MAR.2024 TS 5.0.0 )

* Fun
* Make Javascript mutch better
* I wish was a runtime language, it's a editor plugin.
* There is no Rutime guarantee, it's weak guarantees in the end of the day.
* Like any type system it could be over complex and go too munch into math and algebra.

### Show me the code

My POCs with Typescript: https://github.com/diegopacheco/typescript-playground <br/>

#### 1 - Interfaces

Typescript has interfaces! Like many languages like Java, Scala(traits), Rust(traits). Interfaces are great because they are a contract between the caller(consumer) and the provider(implementer). Python does not have interfaces, Zig does not have interfaces. IMHO is a mistake not having interfaces in a language, and TS started with the right foot.
```Typescript
interface Person {
  firstName: string;
  lastName: string;
}
 
function greeter(person: Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}
 
let user = { firstName: "John", lastName: "Doe" }; 
console.log(greeter(user));
```
Interfaces in typescript are a little different them you would expect in another language like Java. They are not about the interface type(name) but they are about the functions, you have the same functions you have same interface, even with different names. As you see on the code we can create an annonymous object with `{}` and because we have the properties that is required is the same interface. Interfaces in TS are very fluid.

#### 2 - Type
As the name of the language sugest: `Typescript` it's javascript with types. We do have a type system in TS, there is a `number` type, `string` type, `Array` type and many other types. TS also has something called type alias which is represented by `type` keyword. This is the same future as Scala has. 
```Typescript
type Email = String;

function printMail(mail:Email):void {
    console.log(mail);
}

let johnMail:Email = "jhon@doe.com";
printMail(johnMail);

//
// Argument of type 'number' is not assignable to parameter of type 'String'.ts(2345)
//
// printMail(42);
```
`type` alias can be used to represent literal values(also know as literal types like we have in scala), or we can be just a shortcut for well stablished type. We can use type alias to capture all sort of type representations from constructors, parameters, results types, generics, etc...
```Typescript
type Team = "Gremio" | "Inter";

function printTeam(team:Team):void {
    if ("Gremio"===team){
       console.log("The best ${team} !")
    } else {
        console.log("meh ok ${team} !")
    }
}

printTeam("Gremio");

//
// Argument of type '"Juventude"' is not assignable to parameter of type 'Team'.ts(2345)
//
// printTeam("Juventude");
```
Here the `type` `Team` is either `Gremio` or `Inter` it does not allow other values, we could use a enum or class to handle same problem but is nice that we can do that with type alias is much more fluid. 

#### 3 - Object Types
Objects `{}` are types too. We can use the type alias, we can point to a class, interface, enum or could a annonymous object. We could also expect objects as return or parameters of functions.
```Typescript
type Config = {
    theme: "Light" | "Dark"
};

let draculaDeveloper:Config = {
    theme: "Dark"
}
console.log(draculaDeveloper);
```
Config is a object, we expect to have the property `theme` however can only accept 2 values `Light` and `Dark`. If you use a proper TS editor like in VS Code, vim with lsp, whatever you can get auto-complete from this things which is the point of TS in order to reduce bugs bu suggesting the right values reduce the chances of mistakes.

#### 4 - Optinal Fields
Typescrit has this notion of mandatory fields and optional fields. Given a interface we can define what is required and therefore mandatory and what is optional. 
```Typescript
interface LogOptions {
  sufix: string;
  id?: number;
  reason?: number;
}

function printMe(s: String, opt: LogOptions): void {
  let extra: String = "";
  if (typeof opt.id !== "undefined") {
    extra += "id: " + opt.id + "";
  }
  if (typeof opt.reason !== "undefined") {
    extra += "reason: " + opt.reason + "";
  }
  console.log(opt.sufix + " " + s + " " + extra);
}

printMe("Optinal Properties in Interfaces", {
  sufix: "***",
  id: 42,
});
```
Optinal control is done by the elvis operator `?` which is present in other languages like Kotlin, Zig and Groovy. Which means in other words we allow to be `undefined`. We can check the presence of a field with the `typeof` operator. 

#### 5 - Union
Union types allow us to choose between a finite range of types. Scala has the same feature. 
```Typescript
function debug(id: number | string) {
  console.log("Your ID is: " + id);
}

debug(25);
debug("123");
```
Here we say we accept either `number` or `string` but this could be done with other types, our own types, type alias, classes, interfaces, objects. Here is a example with union using objects.
```Typescript
function validate(cc: { num:Number } | { num:string }):bool {
  if (typeof cc.num !== "undefined"){
    let n:number = +cc.num;
    if (n > 0) {
      return true;
    }
  }
  return false;
}

console.log(validate( {
  num: 42
}));
console.log(validate( { num: "42" }));
console.log(validate( { num: "-1" }));
```

#### 6 - Intersection
Intersection is the opposite of Union, imagine you need to combine different functions or different fields from different objects, classes, interfaces or types. Same feature exists in Scala.
```Typescript
type Port = {
    port: number,
};
type Host = {
    hostname: string,
};

function validate(config: Host & Port){
    console.log(config.port);
    console.log(config.hostname);
}

let devServer = {
    hostname: "127.0.0.1",
    port: 3000,
};

validate(devServer);

validate({
    hostname: "127.0.0.10",
    port: 3030,
});

validate({
    hostname: "127.0.0.100",
    port: 8080,
} as Host & Port);
```
As you see when we call the function `validate` we can use our variable `devServer` or we can pass an annonymous object block or we can also convert to a Intersection between to types with `as Host & Port`.
```Typescript
interface PortProvider{
    getPort(): number
}

interface HostProvider{
    getHostname(): string
}

function debug(conf: PortProvider & HostProvider): void {
    console.log(conf.getHostname() + " - " + conf.getPort());
}

let local = {
    getHostname(){
        return "localhost";
    },
    getPort(){
        return 3000;
    }
}
debug(local);
```
Here we are doing the name but now we are using interfaces and we expect 2 functions to be present.

#### 7 - Readonly amd Keyof
Readonly is another interesting feature, other languages often have this of functional programing being immutable vs mutable, scala, rust, haskell, clojure all have that. See readonly as something that should be change just be read.
```Typescript
interface ReadonlyTaxArray {
  readonly [index: number]: string;
}

let taxArray: ReadonlyTaxArray = [ "7%", "8%", "12%", "20%", "40%"];
console.log(taxArray);

// Index signature in type 'ReadonlyTaxArray' only permits reading.ts(2542)
taxArray[0] = "100%";

// TS complain but let you do it.
console.log(taxArray);
```
Here is where TS start letting me down a little bit, and I wish this was enforced in runtime, as you can see, the editor will complain but you can run this code and the mutation will happen just fine. Test this in node 20, deno and bun 1x. Even with `"Use Strict;"` there is the same problem, sure we could block this with `eslint` and part of the "build" but again no rutime guarantee.

`keyof` allow us to represent all properties of an object as a type. For instance the type `Point` has `x` and `y` coordinates pairs and both are the type number. This is useful for validations and dynamic programing.
```Typescript
type Point = { 
    x: number; 
    y: number 
};

// Property of Object Point 
type P = keyof Point;

let prop1:P = "x";
let prop2:P = "y";

// would not work
//let prop3:P = "z";

let p1 = {
    x: 10,
    y: 20
} as Point;
console.log(p1);
console.log(p1[prop1]);
console.log(p1[prop2]);
```
`P` is a keyof `Point` via `type P = keyof Point;` now we can use P type to acce the object like happens here `p1[prop1]`.

#### 8 - Decorators
Decorators are like Java, Scala and Kotlin annotations. They allow us to do meta-programing and use data to describe data. Decorators allow us to be declarative and build lots of intersting generic feature around types in a dynamic fashion. Some people dont like annotations I get it, but this is cool for me.
```Typescript
function logger(originalMethod: any, _context: any) {
  function replacementMethod(this: any, ...args: any[]) {
    console.log("start:", originalMethod.name);
    const result = originalMethod.call(this, ...args);
    console.log("end:", originalMethod.name);
    return result;
  }
  return replacementMethod;
}

class User {
  constructor(private name: string, private age: number) {}

  @logger
  greet() {
    console.log("start: greet");
    console.log(`Hello, my name is ${this.name}.`);
    console.log("end: greet");
  }
}

const user = new User("Harry Potter", 16);
user.greet();
```
More here: https://www.typescriptlang.org/docs/handbook/decorators.html and another good read https://blog.logrocket.com/practical-guide-typescript-decorators/

#### 9 - Generic Functions and Class
Like you would do in Java, Scala, Haskell and Kotlin. Typescript allow generic functions and classes.
```Typescript
interface Wrapper<T> {
  contents: T;
}

type WrapperString = Wrapper<string>;

let toys: WrapperString = { contents: "23 toys" };
let pens: Wrapper<Number> = { contents: 11 };

console.log(toys,pens);
```
Such feature is great for creating data structures and ADTs(Abstract Data Types). I'm using an interface in this sample but it could be done with a function too. Typescript has something also called `conditional types` where literely you can do a IF and based on the result of this IF you either has one type or another(which is pretty cool).
```Typescript
interface Animal {
  speak(): void;
}
  
class Dog implements Animal {
    speak(): void {
        console.log("wolf wolf");
    }
}

class Cat implements Animal {
    speak(): void {
        console.log("meown meown");
    }
}

class Elephant{
    noise(): void {
        console.log("growls aaaarh");
    }
}

type Pet = Elephant extends Animal ? Elephant : Cat;
type PetGeneric<T> = T extends Animal ? T : Cat;

let flufly:Pet = new Cat();
console.log(flufly);
console.log(flufly.speak());

let max:PetGeneric<Dog> = new Dog();
console.log(max);
console.log(max.speak());


//
// This works (because ts looks fot he functions not the class name)
//
let max3:PetGeneric<Elephant> = new Dog();
console.log(max3);
console.log(max3.speak());

//
// dont work
//
// Property 'speak' is missing in type 'Elephant' but required in type 'Cat'.ts(2741)
//
//let max4:PetGeneric<Elephant> = new Elephant();
//console.log(max4);
//console.log(max4.speak());
```
Like I said before, no runtime guarantee, but pretty cool.

#### 10 - Utility Types
I like utility types a lot! Here is where TS is quite unique. I dont remember seeing this in other languages. But there are types that helps us to contruct types based on other types: https://www.typescriptlang.org/docs/handbook/utility-types.html.
There are many utility types like: 
* Partial<Type> to extract a subset of a existent type.
* Pick<Type, Keys> where you can cherry pick properties of a existent type.
* Omit<Type, Keys> get all properties but ommit some, it's a good filter.
* ReturnType<Type> construct a type beased on a return type.
* Uppercase<StringType>, Lowercase<StringType>, Capitalize<StringType>, Uncapitalize<StringType> for advanced string manipulation with types - allow us to get only the getter types, or setter types or whatever pattern we want
And much much more...   
```Typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```
Here we have the interface Todo and we just want tittle and completed we do want description this is what Pick will do. We could use Ommit here as well. 

#### Good TS Resources

* Typescript playground https://www.typescriptlang.org/play
* Typescript handbook https://www.typescriptlang.org/docs/handbook/intro.html

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