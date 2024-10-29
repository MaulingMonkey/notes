# Rust vs C++: Booleans

| Rust                                                                                                                  | C99       | C++98     |
| ----------------------------------------------------------------------------------------------------------------------| ----------| ----------|
| [`bool`](https://doc.rust-lang.org/core/primitive.bool.html)                                                          | `_Bool`   | `bool`    |
| <code>[abibool](https://docs.rs/abibool/)::[bool8](https://docs.rs/abibool/latest/abibool/struct.bool8.html)</code>   | Various   | Various   |
| <code>[abibool](https://docs.rs/abibool/)::[bool32](https://docs.rs/abibool/latest/abibool/struct.bool32.html)</code> | Various   | Various   |

Rust assumes the size of a native boolean type is 1, and that it has the value `0` or `1` - any other value is undefined behavior.
While I believe this is true of all current LLVM targets (and thus by extension `rustc` targets), PPC OS X had `sizeof(bool) == 4`, which this would be incompatible with.
Additionally, many pre-C99 codebases defined `bool` (or similar) to some integer type, not limited to the values `0` or `1`.
See <code>[abibool]</code> for integer-friendly options.

[abibool]:  https://docs.rs/abibool/
