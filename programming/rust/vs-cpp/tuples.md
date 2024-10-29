# Rust vs C++: Tuples



## Types

| Rust                                                                  | C++98                                                                         | C++11                                 |
| ----------------------------------------------------------------------| ------------------------------------------------------------------------------| --------------------------------------|
| [`()`](https://doc.rust-lang.org/std/primitive.tuple.html)            | <code>[boost::tuple]\<\></code> ?                                             | <code>[std::tuple]\<\></code> ?       |
| [`(A)`](https://doc.rust-lang.org/std/primitive.tuple.html)           | <code>[boost::tuple]\<A\></code>                                              | <code>[std::tuple]\<A\></code>        |
| [`(A, B)`](https://doc.rust-lang.org/std/primitive.tuple.html)        | <code>[std::pair]\<A, B\> </code> <br> <code>[boost::tuple]\<A, B\></code>    | <code>[std::tuple]\<A, B\></code>     |
| [`(A, B, C)`](https://doc.rust-lang.org/std/primitive.tuple.html)     | <code>[boost::tuple]\<A, B, C\></code>                                        | <code>[std::tuple]\<A, B, C\></code>  |




## Expressions

| Language          | Construct a tuple                                                                 |
| ------------------| ----------------------------------------------------------------------------------|
| Rust              | `(1, 2)`                                                                          |
| C++98             | `std::make_pair(1, 2)` <br> `boost::make_tuple(1, 2)`                             |
| C++11             | `std::make_pair(1, 2)` <br> `std::make_tuple(1, 2)` <br> `{1, 2}` (some contexts) |

| Language          | Destructure a tuple               | Notes                     |
| ------------------| ----------------------------------| --------------------------|
| Rust              | `let (a, b, c) = ...;`            | Defines new vars
| C++98             | `boost::tie(a, b, c) = ...;`      | Variables must already exist, can't be `const`
| C++11             | `std::tie(a, b, c) = ...;`        | Variables must already exist, can't be `const`
| C++17             | `const auto [a, b, c] = ...;`     | Defines new vars

| Language          | Access a tuple's first element        |
| ------------------| --------------------------------------|
| Rust              | `tuple.0`                             |
| C++98             | `get<0>(tuple)` <br> `pair.first`     |

| Language          | Access a tuple's second element       |
| ------------------| --------------------------------------|
| Rust              | `tuple.1`                             |
| C++98             | `get<1>(tuple)` <br> `pair.second`    |

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
[std::pair]:        https://en.cppreference.com/w/cpp/utility/pair
