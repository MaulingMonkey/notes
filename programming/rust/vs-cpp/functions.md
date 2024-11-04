# Rust vs C++: Functions

Rust and C++ function pretty similarly here.



## Basic Syntax Example: C++

```cpp
float add(float a, float b) { return a + b; }

// main is a little magic
int main(int argc, const char* argv[]) {
    const std::span<const char*> args { argv, argc };

    const float result = add(1.0f, 2.0f);

    return 0;
}
```



## Basic Syntax Example: Rust

```rust
fn add(a: f32, b: f32) -> f32 { return a + b; }

// main is a little magic
fn main() {
    let args = std::env::args();    // UTF-8
    let args = std::env::args_os(); // alternatively: possibly WTF-8

    let result : f32 = add(1.0, 2.0);

    std::process::exit(0);
}
```



## Return Types

| Rust                                                                          | C++ |
| ------------------------------------------------------------------------------| ----|
| [`()`](https://doc.rust-lang.org/std/primitive.unit.html) – read as "unit"    | `void`
| [`!`](https://doc.rust-lang.org/std/primitive.never.html) – read as "never"   | `[[noreturn]] void`



## Calling Conventions

TODO



## Overloading

Rust doesn't have function overloading, but it has several tricks which can make it look like it does:

-   Just give functions different names, like we would in C.
    [`web_sys::console::*`](https://docs.rs/web-sys/latest/web_sys/console/index.html) does this, as a concrete example.
-   [Use traits](https://medium.com/@jreem/advanced-rust-using-traits-for-argument-overloading-c6a6c8ba2e17) to mimic overloading for multiple types. Commonly used traits for this include: [`AsRef`], [`Into`], [`IntoIterator`]
-   Use macros to fake variadic arguments. This is used by <code>[println!]\(...\)</code>, <code>[print!]\(...\)</code>, <code>[format!]\(...\)</code>, <code>[format_args!]\(...\)</code>, etc. in the stdlib.
-   Pass in a structure that's partially constructed from default values:
    `some_function(SomeStruct { field: 42, ..Default::default() })`
-   Pass in a strucure constructed via the builder pattern (especially when creating new instances of things)



<!-- References -->

[`AsRef`]:          https://doc.rust-lang.org/std/convert/trait.AsRef.html
[`Into`]:           https://doc.rust-lang.org/std/convert/trait.Into.html
[`IntoIterator`]:   https://doc.rust-lang.org/std/iter/trait.IntoIterator.html

[println!]:         https://doc.rust-lang.org/std/macro.println.html
[print!]:           https://doc.rust-lang.org/std/macro.print.html
[format!]:          https://doc.rust-lang.org/std/macro.format.html
[format_args!]:     https://doc.rust-lang.org/std/macro.format_args.html
