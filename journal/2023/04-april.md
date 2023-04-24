# Monday April 24th, 2023

*   [sepia](https://github.com/mdn/dom-examples/blob/d99884173bde8a8d8f09f258cfc8465308447645/canvas/pixel-manipulation/color-manipulation.js#L16-L28)

### Efficient wasm png -> webgl texture

*   <code>core::[include_bytes!](https://doc.rust-lang.org/core/macro.include_bytes.html)("some.png")</code>
*   <code>const view = new [DataView](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView/DataView)([memory](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/Memory).[buffer](https://developer.mozilla.org/en-US/docs/WebAssembly/JavaScript_interface/Memory/buffer), ptr, length)</code>
*   <code>const blob = new [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob/Blob)(view, { type: "image/png" })</code>
*   <code>const imageBitmap = **await** [createImageBitmap](https://developer.mozilla.org/en-US/docs/Web/API/createImageBitmap)(blob, { imageOrientation: "flipY", premultiplyAlpha: "premultiply" })</code>
*   <code>[gl](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext).[textImage2D](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/texImage2D)(..., imageBitmap)</code>
