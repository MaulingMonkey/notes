# September 2nd
-   [Wasmi 2.0 - Engineering of the Fastest Wasm Interpreters](https://wasmi-labs.github.io/blog/posts/wasmi-v2.0/) \[[news.ycombinator.com](https://news.ycombinator.com/item?id=49521031)\]
-   [Returned From University And Found My Parents Sold My House, My Belongings Left Outside In Boxes...](https://www.youtube.com/watch?v=To2nARl1JoE)



# September 11th

## Network Cluster w/ `iroh`: Pairing via USB Stick

Existing Cluster:
-   Listen to an [`EndpointId`]
-   Generate a `Temporary Pairing Secret Token`
-   Store both on USB
-   (Later: Discard `Temporary Pairing Secret Token` after pairing session ends)

Joining Member(s):
-   Read *Existing Cluster*'s connection details off of USB
-   Auto-connect
-   Optionally, send a request to disable pairing from said *Joining Member*

## Network Cluster w/ `iroh`: Pairing via QR Codes

Existing Cluster:
-   Listen to an [`EndpointId`]
-   Generate a `Temporary Pairing Secret Token`
-   Expose both via QR Code
-   (Later: Discard `Temporary Pairing Secret Token` after pairing session ends)

Joining Member(s):
-   Generate a `Temporary Pairing Secret Token`
-   Listen to an [`EndpointId`], requiring said `Temporary Pairing Secret Token`
-   Expose via QR Code
-   Accept connections providing `Temporary Pairing Secret Token`
-   Recieve *Existing Cluster*'s Information
-   Connect to *Existing Cluster*

Third Party Phone (Android App or PWA - the latter could be hyperlinked by *Existing Cluter*'s QA code to avoid an app install?):
-   Form Factors:
    1.  Dedicated Android/iOS Apps.
    2.  PWA that directly attempts to acquire QR codes.  Requires advanced browser features, not universal.
    3.  PWA that simply accepts urls in the form of `https://example.com/pairing/{existing,joining}#endpoint=...&secret=...`
        -   Use `#anchors` instead of `?queries` to avoid sending information to the server
        -   Use `localStorage` in case each phone QR Code opens in a separate tab (and thus have separate `sessionStorage`s.)
-   Initialize by scanning *Existing Cluster*'s QR code and collecting said information ([`EndpointId`] + `Temporary Pairing Secret Token`.)
-   For each *Joining Member*:
    -   Scan *Joining Member*'s QR Code
    -   Connect to *Joining Member*, using it's `Temporary Pairing Secret Token` to authenticate phone's use.
    -   Send *Existing Cluster*'s Information
-   On close:
    -   Request any/all devices stop pairing

Console QR Code Rendering
-   2 bits per cell: <code style="white-space: pre; border: 1px solid black; border-radius: 0; padding: 0; color: black; background: white;"> ▄▀█</code> (Unicode: [U+0020, U+2584, U+2580, U+2588], [CP437](https://en.wikipedia.org/wiki/Code_page_437): [0x20, 0xDC, 0xDF, 0xDB])
-   80x25 Console → 80x50 Pixels → Version 8 (49x49) + 31 Columns for Text/Prompts
-   > [...] 4 × version number + 17 dots on each side \[...\] <br>
    > <https://en.wikipedia.org/wiki/QR_code#Information_capacity>
-   > | ECC<br>Lvl  | Data<br>Bits  | Numeric<br>Chars  | Alphanumeric<br>Chars | Binary<br>Chars   | Kanji<br>Chars    |
    > |:-----------:| -------------:| -----------------:| ---------------------:| -----------------:| -----------------:|
    > |      L      |         1,552 |               461 |                   279 |               192 |               118 |
    > |      M      |         1,232 |               365 |                   221 |               152 |                93 |
    > |      Q      |           880 |               259 |                   157 |               108 |                66 |
    > |      H      |           688 |               202 |                   122 |                84 |                52 |
    >
    > <https://www.qrcode.com/en/about/version.html>
-   Prior Art:
    -   [`qr2term`](https://lib.rs/crates/qr2term) (uses [`qrcode`](https://lib.rs/crates/qrcode))
    -   [`qrcode`](https://lib.rs/crates/qrcode) (≈1.6M downloads/month) → `image` (optional)
    -   [`qrcodegen`](https://lib.rs/crates/qrcodegen) (≈0.7M downloads/month) → no dependencies
    -   [`qrcode-generator`](https://lib.rs/crates/qrcode-generator) (≈0.3M downloads/month) → `no_std`, deps (optional)

Links:
-   <https://docs.rs/iroh/>
-   [Develop web apps in Web View](https://developer.android.com/develop/ui/views/layout/webapps/webview)
-   [Barcode Detection API](https://developer.mozilla.org/en-US/docs/Web/API/Barcode_Detection_API) (Browser)
-   [Answer to: Android - QR generator API](https://stackoverflow.com/a/64504871) (Stack Overflow)
-   <https://github.com/googlesamples/android-vision/tree/master/visionSamples/barcode-reader> (Archived)
-   [iroh 0.33.0 - Browsers and Discovery and 0-RTT, oh my!](https://www.iroh.computer/blog/iroh-0-33-0-browsers-and-discovery-and-0-RTT-oh-my)
-   <https://docs.iroh.computer/languages/wasm-browser>
-   <https://www.reddit.com/r/computers/comments/1asy597/qr_codes_not_working_upon_changing_color/> - white-on-black QR codes work, but not everywhere

Browser Support Notes:
-   Disable `iroh`'s default features
-   All connections will flow via relay server

<!-- References -->
[`EndpointId`]:         https://docs.rs/iroh/latest/iroh/type.EndpointId.html



# September 12th

## Google File System
-   [The Most Copied Design in Distributed Storage: Google File System](https://www.youtube.com/watch?v=C3-FIM2xTIw) (pouria on YouTube)
-   The Google File System paper: <https://research.google/pubs/the-google-file-system/>
-   HDFS architecture: <https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html>



# September 13th

## Sparse Files
-   Requires filesystem support (e.g. NTFS)
-   <https://learn.microsoft.com/en-us/windows/win32/fileio/sparse-file-operations>
    -   [FSCTL_SET_ZERO_DATA](https://learn.microsoft.com/en-us/windows/win32/api/winioctl/ni-winioctl-fsctl_set_zero_data) (IOCTL)
    -   [FSCTL_QUERY_ALLOCATED_RANGES](https://learn.microsoft.com/en-us/windows/win32/api/winioctl/ni-winioctl-fsctl_query_allocated_ranges) (IOCTL)

## Pipes
-   <code>[firehazard](https://docs.rs/firehazard/0.0.0-2022-09-10/firehazard/index.html)::[io](https://docs.rs/firehazard/0.0.0-2022-09-10/firehazard/io/index.html)::[~~create_named_pipe_w~~](https://docs.rs/firehazard/0.0.0-2022-09-10/firehazard/io/fn.create_named_pipe_w.html)(...)</code>
-   [Named Pipe Security and Access Rights](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipe-security-and-access-rights) (microsoft.com)

## Sub-Kernel/OS Design
-   Be inspiried by [`distrobox`](https://wiki.archlinux.org/title/Distrobox)'s CLI?
    -   <code>command create</code> &mdash; create a VM with a UUID
    -   <code>command start *name*</code> &mdash; start VM without entering it
    -   <code>command browser *name*</code> &mdash; open a browser tab based shell
    -   <code>command desktop *name*</code> &mdash; `CreateDesktop` and run a native shell
    -   <code>command window *name*</code> &mdash; Run a native shell on the existing desktop
-   [About VHD](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/dd323654(v=vs.85))
    -   [Specification](https://go.microsoft.com/fwlink/p/?linkid=137171) (download link for \*.doc file)

## Reverse HTTP Proxy
-   Fork [`mmuhttpd`](https://github.com/MaulingMonkey/mmuhttpd)?
-   Only a single server at a time can reasonably bind to 127.0.0.1:80
-   Linux restricts non-root binding to port 80.
    -   <https://stackoverflow.com/questions/413807/is-there-a-way-for-non-root-processes-to-bind-to-privileged-ports-on-linux>
    -   Consider `setcap 'cap_net_bind_service=+ep' path/to/program`
        -   <https://man.archlinux.org/man/setcap.8.en>
        -   <https://man.archlinux.org/man/cap_text_formats.7.en>
        -   <https://man.archlinux.org/man/capabilities.7.en#Thread_capability_sets>
        -   `+e` &mdash; Effective      (current state)
        -   `+i` &mdash; Inheritable    (preserved across child processes?)
        -   `+p` &mdash; Permitted      (can ever be effective?)
    -   `authbind` might be more fine grained?
-   [Common Gateway Interface](https://en.wikipedia.org/wiki/Common_Gateway_Interface)
-   [FastCGI](https://en.wikipedia.org/wiki/FastCGI)
-   "No-config" layout: `(root)\`
    -   `localhost.com\`
        -   `static\*.{html,css,png,wasm,...}`
        -   `dynamic\*.wasm`
        -   `tmp\{hex encoded wasi paths}-hex[.bin?]` - temporary variable data?
        -   `var\{hex encoded wasi paths}-hex[.bin?]` - persistent variable data?
    -   `other.localhost.com\`
        -   ....

Path sanitization idea:
-   *maybe* allow basic alphanumeric paths without encoding?
-   anything even remotely sketchy looking gets translated to hexidecimal
-   prefer suffixes to avoid common prefix comparisons
-   use a suffix to avoid hitting reserved windows identifiers such as `COM1` etc.
-   folders too?

## QR Code Login
-   Scan QR code ala `https://maulingmonkey.com/iroh-nonsense/request-login#endpoint=...`
-   Page connects using `iroh` to endpoint & sends endpoint-specific secret from `localStorage`
