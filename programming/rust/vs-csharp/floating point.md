# Rust vs C#: Floating Point



## Types

| Rust                                                                          | IEEE 754-2008     | C#                                                                                                                            | .NET  |
| ------------------------------------------------------------------------------| ------------------| ------------------------------------------------------------------------------------------------------------------------------| ------|
| [`f16`](https://doc.rust-lang.org/core/primitive.f16.html) <sup>\[1\]</sup>   | [`binary16`]      | <span style="opacity: 25%">N/A</span>                                                                                         | <code>System.[Half](https://learn.microsoft.com/en-us/dotnet/api/system.half)</code>          |
| [`f32`](https://doc.rust-lang.org/core/primitive.f32.html)                    | [`binary32`]      | [`float`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)      | <code>System.[Single](https://learn.microsoft.com/en-us/dotnet/api/system.single)</code>      |
| [`f64`](https://doc.rust-lang.org/core/primitive.f64.html)                    | [`binary64`]      | [`double`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)     | <code>System.[Double](https://learn.microsoft.com/en-us/dotnet/api/system.double)</code>      |
| [`f128`](https://doc.rust-lang.org/core/primitive.f128.html) <sup>\[1\]</sup> | [`binary128`]     | <span style="opacity: 25%">N/A</span>                                                                                         | <span style="opacity: 25%">N/A</span>
| ≈ <code>[rust_decimal](https://docs.rs/rust_decimal/1.42.1/rust_decimal/)::[Decimal](https://docs.rs/rust_decimal/1.42.1/rust_decimal/struct.Decimal.html)</code> ? <sup>\[2\]</sup> | <span style="opacity: 25%">N/A</span> | [`decimal`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)    | <code>System.[Decimal](https://learn.microsoft.com/en-us/dotnet/api/system.decimal)</code>    |

1.  Currently a nightly-only experimental API.

2.  I have not audited or used this crate, and thus cannot reasonably vouch for it.



<!-- References -->

[`binary16`]:   https://en.wikipedia.org/wiki/Half-precision_floating-point_format#IEEE_754_half-precision_binary_floating-point_format:_binary16
[`binary32`]:   https://en.wikipedia.org/wiki/Single-precision_floating-point_format#IEEE_754_standard:_binary32
[`binary64`]:   https://en.wikipedia.org/wiki/Double-precision_floating-point_format#IEEE_754_double-precision_binary_floating-point_format:_binary64
[`binary128`]:  https://en.wikipedia.org/wiki/Quadruple-precision_floating-point_format#IEEE_754_quadruple-precision_binary_floating-point_format:_binary128
[`binary256`]:  https://en.wikipedia.org/wiki/Octuple-precision_floating-point_format#IEEE_754_octuple-precision_binary_floating-point_format:_binary256
