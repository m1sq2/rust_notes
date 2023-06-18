```rust
let s = String::from("hello");
```

The double colon `::` operator allows us to namespace this particular `from` function under the `String` type rather than using some sort of name like `string_from`

This kind of string _can_ be mutated:
```rust
    let mut s = String::from("hello");

    s.push_str(", world!"); // push_str() appends
    //a literal to a String

    println!("{}", s); // This will print 
    //`hello, world!`
```

With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

- The memory must be requested from the memory allocator at runtime.
- We need a way of returning this memory to the allocator when we’re done with our `String`.

That first part is done by us: when we call `String::from`, its implementation requests the memory it needs. This is pretty much universal in programming languages.
>Rust takes a different path: the memory is automatically returned once the variable that owns it goes out of scope

```rust
    {
        let s = String::from("hello"); // s is valid
        // from this point forward

        // do stuff with s
    }          // this scope is now over, and s is no
                             // longer valid

```

When a variable goes out of scope, Rust calls a special function for us. This function is called [`drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop), and it’s where the author of `String` can put the code to return the memory. Rust calls `drop` automatically at the closing curly bracket.

# String Literal

je fiksan string i oznacava se sa ""

```rust
let hello = "Hello, world!";
```

String literals are read-only and immutable
takodje su type `&str`