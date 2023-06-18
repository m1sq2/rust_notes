by default, variables are ==immutable== but option to make your variables ==mutable==.

>When a variable is immutable, once a value is bound to a name, you can’t change that value.

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```
```shell
cargo run

error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is: {x}");
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
e
```

Although variables are immutable by default, you can make them mutable by adding `mut` in front of the variable name

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

## Constants

Like ==immutable== variables, _constants_ are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.
- you aren’t allowed to use `mut` with constants
- You declare constants using the `const` keyword instead of the `let` keyword
- Constants can be declared in any scope, including the global scope
- constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
Rust’s naming convention for constants is to use all uppercase with underscores between words.
Constants are valid for the entire time a program runs, within the scope in which they were declared.

## Shadowing

You can declare a new variable with the same name as a previous variable.
The first variable is _shadowed_ by the second, which means that the second variable is what the compiler will see when you use the name of the variable.
>We can shadow a variable by using the same variable’s name and repeating the use of the `let` keyword as follows:

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
Shadowing is different from marking a variable as `mut`:
- reassign variable  using the `let` keyword
- we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name.