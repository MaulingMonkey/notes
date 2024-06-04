# Sunday April 7th, 2024

Dimensions (W x H x D):
-   Top Counter:    76 x 36 x 12 in.
-   Bottom:         60 x 29 x 20 in. (might require more counterspace)
-   Shelves:        45 x __ x 18 | 24 (up to ≈50")
-   New Shelves:    70 x __ x 24      (up to ≈80")

# Saturday April 13th, 2024

Oooh, pattern types!
*   <https://github.com/rust-lang/rust/pull/109993><br>
*   <https://doc.rust-lang.org/nightly/core/macro.pattern_type.html>
*   <https://doc.rust-lang.org/nightly/std/pat/macro.pattern_type.html>

```rust
// https://discord.com/channels/273534239310479360/592856094527848449/1228593227712430100
#![feature(pattern_types)]
#![feature(core_pattern_type)]
type Positive = std::pat::pattern_type!(i32 is 1..);
```

```rust
// https://discord.com/channels/273534239310479360/592856094527848449/1228593574086443068
#![allow(internal_features)]
#![feature(pattern_types)]
#![feature(core_pattern_type)]
type Positive = std::pat::pattern_type!(i32 is 123..=456789);
#[no_mangle] pub unsafe fn demo(x: i32) -> Positive { std::mem::transmute(x) }
```

# Sunday April 21st, 2024

Diagonal for 16:9:
-   18.35755975068582
-   W x 1.147347484417864
-   H x 2.039728861187313

Desired specs:
*   75"
*   120hz Refresh Rate (**not** Motion Rate or any other such nonsense)
*   Fire?
*   FreeSync?
