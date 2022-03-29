# Tuesday March 29th, 2022

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
