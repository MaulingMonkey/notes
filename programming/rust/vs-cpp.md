<https://maulingmonkey.com/guide/cpp-vs-rust/>

# WIP conversion of said guide:

## Type Equivalents

See also [Notes on Type Layouts and ABIs in Rust](https://gankro.github.io/blah/rust-layouts-and-abis/#the-layoutsabis-of-builtins).

This is an intentionally incomplete list with exemplar types and some overlap. Also note that for FFI, the Rust types sometimes impose additional restrictions over the equivalent C types. Bool must be `0`/`1` (not merely truthy), non-listed enum values have undefined behavior (maybe use a `u??` instead at the FFI layer), etc.

| Rust                                                        | C++ |
| ------------------------------------------------------------| ----|
| [`bool`](https://doc.rust-lang.org/std/primitive.bool.html) | `bool`, assuming `sizeof(bool)==1`
| [`abibool::???`](https://docs.rs/abibool/)                  | Exact FFI match for other ≈boolean types
| [`char`](https://doc.rust-lang.org/std/primitive.char.html) (guaranteed [Unicode Scalar Value](https://www.unicode.org/glossary/#unicode_scalar_value)) | ≈ `char32_t` (might not be a [USV](https://www.unicode.org/glossary/#unicode_scalar_value))
| [`u8`](https://doc.rust-lang.org/std/primitive.u8.html), [`u16`](https://doc.rust-lang.org/std/primitive.u16.html), [`u32`](https://doc.rust-lang.org/std/primitive.u32.html), [`u64`](https://doc.rust-lang.org/std/primitive.u64.html)          | `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`
| [`i8`](https://doc.rust-lang.org/std/primitive.i8.html), [`i16`](https://doc.rust-lang.org/std/primitive.i16.html), [`i32`](https://doc.rust-lang.org/std/primitive.i32.html), [`i64`](https://doc.rust-lang.org/std/primitive.i64.html)          | `int8_t`, `int16_t`, `int32_t`, `int64_t`
| [`u128`](https://doc.rust-lang.org/std/primitive.u128.html), [`i128`](https://doc.rust-lang.org/std/primitive.i128.html) | ≈ `__uint128`, `__int128`, but [different alignment](https://github.com/rust-lang/rust/issues/54341)
| [`f32`](https://doc.rust-lang.org/std/primitive.f32.html), [`f64`](https://doc.rust-lang.org/std/primitive.f64.html) | `float`, `double`, on most implementations
| [`usize`](https://doc.rust-lang.org/std/primitive.usize.html), [`isize`](https://doc.rust-lang.org/std/primitive.isize.html) | `uintptr_t`, `intptr_t`
| [`usize`](https://doc.rust-lang.org/std/primitive.usize.html), [`isize`](https://doc.rust-lang.org/std/primitive.isize.html) | ≈ `size_t`, `ptrdiff_t`
| [`()`](https://doc.rust-lang.org/std/primitive.unit.html) – read as "unit"                         | `void`
| [`!`](https://doc.rust-lang.org/std/primitive.never.html) – read as "never"                        | `[[noreturn]] void`
| [`&Struct`](https://doc.rust-lang.org/std/primitive.reference.html) – "(shareable) reference"      | `const Struct &`
| `&DynamicallySizedType` (TODO: link)                                                               | `std::tuple<const DynamicallySizedType *, size_t>`
| <code>&amp;dyn [Trait](https://doc.rust-lang.org/1.8.0/book/traits.html)</code>                    | `std::tuple<const TraitVtable *, const Struct &>`
| `&Trait` – Pre 1.27 version                                                                        | `std::tuple<const TraitVtable *, const Struct &>`
| [`&mut T`](https://doc.rust-lang.org/std/primitive.reference.html) – read as "exclusive reference" | Like !mut, but exclusive access and writable


| Rust                                                        | C++ |
| ------------------------------------------------------------| ----|
| <code>std::option::[Option](https://doc.rust-lang.org/std/option/enum.Option.html)</code> | [`std::optional`](https://en.cppreference.com/w/cpp/utility/optional) (C++17)
| <code>std::result::[Result](https://doc.rust-lang.org/std/result/enum.Result.html)</code> | [`std::expected`](https://en.cppreference.com/w/cpp/utility/expected) (C++23)
| <code>std::result::Result::[Err](https://doc.rust-lang.org/std/result/enum.Result.html#variant.Err)</code> | [`std::unexpected`](https://en.cppreference.com/w/cpp/utility/expected/unexpected) (C++23)
| <code>std::result::Result::[Ok](https://doc.rust-lang.org/std/result/enum.Result.html#variant.Ok)</code> | No extra type/wrapper

<!-- TODO -->

<!-- TODO: Syntax Equivalents -->

<!-- TODO: The Obvious -->

<!-- TODO: The Less Obvious -->

## Interior Mutability

See also [The Rust PL 2nd Ed.](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html) on Interior Mutability.

Like in C++, the `const`ness of the outer object can - but does not always - affect access to it's members. For example, given `const std::vector<int> & v`, you cannot modify the elements of the vector with `v[0] = 42;` - this will fail to compile. Given a `const gsl::span<int> & av`, however, `av[0] = 42;` will compile just fine. Rust calls this second case "interior mutability".

Another example: In C++, given `const some_struct & s`, you cannot modify members with `s.some_field = 42;` unless `some_field` used the `mutable` keyword. Similarly, in Rust, given `s : &SomeStruct`, you cannot modify members with `s.some_field = 42;` unless some_field was wrapped in `Cell` or another type that provides "interior mutability".

Because immutability is tied to the ability to share a value in Rust, "interior mutability" is used in some places that may be suprising to a C++ programmer. For example, you can modify an "immutable" Rust `std::sync::atomic::AtomicUsize`, even though you wouldn't be able to modify a C++ `const std::atomic<size_t>`, because `AtomicUsize` has been given "interior mutability". Without interior mutability, it'd be impossible to share a AtomicUsize between multiple threads while allowing write access - defeating the entire purpouse of having atomic types in the first place.

Some of the types in Rust providing interior mutability - generally, you only need one at a time:

| Rust Type | Description |
| ----------| ------------|
| <code>std::cell::[Cell](https://doc.rust-lang.org/std/cell/struct.Cell.html)&lt;T&gt;</code> | Exists specifically to give you Interior Mutability for "Free". This type works by giving you methods like `get()`, and `set(value)`, and enforcing at compile time that you never hold onto a `&T` or `&mut T` (unless you have an exclusive mutable reference to the whole `Cell` at once.) Cannot be shared between threads. `T` must be `Copy` able.
| <code>std::cell::[RefCell](https://doc.rust-lang.org/std/cell/struct.RefCell.html)&lt;T&gt;</code> | Exists specifically to give you Interior Mutability. Unlike `Cell<T>`, this allows you to get a `&T` or `&mut T` from an immutable instance of `RefCell<T>`, and instead enforces at run time that you'll never have a `&mut T` and any other references at the same time. Methods like `borrow()` can panic, but non-panicing options like `try_borrow()` are also available. Cannot be shared between threads.
| <code>std::cell::[UnsafeCell](https://doc.rust-lang.org/std/cell/struct.UnsafeCell.html)&lt;T&gt;</code> | This exists, but you'll need to use unsafe code and raw pointers. Best of luck! This is probably the closest equivalent to C++'s `mutable` keyword.
| <code>std::sync::[Mutex](https://doc.rust-lang.org/std/sync/struct.Mutex.html)&lt;T&gt;</code> | Similar to `RefCell<T>`, but thread safe, and so you `lock()` it instead of `borrow()`ing it. Doing so multiple times from the same thread is less specified: It may deadlock, or it may panic. It may also panic if another thread paniced while holding onto the mutex.
| <code>std::sync::[RwLock](https://doc.rust-lang.org/std/sync/struct.RwLock.html)&lt;T&gt;</code> | Similar to `Mutex<T>`, only allowing multiple readers simultaniously.
| <code>std::sync::[atomic](https://doc.rust-lang.org/std/sync/atomic/index.html)::[AtomicUsize](https://doc.rust-lang.org/std/sync/atomic/struct.AtomicUsize.html)</code> | Atomic types also implement interior mutability so they can be shared between multiple threads.

<!-- TODO: Dynamically Sized Types -->
<!-- TODO: Virtual Dispatch -->
<!-- TODO: Function Overloading -->
<!-- TODO: Are we MaulingMonkey yet? -->
