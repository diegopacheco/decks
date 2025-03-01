# Non-Blocking IO

Thread Pools
https://gist.github.com/djspiewak/46b543800958cf61af6efa8e072bfd5c

Schedulers 
https://gist.github.com/djspiewak/3ac3f3f55a780e8ab6fa2ca87160ca40

Fibers
https://typelevel.org/cats-effect/docs/concepts

Notes

- The bottom line: IOIts work-stealing scheduler results in a 2x to 5x performance increase (relative to a fixed-size ThreadPoolExecutor)
- Blocking operations are, in practice, unavoidable.
- Consider DNS resolution with is blocking IO - Cats Effect does handle scheduler dance from (thread-stealing compute pool and onto an unbounded blocking pool)
- Cats Effect work-stealing pool is an extremely high-performance implementation derived from the Tokio Rust framework
- JVM pools should be in 3 categories: CPU Bound, Blocking IO, NonBlocking IO Pooling
- Cats Effect Fibers is 150 bytes per fiber (can create tens of millions of fibers)
- Netty, nor Vert.x have any support for asynchronous cancelation(also known as "interruption")
- fiber-aware work-stealing runtime, which is present in Tokio and (to a lesser extent) Akka, but not in Netty or Vert.x