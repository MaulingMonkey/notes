### Type Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`number`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`f64`](https://doc.rust-lang.org/std/primitive.f64.html)
| [`boolean`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)  | [`bool`](https://doc.rust-lang.org/std/primitive.bool.html)
| [`string`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`js_sys::JsString`](https://docs.rs/js-sys/latest/js_sys/struct.JsString.html)
| `"string"`                                                                                                                | [`js_sys::JsString::from("string")`](https://docs.rs/js-sys/latest/js_sys/struct.JsString.html#impl-From%3C%26%27a%20str%3E)
| `object`                                                                                                                  | [`js_sys::Object`](https://docs.rs/js-sys/latest/js_sys/struct.Object.html)
| `{}`                                                                                                                      | [`js_sys::Object::new()`](https://docs.rs/js-sys/latest/js_sys/struct.Object.html#method.new)
| [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) / [`unknown`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type) | [`wasm_bindgen::JsValue`](https://docs.rs/wasm-bindgen/0.2.79/wasm_bindgen/struct.JsValue.html)
| `unknown[]`                                                                                                               | [`js_sys::Array`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html)

### Syntax Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window/window)                                             | [`web_sys::window()`](https://rustwasm.github.io/wasm-bindgen/api/web_sys/fn.window.html) `?`
| [`array.forEach(...)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) | [`array.for_each(&mut \|...\| ...)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html#method.for_each)
| `let v = any.prop;` | `let v =` [`js_sys::Reflect::get`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.get.html) `(&any, &js_sys::String::from("string"))` `?;`
| `any.prop = v;`     | [`js_sys::Reflect::set`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.set.html) `(&any, &js_sys::String::from("string"), &v)` `?;`
| `key in target`     | [`js_sys::Reflect::has`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.has.html) `(&target, &js_sys::String::from("key"))` `?`
| `delete target.key` | [`js_sys::Reflect::delete_property`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.delete_property.html) `(&target, &js_sys::String::from("key"))` `?`
