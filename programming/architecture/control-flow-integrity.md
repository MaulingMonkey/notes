# Abstract

Control Flow Integrity attempts to harden programs against being exploited through function pointer corruption by:
-   Validating the destination is the start of a function (Baseline CFI)
-   Validating the destination function signature matches what the caller expected

Since type based checking is generally C/C++ first, this can lead to some awkward mismatches for Rust codebases.
Does `u32` map to `unsigned int` or `unsigned long`?
Trick question:  Depending on compiler flags, perhaps you should've used [`cfi_types::c_ulong`](<https://docs.rs/cfi-types/>).

I'm aware of at least four major control flow integrity ecosystems:



# Hardware: Intel Control-flow Enforcement Technology (CET)

-   Shadow Stacks:  Detects corrupted return addresses.
-   Indirect Branch Tracking:  Requires indirect `jmp` / `call` destinations to be marked with new `endbr{32,64}` instructions.

References:
-   <https://web.archive.org/web/20240406115253/https://www.intel.com/content/dam/develop/external/us/en/documents/catc17-introduction-intel-cet-844137.pdf>
-   <https://web.archive.org/web/20170814120442/https://software.intel.com/sites/default/files/managed/4d/2a/control-flow-enforcement-technology-preview.pdf>



# Software: CFG / XFG (Microsoft)

-   CFG has basic function start checks.
-   XFG extends CFG with type checking.

References:
-   <https://blog.quarkslab.com/how-the-msvc-compiler-generates-xfg-function-prototype-hashes.html>
-   <https://connormcgarr.github.io/examining-xfg/>



# Software: LLVM-CFI

References:
-   <https://clang.llvm.org/docs/ControlFlowIntegrity.html>



# Software: WASM

Built into the spec.  N.B. type checking is limited as e.g. Rust's `i32`, `u32`, `&Whatever`, etc. *all* get encoded identically as a WASM32 `i32` in typical ABIs.

References:
-   [The assertions for executing `call_indirect`](https://webassembly.github.io/spec/core/exec/instructions.html#exec-call-indirect).



# Misc. References

-   <https://en.wikipedia.org/wiki/Control-flow_integrity>
