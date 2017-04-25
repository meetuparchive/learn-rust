# Rust 101

## The `struct`ure and `impl`ementation of Rust computer programs

---

## Agenda

* What is Rust?
  * Define: Rust
* Why learn Rust?
  * What makes Rust interesting
* How do I install Rust?
  * Getting familiar with Rust's toolchain
* What does Rust look like?
  * How to read Rust code
* Where do I go from here?
  * Beyond the basics

---

## What is Rust?

Rust is a modern, statically typed, high level system's programming language. That sounds like a bit of marketing but it's a pretty accurate description.

As a project, it has the following goals

* Memory safety
  *  all memory is accounted for and managed safely at compile time (no dangling pointers, no segfaults, no memory leaks, no unexpected crashes do to accounting errors!)
* First class concurrency
  *  concurrent programming, [sans the fear of shooting yourself in the foot](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html). The compiler catches data races and a number of other concurrency related problems at compile time rather than forcing you to try your luck and reproducing them at runtime
* [Zero cost abstractions](https://blog.rust-lang.org/2015/05/11/traits.html)
  * as much as possible the high level abstractions provided come at no expense to your runtime
* Efficiency
  * faster is better. minimal/no runtime. no garbage collection

The combination of these things provides a language that suitable for both problems that require low level
efficiency as well as solutions that prefer high level abstractions, a rare combination.

---

## Why learn Rust?

Rust is more than a just new programming language. It takes new approaches to solve long standing
and hard problems. These solutions make notable trade offs, but also provide notable gains.

* Memory safety and efficiency are achieved by an alternative approach to memory management.
 There is no runtime managing a garbage collection. Instead you have [borrow semantics](https://doc.rust-lang.org/book/references-and-borrowing.html).

* Borrow semantics is a system that tracks the lifetime and ownership of objects through type parameterization. Because ownership is included in type information at compile time, Rust's compiler is able to predictably know when to deallocate memory when the owner goes out of scope. This has two benefits: it removes the need for any manual memory management on behavior of the programmer and it removes any need for a persistent garbage collector at runtime at all. While advances in garbage collections have advanced over the years, their use targets on one way of solving a problem. Rust takes a new approach that removes the need for garbage collection entirely.

* Because object lifetime information is available at compile time, the compiler is able to detect and prevent, potentially dangerous access to mutable references across multiple concurrent threads. This prevents an entire class of defects that may lie dormant but ever present within existing software written in other languages tackling concurrent problems.

* Rust's compilation backend leverages the work of [LLVM](http://llvm.org/). Optimizations for producing cost effective and efficient machine code are made within that pipeline.

* Unlike the JVM, Rust defaults to stack allocation for memory storage, rather than the heap. The notable difference is that the stack provides fast and cheap access to memory. The heap provides potentially segmented and slower access to memory. Learn more about stack vs heap allocation [here](https://doc.rust-lang.org/book/the-stack-and-the-heap.html)

* Rust has learned from past mistakes other languages have made and makes strong attempts to avoid them. Notably, there is no `null` value. Instead you have Option semantics. There is no real notion of exceptions. Instead you have error semantics. Errors are just values. They are not special and can't be "thrown" around like a bull in a china shop. They can only be returned, just like any other value.

* Static type inference. It makes leveraging the usefulness of static typing practical.

* Compile time, type level derivation. This sounds like witchcraft, but I assure it is not.

---

### Bonus features

* Reputation for having an amazing and [friendly](https://www.rust-lang.org/en-US/conduct.html) [community](https://www.rust-lang.org/en-US/community.html)

* Really nice toolchain right out of the box. `cargo {build, test, run, publish}`

* Fast growing community ["By our metrics, Rust went from the 46th most popular language on GitHub to the 18th."](https://twitter.com/rustlang/status/842864783741345793)

* Rust has been the most loved programming language for [two](https://twitter.com/rustlang/status/707952865067794432) [years](https://twitter.com/rustlang/status/707952865067794432) in a row according to an annual stack overflow developer survey

* Fast growing [ecosystem](https://crates.io/) of libraries

* Rust's compiler will keep your programs clean. Warns on unused variables and unused imports. Warns on unnecessary mutability. Can be made to warn ( and even fail to compile ) on undocumented public interfaces. And many other things...!

---

### Summing that up the why

* Rust provides new solutions and potentially more useful solutions to old and hard problems

* Rust promotes a safe and predicable concurrency model which frees up programmers minds to focus less on hard to reproduce defects that make their way into production and more on providing value

* Rust is designed for program correctness, making [impossible states impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) freeing up programmers from maintaining error prone code so they can focus on providing value

* Rust is does not [compromise on efficiency](https://blog.rust-lang.org/2015/05/11/traits.html). It's a goal to provide high level abstractions that are just as efficient as if you optimize them by hand.

* Because Rust takes a new approach to existing problems, amazing new things are possible.

---

## How do I install Rust?

Visit [this website](https://www.rust-lang.org/en-US/install.html)

Rust utilizes a tool called [rustup](https://www.rustup.rs/) for managing it's toolchain
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

Rust will always provide at least two latest versions for use: `stable` and `nightly`. The have different target audiences

`nightly` is provided for testing features that may be included in the next release. It is not considered to be bug free and its interfaces may be subject to change. If you are a library author, you may prefer this to stay ahead of your customers upgrading to the next version of Rust.

`stable` is considered to be bug free and production ready. If you are writing applications, you'll want to prefer this.

---

### IDE support

If you use an IDE, you're IDE most likely already has integration for Rust via it's package manager or plugin system

Visit [this website](https://areweideyet.com/) to get a sense of the current state of IDE support.

---

## What does Rust look like?

Rust comes from the `C` [family of programming languages](https://en.wikipedia.org/wiki/List_of_C-family_programming_languages) so it's syntax will
probably already be familiar to you, comments and delimiters included.

However, as you walk up Rust's abstraction stairs, you may find some doors that look
unfamiliar. I'll cover those in just a bit.

---

### Applications

All Rust code is run as an executable binary application. These application are
compiled down programs specialized to run on your native operating system.
Rust calls these compilation "targets".

An application is nothing more than a program with a `main` function.

```rust
fn main() {
  // code goes here
}
```

If you are new to functions, don't worry. I'll explain that in more detail in just a bit.
For now, just note that this is all you need to for the minimal Rust application.

---

### Bootstrapping and running applications

While you could compile simple programs with `rustc`, Rusts native compiler, and run them, you can afford
yourself a greater velocity in future development using a higher level workflow.
Cargo is the tool that will provide that workflow.

`cargo` is Rust's official build tool. Cargo also makes it easy
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

Feel free to use this application as a scratch pad. For exploring rust.

You may also find [Rust playpen](https://play.rust-lang.org/) useful for exploration ( and sharing explorations ).
Rust playpen also has some additional and interesting features, like being able to the assembly
code that Rust's compiler generates. This is sometimes helpful in understanding the differences
between code compiled in debug mode ( the default ) and release mode ( a further optimized format ).

---

### The very basics

Rust language can be divided into two categories of abstractions: data and behavior.

In other languages you will often find the two intertwined in ways that cannot be separated. Mixing data and behavior is often a recipe for awkward and poorly representative design: "a penguin is a bird that can not fly".
It can also be very powerful, when used correctly, but such correctness can not be determined nor enforced by the compiler itself and enforcing correctness should be at the top of the list of requirements for a compiler. Rust takes a proactive approach. In Rust, data and behavior are never mixed by design. Instead, they are composed.

Let's focus on data first.

---

### Data

Rust supports a number of primitive types. Depending on where you're coming from,
these may be greater in number than you may be used to.

The type of a piece of data determines it's shape, possible values, and size. Size is particularly important in Rust.

---

#### Naming types

Types of have names but you can also alias these names to something that may be more appropriate for your application

```rust
type Age = u16;
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
accessed from multiple parts of your program. In most cases you'll want to prefer `const`, a global value that can not change. These values bindings have some additional requirements in that they can only be assigned to values which themselves are constant. Learn more about static and const [here](https://doc.rust-lang.org/book/const-and-static.html)

Note that Rust is statically typed. Since values have types, Rust can often infer
their static type without explicit annotation, given other type information in the surrounding environment. Rust will use all available type
information in scope to order to determine an appropriate type to assign to the
value. When there is a lack of concrete evidence, your program will fail to compile.
In other languages, the result is sometimes resolved, unintentionally, to be a least common denominator
type based on a type inheritance hierarchy. Rust doesn't not support a notion of
inheritance hierarchy so your values will always be assigned to a non-ambiguous and correct type
in a compiled program.

To be more explicit with your types you can add a suffix to your name that includes a `:` and
an explicit type.

```rust
let age: u16 = 32;
```

---

#### Numerics

* `i8`, `i16`, `i32`, `i64`, `isize` signed types ( includes negative numbers )
* `u8`, `u16`, `u32`, `u64`, `usize` unsigned types ( no negative numbers )
* `f32`, `f64` floating point types ( decimal points )

Why all these types!? You may ask. Recall that Rust aims to be memory efficient
and has a dependency on knowing the size of types at compile time in order to provide certain
safety guarantees for you. Each of these types is appropriately sized and its up
to the engineer to chose the size that makes sense for your application.

---

#### Booleans

Booleans value are what you may expect. Their type is referred to as `bool` with
the possible values of `true` or `false`

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

There's a string emphasis on _fixed size_ here. The size is explicitly part of its type.

```rust
let seats: [i32; 3] = [1, 2, 3];
```

This happen's to be very useful in practice. The following cause Rust's compiler to
 yield the following warning

```rust
let seats: [i32; 3] = [1, 2, 3];
// this table only seats 3!
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

#### Functions

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

A struct is just a collection of named fields which may be primitives or an additional nesting of embedded structs

```rust
struct Person {
  name: String,
  age: u16
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
struct Person(String, u16);
```

New type structs sit somewhere in between structs and tuples. Like tuples,
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
  age: u16
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
      age: u16
    }
}
```

Since the number of variants are fixed, Rust is able to do exhaustiveness checks
at compile time when pattern matching over their values. We'll take about pattern matching
in just a bit.

---

### Behavior

So how do we make data do anything interesting in Rust? The answer is `Traits`. Rust's
behavioral surface area is sprinkled in a number of traits which define the capability of types
. Capability is defined as an implementation of that behavior `Trait` for given type. These implementations are formally referred to as 'impls`
To use these implementations in code evidence must be in scope.

Traits are everywhere in Rust. If you write a hello world program in Rust, you've already interacted with traits, perhaps without knowing it.

```rust
println!("hello {}", "world");
```

So what's going on here? `println!` is a rust macro that takes a str slice literal that contains a pattern and a variable set of arguments.
Rust doesn't have variable arguments but macros, a more advanced topic, can enable that anyway.

More importantly here is the structure of the pattern str. `println` supports a number of different directives which determine how a piece of data is rendered.

`{}` is a pattern that indicates that the value to substitute _must_ implement the [Display](https://doc.rust-lang.org/std/fmt/trait.Display.html) trait.
It just so happens that the str slice primitive type has already implemented that.

`{:?}` is a pattern that indicates that the value to substitute _must_ implement the [Debug](https://doc.rust-lang.org/std/fmt/trait.Debug.html) trait.
It just so happens that the str slice primitive type has already implemented that as well.

But what about your own types? If you make an attempt to replace the str slice with your own times your program will likely not compile, and the compile will tell you exactly why.

```rust
/// the trait `std::fmt::Display` is not implemented for `Person`
println!("hello {}", Person { name: "emma".into(), age: 32});
```

```rust
/// the trait `std::fmt::Debug` is not implemented for `Person`
println!("hello {:?}", Person { name: "emma".into(), age: 32});
```

Implementing Rust all of these traits to add behvior do your data types may seem onerous at first, but the idea behind their design is very powerful.

Some builtin Traits like `Debug`, `Clone`, `Copy`, and others can often be generated for you via a feature of Rust
called "type derivation". That is to say, if Rust is able to derive an impl for all of the embedded types within your type,
Rust will be able to implement a trait for you. There is a specific syntax for that called a type [attribute](https://doc.rust-lang.org/book/attributes.html)

To derive an implementation of the `Debug` trait for the `Person` type add the following.

```rust
#[derive(Debug)]
struct Person {
  name: String,
  age: u32
}

println!("hello {:?}", Person { name: "emma".into(), age: 32});
```

Type level derivation is an extremely useful tool in reducing the amount of boiler plate that you'd have to write out by hand otherwise. It's there to use to your advantage so use it often where appropriate.

---

### Inherent traits

Rust defines a special kind of trait for types called an inherent trait. These differ from typical Traits in that they are defining
behavioral interfaces for a type that are unique to just that type.

Inherent traits look like this

```rust
impl YourType {
   //...
}
```

While a typical trait impl would look like

```rust
impl YourBehavior for YourType {
 //...
}
```

It it said that the interface is "inherent" to the type, hence the name.

A common use of an inherent trait is to define factory and helper methods for your type.
A common pattern is to define a `new` method that hides the implementation of how a struct
gets constructed. Don't confuse this with the `new` keyword in other languages. The name `new`
is merely just a common convention.

```rust
impl Person {
  fn new(
    name: String,
    age: u16
  ) -> Person {
    Person {
      name: name,
      age: age
    }
  }
}
```


---

### Traits

A trait is a behavioral interface that may provide both abstract unimplemented methods, default method implementations as well as associated types

There are a handful of decisions you'll want to consider when designing these interfaces so let's go through them one by one.

A basic trait will look like this

```rust
trait Read {
    /// methods...
}
```

> note: In Rust mutability and references are part of a type's identity. By default, data created is owned and immutable. These properties play into
the reference types trait methods are defined for. For instance if a trait method is defined for a reference to a mutable
instance of this type, it will be a compile error to call this method an and owned immutable reference.

You declare a method for a type of reference to a type ( owned / borrowed / mutable / ect ) by declaring the first argument as `self`

```rust
trait Read {
    // this method is defined for references of self
    // that means calling this method will not take ownership
    // over self
    fn read(&self, book: &Book) -> ();
}
```

If you were to implement this method for Person

```rust
impl Read for Person {
  fn read(&self, book: &Book) -> () {
    // all data associated with a person is accessible via self
    // implementation goes here
  }
}
```

Armed with an `impl`, your `Person` can now read.

```rust
emma.read(&book);
```

You will sometimes need to restrict implementations to types that also implement some other behavior.
This provides you certain guarantees when your default implementation needs to know more about the abstract
type which will fill in the rest of implementation.

```rust
trait Read : Remember { // Read can now only be implemented by types that know how to remember
  // ...
}
```

This may appear like subclassing in other languages, but try to avoid drawing these kinds of analogies. This
is a much more powerful utility. It's a declarative form of specifying type requirements through composition.

You are not restricted to making one requirement. You can make as many as you like

```rust
trait Read: Remember + Comprehend + Appreciate { // Read can only be implemented for types that will remember, comprehend, and appreciate it!
  // ...
}

impl Remember for Person {
  // ...
}

impl Comprehend for Person {
  // ...
}

impl Appreciate for Person {
  // ...
}

// you can now teach a person how to read
impl Read for Person {
  // ...
}
```

This is essentially how you achieve behavior through composition in Rust.
You have well defined data types and well defined behavioral traits.
You create implementations of those traits in independent of the definition of the data
and the interface. This is the primary means of how Rust enforces their separation.

Learn more about traits [here](https://doc.rust-lang.org/book/traits.html)

---

### Keeping things coherent

In order to design stable systems Rust employs a set of coherence when it comes to
implementing traits. These rules prevent the existence of multiple implementations
of a Trait for a given type. Getting into the details and implication is a more advanced topic but
be aware that there can only be one impl of a trait for a given type.

---

### About scopes

I mentioned about that in order to use trait implementations in code, evidence must be in scope.
Let's learn about scopes.

Rust code is organized into modules called 'mods'. Modules create scopes
for definitions and may have arbitrary nesting. On disk these these typically translate to file names

Rust libraries have an implied module that exists within a `lib.rs` file and for applications one that exists
within the `main.rs` file

```bash
$ tree app
  |_ lib.rs
  |_ foo.rs
  |_ bar.rs
  |_ main.rs
```

Here it's safe to say this project defines two modules `foo` and `bar`. It's up to the lib.rs to expose those members to the public using the `pub` keyword.

```rust
// lib.rs

// these make the foo and bar mod(ules) accessible to main.rs
pub mod foo;
pub mod bar;
```

It may also be the case that you want to hide your package structure but publish
a subset of types within those modules. This pattern is called [re-exporting](https://doc.rust-lang.org/book/crates-and-modules.html#re-exporting-with-pub-use).

```rust
// lib.rs
mod foo;
mod bar:
pub use foo::FooType;
pub use bar::BarType;
```

This also simplifies the surface area of your library users can interact with and need to know
about so its a good pattern to keep in mind.

The highest unit of compilation is called a crate. A crate is just a collection of modules intended for
use in integrating with other modules. This is the foundation of Rust's library ecosystem.

> note: According to rust's coherence rules you can not implement a Trait defined in a separate crate from a type also defined in an external crate. The thought behind this is that that crate may eventually provide that implementation and then you'll have two implementations in the same program! You can however define your own Trait and provide implementations for external types and you can use
define implementations of external Traits for your own types.

To bring an implementation into scope you import the Trait into your current crate using the `use` keyword.

Often time's you'll find types that impl standard library traits like [Read](https://doc.rust-lang.org/std/io/trait.Read.html) and [Write](https://doc.rust-lang.org/std/io/trait.Write.html). In order use those implementations, you will need to bring those Traits into scope since these traits are not in scope by default.

```rust
use std:io::{Read, Write};
```

Learn more about organizing modules [here](https://doc.rust-lang.org/book/crates-and-modules.html)


---

### control flow

Rust borrows much from the C family of language in terms of control flow plus a few additional features.

One notable differences is that Rust is expression oriented. To return a value from a control flow structure
you do no need an explicit `return` keyword, you just yield the value.

---

#### if/else

```rust
let result = if 1 > 2 { "math works" } else { "math is broken!" };
```

If can also be useful if exemplifying an interesting feature of `let` keyword. The `let` keyword
can take a structural pattern. Mixed with `if` this pattern is commonly known as an `if let` for lack of a better term

```rust
if let Some(value) = optional_value {
  // do something with value
}
```

---

### speaking of patterns

Rust supports a more general form of if/else that allows you to branch over conditions based on the shape of
data. This is known more formally as "pattern matching". This comes in particularly handy with enum types, but is use extends beyond enums.

As mentioned earlier on of the interesting aspects about enums is that their variants are finite in number. Because Rust knows this at compile time Rust is able to prevent you from a certain class of errors Where
you are not handling a case where you should. A common case is handling errors with the [Result enum](https://doc.rust-lang.org/std/result/enum.Result.html), a topic we will cover in a follow up class.

```rust
let final_result = match result {
  Ok(value) => value + 1
}
```

What behavior with your program exhibit is result was the Err variant of result. The answer is undefined
behavior and undefined behavior at runtime is a _bad_ thing. Rust is able to prevent you from permitting
undefined behavior by telling you that your pattern match is not "exhaustive".

You can correct this compiler error in a few different ways

Option 1

```rust
let final_result = match result {
  Ok(value) => value + 1,
  Err(_) => default_value
}
```

Option 2

```rust
let final_result = match result {
  Ok(value) => value + 1,
  _ => default_value
}
```

Both options are technically correct and are valid and both are using different kinds of patterns.
An `_` in a patterns means that you are matching a certain pattern but that you are not introducing a new name to bind the pattern's value to.

In the first option we are matching on the `Err` variant of `Result` but we are omitting a binding to the type that `Err` is wrapping. In this case we are unpacking the structure of the enum. You can also do this with other types like structs.

The the second option we are stating that we are matching any other possible pattern and are not binding that to a name. In this case there is no destructural matching. We are simply handling any additional cases with a fall through option.

Depending on your use case one may sometimes be preferred over the other.

Learn more about pattern matching [here](https://doc.rust-lang.org/book/patterns.html)

---

### getting loopy

It's very common task to iterate over a collection of things, often called "looping".

Rust has a few control structures defined for these needs.

#### for/in

Rust defines an abstract for/in constrol structure which is essentially syntactive sugar
for driving iteration over another rust trait called [Iterators](https://doc.rust-lang.org/std/iter/trait.Iterator.html)

> note: rust iterators provide a number of different combinatorial methods but the
that's most useful to note is `next` which returns an Option type containing either Some of the next value
or None indicating that the Iterator has been exhausted. There are a number of types
which implement the Iterator trait and work ask expected with for/in but those will be covered
in a future class

```rust
for value in vec![1,3,4] {
  println!("{}", value)
}
```

Notably you can skip over an iteration using the `continue` keyword or break out of the iteration using the
`break` keyword.

#### loop

Some times you just need a program to loop without a termination condition. Rust supports the the `loop`
keyword to satisfy that need.

The following program will loop forever printing "hello" every second.

```rust
use std::thread::sleep;
use std::time::Duration;

loop {
  println!("hello");
  sleep(Duration::from_secs(1));
}
```


# Where do I go from here?

In future classes we'll cover more packages in in the std library which you'll likely be interacting with as well as finding and consuming third party crates.

To get head start you may want to get up to speed check out the official online [Rust Book](https://doc.rust-lang.org/book/) which covers most of the material we'll be covering here but in a bit more depth.
