Cheat sheet for the [`windows`](https://microsoft.github.io/windows-docs-rs/doc/windows/) crate.  Note that I don't actually use the `windows` crate.
- [generated docs](https://microsoft.github.io/windows-docs-rs/doc/windows/)
- [github](https://github.com/microsoft/windows-rs/tree/master)
- [crates/samples/windows](https://github.com/microsoft/windows-rs/tree/master/crates/samples/windows)

# Strings

Literals: <code>[`windows::core`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/index.html)::{[`h!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.h.html), [`s!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.s.html), [`w!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.w.html)}</code>

"WinRT" strings: <code>[windows::core](https://microsoft.github.io/windows-docs-rs/doc/windows/core/index.html)::{[HSTRING](https://microsoft.github.io/windows-docs-rs/doc/windows/core/struct.HSTRING.html), [h!](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.h.html)}</code> - refcounted, immutable, utf16ish
- Implements `From`, `TryFrom`, `PartialEq` for many things
- [`impl IntoParam<PCWSTR> for &HSTRING`](https://github.com/microsoft/windows-rs/blob/0a64f0ea5905af961bd4b1223c0ffd7a397e4317/crates/libs/core/src/strings/hstring.rs#L404-L408)

Narrow strings:
- Use `std::ffi::{CStr, CString}`
- Pass to `windows` via `PCSTR(my_cstr.as_ptr().cast())`?
- Avoid - ambiguous locale specific unicode-hating encoding hell
