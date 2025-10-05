## Odin Lang Tiny Essay

created: 22.MAR.2025

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Odin

* Data Oriented
* C competitor like Zig
* Strong among game companies: Capcom, Bethesda, THQNordic, WarnerBros and others.
* Fast

### My Feelings (29-Dez-2024 odin dev-2024-12-nightly )

* Complex
* Felt like C++, lots, lots of options
* Powerful
* Felt like coding in Go (more powerful than Go or V)

### Show me the code

My POCs with Odin: https://github.com/diegopacheco/odin-playground <br/>

#### 1 - Loops

Here is how we can do loops, very similar to Go and V.

```odin
import "core:fmt"
main :: proc() {

	for i := 0; i < 10; i += 1 {
		fmt.println(i)
	}

	for i in 0..<10 {
		fmt.println(i)
	}

}
```

#### 2 - Pointers

Like C, C++ and Zig, you can do pointers.

```odin
package main
import "core:fmt"

main :: proc() {
  i := 123
  p := &i
  fmt.println("p^=", p^) // read `i` through the pointer `p`
  p^ = 1337       // write `i` through the pointer `p`
  x := p^ // ^ on the right
  fmt.println("x=",x)
}
```
#### 3 Unions

Like Scala, Typescript, Odin has support for unions.

```odin
package main
import "core:fmt"

Value :: union {
  bool,
  i32,
  f32,
  string,
}

main :: proc() {
  v: Value
  v = "Hellope"

  // type assert that `v` is a `string` and panic otherwise
  s1 := v.(string)

  // type assert but with an explicit boolean check. This will not panic
  s2, ok := v.(string)

  fmt.println(s1)
  fmt.println(s2)
  fmt.println(ok)
}
```

#### 4 Structucs

Like C, C++, Go or Rust. Odin  has structs.

```odin
package main
import "core:fmt"

main :: proc() {
  Vector2 :: struct {
  	x: f32,
	y: f32,
  }
  v := Vector2{1, 2}
  v.x = 4
  fmt.println(v.x)
}
```

#### 5 Multiple return

Unlike Java you can return multiple values.

```odin
package main
import "core:fmt"

swap :: proc(x, y: int) -> (int, int) {
  return y, x
}

main :: proc() {
  a, b := swap(1, 2)
  fmt.println(a, b) // 2 1
}
```

#### 6 #align

Data Oriented feature where the #align(4) attribute ensures 4-byte memory alignment for better performance on some architectures. The #raw_union means that i and c share the same memory location, so setting one will affect the value of the other.

```odin
package main
import "core:fmt"

Foo :: struct #align(4) {
    b: bool,
}

Bar :: struct #raw_union #align(4) {
    i: i32,
    c: u8,
}

main :: proc() {
    f := Foo{b=true}
    fmt.printf("Foo: %v\n", f)

    b := Bar{c=42}
    fmt.printf("Bar: %v\n", b)
}
```

#### 7 Structure of Arrays (SOA)

`#soa` implements Structure of arrays, which is a memory layout pattern. 

Options:
* AoS (Array of Structures) - The traditional approach where you have an array of struct instances
* SoA (Structure of Arrays) - What your code demonstrates using #soa
* AoSoA (Array of Structures of Arrays) - A hybrid approach

AOS
```
[struct1{x,y,z}, struct2{x,y,z}, struct3{x,y,z}, ...]
```

SOA
```
{
  x: [struct1.x, struct2.x, struct3.x, ...],
  y: [struct1.y, struct2.y, struct3.y, ...],
  z: [struct1.z, struct2.z, struct3.z, ...]
}
```

```odin
package main

import "core:fmt"

// SOA Data Types #
// 
// Array of Structures (AoS), Structure of Arrays (SoA), and Array of Structures of Arrays (AoSoA)
// refer to differing ways to arrange a sequence of data records in memory, with regard to interleaving
// These are of interest in SIMD and SIMT programming.
// 
main :: proc() {
        Vector3 :: struct {x: i8, y: i16, z: f32}

        N :: 3
        v: #soa[N]Vector3
        v[0].x = 1
        v[0].y = 4
        v[0].z = 9

        s: #soa[]Vector3
        s = v[:]
        assert(len(s) == N)
        fmt.println(s)
        fmt.println(s[0].x)

        a := s[1:2]
        assert(len(a) == 1)
        fmt.println(a)
}
```

The `#soa[N]Vector3` declaration means:
* Instead of creating an array of Vector3 structs
* Odin creates separate arrays for each field of Vector3
* These arrays each have length N (3 in this case)

SOA Benefits:
* Improved Cache Locality
* SIMD Optimization
* Better Memory Access Patterns

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