# Wednesday May 10th, 2023

Near term project goals:
*   `ialloc`
*   WASM loader
    *   Asyncify
    *   Config object
    *   Binding imports
*   WASI impl
*   Async WebGL texture loader
*   Dual BFS demo
*   CYOA demo (rustless?)

### Evil
```rs
struct Undroppable;
impl Undroppable { const PANIC : () = panic!(); }
impl Drop for Undroppable { fn drop(&mut self) { let _ = Self::PANIC; } }

fn main() { a() }
// This fails to compile, but only if built as an rlib (comment out `main` above on playground):
fn a() {
    let u = Undroppable;
    core::mem::forget(u);
}
```
