## Friday January 7th, 2022
*   Merged (private) thin3d9 into public [thindx](https://github.com/MaulingMonkey/thindx), pushed <https://docs.rs/thindx/> w/ screenshots
*   Worked on (private) 12-bit fantasy console ISA/emulator (match statements vs fn ptr arrays is interesting to compare codegen for)
*   [microsoft/windows-rs#1409 • Bug: TypedEventHandler is unsound](https://github.com/microsoft/windows-rs/issues/1409)
*   [jni-bindgen](https://github.com/MaulingMonkey/jni-bindgen) CI cleanup
*   Discovered [Syncthing](https://syncthing.net/) \([HN](https://news.ycombinator.com/item?id=29837696), [Article](https://tonsky.me/blog/syncthing/)\),
    which looks close to the ideal sync framework I'd been considering "inventing"
*   [rust_win32_async_sample](https://github.com/MaulingMonkey/rust_win32_async_sample/blob/master/src/main.rs), discovered why C# has `Control.BeginInvoke`
    \(`PostThreadMessage`  is unreliable thanks to dialog message loops, but `PostMessage` to an HWND can dispatch to `WndProc` - see [#windows-dev Discord Ramblings](https://discord.com/channels/273534239310479360/583054410670669833/928069858682355733)\)

### TODO:
*   ~~Merge xinput bindings into thindx from wherever I hid them~~
*   Experiment with Syncthing for code backup, media sync?
*   Fuzz coverage for thindx
*   Finish `thindx::d3d9::*`
*   Example coverage for `thindx::d3d9::*`



## Saturday January 8th, 2022
*   Hidden bindings weren't just XInput.  Reimplemented in thindx.
*   TODO: Test/fuzz coverage of `thindx::xinput`
*   TODO: Audit/use [Rust Test Explorer](https://marketplace.visualstudio.com/items?itemName=swellaby.vscode-rust-test-adapter&ssr=false#overview) (VSCode Extension)?



## Wednesday January 12th, 2022
*   [cargo clippy](https://github.com/rust-lang/rust-clippy) might not be too hard to hack on?
    *   <https://github.com/rust-lang/rust-clippy/blob/master/CONTRIBUTING.md>
    *   <https://github.com/rust-lang/rust-clippy/blob/master/doc/adding_lints.md>
*   Rust Code Coverage
    *   <https://www.reddit.com/r/rust/comments/j0ribi/rust_code_coverage_in_vscode/>
    *   <https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters>
    *   <https://github.com/mozilla/grcov#example-how-to-generate-source-based-coverage-for-a-rust-project>
    *   <https://coveralls.io/>
    *   <https://github.com/tismith/example-cli-rs> -> <https://app.codecov.io/gh/tismith/example-cli-rs/blob/master/src/utils/cmdline.rs>
*   `cargo html` doesn't support library wasm demos very well
*   `cargo install wasm-pack --no-default-features` to workaround openssl vendoring issues



## Saturday January 15th, 2022
*   cargo clippy looks annoying to hack on
    *   [dylint](https://lib.rs/crates/dylint) is conceptually cool
    *   experimented with [wasmer](https://docs.rs/wasmer/2.0.0/wasmer/) + `wasm32-{wasi,unknown-unknown}` - not too annoying
    *   `cargo mmlint` could take a mixture of prebuilt wasm binaries + workspace linter project @ custom target dir?
    *   have a "make your own lint rule" focused readme
*   `#![register_tool(...)]` et all are in [backlog hell](https://github.com/rust-lang/rust/issues/66079)
    ```rust
    // eww gross:
    #![feature(register_tool)]
    #![register_tool(mmlint)]
    #[cfg_attr(mmlint, allow(mmlint::whatever))]
    
    // for now:
    #[cfg_attr(mmlint, allow(mmlint_whatever))]
    // ?
    ```



## Monday January 17th, 2022

> ok, since I've been down this path before, I'll link you some stuff <br>
> https://github.com/maroider/syringe : DLL injector I made based on http://blog.opensecurityresearch.com/2013/01/windows-dll-injection-basics.html <br>
> https://lib.rs/crates/djin: DLL injector someone I once chatted with made <br>
> https://github.com/maroider/asbestos :  a thingy I made using function hooking and DLL injection <br>
> --[maroider](https://discord.com/channels/273534239310479360/583054410670669833/932415966518853712) <br>

## Monday January 24th, 2022

*   I should really be trying my hand at libclang for C++ parsing
    *   <https://clang.llvm.org/doxygen/group__CINDEX.html>
    *   `C:\Program Files\LLVM\bin\libclang.dll` (+ `LLVM-C.dll`?)
    *   <https://lib.rs/crates/clang-sys>
    *   <https://lib.rs/crates/clang>
