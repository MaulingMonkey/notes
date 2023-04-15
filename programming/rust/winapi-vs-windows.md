| feature       | [winapi]                                                           | [windows]<br>[windows-sys] |
| ------------- | ------------------------------------------------------------------ | ------- |
| safety        | ⚠️ everything is `unsafe`, which is at least sound<br>[winapi-rs#945](https://github.com/retep998/winapi-rs/issues/945) (`CONTEXT` is underaligned) | ❌ history of (now fixed) soundness issues like:<br>[windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) (unsound delegates)<br>[win32metadata#917](https://github.com/microsoft/win32metadata/issues/917) (very wrong fn signatures)<br>[win32metadata#1044](https://github.com/microsoft/win32metadata/issues/1044) (`CONTEXT` underaligned)
| api stability | ✔️ fairly stable 0.3.x                                            | ❌ 0.39 and continuing to skyrocket
| codegen       | ⚠️ manual                                                         | ⚠️ from `*.winmd` metadata |
| coverage      | ⚠️ decent coverage of Win32, but missing some stuff               | ✔️ everything under the sun, including new UWP APIs
| docs          | ⚠️ [docs.rs](https://docs.rs/winapi/) at least has all symbols    | ⚠️ [docs.rs](https://docs.rs/windows-sys/) has all symbols for windows<strong>-sys</strong><br>⚠️ [github pages](https://microsoft.github.io/windows-docs-rs/doc/windows/) I can't remember the URL of, for windows
| maintainence  | ❌ abandoned, although the author is alive and on discord         | ✔️ responsive last I checked |
| api design    | ⚠️ 1:1 match of headers and their mishandled constness            | ⚠️ [windows] drowns in traits and newtypes that add little<br>⚠️ [windows-sys] gives up and uses `*mut c_void` for COM

[winapi]:      https://docs.rs/winapi/
[windows]:     https://microsoft.github.io/windows-docs-rs/doc/windows/
[windows-sys]: https://docs.rs/windows-sys/

### Safety

The underlying windows APIs being called into are rife with subtle and undocumented edge cases, overflows, unchecked parameters, etc. that will lead to soundness bugs in practice if naively wrapped.  Neither [winapi] nor [windows] have much of an advantage on this front.  However, where [winapi] at least makes everything `unsafe` - and thus at least sound - [windows] attempts to mark things safe/sound.  One could argue in favor of this as an attempt to reduce code reviewer fatigue.  I, however, argue against this, as it's resulted in a history of systemic soundness issues such as:

* [windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) (unsound delegates)
* [win32metadata#917](https://github.com/microsoft/win32metadata/issues/917) (very wrong fn signatures)

Writing safe wrappers around windows APIs demands particularized attention that I'd argue a metadata-driven crate such as [windows] cannot provide.

### API Stability

This mostly matters if you're releasing libraries to crates.io that might interoperate with windows FFI crate types.
For example, [`mcom::Rc`](https://docs.rs/mcom/latest/mcom/struct.Rc.html) has a generic contraint on [`winapi::Interface`](https://docs.rs/winapi/0.3/winapi/trait.Interface.html), more or less limiting support to one version of winapi at a time.
Attempting to support a constraint on [`windows::core::Interface`](https://microsoft.github.io/windows-docs-rs/doc/windows/core/trait.Interface.html) would force `mcom` to semver churn at the same rate as [windows].  Fortunately, windows brings its own smart pointer types.  *Un*fortunately, AsRef/From/Into interop with said smart pointer types would quickly result in having support for many versions of [windows] behind many feature gates.

[windows-sys] is at least no longer bumping versions unless actual metadata updates change types/fns, but that still means multiple major version bumps a year vs [winapi] remaining on 0.3.x since before [windows]'s first release.

For application development, this is almost entirely moot.  Your `Cargo.lock` file means you'll only upgrade when you want to, and I believe much of the breakage is fairly minor in practice.

### COM

`winapi` COM interface types are ≈1:1 matches to their C++ equivalents, complete with manual lifetime management, unless you use a wrapper such as <code>[mcom](https://docs.rs/mcom/)::[Rc](https://docs.rs/mcom/latest/mcom/struct.Rc.html)</code> or <code>[wio](https://docs.rs/wio/latest/x86_64-pc-windows-msvc/wio/index.html)::com::[ComPtr](https://docs.rs/wio/latest/x86_64-pc-windows-msvc/wio/com/struct.ComPtr.html)</code>.  `windows` COM interface types are implicitly smart pointers and wrap their C++ equivalents in an extra level of indirection:

<table>
    <tr>
        <th>C++</th>
        <th><code>winapi</code></th>
        <th><code>windows</code></th>
    </tr>
    <tr>
        <td><code><a href="[ComPtr](https://learn.microsoft.com/en-us/cpp/cppcx/wrl/comptr-class)">ComPtr</a>&lt;<a href="https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nn-unknwn-iunknown">IUnknown</a>&gt;</code></td>
        <td><code>mcom::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html">Rc</a>&lt;winapi::um::unknwnbase::<a href="https://docs.rs/winapi/latest/winapi/um/unknwnbase/struct.IUnknown.html">IUnknown</a>&gt;</code></td>
        <td><code>windows::core::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/core/struct.IUnknown.html">IUnknown</a></code></td>
    </tr>
    <tr>
        <td><code>IUnknown::<a href="https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nf-unknwn-iunknown-addref">AddRef</a></code></td>
        <td><code>&lt;mcom::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html">Rc</a>&lt;...&gt; as <a href="https://doc.rust-lang.org/std/clone/trait.Clone.html">Clone</a>&gt;::clone</code></td>
        <td><code>&lt;IWhatever as <a href="https://doc.rust-lang.org/std/clone/trait.Clone.html">Clone</a>&gt;::clone</code></td>
    </tr>
    <tr>
        <td><code>IUnknown::<a href="https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nf-unknwn-iunknown-release">Release</a></code></td>
        <td><code>&lt;mcom::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html">Rc</a>&lt;...&gt; as <a href="https://doc.rust-lang.org/std/ops/trait.Drop.html">Drop</a>&gt;::drop</code></td>
        <td><code>&lt;IWhatever as <a href="https://doc.rust-lang.org/std/ops/trait.Drop.html">Drop</a>&gt;::drop</code></td>
    </tr>
    <tr>
        <td><code>IUnknown::<a href="https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q)">QueryInterface</a></code></td>
        <td><code>mcom::Rc&lt;...&gt;::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html#method.try_cast">try_cast</a></code></td>
        <td><code>&lt;... as windows::core::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/Media/Audio/XAudio2/struct.IXAudio2.html#impl-ComInterface-for-IXAudio2">ComInterface</a>&gt;::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/Media/Audio/XAudio2/struct.IXAudio2.html#method.cast">cast</a></td>
    </tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstance">CoCreateInstance</a></code></td>
        <td><code>mcom::Rc&lt;...&gt;::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html#method.co_create">co_create</a></code>*</td>
        <td><code>windows::Win32::System::Com::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/Com/fn.CoCreateInstance.html">CoCreateInstance</a></code></td>
    </tr>
    <tr>
        <td><code><a href="https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstancefromapp">CoCreateInstanceFromApp</a></code></td>
        <td><code>mcom::Rc&lt;...&gt;::<a href="https://docs.rs/mcom/latest/mcom/struct.Rc.html#method.co_create">co_create</a></code>*</td>
        <td><code>windows::Win32::System::Com::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/Com/fn.CoCreateInstanceFromApp.html">CoCreateInstanceFromApp</a></code></td>
    </tr>
    <tr><th colspan="3">COM Init/Teardown</th></tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/objbase/nf-objbase-coinitialize">CoInitialize</a></code></td>
        <td>
            <code>winapi::um::objbase::<a href="https://docs.rs/winapi/latest/winapi/um/objbase/fn.CoInitialize.html">CoInitialize</a></code><br>
            <code>mcom::init::<a href="https://docs.rs/mcom/latest/mcom/init/fn.sta.html">sta</a></code> (more or less)<br>
        </td>
        <td><code>windows::Win32::System::Com::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/Com/fn.CoInitialize.html">CoInitialize</a></code></td>
    </tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex">CoInitializeEx</a></code></td>
        <td>
            <code>winapi::um::combaseapi::<a href="https://docs.rs/winapi/latest/winapi/um/combaseapi/fn.CoInitializeEx.html">CoInitializeEx</a></code><br>
            <code>mcom::init::<a href="https://docs.rs/mcom/latest/mcom/init/fn.mta.html">mta</a></code> (typical MTA init)<br>
            <code>mcom::init::<a href="https://docs.rs/mcom/latest/mcom/init/fn.co_initialize_ex.html">co_initialize_ex</a></code><br>
        </td>
        <td><code>windows::Win32::System::Com::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/Com/fn.CoInitializeEx.html">CoInitializeEx</a></code></td>
    </tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/roapi/nf-roapi-roinitialize">RoInitialize</a></code></td>
        <td>
            <code>winapi::winrt::roapi::<a href="https://docs.rs/winapi/latest/winapi/winrt/roapi/fn.RoInitialize.html">RoInitialize</a></code><br>
            <code>mcom::???</code>: TODO<br>
        </td>
        <td><code>windows::Win32::System::WinRT::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/WinRT/fn.RoInitialize.html">RoInitialize</a></code></td>
    </tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-couninitialize">CoUninitialize</a></code></td>
        <td>
            <code>winapi::um::combaseapi::<a href="https://docs.rs/winapi/latest/winapi/um/combaseapi/fn.CoUninitialize.html">CoUninitialize</a></code><br>
            <code>mcom::init::<a href="https://docs.rs/mcom/latest/mcom/init/fn.uninitialize.html">uninitialize</a></code><br>
        </td>
        <td><code>windows::Win32::System::Com::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/Com/fn.CoUninitialize.html">CoUninitialize</a></code></td>
    </tr>
    <tr>
        <td><code><a href="https://learn.microsoft.com/en-us/windows/win32/api/roapi/nf-roapi-rouninitialize">RoUninitialize</a></code></td>
        <td>
            <code>winapi::winrt::roapi::<a href="https://docs.rs/winapi/latest/winapi/winrt/roapi/fn.RoUninitialize.html">RoUninitialize</a></code><br>
            <code>mcom::???</code>: TODO<br>
        </td>
        <td><code>windows::Win32::System::WinRT::<a href="https://microsoft.github.io/windows-docs-rs/doc/windows/Win32/System/WinRT/fn.RoUninitialize.html">RoUninitialize</a></code></td>
    </tr>
</table>
