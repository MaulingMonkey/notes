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

| Rust                                  | MSVC                                                                      | GNU                                       |
| --------------------------------------| --------------------------------------------------------------------------| ------------------------------------------|
| `extern "system"`                     | `WINAPI`                                                                  | `WINAPI`                                  |
| `extern "efiapi"`                     | `EFIAPI`                                                                  | `EFIAPI`                                  |
| `extern "C"`                          | (default)                                                                 | (default)                                 |
| `extern "Rust"` (default)             | <span style="opacity: 25%">N/A</span>                                     | <span style="opacity: 25%">N/A</span>     |
| `extern "cdecl"`                      | [`__cdecl`](https://learn.microsoft.com/en-us/cpp/cpp/cdecl)              | `__attribute__((cdecl))`                  |
| <span style="opacity: 25%">N/A</span> | [`__clrcall`](https://learn.microsoft.com/en-us/cpp/cpp/clrcall)          | <span style="opacity: 25%">N/A?</span>    |
| `extern "stdcall"`                    | [`__stdcall`](https://learn.microsoft.com/en-us/cpp/cpp/stdcall)          | `__attribute__((stdcall))`                |
| `extern "fastcall"`                   | [`__fastcall`](https://learn.microsoft.com/en-us/cpp/cpp/fastcall)        | `__attribute__((fastcall))`               |
| `extern "thiscall"`                   | [`__thiscall`](https://learn.microsoft.com/en-us/cpp/cpp/thiscall)        | `__attribute__((thiscall))`               |
| `extern "vectorcall"`                 | [`__vectorcall`](https://learn.microsoft.com/en-us/cpp/cpp/vectorcall)    | `__attribute__((vectorcall))`             |

Additional notes:
-   Rust's `extern "..."`s are effectively `noexcept` (will abort instead of unwinding), unless you use their `extern "...-unwind"` variants.

References:
-   <https://doc.rust-lang.org/reference/items/external-blocks.html#abi>
-   <https://doc.rust-lang.org/nomicon/ffi.html#foreign-calling-conventions>
-   <https://wiki.osdev.org/UEFI#Calling_Conventions>
-   <https://en.wikipedia.org/wiki/X86_calling_conventions>
-   <https://clang.llvm.org/docs/AttributeReference.html#calling-conventions>
-   <https://gcc.gnu.org/onlinedocs/gcc/x86-Function-Attributes.html>



## Overloading

Rust doesn't have function overloading, but you can pretend it does:

```rust
  fn main() {
      foo((42));
      foo((12.0, 13.0));
  }

  pub fn foo(args: impl FooArgs) { args.exec() }
  pub trait FooArgs { fn exec(self); }
  impl FooArgs for u32 { fn exec(self) { println!("A number: {}", self) } }
  impl FooArgs for (f32, f32) { fn exec(self) { println!("{} x {} = {}", self.0, self.1, self.0 * self.1) } }
```
<https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=b726f7f6e21c250aba62f9fe61e6d5a0>
```text
  A number: 42
  12 x 13 = 156
```

Abusing tuples like this isn't common.  Using traits - *without* the tuples - is a lot more common.  Alternatives to abusing traits on tuples:

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
