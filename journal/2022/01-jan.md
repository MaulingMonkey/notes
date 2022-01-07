## Friday January 7th, 2022
*   Merged (private) thin3d9 into public [thindx](https://github.com/MaulingMonkey/thindx), pushed <https://docs.rs/thindx/> w/ screenshots
*   Worked on (private) 12-bit fantasy console ISA/emulator (match statements vs fn ptr arrays is interesting to compare codegen for)
*   [microsoft/windows-rs#1409 • Bug: TypedEventHandler is unsound](https://github.com/microsoft/windows-rs/issues/1409)
*   [jni-bindgen](https://github.com/MaulingMonkey/jni-bindgen) CI cleanup
*   Discovered [Syncthing](https://syncthing.net/) \([HN](https://news.ycombinator.com/item?id=29837696), [Article](https://tonsky.me/blog/syncthing/)\),
    which looks close to the ideal sync framework I'd been considering "inventing"
*   [rust_win32_async_sample](https://github.com/MaulingMonkey/rust_win32_async_sample/blob/master/src/main.rs), discovered why C# has `Control.BeginInvoke`
    \(`PostThreadMessage`  is unreliable thanks to dialog message loops, but `PostMessage` to an HWND can dispatch to `WndProc` - see [#windows-dev Discord Ramblings](https://discord.com/channels/273534239310479360/583054410670669833/928069858682355733)\)

### TODO:
*   Merge xinput bindings into thindx from wherever I hid them
*   Experiment with Syncthing for code backup, media sync?
*   Fuzz coverage for thindx
*   Finish `thindx::d3d9::*`
*   Example coverage for `thindx::d3d9::*`
