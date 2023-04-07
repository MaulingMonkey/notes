<https://maulingmonkey.com/guide/cpp-vs-rust/>

# WIP conversion of said guide:

## Type Equivalents

See also [Notes on Type Layouts and ABIs in Rust](https://gankro.github.io/blah/rust-layouts-and-abis/#the-layoutsabis-of-builtins).

This is an intentionally incomplete list with exemplar types and some overlap. Also note that for FFI, the Rust types sometimes impose additional restrictions over the equivalent C types. Bool must be 0/1 (not merely truthy), non-listed enum values have undefined behavior (maybe use a u?? instead at the FFI layer), etc.

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
