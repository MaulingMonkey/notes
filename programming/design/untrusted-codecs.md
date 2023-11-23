I conceptually like the idea of running image, audio, or video codecs in an untrusted sandbox.
*   WASM?
*   Stateless (ensures consistent decoding)
*   ~0 imports (dev-only debug logging/annotation might be OK)

This might include both encoding and decoding.

Minimum viable decoder interface:

```rust
#[no_mangle] pub extern "C" decoder0_decode_image_to_rgba8(image_data: *const u8, image_data_size: usize, on_decode: fn(usize, usize, *const u32), on_error: fn(...)) {
    // ...
}
```

Minimum viable encoder interface:

```rust
#[no_mangle] pub extern "C" encoder0_encode_rgba8_to_image(width: u32, height: u32, pixels: *const u32, on_encode: fn(*const u8, usize), on_error: fn(...)) {
    // ...
}
```

Alternatively, prefer a static fn to a lambda callback for easier raw-FFI?

```rust
#[no_mangle] pub extern "C" decoder0_decode_image_rgba8(image_data: *const u8, image_data_size: usize, flags: u32, hint_width: usize, hint_height: usize) {
    extern "C" {
        fn on_scanline(data: *const u8, y: usize, w: usize);
        fn on_frame(data: *const u8, w: usize, h: usize, stride: usize);
        //fn on_metadata(field: cstr, value: cstr); // author, description, copyright, ...
    }
    // ...
}
```

Metadata about what file extension(s) to encode/decode would also be appropriate.

## Threat Model Categories

*   Malicious codec
    *   The codec author could be malicious
    *   The codec author's machines could be compromised
    *   The build machine or distribution servers could be compromised
    *   The codec could've been downloaded from a malicious third party
*   Malicious image
    *   Could contain malformed images intentionally designed to break a decoder
    *   Could contian images that are illegal, immoral, or traumatizing to posess (probably out of scope for an image codec, but could be in scope for a censoring middleware?)

## Threat: Sandbox escape

A malicious codec could attempt to escape it's sandbox.

Mitigations:
*   Interpret bytecode (WASM?) in 100% safe Rust instead of JITing (misexecution tends to be higher risk) or running code natively (need to defend against native clocks, undocumented instructions)
*   Use OS security primitives

## Threat: Exfiltrate sensitive data

A buggy or malicious codec could attempt to exfiltrate private data (e.g. trade secrets) to a third party.

Mitigations:
*   Make the codec "stateless" (e.g. throw away all state after decoding an image?)
*   The codec shouldn't be able to access anything but provided image data
*   The codec shouldn't be able to perform timing side channel attacks (ban clock/timing info access)
*   The codec shouldn't be able to access fingerprinting-friendly computer metadata without opt-in (author, username, hostname, timezone, ...)
*   ...

## Threat: Denial of Service (Memory)

A buggy or malicious codec could allocate excessive amounts of memory for no reason.

Possible mitigations:
*   Cap memory use based on image size?
*   WASM32 already implies 4 GiB memory cap
*   Cap parallelism of image decoding?

## Threat: Denial of Service (Compute)

A buggy or malicious codec could spin it's wheels decoding image data in infinite loops etc.

Possible mitigations:
*   Cap execution cycles or time

## Threat: Ransomware

A malicious codec could encrypt on encoding, or refuse to decode.

Mitigations:
*   Make codec execution stateless (e.g. can't timebomb execution without clock/cycle/state access)
*   Encoding: Verify the encoded image can be decoded (in a separate sandbox without shared state) matching the original image bit-for-bit.
*   Provide versioning/rollback capabilities (maybe run multiple versions of the codec to verify?)

## Threat: Redirection of Service (Compute)

A malicious codec could include a cryptominer.

Mitigations:
*   Don't provide any run-in-the-background features
*   Decoders: Don't provide any means of exfiltrating the results of the computation
*   Encoders: ???
