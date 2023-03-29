# Syntactic Sugar

### `for` loops

```rust
for x in y { ... }

// ≈ syntactic sugar for:
{
    let mut iter = core::iter::IntoIterator::into_iter(y);
    while let Some(x) = core::iter::Iterator::next(&mut iter) { ... }
}

// or, without spamming fully qualified names for everything:
{
    let mut iter = y.into_iter();
    while let Some(x) = iter.next() { ... }
}
```

References:
*   <https://doc.rust-lang.org/core/iter/trait.Iterator.html>
*   <https://doc.rust-lang.org/core/iter/trait.IntoIterator.html>

### `?` statements

```rust
let x = y?;

// ≈ syntactic sugar for (not 100% verified here - also uses nightly traits!):
let x = {
    match core::ops::Try::branch(y) {
        core::ops::ControlFlow::Continue(c) => c,
        core::ops::ControlFlow::Break(b) => return core::convert::From::from(b),
    }
};
```

References:
*   <https://doc.rust-lang.org/core/ops/trait.Try.html>
*   <https://doc.rust-lang.org/core/ops/enum.ControlFlow.html>
*   <https://doc.rust-lang.org/core/convert/trait.From.html>
