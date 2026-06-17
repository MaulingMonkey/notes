## Realistic Constant Limits

| Unit              | Desc |
|-------------------|------|
| 1.6735327e-27  kg | Mass one one hydrogen atom
| 1.6735327e-24  g  |
| 30             nm3| Volume of a PrP prion (smallest self-replicating biological entity)
| 3e-26          m3 |
| 18.6           nm3| Volume of one hydrogen atom
| 1.86e-26       m3 |
| 1.17549e-38       | `std::numeric_limits<float>::min()`
| 1.4013?e-45       | `std::numeric_limits<float>::denorm_min()`
| 2.22507e-308      | `std::numeric_limits<double>::min()`
| 4.94066e-324      | `std::numeric_limits<double>::denorm_min()`

## IEEE 754 Formats

Reference: <https://en.wikipedia.org/wiki/IEEE_754#Basic_and_interchange_formats>

| IEEE 754<br>Name  | Sign<br>Bits  | Exp<br>Bits   | Significand<br>Bits   |
| -----------------:| -------------:| -------------:| ---------------------:|
| binary16          |             1 |             5 |                  10+1 |
| binary32          |             1 |             8 |                  23+1 |
| binary64          |             1 |            11 |                  52+1 |
| binary128         |             1 |            15 |                 112+1 |
| binary256         |             1 |            19 |                 236+1 |

## Precisions of Note

<style>
    table code { white-space: pre; }
</style>

| Minimum Time Step         | Description
| -------------------------:| --------------------------------------------------------------|
|                           | **Time**
| `≈ 0.1978             μs` | [(time since epoch) / pow(2,53)](https://www.wolframalpha.com/input?i=time+since+epoch+%2F+pow%282%2C53%29) (precision with "ideal" units)
| `≈ 0.2384185791015625 μs` | Precision of binary64 unix time measured in seconds      (exact?)
| `  0.244140625        μs` | Precision of binary64 unix time measured in milliseconds (exact?)
| `  0.25               μs` | Precision of binary64 unix time measured in microseconds (exact!)
| `  0.256              μs` | Precision of binary64 unix time measured in nanoseconds  (exact!)
| `≈ 0.3956             μs` | [(time since epoch) / pow(2,52)](https://www.wolframalpha.com/input?i=time+since+epoch+%2F+pow%282%2C52%29) (precision with "anti-ideal" units)

Note: jumping three magnitudes between [SI prefixes](https://en.wikipedia.org/wiki/International_System_of_Units#Prefixes) also scales precision.
Within a band, the factor is 1.024 - upon crossing a band, the factor is 2.048.

Note: the next exponent threshhold is circa 2043, which will cause precision to halve / time steps to double.
