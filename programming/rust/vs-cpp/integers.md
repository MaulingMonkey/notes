# Rust vs C++: Integers



## Types

| Rust                                                                                                                                  | C [`<stdint.h>`](https://en.cppreference.com/w/c/types/integer) / <br> C++ [`<cstdint>`](https://en.cppreference.com/w/cpp/types/integer) | notes |
| --------------------------------------------------------------------------------------------------------------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------| ------|
| [`i8`](https://doc.rust-lang.org/core/primitive.i8.html)          / [`u8`](https://doc.rust-lang.org/core/primitive.u8.html)          | `int8_t` / `uint8_t`              |
| [`i16`](https://doc.rust-lang.org/core/primitive.i16.html)        / [`u16`](https://doc.rust-lang.org/core/primitive.u16.html)        | `int16_t` / `uint16_t`            |
| [`i32`](https://doc.rust-lang.org/core/primitive.i32.html)        / [`u32`](https://doc.rust-lang.org/core/primitive.u32.html)        | `int32_t` / `uint32_t`            |
| [`i64`](https://doc.rust-lang.org/core/primitive.i64.html)        / [`u64`](https://doc.rust-lang.org/core/primitive.u64.html)        | `int64_t` / `uint64_t`            |
| [`i128`](https://doc.rust-lang.org/core/primitive.i128.html)      / [`u128`](https://doc.rust-lang.org/core/primitive.u128.html)      | `__int128` / `unsigned __int128`  | \[1\]
| [`isize`](https://doc.rust-lang.org/core/primitive.isize.html)    / [`usize`](https://doc.rust-lang.org/core/primitive.usize.html)    | `intptr_t` / `uintptr_t`          |
| [`isize`](https://doc.rust-lang.org/core/primitive.isize.html)    / [`usize`](https://doc.rust-lang.org/core/primitive.usize.html)    | `ptrdiff_t` / `size_t`            | \[2\]
| <code>[core]::[ffi]::[c_char](https://doc.rust-lang.org/core/ffi/type.c_char.html)</code>, [`c_schar`](https://doc.rust-lang.org/core/ffi/type.c_schar.html), [`c_uchar`](https://doc.rust-lang.org/core/ffi/type.c_uchar.html)   | `char`, `signed char`, `unsigned char`   | \[3\]
| <code>[core]::[ffi]::[c_sshort](https://doc.rust-lang.org/core/ffi/type.c_sshort.html)</code>, [`c_ushort`](https://doc.rust-lang.org/core/ffi/type.c_ushort.html)                                                                | `signed short`, `unsigned short`         | \[3\]
| <code>[core]::[ffi]::[c_sint](https://doc.rust-lang.org/core/ffi/type.c_sint.html)</code>, [`c_uint`](https://doc.rust-lang.org/core/ffi/type.c_uint.html)                                                                        | `signed int`, `unsigned int`             | \[3\]
| <code>[core]::[ffi]::[c_slong](https://doc.rust-lang.org/core/ffi/type.c_slong.html)</code>, [`c_ulong`](https://doc.rust-lang.org/core/ffi/type.c_ulong.html)                                                                    | `signed long`, `unsigned long`           | \[3\]
| <code>[core]::[ffi]::[c_slonglong](https://doc.rust-lang.org/core/ffi/type.c_slonglong.html)</code>, [`c_ulonglong`](https://doc.rust-lang.org/core/ffi/type.c_ulonglong.html)                                                    | `signed long long`, `unsigned long long` | \[3\]

1.  Previously, there were some ABI mismatches between Rust and C (different alignments): [rust-lang/rust#54341](https://github.com/rust-lang/rust/issues/54341).
    Hopefully they're all fixed, but caveat emptor!

2.  Rust's standard library and ecosystem currently assumes `sizeof(void*)` = `sizeof(ptrdiff_t`) = `sizeof(size_t)`.
    This wasn't true in the 16-bit era, where far pointers might be 32 bits, but array sizes limited to 16 bits.
    CHERI etc. are threatening to make this a bad assumption today as well, with metadata bloating pointers to 128 bits.
    But *for now*, these assumptions hold.

3.  Rust generally assumes `long` etc. is ABI compatible with some `int*_t` type.
    Some forms of type-checking control flow integrity disagree.
    See <https://docs.rs/cfi-types/> for CFI-friendly <code>[core]::[ffi]::*</code> alternatives... as well as documentation on the compiler flags to use to not need them.



## Semantic Differences: Underflow and Overflow

In C and C++, underflow and overflow are undefined behavior for signed integers.  Scary!

In Rust, the behavior is defined to wrap (default release behavior) or panic (default debug behavior.)
If you always want to wrap instead of panicing, see e.g. the type wrapper <code>[core]::[num]::[Wrapping]\(...\)</code>, or [`wrapping_add`](https://doc.rust-lang.org/std/primitive.u32.html#method.wrapping_add) and it's many friends.
Rust also has many methods if you're planning to handle edge cases in your code that I strongly recommend using:
-   `saturating_*` if you just want to clamp to min/max values.
-   `overflowing_*` if you want to wrap and get a `bool` flag at the same time.
-   `checked_*` methods like [`checked_div`](https://doc.rust-lang.org/std/primitive.u32.html#method.checked_div) for `Option`s to avoid divide by zero bugs etc.

If you absolutely want C and C++'s undefined behavior semantics (you've profiled and actually have a performance issue? nah, you lie...), Rust *does* also provide `unchecked_*`.  Cavet emptor.



## Semantic Differences: Other

| Rust              | C++           | Notes |
| ------------------| --------------| ------|
| `! boolean`       | `! boolean`   | Logical not
| `! integer`       | `~ integer`   | Bitwise not
| `integer == 0`    | `! integer`   | Logical not
| `integer != 0`    | `!! integer`  | The classic double-not trick.

C++'s syntax for logical not performing bitwise not in Rust may look scary, but a lack of implicit integer → boolean conversions in Rust avoid making this a massive footgun in my experience.
...on the other hand, it might trip up code reviewers etc.



<!-- References -->

[`<stdint.h>`]:     https://en.cppreference.com/w/c/types/integer
[`<cstdint>`]:      https://en.cppreference.com/w/cpp/header/cstdint

[core]:             https://doc.rust-lang.org/core/
[ffi]:              https://doc.rust-lang.org/core/ffi/
[num]:              https://doc.rust-lang.org/core/num/index.html
[Wrapping]:         https://doc.rust-lang.org/core/num/struct.Wrapping.html

