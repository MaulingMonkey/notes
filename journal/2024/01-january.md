## Monday January 15th, 2024
*   Worked on [`abistr`](https://docs.rs/abistr/)
    *   `Encoding` / 0.3 branch overhaul
*   Worked on [`hwnd`](https://docs.rs/hwnd/)
    *   `Encoding` / 0.3 branch overhaul
    *   ???
*   Considered working on [`thindx`](https://docs.rs/thindx/)
    *   Extract `thindx-xinput`?
    *   Extract `thindx-bridge-types`?
        *   Can I `type _ = mcom::Rc<winapi::shared::d3d9::IDirect3DDevice9>;`?  Probably not thanks to orphan rules.
        *   Take inspiration from ~~`winapi::shared::d3d9::{L,LP}DIRECT3DDEVICE9`~~?  But that's raw pointers.
        *   `IDirect3DDevice9Ptr`
        *   `com_ptr::IDirect3DDevice9`
        *   `ComPtr::IDirect3DDevice9`
        *   `ptr::IXAudio2VoiceCallback` XXX: but this could be specific to an xaudio2 version?
        *   `ref::IXaudio2VoiceCallback`?  (implies non-null / valid)
