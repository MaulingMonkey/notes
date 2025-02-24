# Sub-OS

The term "SubOS" has been [previously coined](https://en.wikipedia.org/wiki/SubOS) to mean several different things.
I am using the term in the sense of:
-   In the strictest sense, a "Guest" Operating System *designed* to run atop another "Host" Operating System.
    -   Windows 3.1 (and 9x?) ran on MS-DOS
    -   Chrome (if you squint hard enough about the definition of "Operating System" - viable considering the existence of [ChromeOS](https://en.wikipedia.org/wiki/ChromeOS) IMO) runs atop Windows, Linux, OS X, etc.
    -   My fantasy OSes typically runs atop Windows, Linux, OS X, etc.
-   In the loosest sense, any "Guest" Operating System running atop of / subservient to another "Host" Operating System.
    -   This often involves a VM system of some sort
        -   Kubernates
        -   Docker
        -   QEMU
        -   DosBOX
        -   WSL (Windows Subsystem for Linux)
    -   This *may* involve drivers for the Guest OS to run better in those VMs
        -   Drivers for absolute mouse positioning
        -   Drivers for clipboard sharing between host and guest
        -   Drivers for filesystem sharing between host and guest
        -   GPU passthrough
        -   ...
    -   Goals vary, and may include
        -   Accessing OS-specific programs from another OS
        -   Cross-platform testing
        -   Containerization for dependency management
        -   Time splitting / resource separation / resource isolation



## Driver Design

There are several models for driver design:
-   Baked into VM executable (often exposed to Guest OS in the form of a fake hardware device.)
-   DLL per driver (allows control over process model)
-   EXE per driver (allows control over ASLR settings etc.)

These have varying strengths.  Consider:
-   Audio.  Windows gives each process it's own volume slider.  To ascribe that to the appropriate Sub-OS process:
    -   WebAudio should ideally be used from individual browser tabs
    -   XAudio2(?) should ideally be used from a host executable generated for each guest executable
-   Console.  On windows, multiple processes are needed for multiple consoles.
    -   If a single process wants multiple consoles, it needs multiple sub-processes.
    -   If multiple processes want to share a console, they may need IPC.
-   Security and Isolation.
    -   Each driver having it's own process will ensure drivers don't corrupt each other and that crashes are driver-specific, and sandboxing can be maximized.
    -   This is compatible with the DLL per driver model: `svchost.exe` comes to mind?
    -   Mixing multiple drivers into the same process reduces isolation, but might be "preferred" for audio or "trusted" stable drivers?



## Package Design

```text
package/
    apps/{...}
    codecs/
        archive/{tar, zip, ...}.wasm
        audio/{mp3, ogg, wav, webm?, ...}.wasm
        executable/{pe, wasm, ...}.wasm
        hash/{md5, sha1, sha256, ...}.wasm
        image/{bmp, tga, png, jpeg, webm, ...}.wasm
        video/{avi, mp4, ...}.wasm
    drivers/{browser, linux, osx, windows}/{x86, x64, arm, arm64, ...}/gamepad.{dll, exe, js, so, wasm}
```
