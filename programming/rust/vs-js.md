### Type Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`number`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`f64`](https://doc.rust-lang.org/std/primitive.f64.html)
| [`boolean`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)  | [`bool`](https://doc.rust-lang.org/std/primitive.bool.html)
| [`string`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`js_sys::JsString`](https://docs.rs/js-sys/latest/js_sys/struct.JsString.html)
| `object`                                                                                                                  | [`js_sys::Object`](https://docs.rs/js-sys/latest/js_sys/struct.Object.html)
| [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) / [`unknown`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type) | [`wasm_bindgen::JsValue`](https://docs.rs/wasm-bindgen/0.2.79/wasm_bindgen/struct.JsValue.html)
| `unknown[]`                                                                                                               | [`js_sys::Array`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html)

### Syntax Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`array.forEach(...)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) | [`array.for_each(&mut \|...\| ...)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html#method.for_each) |
