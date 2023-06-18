taking [[OwnerShip|ownership ]]and then returning ownership with every function is a bit tedious. ==What if we want to let a function use a value but not take ownership?== It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might want to return as well.
```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the 
    //length of a String

    (s, length)
}
```

>Rust has a feature for using a value without transferring ownership, called _references_. 
>A _reference_ is like a pointer in that it’s an address we can follow to access the data stored at that address; that data is owned by some other variable. Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```
![[Pasted image 20230617193804.png]]

> dakle umesto da kopiramo ili pomerimo vrednost, mozemo samo da je referenciramo i onda koristimo, a kad zavrsimo sa koriscenjem reference, onda se vrati originalnom vlasniku

>pozamljene i referencirane vrednosti nmg biti modifikovane od stane variable koja ih pozamljuje, jer ona nije vlasnik

```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```
```shell
error[E0596]: cannot borrow `*some_string` as mutable, as it is behind a `&` reference
 --> src/main.rs:8:5

```

Note: The opposite of referencing by using `&` is _dereferencing_, which is accomplished with the dereference operator, `*`.

## Mutable References

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
Mutable references have one big restriction: if you have a mutable reference to a value, ==you CAN NOT have other references to that value==.
This code that attempts to create two mutable references to `s` will fail

A _data race_ is similar to a race condition and happens when these three behaviors occur:

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data

>As always, we can use curly brackets to create a new scope, allowing for multiple mutable references, just not _simultaneous_ ones:

```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;

```
Rust enforces a similar rule for combining mutable and immutable references. This code results in an error:
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    let r3 = &mut s; // BIG PROBLEM

    println!("{}, {}, and {}", r1, r2, r3);

```
```shell
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
 --> src/main.rs:6:14
```

```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```

