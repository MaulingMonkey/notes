# Friday May 2nd, 2025

## DLL Injection

Every example on the internet seems to use addresses from the *current* (same-arch) process to spawn threads in a *remote* process.
How fucking horrifying.
And yet it works: <https://stackoverflow.com/questions/22750112/dll-injection-with-createremotethread>.

~~TL;DR: everything gets kernel32.dll ([even no-import bins!](https://github.com/MaulingMonkey/firehazard/blob/bfaae74ccfd811c1185049aaeaf7446dd43772fb/examples/max_sandbox/settings.rs#L79-L80)), and ASLR applies at *boot* time.~~

Actually, `trivial.exe` still imports stuff.  Check `trivialest.exe`.



# Saturday May 3rd, 2025

*   [Inside Native Applications](https://learn.microsoft.com/en-us/sysinternals/resources/inside-native-applications) (learn.microsoft.com)
*   [Calling an imported function, the naive way](https://devblogs.microsoft.com/oldnewthing/20060721-06/?p=30433)

## Container Brainstorming

### Existing Containers

*   `abistr::CStrNonNull`
*   `abistr::CStrPtr`
*   `abistr::CStrBuf`
*   `ialloc::ABox`
*   `ialloc::AVec`
*   ~~`firehazard::CBox`~~      &mdash; yeeted in favor of `ialloc::ABox`
*   `firehazard::CBoxSized`     &mdash; to yeet into `ialloc`?
*   `firehazard::CString`       &mdash; to yeet?

### Missing: String Slices/Views

*   Flavors: no terminal `\0`, counted terminal `\0`, uncounted terminal `\0`
*   Layouts:
    *   `(usize, NonNull<?>)`
    *   `(u32,   NonNull<?>)`
    *   `(NonNull<?>, u32  )`
    *   `(NonNull<?>, usize)` &mdash; most likely direct equivalent to C++'s [`std::basic_string_view`](https://en.cppreference.com/w/cpp/header/string_view)... although perhaps that would be nullable?
    *   `(usize, *const ?)` &mdash; null requires 0-length? prefer making the entire thing `Option<(usize, NonNull<?>)>`?
    *   `(u32,   *const ?)` &mdash; null requires 0-length? prefer making the entire thing `Option<(u32,   NonNull<?>)>`?
    *   `(*const ?, u32  )` &mdash; null requires 0-length? prefer making the entire thing `Option<(NonNull<?>, u32  )>`?
    *   `(*const ?, usize)` &mdash; null requires 0-length? prefer making the entire thing `Option<(NonNull<?>, usize)>`?
    *   counted terminal `\0`s could use `NonZeroUsize` and `NonZeroU32` counts
*   Alternative spelling brainstorming:
    *   `(usize,        [u8]                 )`
    *   `(usize, Option<[u8]>, nul::Counted  )` &mdash; `count` should include `\0`
    *   `(usize, Option<[u8]>, nul::Uncounted)` &mdash; `count` should exclude `\0`
    *   `(usize, Option<[u8]>, nul::Missing  )`
    *   `(usize, Option<[u8]>, nul::Unknown  )` &mdash; heckin' evil?
    *   `(       [u8] , usize                )`
    *   `(Option<[u8]>, usize, nul::Counted  )`
    *   `(Option<[u8]>, usize, nul::Uncounted)`
    *   `(Option<[u8]>, usize, nul::Missing  )`
*   Out of generic scope?
    *   [`UNICODE_STRING`](https://learn.microsoft.com/en-us/windows/win32/api/subauth/ns-subauth-unicode_string) &mdash; counting in *bytes*, capacity field, etc.

```rust
mod imp {
    #[repr(C)] struct LengthData<Length, Data> {
        pub length: Length,
        pub data:   Data,
    }

    #[repr(C)] struct DataLength<Data, Length> {
        pub data:   Data,
        pub length: Length,
    }
}

unsafe impl StringViewAbi for (usize, Option<[u8]>, nul::Counted) {
    type Abi = imp::LengthData<usize, *const u8>;
    //type Len = usize;
    //type Ptr = *const u8;
    type Unit = u8;
    const NUL_TERM_STYLE : NulTermStyle = NulTermStyle::Counted;
    //const ALLOW_NULL          : bool = true;
    //const ALLOW_ZERO_LEN      : bool = true;
    //const ALLOW_INTERIOR_NUL  : bool = false;
}

unsafe impl StringViewFromPtrLen<*const u8,   usize       > for (usize, Option<[u8]>, nul::Counted) { fn from_ptr_len(ptr: *const u8, len: usize) -> Self::Abi { ... } }
unsafe impl StringViewFromPtrLen<NonNull<u8>, usize       > for (usize, Option<[u8]>, nul::Counted) { fn from_ptr_len(ptr: *const u8, len: usize) -> Self::Abi { ... } }
unsafe impl StringViewFromPtrLen<*const u8,   NonZeroUsize> for (usize, Option<[u8]>, nul::Counted) { fn from_ptr_len(ptr: *const u8, len: usize) -> Self::Abi { ... } }
unsafe impl StringViewFromPtrLen<NonNull<u8>, NonZeroUsize> for (usize, Option<[u8]>, nul::Counted) { fn from_ptr_len(ptr: *const u8, len: usize) -> Self::Abi { ... } }
```

### Missing: Owned Strings

*   `CStringNonNull`            &mdash; owned equivalent to `CStrNonNull`.
    *   Stateless: sane.
    *   Stateful: Metadata would need to be postfixed after `\0` with reads and writebacks on any (re)alloc?  Gross.

*   `CStringPtr`                &mdash; owned equivalent to `CStrPtr`.  Skip resizability?
    *   Stateless: sane, can even `const fn new() -> Self`.
    *   Stateful: Not impossible for the null case.

*   `CString???`                &mdash; thin, with prefixed metadata containing allocator, size, capacity.  Allocator FFI implications for skipping the header!
*   `CString???`                &mdash; fat?



# Friday May 9th, 2025

*   [Detecting whether a SID is well-known SID](https://devblogs.microsoft.com/oldnewthing/20141212-00/?p=43413)
*   [What’s the point of giving my unnamed object proper security attributes since unnamed objects aren’t accessible outside the process anyway (or are they?)](https://devblogs.microsoft.com/oldnewthing/20150604-00/?p=45451)
*   [An easy way to determine whether you have a particular file permission](https://devblogs.microsoft.com/oldnewthing/20040604-00/?p=39023)
*   [Standard handles are really meant for single-threaded programs](https://devblogs.microsoft.com/oldnewthing/20141008-00/?p=43893)
*   <https://github.com/mity/old-new-win32api#security-permissions-attributes-and-identifiers>



# Sunday May 18th, 2025

## Spaced Repetition

*   [Spaced Repetition Systems Have Gotten Way Better](https://domenic.me/fsrs/) ([news.ycombinator.com discussion](https://news.ycombinator.com/item?id=44020591))
*   Anki
    *   <https://apps.ankiweb.net/> (downloads)
    *   <https://docs.ankiweb.net/> (documentation)
    *   Add ons:
        *   [Heatmap](https://youtu.be/8zaKVFC9Eu4?t=10773)
        *   Frozen Fields
        *   Speed Focus
*   FSRS
    *   <https://github.com/open-spaced-repetition/fsrs4anki/wiki/ABC-of-FSRS>
    *   <https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm>
    *   <https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-mechanism-of-optimization>



# Wednesday May 21st, 2025

*   [Writing into uninitialized buffers in Rust](https://blog.sunfishcode.online/writingintouninitializedbuffersinrust/) \[[hn](https://news.ycombinator.com/item?id=44032680)\]
*   [`rustix::buffer::Buffer`](https://docs.rs/rustix/1.0.7/rustix/buffer/trait.Buffer.html)
*   [`rustix`](https://docs.rs/rustix/1.0.7/rustix/index.html)
*   [`cap-std`](https://crates.io/crates/cap-std)
