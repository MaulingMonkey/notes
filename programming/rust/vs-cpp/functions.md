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

TODO
