# Error Handling



## Data Collection

Upon an error, there are multiple tiers of information collection that might be resorted to with different tradeoffs:

**None**
-   ✔️ Fast!
-   ✔️ `core` friendly!
-   ❌ No information!

**Error Code Only**
-   ✔️ Fast!
-   ✔️ `core` friendly!
-   ❌ Minimal information (no paths, etc.)

**Error Code + Path Information**
-   ⚠️ Decent speed
-   ⚠️ Heap allocations require at least `alloc` unless you want to sacrifice `'static` with borrows of the argument
-   ❌ PII Concern

**Error Code + Path Information + Stack Trace + Env Var Captures + ...**
-   ✔️ Excellent for debugging
-   ❌ Performance
-   ❌ `#![no_std]` unfriendly
-   ❌ PII Concern



## Caller Behavior

Upon encountering an error, they might resort to:
-   **Forward**         &mdash; Forward the error to the caller's caller.
-   **Re-annotate**     &mdash; Per **Forward**, but with additional information (paths or other context.)
-   **Consume**         &mdash; Take action on the error (e.g. retrying a connection.)
-   **Ignore**          &mdash; Squelch the error ala `drop(...)` or `let _ = ...;`
-   **Log**             &mdash; Log the error (to a file, a stream, dev console, [sentry.io](https://sentry.io/), etc.)
-   **Crash**           &mdash; Uncaught `panic!(...)` or other indicator of an uncaught bug.
-   **Die**             &mdash; For command line programs, it may be appropriate to print an error and `exit(nonzero)`.  Might not be a bug.
-   **Breakpoint**      &mdash; Continuable, but also includes modal message boxes that might occur without a debugger attached.  I consider fatal breakpoints a kind of crash.
-   **Customizeable**   &mdash; On at least one project, I enjoyed having `.ini`-configurable debug checks.
    -   Crash/Breakpoint was useful for Dev when working on a module, or QA when verifying all data was collected
    -   Log/Ignore was useful to unblock coworkers while potentially keeping them appraised of errors
    -   Ignore was useful to e.g. disable leaderboards when no leaderboards were configured for a new title
    -   Ignore was often better than crashing for misc. non-loadbearing gameplay code



## Examples (Rust)
**<https://doc.rust-lang.org/std/>:**
```rust
result?;                                                    // forward
result.map_err(|err| format!("{err} opening {path}"))?;     // re-annotate (prefer a real error type like std::io::Error?)
result.unwrap_or(DEFAULT);                                  // consume
let _ = result;                                             // ignore
drop(result);                                               // ignore
result.unwrap_or_else(|err| eprintln!("..."));              // log
panic!("...");                                              // crash
assert!(..., "...");                                        // crash
assert_eq!(a, b, "...");                                    // crash
result.unwrap();                                            // crash
result.expect("...");                                       // crash
result.unwrap_or_else(|err| panic!("..."));                 // crash
result.unwrap_or_else(|err| { eprintln!("..."); exit(1) }); // die
```
**<https://docs.rs/mmrbi/>:**
```rust
result.unwrap_or_else(|err| mmrbi::warning!("..."));        // log
result.unwrap_or_else(|err| mmrbi::fatal!("..."));          // die
```
**<https://docs.rs/bugsalot/>:**
```rust
result.unwrap_or_else(|err| bugsalot::debugln!("..."));     // log
result.unwrap_or_else(|err| bugsalot::bug!("..."));         // breakpoint
bugsalot::unwrap!(result, continue);                        // breakpoint
bugsalot::expect!(result, "...", continue);                 // breakpoint
```

## Examples (C++)
*NDAs intensify*
