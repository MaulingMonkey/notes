# Rust vs C++: Hello World!



## C++

TODO: Installation, build, and run instructions?

```cpp
// main.cpp
#include <iostream>

int main() {
    std::cout << "Hello, world!\n";
}
```



## Rust

### 1. Install command line tools.

Follow the instructions at <https://rustup.rs/>.
This will install the latest versions of your bread and butter, `cargo` and `rustc`.
You may need to sign out or restart for your `PATH` to be updated to include them.
You'll also be able to update them with `rustup update`.

### 2. Write a simple program.

```rust
// src/main.rs
fn main() {
    println!("Hello, world!");
}
```

### 3. Compile and run it manually, because you hate yourself.

This is ["uphill both ways through snow"](https://www.reddit.com/r/NoStupidQuestions/comments/k8m9eb/where_did_the_trope_of_when_i_was_your_age_i_had/) territory.
Honestly, don't do this, unless you're writing your own package manager or something similarly crazy and want to do everything in some nonstandard way.
Rust's entire ecosystem is incredibly `cargo`-centric.
You have been warned.

```bat
:: Windows: creates `main.exe` (program) and `main.pdb` (debug info)
rustc src\main.rs
main
```

```sh
# Linux, OS X, etc.: creates `main`
rustc src/main.rs
./main
```

### 4. Okay, let's flip a coin.

Documentation for the version of `rand` we'll use: <https://docs.rs/rand/0.9.0/>.
I don't even know how to download `rand` except via `cargo` so we'll use it.
N.B. while I'm writing all files by hand here, `cargo new --bin flip` would've created `.gitignore`, `Cargo.toml`, and `src/main.rs` as a start for us.

```toml
# Cargo.toml
[package]
name    = "flip"        # Required: ≈arbitrary, but it's also used as the basis of your executable's name.
edition = "2024"        # "Optional", but we'll get annoying warnings about defaulting to the ancient "2015" without it and be stuck with old school syntax and quirks.

[dependencies]
rand    = "=0.9.0"      # If you want to ensure you're following the tutorial exactly
#rand   = "0.9"         # If you want your code to be broken/fixed by e.g.  0.9.1 ("minor" semver bumps)
#rand   = "*"           # If you want your code to be broken/fixed by e.g. 0.13.0 ("major" semver bumps)
```

```rust
// src/main.rs
use rand::prelude::*;

fn main() {
    let mut prng = rand::rng();
    let flip = ["heads", "tails"].choose(&mut prng).unwrap();
    println!("Flipped a coin and got {flip}");
}
```

### 5. Download dependencies, compile, and run

```sh
cargo run
```

This will:
-   Download and cache `rand` and it's dependencies
-   Create or update `Cargo.lock`, listing the exact versions of all dependencies chosen (for e.g. build reproducability purpouses.)
-   Compile your dependencies to `target\debug\...`
-   Compile your program to `target\debug\flip.exe`
-   Run your program

```text
C:\local\_archive\2025\flip>cargo run
    Updating crates.io index
     Locking 25 packages to latest Rust 1.85.0 compatible versions
  Downloaded getrandom v0.3.1
  Downloaded windows-targets v0.52.6
  Downloaded rand_core v0.9.3
  Downloaded rand_chacha v0.9.0
  Downloaded ppv-lite86 v0.2.21
  Downloaded rand v0.9.0
  Downloaded zerocopy v0.8.23
  Downloaded windows_x86_64_msvc v0.52.6
  Downloaded 8 crates (1.3 MB) in 1.35s
   Compiling windows_x86_64_msvc v0.52.6
   Compiling zerocopy v0.8.23
   Compiling getrandom v0.3.1
   Compiling cfg-if v1.0.0
   Compiling windows-targets v0.52.6
   Compiling rand_core v0.9.3
   Compiling ppv-lite86 v0.2.21
   Compiling rand_chacha v0.9.0
   Compiling rand v0.9.0
   Compiling flip v0.0.0-git (C:\local\_archive\2025\flip)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.44s
     Running `target\debug\flip.exe`
Flipped a coin and got heads

C:\local\_archive\2025\flip>cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\flip.exe`
Flipped a coin and got tails
```

If you want an optimized build, consider `cargo run --release` instead.
Files will be generated under `target\release`.
