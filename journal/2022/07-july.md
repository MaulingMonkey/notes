## Sunday July 3rd, 2022

#### Why is TLS codegen so gross

It doesn't have to be!  TL;DR:
* Use [`-Z tls-model=local-exec`](https://doc.rust-lang.org/beta/unstable-book/compiler-flags/tls-model.html) to eliminate `__tls_get_addr` in favor of `fs:...` addressing.
    * <https://www.akkadia.org/drepper/tls.pdf>
* Use nightly `#[thread_local]` to eliminate a bunch of panicy branchy "if initialized" / "if torn down" boilerplate
* `AtomicUsize` could be `Cell<usize>` for thread locals.

```rust
#![feature(thread_local)]
use std::sync::atomic::*;

static G : AtomicUsize = AtomicUsize::new(0);
thread_local! { static TL : AtomicUsize = AtomicUsize::new(0); }
#[thread_local] static TL2 : AtomicUsize = AtomicUsize::new(0);

#[inline(never)] pub fn a() -> usize {
    G.fetch_add(1, Ordering::Relaxed)
}

#[inline(never)] pub fn b() -> usize {
    TL2.fetch_add(1, Ordering::Relaxed)
}

#[inline(never)] pub fn c() -> usize {
    TL.with(|tl| tl.fetch_add(1, Ordering::Relaxed))
}
```
\[[godbolt](https://rust.godbolt.org/z/f93YrhdvG)\]
