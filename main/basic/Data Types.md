Every value in Rust is of a certain _data type_, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: `scalar` and `compound`.

## Scalar Types

A _scalar_ type represents a single value. Rust has four primary scalar types: 
- integers
- floating-point numbers
- Booleans
- characters

##### Integer Types
An _integer_ is a number without a fractional component.
>integer types default to `i32`

|Length|Signed|Unsigned|
|---|---|---|
|8-bit|`i8`|`u8`|
|16-bit|`i16`|`u16`|
|32-bit|`i32`|`u32`|
|64-bit|`i64`|`u64`|
|128-bit|`i128`|`u128`|
|arch|`isize`|`usize`|

razlika izmedju `i` i `u` je u tome sto
`i` predstavlja **signed** 
`u` predstavlja **unsigned**

>It’s like writing numbers on paper: when the sign matters, a number is shown with a plus sign or a minus sign; however, when it’s safe to assume the number is positive, it’s shown with no sign.

Each signed variant can store numbers from -(2n - 1) to 2n - 1 - 1 inclusive, where _n_ is the number of bits that variant uses. So an `i8` can store numbers from -(27) to 27 - 1, which equals -128 to 127. Unsigned variants can store numbers from 0 to 2n - 1, so a `u8` can store numbers from 0 to 28 - 1, which equals 0 to 255.

`isize` and `usize` types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

You can write integer literals in any of the forms

|Number literals|Example|
|---|---|
|Decimal|`98_222`|
|Hex|`0xff`|
|Octal|`0o77`|
|Binary|`0b1111_0000`|
|Byte (`u8` only)|`b'A'`|
Note that number literals that can be multiple numeric types allow a type suffix, such as `57u8`, to designate the type. Number literals can also use `_` as a visual separator to make the number easier to read, such as `1_000`, which will have the same value as if you had specified `1000`.

##### Floating-Point Types
Numbers with decimal points. Rust’s floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respectively. The default type is `f64` because on modern CPUs, it’s roughly the same speed as `f32` but is capable of more precision. All floating-point types are signed.

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
}
```
##### Boolean Type

Boolean type in Rust has two possible values: `true` and `false`. Booleans are one byte in size. The Boolean type in Rust is specified using `bool`

```rust
fn main() {
    let t = true;
    let f: bool = false; // with explicit 
    //type annotation
}
```

##### Character Type

`char` type is the language’s most primitive alphabetic type

```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit 
    //type annotation
    let heart_eyed_cat = '😻';
}
```

## Compound Types

_Compound types_ can group multiple values into one type.
Two primitive compound types:
- tuples
- arrays
##### Tuple Type
>A _tuple_ is a general way of grouping together a number of values with a variety of types into one compound type.
Tuples have a fixed length: once declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same. We’ve added optional type annotations in this example:

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

The variable `tup` binds to the entire tuple because a tuple is considered a single compound element. To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value, like this:

```rust
fn main() {
    let tup = (500, 6.4, 1);
    let (x, y, z) = tup;
    println!("The value of y is: {y}");
}
```

This program first creates a tuple and binds it to the variable `tup`. It then uses a pattern with `let` to take `tup` and turn it into three separate variables, `x`, `y`, and `z`. This is called ==destructuring== because it breaks the single tuple into three parts. Finally, the program prints the value of `y`, which is `6.4`.

We can also access a tuple element directly by using a period (`.`) followed by the index of the value we want to access. For example:
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);
    let five_hundred = x.0;
    let six_point_four = x.1;
    let one = x.2;
}
```

The tuple without any values has a special name, ==unit==. This value and its corresponding type are both written `()` and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.

##### Array Type

 Unlike a tuple, every element of an **array must have the same type**. Unlike arrays in some other languages, arrays in Rust have a fixed length.
 ```rust
 fn main() {
    let a = [1, 2, 3, 4, 5];
}
```
Arrays are useful when you want your data allocated on the [[Stack and the Heap#Stack|stack]] rather than the [[Stack and the Heap#Heap|heap]]
or when you want to ensure you always have a fixed number of elements

An array isn’t as flexible as the [[Vectors|vector]] type, though. A _vector_ is a similar collection type provided by the standard library that _is_ allowed to grow or shrink in size. If you’re unsure whether to use an array or a vector, chances are you should use a vector.
However, arrays are more useful when you know the number of elements will not need to change.

>You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array, like so:
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```
`i32` is data type of array
`5` is the number of elements
```rust
let a = [3; 5];
```
array `a` will have `5` elements, and they will all be `3`

Accessing Array Elements
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```
