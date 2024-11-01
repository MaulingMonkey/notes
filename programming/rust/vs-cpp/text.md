# Rust vs C++: Text



## Encoding

C++ comes from a pre-unicode world.
Text encoding is often vauge and garbled.
What is a `std::string`?
UTF-8?
Windows-1251?
Codepage 437?
Shift-JIS?
EBCDIC?

Rust comes from a post-unicode world.
The native string types are UTF-8.
*Valid* UTF-8.
**This is enforced on pain of undefined behavior.**
(Alternative string types are provided for non-UTF8 text.)



## `\0`-Termination

C and C++ strings are typically `\0`-terminated, with APIs omitting an explicit string length parameter.
This causes some awkwardness with string slices - which must often be deep copied so said `\0` can be added.
This also causes issues when a pointer + length pair covers a range which contains interior, non-terminal `\0`s.

Rust strings **aren't** typically `\0`-terminated.  Everything is pointer + length pairs.
This *does* dial up the awkwardness with string slices to 11 when calling C or C++ code from Rust.



## Locale

C and C++ APIs often depend on ambient global locale state to determine how strings are formatted and sorted.
This often leads to horrors like the French being forced to set `LANG=C.UTF-8` lest they end up with pi encoded as `3,1415` in their JSON instead of `3.1415`.

Rust's standard library is all effectively invariant locale (≈`C.UTF-8`).
Third party crates are available for locale-aware text manipulation.



## Types

| Use case                      | Rust                                                          | C++                                       |
| ------------------------------| --------------------------------------------------------------| ------------------------------------------|
| UTF-8 string storage          | <code>[alloc]::[string]::[String]</code>  <sup>\[1\]</sup>    | [`std::u8string`]                         |
| Path storage (Linux)          | <code>[std]::[path]::[PathBuf]</code>     <sup>\[2\]</sup>    | [`std::string`]                           |
| Path storage (Windows)        | <code>[std]::[path]::[PathBuf]</code>     <sup>\[2\]</sup>    | [`std::wstring`]                          |
| UTF-8 string slices           | <code>&amp;[str]</code>                   <sup>\[1\]</sup>    | [`std::u8string_view`]                    |
| Bytes                         | <code>[u8]</code>                                             | `char`                <sup>\[3\]</sup>    |
| Ambiguous narrow code units   | <code>[u8]</code>                                             | `char`                <sup>\[3\]</sup>    |
| UTF-8  code unit              | <code>[u8]</code>                                             | `char8_t`                                 |
| UTF-16 code unit              | <code>[u16]</code>                                            | `char16_t`                                |
| UTF-32 code unit              | <code>[u32]</code>                                            | `char32_t`                                |
| Portability Nightmares        | <code>[u16]</code> or <code>[u32]</code>                      | `wchar_t`             <sup>\[4\]</sup>    |
| [Unicode Scalar Value]s       | <code>[char]</code>                       <sup>\[5\]</sup>    | `char32_t`                                |

1.  Rust's UTF-8 string types **must** be valid UTF-8, on pain of undefined behavior.
2.  Rust's `OsStr` family of string types need not be valid UTF-8.  E.g. in practice on Windows they're [WTF-8](https://simonsapin.github.io/wtf-8/), on Linux they're byte buffers.
3.  C++'s `char` might technically be signed, or have more than 8 bits.
4.  C++'s `wchar_t` is a portability nightmare.  It's often not even the correct native code unit type for your platform - e.g. iOS uses UTF-16, with a 16-bit `unichar`... and a 32-bit `wchar_t`.
5.  Rust's `char` **must** be a valid [Unicode Scalar Value], on pain of undefined behavior.



<!-- TODO: ## Console Output -->

<!-- TODO: ## Formatting -->

<!-- TODO: ## Parsing -->

<!-- TODO: ## Localization -->

<!-- TODO: compare/contrast Rust vs C++'s iostreams. -->

<!-- TODO: rant about env var based sprintf breaking things when e.g. `,.` are swapped -->

<!-- TODO: OsStr, Path slices, CStr, CString, other crates, ... -->



<!-- References -->

[`std::u8string`]:      https://en.cppreference.com/w/cpp/string/basic_string
[`std::string`]:        https://en.cppreference.com/w/cpp/string/basic_string
[`std::wstring`]:       https://en.cppreference.com/w/cpp/string/basic_string

[`std::u8string_view`]: https://en.cppreference.com/w/cpp/string/basic_string_view
[`std::string_view`]:   https://en.cppreference.com/w/cpp/string/basic_string_view
[`std::wstring_view`]:  https://en.cppreference.com/w/cpp/string/basic_string_view

[core]:                 https://doc.rust-lang.org/core/index.html
[alloc]:                https://doc.rust-lang.org/alloc/index.html
[std]:                  https://doc.rust-lang.org/std/index.html

[ffi]:                  https://doc.rust-lang.org/std/ffi/index.html
[string]:               https://doc.rust-lang.org/alloc/string/index.html
[path]:                 https://doc.rust-lang.org/std/path/struct.PathBuf.html

[String]:               https://doc.rust-lang.org/std/string/struct.String.html
[OsStr]:                https://doc.rust-lang.org/std/ffi/struct.OsStr.html
[OsString]:             https://doc.rust-lang.org/std/ffi/struct.OsString.html
[Path]:                 https://doc.rust-lang.org/std/path/struct.Pat.html
[PathBuf]:              https://doc.rust-lang.org/std/path/struct.PathBuf.html

[char]:                 https://doc.rust-lang.org/std/primitive.char.html
[str]:                  https://doc.rust-lang.org/std/primitive.str.html
[u8]:                   https://doc.rust-lang.org/std/primitive.u8.html
[u16]:                  https://doc.rust-lang.org/std/primitive.u16.html
[u32]:                  https://doc.rust-lang.org/std/primitive.u32.html

[Unicode Scalar Value]: https://www.unicode.org/glossary/#unicode_scalar_value
