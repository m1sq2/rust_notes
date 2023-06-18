==is Rust’s build system and package manager.== Building your code, downloading the libraries your code depends on, and building those libraries.

```shell
cargo --version
```

Creating new project

```shell
cargo new hello_cargo
```
will create:
- hello_cargo directory
	- Cargo.toml
	- src directory
		- main.rs
It has also initialized a new Git repository along with a _.gitignore_ file. Git files won’t be generated if you run `cargo new` within an existing Git repository; you can override this behavior by using `cargo new --vcs=git`.

Note: Git is a common version control system. You can change `cargo new` to use a different version control system or no version control system by using the `--vcs` flag. Run `cargo new --help` to see the available options.

### cargo.toml
```TOML
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```
This file is in the [_TOML_](https://toml.io/) (_Tom’s Obvious, Minimal Language_) format, which is Cargo’s configuration format.

The first line, `[package]`, is a section heading that indicates that the following statements are configuring a package. As we add more information to this file, we’ll add other sections.

The next three lines set the configuration information Cargo needs to compile your program: the name, the version, and the edition of Rust to use. We’ll talk about the `edition` key in [Appendix E](https://doc.rust-lang.org/book/appendix-05-editions.html).

The last line, `[dependencies]`, is the start of a section for you to list any of your project’s dependencies. In Rust, packages of code are referred to as _crates_. We won’t need any other crates for this project, but we will in the first project in Chapter 2, so we’ll use this dependencies section then.

### src/main.rs
```rust
fn main() { println!("Hello, world!"); }
```
Cargo expects your source files to live inside the ==_src_== directory. The top-level project directory is just for README files, license information, configuration files, and anything else not related to your code.

## Building and Running a Cargo Project

```shell
cargo build
```
This command creates an executable file in `target/debug/hello_cargo` rather than in your current directory. Because the default build is a debug build, Cargo puts the binary in a directory named _debug_. You can run the executable with this command:
```shell
./target/debug/hello_cargo
```
Running `cargo build` for the first time also causes Cargo to create a new file at the top level: ==_Cargo.lock_==. This file keeps track of the exact versions of dependencies in your project. This project doesn’t have dependencies, so the file is a bit sparse. You won’t ever need to change this file manually; Cargo manages its contents for you.

We just built a project with `cargo build` and ran it with `./target/debug/hello_cargo`, but we can also use `cargo run` to compile the code and then run the resultant executable all in one command:
```shell
cargo run
```
Using `cargo run` is more convenient than having to remember to run `cargo build` and then use the whole path to the binary, so most developers use `cargo run`.
==cargo run== = cargo build && ./target/debug/hello_cargo

Cargo also provides a command called `cargo check`. This command quickly checks your code to make sure it compiles but doesn’t produce an executable.

### Building for Release

When your project is finally ready for release, you can use `cargo build --release` to compile it with optimizations. This command will create an executable in _target/release_ instead of _target/debug_. The optimizations make your Rust code run faster, but turning them on lengthens the time it takes for your program to compile. This is why there are two different profiles: one for development, when you want to rebuild quickly and often, and another for building the final program you’ll give to a user that won’t be rebuilt repeatedly and that will run as fast as possible. If you’re benchmarking your code’s running time, be sure to run `cargo build --release` and benchmark with the executable in _target/release_.

### Cargo as Convention

With simple projects, Cargo doesn’t provide a lot of value over just using `rustc`, but it will prove its worth as your programs become more intricate. Once programs grow to multiple files or need a dependency, it’s much easier to let Cargo coordinate the build.

Even though the `hello_cargo` project is simple, it now uses much of the real tooling you’ll use in the rest of your Rust career. In fact, to work on any existing projects, you can use the following commands to check out the code using Git, change to that project’s directory, and build:

```shell
git clone example.org/someproject
cd someproject
cargo build
```