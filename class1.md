# Rust 101: class one

---

## agenda

* What is Rust?
* Why learn Rust?
* How do I install Rust?
* What does Rust look like?
* Where do I go from here?

---

## What is Rust?

Rust is a modern, statically typed high level system's programming language.

It has the following goals

* memory safety
  *  all memory is accounted for and managed safely. (no dangling pointers, no segfaults, no unexpected crashes)
* concurrency
  *  concurrent programming, [sans the fear of shooting yourself in the foot](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html). The compiler catches data races and a number of other concurrency related problems at compile time rather than forcing you to try your luck and reproducing them at runtime
* [zero cost abstractions](https://blog.rust-lang.org/2015/05/11/traits.html)
  * as much as possible the abstractions provided come at no expense to your runtime
* efficiency
  * faster is better. small/no runtime. no garbage collection

The combination of these things provides a language that suitable for both problems that require low level
efficiency as well as solutions that prefer high level abstractions.

---

## Why learn Rust?

Rust more than a new language. It takes new approaches to long standing and hard problems.
These solutions make notable trade offs but also provide notable gains.

* Memory safety and efficiency is are achieved by an alternative approach memory management.
 There is no runtime managing a garbage collection. Instead you have "Borrow" semantics.

* Borrow semantics is a system that tracks the lifetime and ownership of objects through type parameterization.
Because ownership is included in a type information. Rust's compiler is able to predictably know when to
deallocate memory when the owner goes out of scope.

* Because this information is available at compile time, the compiler is able to detect and prevent,
 potentially dangerous access to mutable references across multiple concurrent threads. This prevents an entire class
 of defects that may lie dormant but ever present within existing software written in other languages tackling concurrent
 problems.

* Rust's compilation backend leverages the work of [LLVM](http://llvm.org/). Optimizations for producing cost effective and efficient
machine code are made within that pipeline.

* Unlike the JVM, Rust defaults to stack allocation for memory storage, rather than the heap. The notable difference is that the stack provides fast and cheap access to memory. The heap provides potentially segmented and slower access to memory.

* Rust has learned from past mistakes in other languages and makes strong attempts to avoid them. There is no `null` value instead you have Option semantics. There is no real notion of exceptions, instead you have error semantics. Errors are just values. They can't be "thrown" around like a bull in a china shop. They can only be returned, just like any other value.

* Static type inferrence.

---

### Bonus features

* Reputation for having a amazing and [friendly](https://www.rust-lang.org/en-US/conduct.html) [community](https://www.rust-lang.org/en-US/community.html)

* Really nice toolchain right out of the box. `cargo {build, test, run, publish}`

* Fast growing community ["By our metrics, Rust went from the 46th most popular language on GitHub to the 18th."](https://twitter.com/rustlang/status/842864783741345793)

* Rust has been the most loved programming language for [two](https://twitter.com/rustlang/status/707952865067794432) [years](https://twitter.com/rustlang/status/707952865067794432) in a row according to an annual stack overflow developer survey

* Fast growing ecosystem

---

### Summing that up the why

* Rust provides new solutions and potentially more useful solutions to old and hard problems

* Rust promotes a safe and predicable concurrency model

* Rust is designed for program correctness, making [impossible states impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8)

* Rust is does not compromise on efficiency.

* Because rust takes a new approach to existing problems, amazing new things are possible.

---

## How do I install Rust?

Visit [this website](https://www.rust-lang.org/en-US/install.html)

Rust utilizes a tool called [rustup](https://www.rustup.rs/) for managing toolchain
installations

```bash
$ curl https://sh.rustup.rs -sSf | sh
```

### Staying up to date

If you have rustup installed, you can update your local rust toolchain version with

```bash
$ rustup update stable
```

There are number of other commands to preform various tasks you can learn about by visiting [rustup's documentation](https://github.com/rust-lang-nursery/rustup.rs#rustup-the-rust-toolchain-installerh)

---

### stable vs nightly

Another one of Rust's goals is stability without stagnation.

Rust will always provide at least two latest versions for use: `stable` and `nightly`

`nightly` is provided for testing features that may be included in the next release. It is not considered to be bug free. If you are a library author, you may prefer this to stay ahead of your customers.

`stable` is considered to be bug free and production ready. If you are writing applications, you'll want to use this

---

## What does Rust look like?

Rust comes from the `C` [family of programming languages](https://en.wikipedia.org/wiki/List_of_C-family_programming_languages) so it's syntax will
probably already be familiar to you, comments and delimiters included.

However, as you walk up Rust's abstraction stairs, you may find some doors that look
unfamiliar. I'll cover those in just a bit.

---

### applications

All rust code is run as an executable binary application. These application are
compiled down programs specialized to run on your native operating system.
Rust calls these "targets".

An application is just a program with a `main` function.

```rust
fn main() {
  // code goes here
}
```

If you are new to functions. I'll explain that in more detail in just a bit.
For now, just note that this is all you need to for the minimal rust application.

---

### bootstrapping and running applications

Remember `cargo`? `cargo` is Rust's official build tool. Cargo also makes it easy
to bootstrap applications.

```bash
$ cargo new --bin hello
```

This should have generated a directory called `hello` with some files inside.

```bash
$ ls hello/**
hello/Cargo.toml

hello/src:
main.rs
```

The files with `.rs` extensions are source files that contain Rust code.

Step into the `hello` directory and run `cargo run`

You should see something like this

```bash
$ cargo run
   Compiling hello v0.1.0 (file:///private/tmp/hello)
    Finished dev [unoptimized + debuginfo] target(s) in 0.87 secs
     Running `target/debug/hello`
Hello, world!
```

If you look at the contents of `src/main.rs` you'll find a pre-generated
application ( with a `main` function )


---

### the very basics

Let's use this application as our scratch pad.

Rust language can be divided into two categories of abstractions: data and behavior.
In languages will often mix the two together. Mixing data and behavior is often a
recipe for for awkward and poorly representative design. It can also be very powerful,
when used correctly but such correctness can not be determined by the compiler itself.
Rust takes a proactive approach. In Rust, data and behavior are never mixed by design,
they are composed.

Let's focus on data first.

### data

Rust supports a number of primitive types. Depending on where you're coming from,
these may be greater in number than you may be used to.

The type of a piece of data determines it's shape, possible values, and size. Size is particularly important in Rust.
Types of have names but you can also alias these names to something that may be more appropriate for your application

#### Naming types

```rust
type Age = i16;
```

The `type` keyword allows for that. This is also useful when you may wish to
simplify the name of a more complicated type.

#### Naming values

In Rust, you assign values to names using the keywords `let`, `static`, or `const`

```rust
let age = 32;
```

`let` will be the most common way you name values embedded with in your program.
`static` and `const` are used to name values that have a wider scope and can be
accessed from multiple parts of your program.

Note that Rust is statically typed. Since values have types, Rust can often infer
their static type without explicitly annotation. Rust will use all available type
information in scope to order to determine an appropriate type to assign to the
value. When there is a lack of concrete evidence, your program will fail to compile.
In other languages the result is some times inferred to be a least common denominator
type based on a type inheritance hierarchy. Rust doesn't not support a notion of
inheritance hierarchy so your values will always be assigned to a non ambiguous type
in a compiled program

To be more explicit with your types you can add a suffix to your name that includes a `:` and
an explicit type.

```rust
let age: i16 = 32;
```

##### Numerics

* `i8`, `i16`, `i32`, `i64`, `isize` signed types ( includes negative numbers )
* `u8`, `u16`, `u32`, `u64`, `usize` unsigned types ( no negative numbers )
* `f32`, `f64` floating point types (decimal points)

Why all these types!? You may ask. Recall that Rust aims to be memory efficient
and has a dependency on knowing the size of types at compile time in order to provide certain
safety guarantees for you. Each of these types is appropriately sized and its up
to the engineer to chose the size that makes sense for your application.

#### Tuples

Tuples are a container of values where the elements of the inside the may vary in
specific types.

`(1, -3, 4.5)` is an example of a 3 element tuple.


For more information on primitive types, check out this [website](https://doc.rust-lang.org/book/primitive-types.html)


---

# Where do I go from here?
