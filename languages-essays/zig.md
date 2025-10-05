## Tiny Zig Essay

created: 12.FEB.2024

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Zig

* Lightning Fast, really!
* No Hidden control flows
* No Hidden memory allocations
* No preprocessor, no macros(but Zig has Comptime)
* Modern and Fun language
* Created by Andrew Kelley in 2016

### My Feelings (12.FEB.2024 1.9.10)

* I love new Languages - Zig is very promissing system language.
* Zig makes unsafe code easier than unsafe Rust
* Get your C or C++ project and continue where you left.
* Zig compiler compiles to C and cross-compile like a charm.
* IF you like performance like I do, Zig is the lang for you.
* Documentation and examples are difficult to find(new language problem)
* Compiler errors are difficult and sometimes you have a coredump/segfault. (should improve over time)
* You can easily wrap C lib and call in zig, but there is not many libs in Zig
* Like Scala zig is breaking binary compatibility between version 0.10, 0.11, 0.12. 
* IMHO is a great language, it's a bit hispster and niche at the momnent but very very very promissing
* I dont like the fact that there is no Interfaces, Strings are a bit painful, but besides that is amazing.
* Overall I love Zig, you should give it a shot.
* Companies using Zig: Uber, TiggerBettle(Finantial Accounting DB written in Zig), Oven(company bevind Bun - fast nodejs runtine written in zig).

### Show me the code

My POCs with Zig: https://github.com/diegopacheco/zig-playground<br/>

#### 0 - Comptime
Macros suck in any language. Because macros often have another syntax and they are painfull in C. Zig has a complete fresh approach for that, you can have some code running in compile time just using Zig, same language same syntax. Call any function at compile-time. Manipulate types as values without runtime overhead. Comptime emulates the target architecture. Zig uses compatime for generic types.
```Zig
comptime {
  // Zig code
}
```
#### 1 - Compiler, Performance and Basics
Zig is really about efficiency. You can track Zig performance here: https://ziglang.org/perf/
Zig compilere can cross-compile to mac(M1), windows, Linux, WebAssembly. 
Zig build system is written in Zig and you code in zig to build your projects.
Here is how to build it. 
```Zig
zig build
```
By default variables are not null.
```Zig
const notNull: i32 = 42;        // const(immuable), cant change - cant be null
var mightBeNull: ?*i32 = null;  // var(muttable) ? means it can be null is it's optinal
```
Control flows like `if` and `for` check for optinals for us, pretty neat. 
```Zig
if (value) | v | {
   std.debug.print("Not Null: {}\n", .{v.*});
} else {
   std.debug.print("Oopsy Daise !!! Null! \n", .{});
}
```
We can assign and return ifs/fors as well pretty awesome.

#### 2 - Structs
Like Rust, Zig has struct, however Zig dont have traits.
```Zig
const std = @import("std");

const Point = struct{
    x: f64 = 0.0,
    y: f64,
};

pub fn main() void {
    var p: Point = .{.y = 0.1};
    std.debug.print("v: {}\n", .{p});
}
```
We can use structs to create code similar to objects, by default everything is public, I dont link this well, but zig did it for performance reasons, they recomend you document your api. 

#### 3 - Loops
Zig has powerful loops, like in Python, Rust and Kotlin.
```Zig
const string = [_]u8{ 'Z', 'i', 'g', '!' };

for (string, 0..) |character, index| {
    _ = character;
    _ = index;
}

for (string) |character| {
    _ = character;
}

for (string, 0..) |_, index| {
    _ = index;
}
```
#### 4 - Error Handing
IMHO Zig has state of art in error handling. Go is pretty annoying because you need todo a IF in every function call. Java is good because of RuntimeException but Zig beats java.
```Zig
const MyError = error{
    GenericError,  
};

pub fn main() !void {
    return MyError.GenericError;
}
```
IF your function might return an error - you just add "!" alongside with the result.
The caller just do `try` and them it can ignore the error, if the caller wants to handle the error there is a `catch`.
Here is a try example:
```Zig
fn echo(v: i32) !i32 {
    if (v == -1) return MyError.GenericError;
    return v;
}

pub fn main() void {
   _ = try echo(100);   
}
```
Here is an example with catch:
```Zig
_ = foo(-1) catch |err| {
    std.debug.print("error: {}\n", .{err});
};
```

#### 5 - Strings
String literals goes to a special area in a program (similar to Rust).Zig compiler does something called String interning to remove duplicates. But like Erlang, Strings are painful in Zig. The benefit is performance! Strings are just byte arrays in Zig, you want a dynamic string? you need an allocator.
```Zig
fn return_string_literal() []const u8 {
    return "It works!";
}
```
Here are a series of manipulations(commons recipies) for Strings in Zig: https://github.com/diegopacheco/zig-playground/blob/main/chars-strings-pocs/src/main.zig

#### 6 - Allocators
Most of types and things in zig leave on the stack, however some dynamic programing, datastructure and Strings will require usage of Heap, theregore Zig has allocators, what is nice is that is very explicit, you need to alloc and dealloc memory, usually in structs there is a `init` and `deinit` functions reciving alloctor. Zig has many allocators like:
* std.heap.page_allocator          // Ask OS for entire pages.
* std.heap.ArenaAllocator          // Like an arena(cencert/show), allows you to allocate many times and only free once.
* std.heap.FixedBufferAllocator    // Allocates memory into a fixed buffer and does not make any heap allocations
```Zig
var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
defer arena.deinit();
const allocator = arena.allocator();

_ = try allocator.alloc(u8, 1);
_ = try allocator.alloc(u8, 10);
_ = try allocator.alloc(u8, 100);
```
Zig has also a test allocator capable of detecting memory leaks, it's pretty useful. 

#### 7 - Interop with C
Let's see how easy is to call Zig from C.

mathtest.h
```C
int add(int, int);
```
now let's write a simple lib in Zig that implements this header file

mathtest.zig
```Zig
export fn add(a: i32, b: i32) i32 {
    return a + b;
}
```
Let's call this in C.

test.c
```C
#include "mathtest.h"
#include <stdio.h>

int main(int argc, char **argv) {
    printf("Running C and Zig all compiled together WOW \n");
    int result = add(42, 1337);
    printf("%d\n", result);
    return 0;
}
```
More here: https://github.com/diegopacheco/zig-playground/tree/main/zig-cross-compile

#### Useful Links

* [Comptime](https://medium.com/@edlyuu/zig-comptime-wtf-is-zig-comptime-and-inline-e6ea40e0cb41)
* [Zig language reference](https://ziglang.org/documentation/0.11.0/)
* [Learn Zig by example](https://zig-by-example.com/)
* [Zig in 30min](https://gist.github.com/ityonemo/769532c2017ed9143f3571e5ac104e50)
* [Stack memory](https://www.openmymind.net/learning_zig/stack_memory/)
* [Awesome Zig](https://github.com/C-BJ/awesome-zig)
* [Zig Forum](https://ziggit.dev/latest)

## Other Tiny Essays 

* Typescript: https://gist.github.com/diegopacheco/98c85dec602d308f533edb4d0df35471
* Rust: https://gist.github.com/diegopacheco/4b7dfeb781ad3455ae2a6b090d9deaa7
* Scala: https://gist.github.com/diegopacheco/1b5df4287dd1ce4276631fd630267311
* Kotlin: https://gist.github.com/diegopacheco/f6beabf1451cfe1ec2dc89a19a78fdc5
* Clojure: https://gist.github.com/diegopacheco/9453877378f007e8903a359f298a0afa
* Haskell: https://gist.github.com/diegopacheco/057087dc7ae236bdd0700014a31c88ef
* V Lang: https://gist.github.com/diegopacheco/3d0b176eb83e569da582a0770209e22f
* Nim lang: https://gist.github.com/diegopacheco/0fb84d881e2423147d9cb6f8619bf473
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/