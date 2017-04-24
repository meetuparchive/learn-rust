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

* Static type inference. It makes leveraging the usefulness of static typing practical.

---

### Bonus features

* Reputation for having a amazing and [friendly](https://www.rust-lang.org/en-US/conduct.html) [community](https://www.rust-lang.org/en-US/community.html)

* Really nice toolchain right out of the box. `cargo {build, test, run, publish}`

* Fast growing community ["By our metrics, Rust went from the 46th most popular language on GitHub to the 18th."](https://twitter.com/rustlang/status/842864783741345793)

* Rust has been the most loved programming language for [two](https://twitter.com/rustlang/status/707952865067794432) [years](https://twitter.com/rustlang/status/707952865067794432) in a row according to an annual stack overflow developer survey

* Fast growing ecosystem

* Rusts compiler will keep you programs clean. Warns on unused variables and unused imports. Warns on unnecessary mutability. Can be made to warn ( and even fail to compile ) on undocumented public interfaces. And many other things...

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
recipe for for awkward and poorly representative design: "a penguin is a bird that can not fly".
It can also be very powerful, when used correctly, but such correctness can not be determined nor enforced by the compiler itself.
Rust takes a proactive approach. In Rust, data and behavior are never mixed by design,
they are composed.

Let's focus on data first.

---

### data

Rust supports a number of primitive types. Depending on where you're coming from,
these may be greater in number than you may be used to.

The type of a piece of data determines it's shape, possible values, and size. Size is particularly important in Rust.

---

#### Naming types

Types of have names but you can also alias these names to something that may be more appropriate for your application

```rust
type Age = i16;
```

The `type` keyword allows for that. This is also useful when you may wish to
simplify the name of a more complicated type.

---

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

---

#### Numerics

* `i8`, `i16`, `i32`, `i64`, `isize` signed types ( includes negative numbers )
* `u8`, `u16`, `u32`, `u64`, `usize` unsigned types ( no negative numbers )
* `f32`, `f64` floating point types (decimal points)

Why all these types!? You may ask. Recall that Rust aims to be memory efficient
and has a dependency on knowing the size of types at compile time in order to provide certain
safety guarantees for you. Each of these types is appropriately sized and its up
to the engineer to chose the size that makes sense for your application.

---

#### Booleans

Booleans value what you may expect. Their type is referred to as `bool` with
the possible values `true` or `false`

```rust
let sunny = true;
let cloudy: bool = false;
```

Learn more about booleans [here](https://doc.rust-lang.org/std/primitive.bool.html)

---

#### Characters

Characters represent a single scalar unicode value surrounded by a single quote.
Their type is referred to as `char`.

```rust
let x = 'x';
let two_hearts: char = '💕';
```

Learn more about characters [here](https://doc.rust-lang.org/std/primitive.char.html)

---

#### Arrays

Arrays represent a fixed size collection of things that have the same type.


```rust
let seats = [1, 2, 3];
```

There's an emphasize on _fixed size_ here. The size is explicitly part of its type.

```rust
let seats: [i32; 3] = [1, 2, 3];
```

This happen's to be very useful in practice. The following cause Rust's compiler to
 yield the following warning

```rust
let seats: [i32; 3] = [1, 2, 3];
// this table only seats 3
seats[4]; /// warning: this expression will panic at run-time
```

Because the size of the array is part of its type the following would not compiler

```rust
///  expected an array with a fixed size of 2 elements, found one with 3 elements
let bicycle_wheels: [i32; 2] = [1, 2, 3];
```

---

#### Slice

A slice is closely related to an array and a reference. It's very much just a
reference to a view into a "slice" of an array.

```rust
let pizza = [1, 2, 3, 4];
let half_pizza = &pizza[0..pizza.len()/2]; /// [1,2]
```

Learn more about slices [here](https://doc.rust-lang.org/std/primitive.slice.html)

---

#### Tuples

Tuples are a container of values where the elements of the inside the may vary in
specific types.

`(1, -3, 4.5)` is an example of a 3 element tuple.

---

#### Strings

Strings are an interesting but sometimes confusing stumbling block for those new
to Rust.

A string is a sequence of valid utf-8 characters but they come in a few different
flavors each with different use cases.

#### str

A str is an unsized type but is most often a reference (`&`) to a string of bytes.

```rust
let name = "emma";
```

> note: The formal type of the above is really a `&'static str`. This may be the first
time you are seeing the lifetime type attribute ( a `'` character followed by a name);
`'static` is a special lifetime. It is typically declared at the entry point of your
application. Local lifetimes, references declared in functions have shorter lifespans
and can be given shorter names. Lifetime parameterization is ever present with references
but is often invisible to user code because of [lifetime elision](https://doc.rust-lang.org/nomicon/lifetime-elision.html)
in which the compiler fills in the lifetime encodings for you

Learn more about `str` type [here](https://doc.rust-lang.org/std/primitive.str.html)

The other type of string you're likely be working with an an owned string, referred to as a `String`

You can create owned `Strings` a number of different ways. Typically patterns include lifting a str slice
into a `String`. Depending on the context one may be preferable to another.

```rust
let name = String::from("emma");
let name2 = "emma".to_owned(); // provided by ToOwned impl
let name3: String = "emma".into(); // provided Into impl
let name4: String = "emma".to_string(); // provided by ToString impl
```

One notable difference from str slices is that `Strings` are growable, meaning the may
be appended to (`push`, `push_str` )

---

#### functions

Functions are first class values in Rust, as such they also have types.

```rust
fn foo(x: i32) -> i32 { x }

let x: fn(i32) -> i32 = foo;
```

For more information on primitive types, check out this [website](https://doc.rust-lang.org/book/primitive-types.html)

---

### Beyond primitives

At some point you'll find it useful to create your own data types that can more closely represent your domain.

Rust provides the following Optimizations

#### Structs

A struct is just a named collection of fields which may be primitives additional embedded structs

```rust
struct Person {
  name: String,
  age: u32
}
```

Do not confuse structs with what you may call a "class" in other languages. Rust structs consist only of data.
Rust makes a strong separation between data and behavior. We'll talk how to associate behavior with data later.
You can access fields by name. You can create instances of structs using the following syntax.

```rust
let emma = Person {
  name: "emma".into(),
  age: 32
};
let age = emma.age;
```


As less common type of struct is called a "new type" struct. A new type struct

```rust
struct Person(String, u32);
```

New type structs are like a combination between structs and tuples. Like tuples,
these struct fields are referenced by index, they don't have names. Like vanilla structs,
they have named types.

```rust
let emma = Person("emma".into(), 32);
let age = emma.1;
```

The typical use case for new type structs are to hide wrap your api's internal dependencies in
ways that prevent them from leaking through your public interfaces.

One thing to note in your struct design is how lifetimes play into references within your struct.

The lifetime of a borrowed type is part of its type's identity. As such you're type will need to take
those into account, typically through type parameterization.

```rust
struct Person<'a> {
  name: &'a str,
  age: u32
}
let emma = Person {
  name: "emma",
  age: 32
};
```

---

#### Enums

Rust's enum types are very powerful and vary flexible. Rust's structs are "tagged unions".
This means that their variants may differ in shape but are unified by their enum type.


```rust
enum Animal {
    Cat {
      color: String
    },
    Person {
      name: String,
      age: u32
    }
}
```

Since the number of variants are fixed, rust is able to do exhaustiveness checks
when pattern matching over their values. We'll take about this later.

---

### Behavior

So how do we do anything interesting with data in Rust? The answer is `Traits`. Rust's
behavioral surface area is decorated in a number of traits which define the capability of a type
. Capability is defined as an implementation of that behavior for given type. This implementations are referred to as 'impls`
To use these implementations in code evidence must be in scope.

Traits are everywhere in Rust. If you write a hello world program in Rust, you've interacted with traits
without knowing it.

```rust
println!("hello {}", "world");
```

So what's going on here? `println!` is a rust macro that takes a str slice literal that contains a pattern and a variable set of arguments.
Rust doesn't have variable arguments but macros can enable that anyway. More importantly is the structure of the pattern str.

`{}` is a pattern that indicates, the value to substitute _must_ implement the [Display](https://doc.rust-lang.org/std/fmt/trait.Display.html) trait.
It just so happens that the str slice primitive type has already implemented that.

`{:?}` is a pattern that indicates, the value to substitute _must_ implement the [Debug](https://doc.rust-lang.org/std/fmt/trait.Debug.html) trait.
It just so happens that the str slice primitive type has already implemented that as well.

But what about your own types? If you make an attempt to do so your program will likely not compile.

```rust
/// the trait `std::fmt::Display` is not implemented for `Person`
println!("hello {}", Person { name: "emma".into(), age: 32});
```

```rust
/// the trait `std::fmt::Debug` is not implemented for `Person`
println!("hello {:?}", Person { name: "emma".into(), age: 32});
```

Implementing Rust traits may seem onerous at first, but the idea behind their design is vary powerful.

Some builtin Traits like `Debug`, `Clone`, `Copy`, and others can often be generated for you via a feature of Rust
called "type derivation". That is to say, if Rust is able to derive an impl for all of the embedded types within your type,
Rust will be able to implement a trait for you. There is a specific syntax for that called a type [attribute](https://doc.rust-lang.org/book/attributes.html)

To derive an implementation of the Debug trait for the person type add the following.

```rust
#[derive(Debug)]
struct Person {
  name: String,
  age: u32
}
println!("hello {:?}", Person { name: "emma".into(), age: 32});
```

Type level derivation is an extremely useful to in reducing the amount of boiler plate that you'd have to write out by hand otherwise.

---

### Inherent traits

Rust defines a special kind of trait for types called an inherent trait. These differ from typical Traits in that they are defining
behavioral interfaces for a type that are unique to that type.

Inherent traits look like this

```rust
impl YourType {
   ...
}
```

While a typical trait impl would look like

```rust
impl YourBehavior for YourType {
 ...
}
```

It it said that the interface is "inherent" to the type, hence the name


---

### Traits

A trait is a behavior interface that may provide both abstract, undefined methods, default method implementations as well as associated types

There are a handful of decisions you'll want to consider when designing this interface so let's go through them one by one.

A basic trait will look like this

```rust
trait Read {
    /// methods...
}
```

> note: In Rust mutability and references are part of a types identity. By Defalut data created is owned and immutable. These properties play into
the reference types trait methods are defined for. For instance if a trait method is defined for a reference to a mutable
instance of this type, it will be a compile error to call this method an and owned immutable reference.

You declare a method for a type of reference to a type ( owned / borrowed / mutable / ect ) by declaring the first argument as `self`

```rust
trait Read {
    fn read(&self, book: &Book) -> ();
}
```

If you were to implement this method for Person

```rust
impl Read for Person {
  fn read(&self, book: &Book) -> () {
    // all data associated with a person is accessible via self
  }
}
```

Armed with an impl, Person can now read.

```rust
emma.read(&book);
```

You will sometimes need restrict implementations to types that also implement some other behavior.

```rust
trait Read : Remember { // Read can now only be implemented by types that now how to remember
  // ...
}
```

This may appear like subclassing in other languages, but try to avoid drawing these kinds of analogies. This
is a much more powerful utility. It's a declarative form of type requirements.

You are not restricted to making one requirement. You can make as many as you like

```rust
trait Read: Remember + Comprehend + Appreciate { // Read can only be implemented for types that will remember, comprehend, and appreciate it!
  // ...
}
```

# Where do I go from here?
