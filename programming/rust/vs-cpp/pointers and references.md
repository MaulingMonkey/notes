# Rust vs C++: Pointers &amp; References



## Types

#### Basic Reference Types

| Rust                                                                                                      | C++ |
| ----------------------------------------------------------------------------------------------------------| ----|
| [`& Struct`](https://doc.rust-lang.org/std/primitive.reference.html) – "(shareable) reference"            | `const Struct *`      &mdash; must point to a valid `Struct`  |
| [`& mut Struct`](https://doc.rust-lang.org/std/primitive.reference.html) – read as "exclusive reference"  | `Struct * restrict`   &mdash; must point to a valid `Struct`  |
| [`* const Struct`](https://doc.rust-lang.org/std/primitive.pointer.html)                                  | `const Struct *`  (may point to null, invalid, dangle, etc.)  |
| [`* mut Struct`](https://doc.rust-lang.org/std/primitive.pointer.html)                                    | `Struct *`        (may point to null, invalid, dangle, etc.)  |

#### References to Dynamically Sized Types

| Rust                                                                              | C++                                                               |
| ----------------------------------------------------------------------------------| ------------------------------------------------------------------|
| <code>&amp;dyn [Trait](https://doc.rust-lang.org/1.8.0/book/traits.html)</code>   | <code>[std::tuple]\<const TraitVtable *, const Struct &\></code>  |
| <code>&amp;[str](https://doc.rust-lang.org/std/primitive.str.html)</code>         | <code>[std::u8string_view]</code>                                 |
| <code>&amp;[\[u8\]](https://doc.rust-lang.org/std/primitive.slice.html)</code>    | <code>[std::span]\<const uint8_t\></code>                         |

[std::tuple]:           https://en.cppreference.com/w/cpp/utility/tuple
[std::u8string_view]:   https://en.cppreference.com/w/cpp/string/basic_string_view
[std::span]:            https://en.cppreference.com/w/cpp/container/span

#### Smart Pointers

| Rust                                                                                          | C++03                                 | C++11                                 |
| ----------------------------------------------------------------------------------------------| --------------------------------------| --------------------------------------|
| <code>[std]::[boxed]::[Box]\<T\></code>                                                       | <code>[boost::scoped_ptr]\<T\></code> | <code>[std::unique_ptr]\<T\></code>   |
| <code>[std]::[sync]::[Arc]\<T\></code>                                                        | <code>[boost::shared_ptr]\<T\></code> | <code>[std::shared_ptr]\<T\></code>   |
| <code>[std]::[sync]::[Weak](https://doc.rust-lang.org/std/sync/struct.Weak.html)\<T\></code>  | <code>[boost::weak_ptr]\<T\></code>   | <code>[std::weak_ptr]\<T\></code>     |
| <code>[std]::[rc]::[Rc]\<T\></code> †                                                         | <code>[boost::shared_ptr]\<T\></code> | <code>[std::shared_ptr]\<T\></code>   |
| <code>[std]::[rc]::[Weak](https://doc.rust-lang.org/std/rc/struct.Weak.html)\<T\></code> †    | <code>[boost::weak_ptr]\<T\></code>   | <code>[std::weak_ptr]\<T\></code>     |

† Rust's <code>[std]::[rc]::\*</code> uses non-atomic refcounts

[boxed]:                https://doc.rust-lang.org/std/boxed/
[rc]:                   https://doc.rust-lang.org/std/rc/
[sync]:                 https://doc.rust-lang.org/std/sync/
[Arc]:                  https://doc.rust-lang.org/std/sync/struct.Arc.html
[Box]:                  https://doc.rust-lang.org/std/boxed/struct.Box.html

[boost::shared_ptr]:    https://www.boost.org/doc/libs/1_87_0/libs/smart_ptr/doc/html/smart_ptr.html#shared_ptr
[boost::scoped_ptr]:    https://www.boost.org/doc/libs/1_87_0/libs/smart_ptr/doc/html/smart_ptr.html#scoped_ptr
[boost::weak_ptr]:      https://www.boost.org/doc/libs/1_87_0/libs/smart_ptr/doc/html/smart_ptr.html#weak_ptr
[std::shared_ptr]:      https://en.cppreference.com/w/cpp/memory/shared_ptr
[std::unique_ptr]:      https://en.cppreference.com/w/cpp/memory/unique_ptr
[std::weak_ptr]:        https://en.cppreference.com/w/cpp/memory/weak_ptr



<!-- Rust -->
[alloc]:        https://doc.rust-lang.org/alloc/index.html
[core]:         https://doc.rust-lang.org/core/index.html
[std]:          https://doc.rust-lang.org/std/index.html

<style>
    code { white-space: pre; }
</style>
