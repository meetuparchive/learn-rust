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

Rust utilizes a tool called [rustup](https://www.rustup.rs/) for managing tool installations

```bash
$ curl https://sh.rustup.rs -sSf | sh
```

If you have rustup installed around you can update your local rust version with

```bash
$ rustup update stable
```

There are number of other commands to preform various tasks you can learn about by visiting [rustup's documentation](curl https://sh.rustup.rs -sSf | sh)

---

### stable vs nightly

Another one of Rust's goal is stability without stagnation.

Rust will always provide at least two latest versions for use: `stable` and `nightly`

`nightly` is provided for testing features that may be included in the next release. It is not considered to be bug free. If you are a library author, you may prefer this to stay ahead of your customers.

`stable` is considered to be bug free and production ready. If you are writing applications, you'll want to use this

---

## What does Rust code look like?

---

# Where do I go from here?
