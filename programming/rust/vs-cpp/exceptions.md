# Rust vs C++: Exceptions

When Rust is configured to unwind on panic (default behavior), panics are effectively exceptions.

Rust can also be configured to abort on panic.
This is similar to disabling exceptions in C++, but without forcing you to rewrite every `throw ...;` and `catch (...) { ... }`.



### Similarities

Assuming `panic="unwind"`:
-   Rust panics and C++ exceptions unwind the stack, executing destructors.
-   Rust panics and C++ exceptions use [SEH](https://learn.microsoft.com/en-us/cpp/cpp/structured-exception-handling-c-cpp) on the MSVC ecosystem.
-   Rust panics and C++ exceptions can be caught.
-   Rust panics and C++ exceptions can be rethrown.
-   Rust panics and C++ exceptions can have ≈any type.



## Differences

-   Rust strongly discourages using panics for anything other than bugs and other "unrecoverable" errors.
    Some parts of the C++ community avoid exceptions in a similar way (e.g. the gamedev community), but others do not (e.g. C++/CLI, C++/CX, etc.)

-   Rust lacks exception filtering.
    This mostly amounts to some minor differences in the callstacks shown in your debugger for unhandled exceptions.



## Example: C++

```cpp
#include <format>
#include <string>
#include <iostream>

int main() {
    try {
        throw std::format("exception: {}", "...");  // will be caught as `a`
        throw "exception";                          // will be caught as `b` if the above line is removed
    } catch (const std::string & a) {
        std::cerr << "a: " << a << "\n";
        // eat exception
    } catch (const char * b) {
        std::cerr << "b: " << b << "\n";
        throw; // rethrow exception
    }
    // uncaught exceptions will continue unwinding
}
```
```text
a: exception: ...
```
```text
b: exception
libc++abi: terminating due to uncaught exception of type char const*
```
<https://wandbox.org/permlink/8AQ5nDtg2Wf9MGH7>



## Example: Rust

```rust
fn main() {
    if let Err(e) = std::panic::catch_unwind(||{
        let n = 42;
        panic!("exception: {n}");       // will be caught as `a`
        panic!("exception: {}", "..."); // will be caught as `b` if the above line is removed
        //                              // (macro optimizes away string concatination)
    }) {
        if let Some(a) = e.downcast_ref::<String>() {
            eprintln!("a: {a}");
            // eat exception
        } else if let Some(b) = e.downcast_ref::<&str>() {
            eprintln!("b: {b}");
            std::panic::resume_unwind(e) // ≈ rethrow exception
        } else {
            // since we catch *everything*, un*handled* exceptions must be explicitly resumed if that's what you want.
            std::panic::resume_unwind(e)
        }
    }
}
```
```text
thread 'main' panicked at src/main.rs:4:9:
exception: 42
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
a: exception: 42
```
```text
thread 'main' panicked at src/main.rs:5:9:
exception
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
b: exception
Exited with status 101
```
<https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=c4a2db8f4a8e4aa1c140104db5506e02>
