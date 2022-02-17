### Type Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) / [`unknown`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type) | [`wasm_bindgen::JsValue`](https://docs.rs/wasm-bindgen/0.2.79/wasm_bindgen/struct.JsValue.html)
| [`number`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`f64`](https://doc.rust-lang.org/std/primitive.f64.html)
| [`boolean`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)  | [`bool`](https://doc.rust-lang.org/std/primitive.bool.html)
| [`string`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)   | [`js_sys::JsString`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.JsString.html)
| `"string"`                                                                                                                | [`js_sys::JsString::from("string")`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.JsString.html#impl-From%3C%26%27a%20str%3E)
| `object`                                                                                                                  | [`js_sys::Object`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html)
| `{}`                                                                                                                      | [`js_sys::Object::new()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.new)
| `T?` | [`Option<T>`](https://doc.rust-lang.org/std/option/enum.Option.html)
| `undefined` | N/A / [`Option::None`](https://doc.rust-lang.org/std/option/enum.Option.html#variant.None)
| `null` | N/A / [`Option::None`](https://doc.rust-lang.org/std/option/enum.Option.html#variant.None)
| `unknown[]`                                                                                                               | [`js_sys::Array`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html) (JS Owned) <br> `Vec<`[`wasm_bindgen::JsValue`](https://rustwasm.github.io/wasm-bindgen/api/wasm_bindgen/struct.JsValue.html)`>` (Rust Owned)
| [`Float32Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float32Array)           | [`js_sys::Float32Array`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Float32Array.html) (JS Owned) <br> `Vec<f32>` / `Box<[f32]>` (Rust Owned) <br> [`js_sys::Float32Array::view(..)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Float32Array.html#method.view) (Rust Owned, Lent to JS)

### Syntax Equivalents

| JavaScript or TypeScript | Rust |
| ------------------------ | ---- |
| [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window/window)                                             | [`web_sys::window()`](https://rustwasm.github.io/wasm-bindgen/api/web_sys/fn.window.html) `?`
| [`array.forEach(...)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) | [`array.for_each(&mut \|...\| ...)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Array.html#method.for_each)
| `let v = any.prop;` | `let v =` [`js_sys::Reflect::get`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.get.html) `(&any, &js_sys::String::from("string"))` `?;`
| `any.prop = v;`     | [`js_sys::Reflect::set`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.set.html) `(&any, &js_sys::String::from("string"), &v)` `?;`
| `key in target`     | `js_sys::String::from("key").` [`js_in`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.JsString.html#method.js_in) `(&target)`
| `key in target`     | [`js_sys::Reflect::has`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.has.html) `(&target, &js_sys::String::from("key"))` `?`
| `delete target.key` | [`js_sys::Reflect::delete_property`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/Reflect/fn.delete_property.html) `(&target, &js_sys::String::from("key"))` `?`

### Conversions

| Conversion       | Rust |
| ---------------- | ---- |
| `any -> bool?`   | [`any.as_bool()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.as_bool)
| `any -> number?` | [`any.as_f64()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.as_f64)
| `any -> string?` | [`any.dyn_ref::<js_sys::JsString>()`](https://rustwasm.github.io/wasm-bindgen/api/wasm_bindgen/trait.JsCast.html#method.dyn_ref) (JS) <br> [`js_sys::JsString::try_from(&any)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.JsString.html#method.try_from) (JS) <br> [`any.as_string()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.as_string) (Rust)
| `any === undefined` | [`any.is_undefined()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.is_undefined)
| `any === null`      | [`any.is_null()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.is_null)
| `any === true`      | [`any.as_bool()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.as_bool) `==` `Some(true)`
| `any == true`       | [`any.is_truthy()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.is_truthy)
| `any === false`     | [`any.as_bool()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.as_bool) `==` `Some(false)`
| `any == false`      | [`any.is_falsy()`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.is_falsy)
| `a == b`            | [`a.loose_eq(&b)`](https://rustwasm.github.io/wasm-bindgen/api/js_sys/struct.Object.html#method.loose_eq)
| `a === b`           | `a == b` ?
