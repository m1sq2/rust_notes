[**Rust**](https://doc.rust-lang.org/book/) is a
- [multi-paradigm](https://en.wikipedia.org/wiki/Programming_paradigm "Programming paradigm"),
- low-level,
- _statically typed_,
- expression-based,
- [general-purpose programming language](https://en.wikipedia.org/wiki/General-purpose_programming_language "General-purpose programming language") that emphasizes
	- [performance](https://en.wikipedia.org/wiki/Computer_performance "Computer performance"),
	- [type safety](https://en.wikipedia.org/wiki/Type_safety "Type safety"), 
	- and [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science) "Concurrency (computer science)").

It enforces [memory safety](https://en.wikipedia.org/wiki/Memory_safety "Memory safety")—ensuring that all [references](https://en.wikipedia.org/wiki/Reference_(computer_science) "Reference (computer science)") point to valid memory—without requiring the use of a [garbage collector](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science) "Garbage collection (computer science)") or [reference counting](https://en.wikipedia.org/wiki/Reference_counting "Reference counting") present in other memory-safe languages.

To simultaneously enforce memory safety and prevent concurrent [data races](https://en.wikipedia.org/wiki/Race_condition "Race condition"), its ``"borrow checker"`` tracks the [object lifetime](https://en.wikipedia.org/wiki/Object_lifetime "Object lifetime") of all references in a program during [compilation](https://en.wikipedia.org/wiki/Compilation_(computing) "Compilation (computing)"). Rust borrows ideas from [functional programming](https://en.wikipedia.org/wiki/Functional_programming "Functional programming"), including [static types](https://en.wikipedia.org/wiki/Static_types "Static types"), [immutability](https://en.wikipedia.org/wiki/Immutable_object "Immutable object"), [higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function "Higher-order function"), and [algebraic data types](https://en.wikipedia.org/wiki/Algebraic_data_type "Algebraic data type"). It is popularized for [systems programming](https://en.wikipedia.org/wiki/Systems_programming "Systems programming").

> rustc je compailer i njime pravis bineris
```shell
rustc --version
```
a ovako se updatuje
```shell
rustup update
```
Rust also brings contemporary developer tools to the systems programming world:
- `Cargo`, the included dependency manager and build tool
- `The Rustfmt` formatting tool ensures a consistent coding style across developers.
- `The Rust Language Server` powers Integrated Development Environment (IDE) integration for code completion and inline error messages.

##### Use Case
1. Systems Programming
2. Web Development
3. Networking and Distributed Systems
4. Game Development
5. Command-line Tools
6. Cryptography and Security

