# "Misfeatures"

Probably an overly critical title.
More like "a list of things I would prefer to change if creating a new language inspired by Rust".

*   Self borrowing structs are a horribly unsolved problem (does [Ouroboros](https://github.com/someguynamedjosh/ouroboros) still have soundness issues?)
*   Self lending iterators are awkward/impossible based on current stdlib design.
*   References are non-null (nice!), but pointers are nullable, but `NonNull` isn't, but that's ≈`mut` only... should've just made plain pointers non-null IMO.
*   `CStr` is a fat pointer.  That's not C-style!
*   Box's weird custom deref-into-inner behavior is a funky edge case that always trips me up.
*   Rust assumes `'static` is `'eternal`, but it might not be if you unload DLLs.
*   It would be nice to have "by-value borrows" (e.g. borrows that don't preserve the address of `self` but otherwise act as `&self`.)
    *   See my [`valrow`](https://docs.rs/valrow/latest/valrow/) crate.  Sadly, this is awkward enough it's mostly for FFI rather than general optimization, but it *is* neato.
*   Would be nice to have improved panic-free coding tools.
    *   Linux's custom stdlib / cfg experiments involve eliminating sources of panics.
    *   Intrinsics like slice indexing bake panics into core language features however.
    *   See also the fun/hacky [`#[no_panic]`](https://github.com/dtolnay/no-panic).



## Drop

[`Drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html)'s semantics are a little awkward.
It'd be nice to be able to write something like:

```rust
struct Complicated { a: A, b: b }
impl Drop for Complicated {
    fn drop(self) {
        let Complicated { a, b } = self;
        // and then this, or something more complicated:
        drop(a);
        drop(b);
    }
}
```

However, `drop` instead takes `&mut self`.  This has one slight advantage in that this won't infinitely recurse:

```rust
struct Complicated { a: A, b: b }
impl Drop for Complicated {
    fn drop(self) {}
}
```

There's also the whole awkwardness with `#[may_dangle]`.



## DSTs

Dynamically Sized Types are half baked.  Also, footguns.

```rust
use std::alloc::{alloc_zeroed, Layout};

struct Foo {
    header: u64,
    bytes: [u8],
}

fn main() {
    let n = 3;
    let layout = Layout::new::<u64>().extend(
        Layout::array::<u8>(n).unwrap()
    ).unwrap().0;

    let p = unsafe { alloc_zeroed(layout) };
    let foo = std::ptr::slice_from_raw_parts_mut(p, n) as *mut Foo;
    let foo_ref = unsafe { &*foo };

    dbg!(size_of_val(foo_ref));

    dbg!(foo_ref.header);
    dbg!(&foo_ref.bytes);
}
```

```text
error: Undefined Behavior: pointer not dereferenceable: pointer must be dereferenceable for 16 bytes, but got alloc462 which is only 11 bytes from the end of the allocation
  --> src/main.rs:16:28
   |
16 |     let foo_ref = unsafe { &*foo };
   |                            ^^^^^ Undefined Behavior occurred here
```

<https://discord.com/channels/273534239310479360/592856094527848449/1396590760480083988>
