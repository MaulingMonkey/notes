## Thursday March 10th, 2022

#### winsqlite3.h debacle

*   [zuurr on discord](https://discord.com/channels/273534239310479360/583054410670669833/951596077268758659) first noticed the problem
*   `sqlite3.dll`       is built with `SQLITE_APICALL` being `cdecl`
*   `winsqlite3.dll`    is built with `SQLITE_APICALL` being `stdcall`
*   winsqlite3.h can link against statically linked sqlite, which might have a version mismatch
    *   <https://rust-lang.github.io/rfcs/2627-raw-dylib-kind.html> is meant to fix this kind of thing, but is not yet available on even nightly
*   `SQLITE_CDECL` APIs mistakenly labeled as `extern "system"` by `windows[-sys]`
*   `SQLITE_CDECL` APIs don't correctly do varargs in `windows[-sys]`
*   `C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\um\winsqlite\winsqlite3.h`
*   `C:\Windows\System32\winsqlite3.dll`
*   <https://github.com/microsoft/windows-rs>



## Tuesday March 29th, 2022

* Fuck taxes.
* Experimented with bending git formats.  Bending fails `git fsck`
    * Incorrect length explodes.
    * Padding whitespace explodes.
    * Padding 0s explodes.
    * No size explodes.
* winsqlite3.h / lib / .dll is a fun rabbit hole
    * [#windows-dev discussion](https://discord.com/channels/273534239310479360/583054410670669833/951596077268758659) (discord)
    * https://github.com/microsoft/win32metadata/issues/824
    * https://github.com/microsoft/windows-rs/issues/1592
    * https://github.com/rusqlite/rusqlite/pull/1109
    * different abis ("system" vs "cdecl") for winsqlite3.dll vs sqlite3.dll ?
