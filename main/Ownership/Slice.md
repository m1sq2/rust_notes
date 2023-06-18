>_Slices_ let you reference a contiguous sequence of elements in a collection rather than the whole collection.
>A slice is a kind of [[References and Borrowing|reference]], so it does not have [[OwnerShip|ownership]].

Function that takes a string of words separated by spaces and returns the first word it finds in that string
```rust
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}

```
- The `first_word` function has a `&String` as a parameter.
- Because we need to go through the `String` element by element and check whether a value is a space, we’ll convert our `String` to an array of bytes using the `as_bytes` method
- we create an iterator over the array of bytes using the `iter` method
- `iter` is a method that returns each element in a collection and that `enumerate` wraps the result of `iter` and returns each element as part of a tuple instead
- `for` loop, we specify a pattern that has `i` for the index in the tuple and `&item` for the single byte in the tuple
- Inside the `for` loop, we search for the byte that represents the space by using the byte literal syntax

## String Slices

```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```
We create slices using a range within brackets by specifying `[starting_index..ending_index]`, where `starting_index` is the first position in the slice and `ending_index` is one more than the last position in the slice

ako pocinje od 0 indexa, mozemo  pocetak da ostavimo prazno, i ako ide do kraja, mozemo kraj da ostavimo prazno

`Note`: String slice range indices must occur at valid UTF-8 character boundaries. If you attempt to create a string slice in the middle of a multibyte character, your program will exit with an error. For the purposes of introducing string slices, we are assuming ASCII only in this section

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

## String Literals as Slices

Recall that we talked about string literals being stored inside the binary. Now that we know about slices, we can properly understand string literals:

```rust
let s = "Hello, world!";
```
The type of s here is &str: it’s a slice pointing to that specific point of the binary. This is also why string literals are immutable; &str is an immutable reference.

>Defining a function to take a string slice instead of a reference to a `String` makes our API more general and useful without losing any functionality:

# Other Slices

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

This slice has the type `&[i32]`.
It works the same way as string slices do, by storing a reference to the first element and a length.
You’ll use this kind of slice for all sorts of other collections.