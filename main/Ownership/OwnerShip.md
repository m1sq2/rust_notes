>_Ownership_ is a set of rules that govern how a Rust program manages memory.

 Keep these rules in mind as we work through the examples that illustrate them:

- Each value in Rust has an _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped

```rust
    {                     // s is not valid here,
    // it’s not yet declared
        let s = "hello";   // s is valid from this
        // point forward
        
        // do stuff with s
    }              // this scope is now over, and s
    // is no longer valid
```


## Move

```rust
    let x = 5;
    let y = x;
```

We can probably guess what this is doing: “bind the value `5` to `x`; then make a copy of the value in `x` and bind it to `y`.” We now have two variables, `x` and `y`, and both equal `5`. This is indeed what is happening, because integers are simple values with a known, fixed size, and these two `5` values are pushed onto the stack.

Now let’s look at the [[String|String]] version:
```rust
    let s1 = String::from("hello");
    let s2 = s1;
```

A `String` is made up of three parts,
shown on the ==left==: a pointer to the memory that holds the **contents** of the string, a **length**, and a **capacity**. This group of data is stored on the stack.
On the ==right== is the memory on the heap that holds the contents
![[Pasted image 20230617175552.png]]

- The **length** is how much memory, in bytes, the contents of the `String` are currently using.
- The **capacity** is the total amount of memory, in bytes, that the `String` has received from the allocator.
When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. ==We do not copy the data on the heap that the pointer refers to.==
![[Pasted image 20230617175951.png]]
both data pointers pointing to the same location.
This is a problem: when `s2` and `s1` go out of scope, they will both try to free the same memory.
This is known as a _double free_ error and is one of the memory safety bugs we mentioned previously.
Freeing memory twice can lead to memory corruption, which can potentially lead to security vulnerabilities.
>To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes out of scope

>`dalkle ovo nije prava kopija, nego je samo promenjen vlasnik memorije`

## Clone
ako zelimo da napravimo pravu kopiju onda mozemo da koristimo `clone` metodu
```rust 
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```

ovo je izuzetno resurki skupa praksa, tkd IZBEGAVAJ OVO DA RADISH I SAMO KADA TI BASH TREBA

## Copy

```rust
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
```
>The reason is that types such as integers that have a known size at compile time are stored entirely on the stack, so copies of the actual values are quick to make.

That means there’s no reason we would want to prevent `x` from being valid after we create the variable `y`. In other words, there’s no difference between deep and shallow copying here

Rust has a special annotation called the `Copy` trait that we can place on types that are stored on the stack, as integers are
Here are some of the types that implement `Copy`:
- All the integer types, such as `u32`.
- The Boolean type, `bool`, with values `true` and `false`.
- All the floating-point types, such as `f64`.
- The character type, `char`.
- Tuples, if they only contain types that also implement `Copy`. For example, `(i32, i32)` implements `Copy`, but `(i32, String)` does not.

---
Functions
>The mechanics of passing a value to a ==function== are similar to those when assigning a value to a variable.
>Passing a variable to a function will ==move== or ==copy==, just as assignment does.

```rust
fn main() {
    let s = String::from("hello");  // s comes into
    // scope

    takes_ownership(s);             // s's value 
    //moves into the function...
                                    // ... and so is
                                    // no longer 
                                    //valid here

    let x = 5;                      // x comes into
    // scope

    makes_copy(x);                  // x would move
    // into the function,
                                    // but i32 is 
                                    //Copy, so it's
                                    // okay to still
                                    // use x
                                    // afterward

} // Here, x goes out of scope, then s. But because
//s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

Return
>Returning values can also transfer ownership.

```rust
fn main() {
    let s1 = gives_ownership();  // gives_ownership
    // moves its return
                                        // value
                                        // into s1

    let s2 = String::from("hello");     // s2 comes
    // into scope

    let s3 = takes_and_gives_back(s2);  // s2 is
    // moved into
                // takes_and_gives_back, which also
                                        // moves its
                                        // return 
                                        //value into
                                        // s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String { // gives_ownership will move its
                // return value into the function
                        // that calls it

    let some_string = String::from("yours");
     // some_string comes into scope

    some_string       // some_string is returned and
                    // moves out to the calling
                                 // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                        // scope

    a_string  // a_string is returned and moves 
    //out to the calling function
}
```


>`The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it.`

