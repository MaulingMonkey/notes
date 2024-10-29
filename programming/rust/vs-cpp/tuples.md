# Rust vs C++: Tuples



## Types

| Rust                                                                  | C++98     | C++11     |
| ----------------------------------------------------------------------| ----------| ----------|
| [`()`](https://doc.rust-lang.org/std/primitive.tuple.html)            | <code>[boost::tuple]\<\></code> ?         | <code>[std::tuple]\<\></code> ?         |
| [`(T1)`](https://doc.rust-lang.org/std/primitive.tuple.html)          | <code>[boost::tuple]\<T1\></code>         | <code>[std::tuple]\<T1\></code>         |
| [`(T1, T2)`](https://doc.rust-lang.org/std/primitive.tuple.html)      | <code>[boost::tuple]\<T1, T2\></code>     | <code>[std::tuple]\<T1, T2\></code>     |
| [`(T1, T2, T3)`](https://doc.rust-lang.org/std/primitive.tuple.html)  | <code>[boost::tuple]\<T1, T2, T3\></code> | <code>[std::tuple]\<T1, T2, T3\></code> |




## Expressions

| Language          | Construct a tuple                                             |
| ------------------| --------------------------------------------------------------|
| Rust              | `(1, 2, 3)`                                                   |
| C++98             | `boost::make_tuple(1, 2, 3)`                                  |
| C++11             | `std::make_tuple(1, 2, 3)` <br> `{1, 2, 3}` (some contexts)   |

| Language          | Destructure a tuple               | Notes                     |
| ------------------| ----------------------------------| --------------------------|
| Rust              | `let (a, b, c) = ...;`            | Defines new vars
| C++98             | `boost::tie(a, b, c) = ...;`      | Variables must already exist, can't be `const`
| C++11             | `std::tie(a, b, c) = ...;`        | Variables must already exist, can't be `const`
| C++17             | `const auto [a, b, c] = ...;`     | Defines new vars

| Language          | Access a tuple's first element    |
| ------------------| ----------------------------------|
| Rust              | `tuple.0`                         |
| C++98             | `get<0>(tuple)`                   |

| Language          | Access a tuple's `int` element    |
| ------------------| ----------------------------------|
| Rust              | *N/A*                             |
| C++98             | `get<int>(tuple)`                 |



## Tuple Structs

Rust also allows the defining of *tuple structs*.
These aren't just a `typedef`, but their own strong types.

```rust
#[derive(Clone, Copy, Debug, PartialEq, Eq, PartialOrd, Ord)]
pub struct MyPair(pub u32, &'static str);

fn main() {
    let tuple = MyPair(42, "Hello, world!"); // construction
    dbg!(tuple.0); // prints 42
    dbg!(tuple.1); // private, so it's only accessable in this module
    dbg!(tuple == tuple); // comparable
    let MyPair(integer, string) = tuple; // destructuring
}
```

The C++ equivalent is likely a horror show that involves deriving from `std::tuple`, or rolling your own everything.
Which *is* possible, but...

```cpp
struct MyPair : std::tuple<uint32_t, std::u8string_view> {
    // can the u8string_view be made private?

    // TODO: implement:
    // ...forwarding and conversion constructors?
    // ...comparison operators?
    // ...operator<<
    // ...destructing operators?
    // ...???
};

int main() {
    const auto tuple = MyPair { 42, u8"Hello, world!"sv };
    std::cerr << "get<0>(tuple) = " << get<0>(tuple) << "\n";
    std::cerr << "get<1>(tuple) = " << get<1>(tuple) << "\n";
    const auto [integer, string] = tuple; // C++17
}
```



<!-- References -->

[boost::tuple]:     https://www.boost.org/doc/libs/1_57_0/libs/tuple/doc/tuple_users_guide.html
[std::tuple]:       https://en.cppreference.com/w/cpp/utility/tuple
