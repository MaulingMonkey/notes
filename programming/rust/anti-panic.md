# Panic Avoidance Strategies

Why avoid panics?
-   Panics don't play well with exception-unaware code
-   Panics might not be catchable
-   Uncaught panics crash
-   Crashes are probably Denial-of-Service (DoS) CVEs

Of course, this isn't 100% generic advice.  Panics are fine for:
-   `const { ... }` blocks where they'll be caught at compile time instead of after deployment
-   Unit tests expecting them
-   Preventing worse consequences (unconstrained undefined behavior, etc.)
-   Quick and dirty CLI failures (although I tend to prefer more polished macros that explicitly `eprintln!(...)` and `exit(nonzero)`, allowing proper error reporting.)
-   Errors in dev-time stuff that can be caught by CI / test coverage

# Shenannigans
-   <https://github.com/dtolnay/no-panic>

# Clippy Configuration

Reference:
-   [No Boilerplate](https://www.youtube.com/@NoBoilerplate)'s [My 2026 Rust Toolkit](https://www.youtube.com/watch?v=jlWnoRsGJNQ) (youtube.com)
-   <https://www.namtao.com/rust/>

```toml
# Cargo.toml
[lints.clippy]
pedantic = { level = "deny", priority = -1 }    # UM, ACTUALLY
nursery = { level = "deny", priority = -1 }     # BETA LINTS
# DENY PANICS
unwrap_used = "deny"
expect_used = "deny"
indexing_slicing = "deny"
arithmetic_side_effects = "deny"
unreachable = "deny"
unimplemented = "deny"
unchecked_time_subtraction = "deny"
todo = "deny"
string_slice = "deny"
panic_in_result_fn = "deny"
panic = "deny"
exit = "deny"
as_conversions = "deny"
```
```toml
# clippy.toml
allow-unwrap-in-tests = true
allow-expect-in-tests = true
allow-panic-in-tests = true
allow-indexing-slicing-in-tests = true
```
```sh
cargo clippy -- -D clippy::pedantic -D clippy::nursery
```
