# Monday April 24th, 2023

*   [sepia](https://github.com/mdn/dom-examples/blob/d99884173bde8a8d8f09f258cfc8465308447645/canvas/pixel-manipulation/color-manipulation.js#L16-L28)

### Efficient wasm png -> webgl texture

*   <code>core::[include_bytes!](https://doc.rust-lang.org/core/macro.include_bytes.html)("some.png")</code>
*   <code>const view = new [DataView](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView/DataView)(this.[memory](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/Memory).[buffer](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/Memory/buffer), ptr, length)</code>
*   <code>const blob = new [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob/Blob)(\[view\], { type: "image/png" })</code>
*   <code>const imageBitmap = **await** [createImageBitmap](https://developer.mozilla.org/en-US/docs/Web/API/createImageBitmap)(blob, { imageOrientation: "flipY", premultiplyAlpha: "premultiply" })</code>
*   <code>[gl](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext).[texImage2D](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/texImage2D)(..., imageBitmap)</code>

### Debugging browser wasm

*   `wasm32-wasi` dwarf not sufficient?
*   <https://developer.chrome.com/blog/wasm-debugging-2020/>
*   <https://chrome.google.com/webstore/detail/cc%20%20-devtools-support-dwa/pdcpmagijalfljmkmjngeonclgbbannb>

### x86 MSVC alignment is incredibly, *incredibly* cursed:

> 32-bit MSVC has the bizarre behaviour that `alignas(int64_t) int64_t tmp` is not the same as `int64_t tmp;`, and emits extra instructions to align the stack. That's because `alignas(int64_t)` is like `alignas(8)`, which is more aligned than the actual minimum.
> 
> https://stackoverflow.com/a/55930645/953531

[`#windows-dev` discord rabbit hole vis-à-vis IP_ADAPTER_ADDRESSES](https://discord.com/channels/273534239310479360/583054410670669833/1101754048836804698)
