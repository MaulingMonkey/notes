 # Nightly-on-Stable Shenannigans
 
* [cargo::core::Features::enable_nightly_features](https://docs.rs/cargo/0.48.0/cargo/core/features/fn.enable_nightly_features.html)
* https://doc.rust-lang.org/book/appendix-07-nightly-rust.html
* for cargo: [`__CARGO_TEST_CHANNEL_OVERRIDE_DO_NOT_USE_THIS=nightly`](https://github.com/rust-lang/cargo/blob/dbbab425859dc0cb121175cb8631d391b63d4eff/src/cargo/core/features.rs#L508-L550)
* for rustc: [`RUSTC_BOOTSTRAP=1`](https://github.com/rust-lang/rust/blob/ac48e62db85e6db4bbe026490381ab205f4a614d/library/test/src/cli.rs#L276-L284)
* `-Z no-index-update`
* [cargo: Unstable Features](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html)
* [rustc: The Unstable Book](https://doc.rust-lang.org/unstable-book/the-unstable-book.html)
* [rustdoc: Unstable features](https://doc.rust-lang.org/rustdoc/unstable-features.html)

# rustc/cargo profiling

https://blog.rust-lang.org/inside-rust/2020/02/25/intro-rustc-self-profile.html

```cmd
rustup update nightly

cargo +nightly check -Z timings
set RUSTFLAGS=-Zself-profile=profiles
cargo +nightly check
set RUSTFLAGS=

:: Text output
cargo install --git https://github.com/rust-lang/measureme --tag 0.7.0 summarize
summarize summarize profiles\crate-12345 | more

:: SVG output (open rustc.svg in Chrome or another browser)
cargo install --git https://github.com/rust-lang/measureme --tag 0.7.0 flamegraph
flamegraph          profiles\crate-12345

:: chrome_profiler.json output (open in Chrome devtools performance tab via RMB -> "Load profile")
cargo install --git https://github.com/rust-lang/measureme --tag 0.7.0 crox
crox                profiles\crate-12345
```
