# Rust vs C++: Copy and Move Semantics

I'm going to break things down into various categories of semantics:

| # | Semantics         | C++                   | Rust                  | Description   |
|---| ------------------| ----------------------| ----------------------| --------------|
| 1 | Trivial copy      | copy ctors, builtins  | <code>[Copy]</code>   | Can be implemented via `memcpy`
| 2 | Deep copy         | copy ctors            | <code>[Clone]</code>  | Deep copies and heap allocs
| 3 | Trivial move      | move ctors            | <code>\![Copy]</code> | Trivial ≈`memcpy` and invalidate old value
| 4 | Nontrivial move   | move ctors            | *N/A*                 | Types that self-register for intrusive linked lists




1.  Trivial copies, that can be implemented via e.g. `memcpy`.
2.  Deep copies, which may heap allocate etc. and execute arbitrary code.
3.  Move semantics, which allow cheaply moving around a type that might otherwise require an expensive deep copy by invalidating the old value.

C++ conflates the syntax for 1 and 2.
Rust conflates the syntax for 1 and **3**.
This looks like:

```cpp
// C++

#include <string>
struct S { int a; int b; };

int main() {
    int trivial = 42;
    S simple = S { 1, 2 };
    std::string complex = "complex";

    auto a = trivial;               // 1. trivial copy: no ctors involved
    auto b = simple;                // 1. trivial copy: auto-generated copy ctor involved
    auto c = complex;               // 2. deep copy: possible heap allocs, exceptions (`std::bad_alloc`), etc. - uses a copy constructor.
    auto d = std::move(complex);    // 3. move: `complex` likely becomes an empty string, but exact behavior depends on the move constructor.

    std::cout << complex << "\n";   // probably a bug: `complex` was moved from!  exact semantics depend on if a move constructor was implemented, it's exact behavior, etc.
}
```

```rust
// Rust

#[derive(Default)]  // ≈ `S() = default;`
#[derive(Clone)]    // ≈ `S(const S &) = default;`
#[derive(Copy)]     // ≈ `S(const S &) = default;` is just a `memcpy`, don't bother with move semantics
struct S { a: i32, b: i32 }

fn main() {
    let trivial : i32 = 42;
    let simple = S { a: 1, b: 2 };
    let complex : String = "complex".into();

    let a = trivial;                // 1. trivial copy: i32 is `Copy`, no ctors involved
    let b = simple;                 // 1. trivial copy: S is `Copy`, no ctors involved
    let c = complex.clone();        // 2. deep copy: possible heap allocs, panics, etc. - uses `Clone::clone()`.
    let d = complex;                // 3. move: `complex` is *invalidated* and is no longer accessible.  Behavior is a ≈memcpy, there are no move constructors to write.

    println!("{complex}");          // error[E0382]: borrow of moved value: `complex`
}
```

If you wanted C++'s behavior of being able to reuse `complex` in Rust, you could instead write:
```rust
use core::mem::{take, replace, swap};

fn main() {
    let mut complex : String = "complex".into();

    // each of these will replace `complex` with `Default::default()` (an empty string.)
    let e = take(&mut complex);
    let f = replace(&mut complex, Default::default());
    let mut g = String::default(); swap(&mut complex, &mut g);

    println!("{complex}");          // prints an empty string
}
```

One use of nontrivial C++ move constructors is implementing types that self-register with intrusive linked lists.
I've used this when implementing horrific debug macros that allow enumeration of all instances of a given type.
Rust doesn't provide any great means of implementing this for types that don't also self-allocate / [pin their positions](https://doc.rust-lang.org/std/pin/index.html).



[Clone]:    https://doc.rust-lang.org/std/clone/trait.Clone.html
[Copy]:     https://doc.rust-lang.org/std/marker/trait.Copy.html
