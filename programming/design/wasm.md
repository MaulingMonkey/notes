# WASM imports registry

### Problem

* Composing multiple sources of JS imports is problematic as heck
* Circular dependency issue: Many WASM imports also rely on `wasm_instance.exports.memory`

### Solution

* `(imports) => (exports) => undefined`

### Goals

* Add diagnostics for conflicting symbols
    * e.g. if two scripts provide `env.glBindProgram`, that's probably an issue
    * displaying provider names/versions would be useful
* Weak symbols for fallback support (e.g. `wasi_snapshot_preview1.args_sizes_get` might have a noop fallback)
* `<script src="..."></script>` tags might be nice for browser composition

### Prototypes / Prior Art

* [cargo-html: `WasmPlugin`](https://github.com/MaulingMonkey/cargo-html/blob/cb1772590db00891df25d8b8296511a9fe430662/script/exec-wasm.ts#L6)
* [quad-snd: `miniquad_add_plugin`](https://github.com/not-fl3/quad-snd/blob/f1b04bb32c56073388d25a14bd762d810046ec39/js/audio.js#L191)
