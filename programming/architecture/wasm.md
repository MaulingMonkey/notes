# WASM
* Core tech is great.
* Ecosystem is meh.

## WASI
* A posix-ish system interface for wasm modules.
* Usable prototype for stdio, file I/O, time information.
* Poor fit for web due to blocking I/O, but can be done.
* Lacks basic shit like thread creation.
* Very low velocity / near-dead.
* <https://github.com/WebAssembly/WASI>

## WASM VMs for Rust
* Basics relatively straightforward, comparable to low level script binding APIs
    * [wasmtime](https://docs.rs/wasmtime/)
    * [wasmer](https://docs.rs/wasmer/)

## WASM in the Browser
* Ecosystems:
    * Manual FFI ala [miniquad](https://github.com/not-fl3/miniquad): what I'm leaning towards these days
    * [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) / wasm-pack / web_sys / js_sys: maintainence mode / low velocity, but still works
    * [stdweb](https://github.com/koute/stdweb): dead and now broken
    * [emscripten](https://emscripten.org/): when you really want to force a full desktop application into the browser
* Needs JS boilerplate, and none of the tools play nicely with each other.
* WASM modules tend to import JS functions that take pointers, without providing memory access to said wasm modules.
    * Easily worked around with manually written javascript
    * No automatic composability ala JS module imports
* ["Multithreading" with web workers](https://rustwasm.github.io/2018/10/24/multithreading-rust-and-wasm.html) is currently a hack at best, requires https? for cors + shared memory
* Blocking primitives (I/O, mutexes) don't play well with the browser.
    * [Asyncify](https://kripken.github.io/blog/wasm/2019/07/16/asyncify.html) is an interesting hackaround.  Manually applied.

## My own nonsense
* <https://github.com/MaulingMonkey/rust_wasm_sample>
* <https://github.com/MaulingMonkey/cargo-html> (WASI + Asyncify junk)

## Misc.
* <https://github.com/mbasso/awesome-wasm>
* <https://kripken.github.io/blog/wasm/2019/07/16/asyncify.html>
* <https://github.com/rustwasm/walrus>
* Trunk
    * <https://trunkrs.dev/>
    * <https://github.com/trunk-rs/trunk>
* <https://github.com/kettle11/wasm_set_stack_pointer>
* <https://github.com/ktock/container2wasm>

## Debug
* <https://rustwasm.github.io/book/reference/debugging.html#using-a-debugger>
* <https://github.com/yurydelendik/wasm-dwarf>

## Optimization
* <https://rustwasm.github.io/docs/book/reference/code-size.html>

## Reference
* <https://github.com/sunfishcode/wasm-reference-manual/blob/master/WebAssembly.md>
