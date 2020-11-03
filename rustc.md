# Docs

* [Project Layout](https://doc.rust-lang.org/cargo/guide/project-layout.html), [The Manifest Format](https://doc.rust-lang.org/cargo/reference/manifest.html), [Category Slugs](https://crates.io/category_slugs)
* std docs: [stable](https://doc.rust-lang.org/stable/std/), [nightly](https://doc.rust-lang.org/nightly/std/)
* [docs.rs](https://docs.rs/) - documentation for crates
* [lib.rs](https://lib.rs/) - search for crates

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

# Profiles

* Custom named profiles:                https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#custom-named-profiles
* Documentation of default profiles:    https://doc.rust-lang.org/cargo/reference/manifest.html#the-profile-sections

```
cargo build             # profile.dev
cargo build --release   # profile.release
cargo test --no-run     # profile.test
cargo bench --no-run    # profile.bench

cargo rustc [--profile=dev]             # profile.dev
cargo rustc --release [--profile=dev]   # profile.release
cargo rustc --profile=test              # profile.test
cargo rustc --release --profile=bench   # profile.bench
```

Prefer the former commands as the latter only work for one local crate at a time, whereas the former can span the entire workspace.

# link.exe nonsense

https://discordapp.com/channels/186813135263367169/186813135263367169/560552502542598170
@Mushu#0305 so it looks like if `VCINSTALLDIR` is set, it'll use link.exe from PATH (e.g. probably:)
```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\Hostx86\x86\link.exe
```
which will target 32-bit by default, otherwise it'll use a different linker depending on `--target`, e.g. one of:
```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\HostX64\x64\link.exe
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\HostX64\x86\link.exe
```

https://discordapp.com/channels/186813135263367169/186813135263367169/560545881519030272
Vanilla cmd.exe:
```
C:\rust>cargo build --target=i686-pc-windows-msvc
Compiling rustexe v0.1.0 (C:\rust\rustexe)
Compiling rustlib v0.1.0 (C:\rust\rustlib)
Finished dev [unoptimized + debuginfo] target(s) in 0.73s

C:\rust>cargo build --target=x86_64-pc-windows-msvc
Compiling rustlib v0.1.0 (C:\rust\rustlib)
Compiling rustexe v0.1.0 (C:\rust\rustexe)
Finished dev [unoptimized + debuginfo] target(s) in 0.45s
```

"Developer Command Prompt for VS 2017":
```
C:\rust>cargo build --target=i686-pc-windows-msvc
Compiling rustexe v0.1.0 (C:\rust\rustexe)
Compiling rustlib v0.1.0 (C:\rust\rustlib)
Finished dev [unoptimized + debuginfo] target(s) in 0.75s

C:\rust>cargo build --target=x86_64-pc-windows-msvc
Compiling rustlib v0.1.0 (C:\rust\rustlib)
Compiling rustexe v0.1.0 (C:\rust\rustexe)
error: linking with `link.exe` failed: exit code: 1112
|
= note: [...extremely detailed link.exe command line snipped...]
= note: msvcrt.lib(chkstk.obj) : fatal error LNK1112: module machine type 'x86' conflicts with target machine type 'x64'


error: aborting due to previous error

error: Could not compile `rustexe`.

To learn more, run the command again with --verbose.
```

# Building rustc

https://github.com/rust-lang/rust#building-on-windows
https://rust-lang.github.io/rustc-guide/how-to-build-and-run.html
https://www.msys2.org/

# Fuzzing

https://github.com/PistonDevelopers/image-png/blob/master/png-afl/src/main.rs
