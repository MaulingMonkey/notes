# Monday January 4th, 2021

## Vaguely related crates

* https://github.com/matklad/once_cell
* https://github.com/tamschi/cervine
* https://lib.rs/crates/maybe-owned
* https://lib.rs/crates/thread-scoped-ref
* https://lib.rs/crates/borrow_with_ref_obj




## Self-borrowing crates

### owning_ref
* https://lib.rs/crates/owning_ref
* https://github.com/Kimundi/owning-ref-rs

Cons:
* Abandoned
* Unsound as fuck
    * https://github.com/Kimundi/owning-ref-rs/issues/49
    * https://github.com/Kimundi/owning-ref-rs/issues/61

### rental
* https://lib.rs/crates/rental
* https://github.com/jpernst/rental

Cons:
* Macro heavy
* Probably also unsound similar to owning-ref? (stable_deref_trait)
* Abandoned (archived github)

### ouroboros
* https://lib.rs/crates/ouroboros

Cons:
* Yank heavy
* Macro heavy
* Probably also unsound similar to owning-ref? (stable_deref_trait)

### once_self_cell
* https://lib.rs/crates/once_self_cell
* https://users.rust-lang.org/t/experimental-safe-to-use-proc-macro-free-self-referential-structs-in-stable-rust/52775

Cons:
* Probably also unsound similar to owning-ref? (no stable_deref_trait, but looks like it can borrow String s / might assume stable derefs)

### schroedinger

* https://github.com/dureuill/sc
* https://github.com/dureuill/sc/blob/master/explanation.md

Cons:
* Unsound as fuck (relies on drop)

### ref-portals

Cons:
* Weird semantic versioning scheme
* Prefer Arc&lt;RefCell&lt;T&gt;&gt; or similar as less buggy, possibly more sound (ref_portals::sync::Anchor drop merely panics but that's nonfatal?)

### ccl_stable_deref_trait

* https://lib.rs/crates/ccl_stable_deref_trait

Cons:
* Same bugs as stable_Deref_trait?

### reffers

* https://docs.rs/reffers/0.6.0/reffers/aref/struct.ARef.html

Cons:
* ~~Same stable deref bugs, probably.~~
    * https://docs.rs/reffers/0.6.0/src/reffers/aref.rs.html#17
    * https://docs.rs/reffers/0.6.0/src/reffers/aref.rs.html#653
* Actually maybe not

### refstruct

* https://github.com/diwic/refstruct-rs
