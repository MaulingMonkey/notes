## Sunday March 2nd, 2025

*   Discovered [`#[warn(non_exhaustive_omitted_patterns)]`](https://doc.rust-lang.org/rustc/lints/listing/allowed-by-default.html#non-exhaustive-omitted-patterns) for `match ... { ... }`
*   Remembered I want a universal remote / programmable IR blaster / ???
*   Remembered I hate Rust's feature spam.  I want a nice shared precompiled all-features `winapi`.  TODO: neo-cargo.
*   TODO: Convert <https://maulingmonkey.com/> to notes

#### Console Shenannigans

*   Steal and Rustify `ShinyConsole` from [TtyRecMonkey](https://github.com/MaulingMonkey/TtyRecMonkey)?
*   `shinyconsole{-stable}`?
*   I can't seem to find an equivalent to `docs.rs/crate/latest` for pre-release docs

#### C++ vs Rust Topic Ideas

*   Move semantics
*   Captures / anonymous fns
*   IDE setup
*   Hello world
*   argc, argv
*   Windows subsystem
*   Manifest?



## Wednesday March 12th, 2025

*   [20 AAA Beating Space Games In 2025 - The Indie Resurgence](https://www.youtube.com/watch?v=A57R3QwW-7c)
*   [NixOS: Everything Everywhere All At Once](https://www.youtube.com/watch?v=CwfKlX3rA6E)

#### Network Drive Shenannigans

```text
C:\local>subst M: "\\nas1\all"

C:\local>subst
M:\: => UNC\nas1\all

C:\local>subst M: /D

C:\local>subst

C:\local>
```

Consider also registry keys:

```reg
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices]
"W:"="\\??\\C:\\Users\\Public\\Documents"
```



## Tuesday March 18th, 2025

Rafael [at 12:56 PM](https://discord.com/channels/273534239310479360/583054410670669833/1351645412292956260): A quick hack for your `cargo run` scenario is to set env var `__COMPAT_LAYER` to `RunAsAdmin` (e.g., `PS> $env:__COMPAT_LAYER="RunAsAdmin"`)



## Friday March 21st, 2025

`IronRDP`: a rust-lang RDP implementation (client? server?)
*   <https://github.com/Devolutions/IronRDP>
*   <https://news.ycombinator.com/item?id=43436894>



## Monday March 24th, 2025

*   TODO: reorg nas git
*   TODO: script backup/fsck of nas git



## Saturday March 29th, 2025

*   [Changing a Signed Executable without Altering Windows Digital Signatures](https://blog.barthe.ph/2009/02/22/change-signed-executable/)
    *   Use case: Signed exectuable containing e.g. zipped data?



## Monday March 31st, 2025

#### Pipes!

*   [`CallNamedPipeW`](https://learn.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-callnamedpipew) and [`TransactNamedPipe`](https://learn.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-transactnamedpipe) seem similar... do they differ in anything other than arguments?
    *   The former takes a pipe name, the latter a handle.
    *   The former takes a timeout, the latter an `OVERLAPPED`.
    *   The latter notes "The maximum guaranteed size of a named pipe transaction is 64 kilobytes."
*   [`ImpersonateNamedPipeClient`](https://learn.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-impersonatenamedpipeclient) implies all kinds of fun nonsense.
*   <https://learn.microsoft.com/en-us/windows/win32/ipc/pipe-functions>
*   <https://learn.microsoft.com/en-us/windows/win32/ipc/about-pipes>
*   <https://learn.microsoft.com/en-us/windows/win32/ipc/using-pipes>
*   Can I atomically [`WriteFileGather`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-writefilegather) to a message pipe? (`OVERLAPPED` only.)
*   Could flesh out [`firehazard::io`](https://docs.rs/firehazard/latest/firehazard/io/index.html).  `firehazard` uses `0.0.0-yyyy-mm-dd` scheme so no semver to worry about!
*   Consider using e.g. `PIPE_REJECT_REMOTE_CLIENTS`
*   A specific named pipe: `\\\\.\\pipe\\mynamedpipe` ([ref](https://learn.microsoft.com/en-us/windows/win32/ipc/transactions-on-named-pipes))
*   RIIR <https://learn.microsoft.com/en-us/windows/win32/ipc/multithreaded-pipe-server> ?
*   <https://csandker.io/2021/01/10/Offensive-Windows-IPC-1-NamedPipes.html>

#### RISC-V Nonsense

*   <https://five-embeddev.com/riscv-user-isa-manual/Priv-v1.12/instr-table.html>
*   <https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/notebooks/RISCV/RISCV_CARD.pdf>
