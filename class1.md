# Rust 101: class one

---

## agenda

* What is Rust?
* Why learn Rust?
* How do I installing Rust?
* What does Rust look like?
* Where do I go from here?

---

## What is Rust?

Rust is a modern high level system's programming language.

It has the following goals

* memory safety
  *  all memory is accounted for and managed safely. (no dangling pointers, no segfaults, no unexpected crashes)
* concurrency
  *  concurrent programming, sans out fear of shooting yourself in the foot. compiler caches data races and a number of other concurrency related problems at compile time rather than forcing you to try your luck and reproducing them at runtime
* efficiency
  * fast is good. small/no runtime
* zero cost abstractions
  * as much as possible the abstractions provided come at no expense to your runtime

The combination of these things provides a language that suitable for problems that require low level
efficiency as well as problems that prefer high level abstractions.

---

## Why learn Rust?

Rust more than a new language. It provides new means of approaching existing problems.
These solutions make have notable different trade offs but also notable gains.

* Memory safety and efficiency is are achieved by an alternative approach memory management.
 There is no runtime managing a garbage collector. Instead you have "Borrow" semantics.

* Borrow semantics is a system that tracks the lifetime of objects through type parameterization.

* Because this information is available at compile time the compiler is able to detect and prevent,
 potentially dangerous access to mutable references across multiple concurrent threads.

* Rust's compilation module is built on [LLVM](http://llvm.org/). Optimizations for producing minimally
 obstructive code are made within that pipeline.

* Unlike the JVM, Rust defaults to stack allocation for memory storage, rather than the heap. The notable difference is that the stack provides fast and cheap access to memory. The heap provides potentially segmented and slower access to memory.

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

Another one of Rust's goal is stability without stagnation.

Rust will always provide at least two latest versions for use: `stable` and `nightly`

`nightly` is provided for testing features that may be included in the next release. It is not considered to be bug free. If you are a library author, you may prefer this to stay ahead of your customers.

`stable` is considered to be bug free and production ready. If you are writing applications, you'll want to use this

---

## What does Rust look like?

Rust comes from the `C` family of programming languages so it's syntax will
already probably be familiar to you, comments and delimiters included.

However as you walk up Rust's abstraction stairs, you may find some doors that look
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

Remember `cargo`? `cargo` is Rust's official build tool. Cargo makes it easy
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

step into the `hello` directory and run `cargo run`

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

Let's focus on data first.

#### data

Rust supports a number of primitive types. Depending on where you're coming from,
these may be greater in number than you may be used to.

A piece of data's type determines it's shape and size. Size is very important in Rust.
Types of have names but you can also bind data to a more contextual name within your application
using two forms.

### Naming types

```rust
type Foo = Bar;
```



The allocation of a type results in a value.

Let's say type Bar exists but it's more useful to alias that name as `Foo` for your application.
The `type` keyword allows for that. This is more useful when simplifying the name of a more
complicated type.

### assigning values

In rust you assign values to names using the keywords `let`, `static`, or `const`

```rust
let foo = bar;
```



##### Numerics

* `i8`, `i16`, `i32`, `i64`, `isize` signed types
* `u8`, `u16`, `u32`, `u64`, `usize` unsigned types
* `f32`, `f64` floating point types

Why all these types!? You may ask. Recall that Rust aims to be memory efficient
and as a dependency on knowing the size of types in order to provide certain
safety guarantees for you. Each of these types is appropriately sized and its up
to the engineer to chose the size that makes sense for your application.

#### Tuples

Tuples are like a collection where the elements of the collection may vary in
specific types.

`(1, -3, 4.5)` is an example of a 3 element tuple.

For more information on primitive types, check out this [website](https://doc.rust-lang.org/book/primitive-types.html)


---

# Where do I go from here?
