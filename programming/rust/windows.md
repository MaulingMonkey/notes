Cheat sheet for the [`windows`](https://microsoft.github.io/windows-docs-rs/doc/windows/) crate.  Note that I don't actually use the `windows` crate.
- [generated docs](https://microsoft.github.io/windows-docs-rs/doc/windows/)
- [crates/samples/windows](https://github.com/microsoft/windows-rs/tree/master/crates/samples/windows)

# Strings

Literals: <code>[`windows::core`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/index.html)::{[`h!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.h.html), [`s!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.s.html), [`w!`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.w.html)}</code>

"WinRT" strings: <code>[windows::core](https://microsoft.github.io/windows-docs-rs/doc/windows/core/index.html)::{[HSTRING](https://microsoft.github.io/windows-docs-rs/doc/windows/core/struct.HSTRING.html), [h!](https://microsoft.github.io/windows-docs-rs/doc/windows/core/macro.h.html)}</code> - refcounted, immutable, utf16ish
- Implements `From`, `TryFrom`, `PartialEq` for many things
- Can be passed (by reference?) to a `IntoParam<PCWSTR>`?
