## Haxe Lang Tiny Essay

created: 23.MAR.2025

I love to learn new programing languages, it help to open the mind to new possibilities and compare different approaches. For instance, I learned Ruby and Scala in 2010, Clojure and Haskell in 2011, Go in 2015, Kotlin 2016, Rust in 2018 and Idris, TypeScript in 2019, 2020 Pandemic strike did a bunch of pocs but not with new langs(crazy year), Zig in 2021, 2022(coding in lots of langs but nothing new) - in 2023 I'm learning Nim and V. Learn at least one lang per year. This post is not complain, it's just to share some toughts, notes and impressions. 

### Why Haxe

* Multi-plataform
* Good for Games
* High-level strictly-typed
* Can build cross-platform applications targeting JS, C++, C#, Java, JVM, Python, Lua, PHP, Flash

### My Feelings (12-Jan-2025 haxe 4.3.6)

* Cool
* Fun
* Syntax feels like JavaScript
* You will see slighly different outputs depending on the target language

### Show me the code

My POCs with Haxe: https://github.com/diegopacheco/haxe-playground <br/>

#### 1 - OOP Support

Haxe has classes, constructors and several OOP features like Java.
```haxe
class Point {
    var x:Int;
    var y:Int;
  
    public function new(x, y) {
      this.x = x;
      this.y = y;
    }
  
    public function toString() {
      return "Point(" + x + "," + y + ")";
    }
}

class Main {
    static public function main() {
        var p = new Point(10, 20);
        trace(p.toString());
    }
}
```

#### 2 - Generics

Like Java, Scala or Kotlin, Haxe has support for generics.
```haxe
@:generic
class MyValue<T> {
  public var value:T;

  public function new(value:T) {
    this.value = value;
  }
}

class Main {
  static public function main() {
    var a = new MyValue<String>("Hello");
    trace(a);

    var b = new MyValue<Int>(42);
    trace(b);
  }
}
```

#### 3 - Pattern Matcher

Like Haskell, Scala or Rust - Haxe has Pattern matcher.
```haxe
class Main {
    static public function main() {
        var myArray = [7, 6];
        var s = switch (myArray) {
            case [a, b] if (b > a):
                b + ">" + a;
            case [a, b]:
                b + "<=" + a;
            case _: "found something else";
        }
        trace(s); // 6<=7
    }
}
```

#### 4 - Maps

Like any decent language, there are collections, here is how we use Maps.
```haxe
class Main {
    static public function main() {
        var capitals = ["Brazil" => "Brasilia", "USA" => "Washington", "Argentina" => "Buenos Aires"];
        trace(capitals);
        trace(capitals["Brazil"]);
        for (country in capitals.keys()) {
            trace(country + " => " + capitals[country]);
        }
    }
}
```

#### 5 - STD Lib support for Unit Tests

I love languages that have good STD libs so it minimize how many external libs you need to use.
Haxe has Unit Tests support, built-in, IMHO a bit verbose but still cool.
```haxe
import utest.Runner;
import utest.ui.Report;
import utest.Test;
import utest.Assert;

class MyTestCase extends Test {
    public function testBasic() {
        Assert.equals("A", "A");
    }
}

class Main {
  public static function main() {
    //the long way
    var runner = new Runner();
    runner.addCase(new MyTestCase());
    Report.create(runner);
    runner.run();
    //the short way in case you don't need to handle any specifics
    utest.UTest.run([new MyTestCase()]);
  }
}
```

#### 6 - Operator Overloading

Haxe support Operator overloading like Scala.
```haxe
abstract MyAbstract(String) {
    public inline function new(s:String) {
      this = s;
    }
  
    @:op(A * B)
    public function repeat(rhs:Int):MyAbstract {
      var s:StringBuf = new StringBuf();
      for (i in 0...rhs)
        s.add(this);
      return new MyAbstract(s.toString());
    }
  }
  
  class Main {
    static public function main() {
      var a = new MyAbstract("foo");
      trace(a * 3); // foofoofoo
    }
  }
```

#### 7 - Building to Target Langs

Haxe allow you to build to several target langs such as JS, C++, C#, Java, JVM, Python, Lua, PHP, Flash.
Here are some examples

Java
```bash
haxe --main Main --jvm Main.jar
```

JS
```bash
haxe --main Main --js Main.js
```

Python
```bash
haxe --main Main --python main.py
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
* Gleam: https://gist.github.com/diegopacheco/2fdb5be0446ccb8f07d02105a46aab75  
* Misc https://gist.github.com/diegopacheco/49329d726d0e2bd1c709ba1187a92c97

#### About me

* https://diegopacheco.github.io/
* http://diego-pacheco.blogspot.com/