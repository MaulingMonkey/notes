| feature       | [winapi]                                                           | [windows]<br>[windows-sys] |
| ------------- | ------------------------------------------------------------------ | ------- |
| safety        | ⚠️ everything is `unsafe`, which is at least sound<br>[winapi-rs#945](https://github.com/retep998/winapi-rs/issues/945) (`CONTEXT` is underaligned) | ❌ history of (now fixed) soundness issues like:<br>[windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) (unsound delegates)<br>[win32metadata#917](https://github.com/microsoft/win32metadata/issues/917) (very wrong fn signatures)<br>[win32metadata#1044](https://github.com/microsoft/win32metadata/issues/1044) (`CONTEXT` underaligned)
| api stability | ✔️ fairly stable 0.3.x                                            | ❌ 0.39 and continuing to skyrocket
| codegen       | ⚠️ manual                                                         | ⚠️ from `*.winmd` metadata |
| coverage      | ⚠️ decent coverage of Win32, but missing some stuff               | ✔️ everything under the sun, including new UWP APIs
| docs          | ⚠️ [docs.rs](https://docs.rs/winapi/) at least has all symbols    | ⚠️ [docs.rs](https://docs.rs/windows-sys/) has all symbols for windows<strong>-sys</strong><br>⚠️ [github pages](https://microsoft.github.io/windows-docs-rs/doc/windows/) I can't remember the URL of, for windows
| maintainence  | ❌ abandoned, although the author is at least alive               | ✔️ responsive last I checked |
| api design    | ⚠️ 1:1 match of headers and their mishandled constness            | ⚠️ [windows] drowns in traits and newtypes that add little<br>⚠️ [windows-sys] gives up and uses `*mut c_void` for COM

[winapi]:      https://docs.rs/winapi/
[windows]:     https://microsoft.github.io/windows-docs-rs/doc/windows/
[windows-sys]: https://docs.rs/windows-sys/

### Safety

The underlying windows APIs being called into are rife with subtle and undocumented edge cases, overflows, unchecked parameters, etc. that will lead to soundness bugs in practice if naively wrapped.  Neither [winapi] nor [windows] have much of an advantage on this front.  However, where [winapi] at least makes everything `unsafe` - and thus at least sound - [windows] attempts to mark things safe/sound.  One could argue in favor of this as an attempt to reduce code reviewer fatigue.  I, however, argue against this, as it's resulted in a history of systemic soundness issues such as:

* [windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) (unsound delegates)
* [win32metadata#917](https://github.com/microsoft/win32metadata/issues/917) (very wrong fn signatures)

Writing safe wrappers around windows APIs demands particularized attention that I'd argue a metadata-driven crate such as [windows] cannot provide.

### API Stability

This mostly matters if you're releasing libraries to crates.io that might interoperate with windows FFI crate types.
For example, [`mcom::Rc`](https://docs.rs/mcom/latest/mcom/struct.Rc.html) has a generic contraint on [`winapi::Interface`](https://docs.rs/winapi/0.3/winapi/trait.Interface.html), more or less limiting support to one version of winapi at a time.
Attempting to support a constraint on [`windows::core::Interface`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/trait.Interface.html) would force `mcom` to semver churn at the same rate as [windows].  Fortunately, windows brings its own smart pointer types.  *Un*fortunately, AsRef/From/Into interop with said smart pointer types would quickly result in having support for many versions of [windows] behind many feature gates.

[windows-sys] is at least no longer bumping versions unless actual metadata updates change types/fns, but that still means multiple major version bumps a year vs [winapi] remaining on 0.3.x since before [windows]'s first release.

For application development, this is almost entirely moot.  Your `Cargo.lock` file means you'll only upgrade when you want to, and I believe much of the breakage is fairly minor in practice.
