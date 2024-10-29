# Rust vs C++: Floating Point



## Types

| Rust                                                                          | IEEE 754-2008     | C / C++                           |
| ------------------------------------------------------------------------------| ------------------| ----------------------------------|
| [`f16`](https://doc.rust-lang.org/core/primitive.f16.html) <sup>\[1\]</sup>   | [`binary16`]      |                                   |
| [`f32`](https://doc.rust-lang.org/core/primitive.f32.html)                    | [`binary32`]      | `float` <sup>\[2\]</sup>          |
| [`f64`](https://doc.rust-lang.org/core/primitive.f64.html)                    | [`binary64`]      | `double` <sup>\[2\]</sup>         |
| [`f128`](https://doc.rust-lang.org/core/primitive.f128.html) <sup>\[1\]</sup> | [`binary128`]     | `__float128` <sup>\[3\]</sup>     |
| N/A                                                                           | *varies wildly*   | `long double` <sup>\[4\]</sup>    |

1.  Currently a nightly-only experimental API.

2.  Assumes the C or C++ standard complies with Annex F, binding their types to IEEE 754-2008.
    Should be a valid assumption for all Rust targets, but it's worth noting there are C and C++ implementations out there that have different implementations for `float` / `double`.

3.  Nonstandard.  Cavet emptor.

4.  `long double` is all over the place: Sometimes `binary64`, sometimes `binary128`, sometimes "double double", sometimes Intel's 80-bit nonsense...



<!-- References -->

[`binary16`]:   https://en.wikipedia.org/wiki/Half-precision_floating-point_format#IEEE_754_half-precision_binary_floating-point_format:_binary16
[`binary32`]:   https://en.wikipedia.org/wiki/Single-precision_floating-point_format#IEEE_754_standard:_binary32
[`binary64`]:   https://en.wikipedia.org/wiki/Double-precision_floating-point_format#IEEE_754_double-precision_binary_floating-point_format:_binary64
[`binary128`]:  https://en.wikipedia.org/wiki/Quadruple-precision_floating-point_format#IEEE_754_quadruple-precision_binary_floating-point_format:_binary128
[`binary256`]:  https://en.wikipedia.org/wiki/Octuple-precision_floating-point_format#IEEE_754_octuple-precision_binary_floating-point_format:_binary256
