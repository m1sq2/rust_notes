Let’s say you have a variable of type `u8` that can hold values between 0 and 255. If you try to change the variable to a value outside that range, such as 256, [[Data Types#Integer Types|integer]] overflow will occur, which can result in one of two behaviors. When you’re compiling in debug mode, Rust includes checks for integer overflow that cause your program to _panic_ at runtime if this behavior occurs. Rust uses the term _panicking_ when a program exits with an error;[“Unrecoverable Errors with `panic!`”](https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html)

When you’re compiling in release mode with the `--release` flag, Rust does _not_ include checks for integer overflow that cause panics. Instead, if overflow occurs, Rust performs _two’s complement wrapping_. In short, values greater than the maximum value the type can hold “wrap around” to the minimum of the values the type can hold. In the case of a `u8`, the value 256 becomes 0, the value 257 becomes 1, and so on. The program won’t panic, but the variable will have a value that probably isn’t what you were expecting it to have. Relying on integer overflow’s wrapping behavior is considered an error.

To explicitly handle the possibility of overflow, you can use these families of methods provided by the standard library for primitive numeric types:

- Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`.
- Return the `None` value if there is overflow with the `checked_*` methods.
- Return the value and a boolean indicating whether there was overflow with the `overflowing_*` methods.
- Saturate at the value’s minimum or maximum values with the `saturating_*` methods.