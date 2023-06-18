keyword: `fn`
uses _snake case_ as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words. Here’s a program that contains an example function definition:
```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

Note that we defined `another_function` _after_ the `main` function in the source code; we could have defined it before as well.

## Parameters
#arguments
We can define functions to have _parameters_, which are special [[Variables and Mutability|variables]] that are part of a function’s signature.
Technically, the concrete values are called ==arguments==

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}

```

>In function signatures, you _must_ declare the type of each parameter.

This is a deliberate decision in Rust’s design: requiring type annotations in function definitions means the compiler almost never needs you to use them elsewhere in the code to figure out what type you mean.

>When defining multiple parameters, separate the parameter declarations with commas, like this

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

## Statements and Expressions

- **Statements** are instructions that perform some action and do not return a value.
```rust
fn main() {
    let y = 6;
    println!("The value of y is: {y}");
}
```

- **Expressions** evaluate to a resultant value.
```rust
fn main() {
    let y = {
        let x = 5;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

```rust
{
    let x = 3;
    x + 1
}
```
is a block that, in this case, evaluates to `4`. That value gets bound to `y` as part of the `let` statement.
Note that the `x + 1` line doesn’t have a semicolon at the end, which is unlike most of the lines you’ve seen so far.
>Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value.

## Functions with Retunr Values

Functions can return values to the code that calls them.
We don’t name return values, but we must declare their type after an arrow (`->`)
You can return early from a function by using the `return` keyword and specifying a value, but most functions return the last expression implicitly.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```

There are two important bits:
- the line `let x = five();` shows that we’re using the return value of a function to initialize a variable.
- the `five` function has no parameters and defines the type of the return value

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```