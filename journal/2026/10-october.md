# October 6th

## IDE Snapshot/VFS Nonsense
I like to edit after triggering a build.
This annoys the build system, and tends to cause various build corruption due to widespread TOUTOC issues throughout the entire build process.
To combat this, my IDE bucket list includes `Ctrl`+`Shift`+`B` or other build triggers creating an atomic filesystem snapshot and building that.

-   For large projects, FUSE or other virtual filesystem nonsense should be used to make such snapshots lightning fast / instant.
-   For small projects, an initial dumb platform-independent deep copy should work fine (no need for FUSE/ProjFS/???).
-   Good news: All my personal projects are small projects!



# October 7th

-   <https://glinscott.github.io/>
    -   [How Machines Learned Precision](https://glinscott.github.io/how-machines-learned-precision/) \[[hn thread](https://news.ycombinator.com/item?id=49980626)\]
    -   [The Beam Engine](https://glinscott.github.io/beam-engine/)



# October 15th

Notes on automating web downloads from the command line:
-   <https://github.com/MaulingMonkey/notes/blob/315418c4e23013152a0d6e45970907f8cd60acae/journal/2022/09-september.md?plain=1#L4>
-   <https://github.com/MaulingMonkey/jni-bindgen/blob/01a154fde04b49a9aa9ead82e2e207928a5324eb/scripts/install.sh#L13>
-   <https://github.com/MaulingMonkey/rust-opendingux-test/blob/master/scripts/install-gcw0.sh>

Notes on automating comfy:
-   Placeholders like `%Empty Latent Image.width%` work in UI but not in `comfy run --workflow ...` automation.
-   `Comfy Logo` > `File` > `Export (API)` downloads workflows in a simplified JSON that's saner for generation (discards node positioning, localization, etc.)
-   Replacing values in API JSON with placeholders ala `"{{WIDTH}}"` makes for easy programatic search-replace



# October 16th

## Web IDE Nonsense

Discussion in [`#webassembly-and-wasi`](https://discord.com/channels/273534239310479360/1186749695700242523) (Discord: Rust Programming Language Community):

> [**Chad | &'a String \[::<>\] 2:35 PM**](https://discord.com/channels/273534239310479360/1186749695700242523/1560768066445516874)<br>
> Is there a language suitable for compiling from source to wasm with an embedded compiler?
>
> I'm thinking about a wasm plugin system that can accept .wasm files or source code.

> **me \[::<>\] 2:51 PM**<br>
> rust<br>
> [💬 ⁠Rust compiler in the browser](https://discord.com/channels/273534239310479360/1529693678870724628)<br>
> you can run a wasmed python interpreter, then you don't need a compiler<br>
> <https://github.com/brettcannon/cpython-wasi-build><br>

Relevant Links:
-   <https://weblings.forest-anderson.ca/#>
-   <https://github.com/AngelOnFira/weblings>
-   <https://github.com/bjorn3/rust/tree/compile_rustc_for_wasm20>



# October 18th

-   [Why (and how) I’m building an MMO for 16,000 player battles](https://www.youtube.com/watch?v=_q6SmNYa06U) (youtube.com)

## SELinux Nonsense

I don't want my service running as root, I don't want port forwarding nonsense in some global `/etc` config to bind port 80.
SELinux's `cap_net_bind_service` comes to the rescue!

```sh
$ sudo pacman -S extra/openbsd-netcat
$ cp /usr/bin/nc ./nc
```

Starts out with no permissions, right?
```sh
$ getcap ./nc
$ ./nc -l localhost 80
nc: Permission denied
```


If I add the cap from within a distrobox subcontainer, it shows up in getcap outside of the subcontainer but doesn't work.  Which is a nasty little footgun.
```sh
$ distrobox enter dev -- sudo setcap "cap_net_bind_service=+ep" ./nc
$ getcap ./nc
./nc cap_net_bind_service=ep
$ ./nc -l localhost 80
nc: Permission denied
```


If I do it outside of distrobox it works, although the getcap output doesn't change one iota.
```sh
$ sudo setcap "cap_net_bind_service=+ep" ./nc
$ getcap ./nc
./nc cap_net_bind_service=ep
$ ./nc -l localhost 80
# actually listens now!
```

The internet implies getfattr should be showing me stuff but it doesn't by default.
```sh
$ distrobox enter dev -- getfattr ./nc
$ getfattr ./nc
```

Important flags: `-e hex` (default base64 is silly), `-d` (dump values), `-m -` (filter out nothing)
```sh
$ getfattr -e hex -d -m - ./nc
# file: nc
security.capability=0x0100000200040000000000000000000000000000
```

`attr` can show stuff too:
```sh
$ attr -l ./nc
Attribute "capability" has a 20 byte value for ./nc
```

Apparently I need to use the `-S`ecurity flag to actually read it though.  Without the flag:
```sh
$ attr -g capability ./nc
attr_get: No data available
Could not get "capability" for ./nc
```

With `cap_net_bind_service=+ep` set outside of distrobox:
```sh
$ attr -Sq -g capability ./nc | xxd
00000000: 0100 0002 0004 0000 0000 0000 0000 0000  ................
00000010: 0000 0000                                ....
```

Also shown by strace:
```sh
$ strace getcap ./nc1 2>&1 | grep getxattr
getxattr("./nc1", "security.capability", "\1\0\0\2\0\4\0\0\0\0\0\0\0\0\0\0\0\0\0", 24) = 20
```

With `cap_net_bind_service=+ep` set inside the `dev` distrobox:
```sh
$ attr -Sq -g capability ./nc | xxd
00000000: 0100 0003 0004 0000 0000 0000 0000 0000  ................
00000010: 0000 0000 a086 0100                      ........
```
(For the distrobox version, byte at index 3 is different (`02` -> `03`) and 4 more bytes are appended.)

## Socket activated services

[`inetd`](https://en.wikipedia.org/wiki/Inetd) sound close to what I'm mulling over creating.  Caveats:
-   Process per connection?
-   Effectively replaced by systemd services and [systemd.socket](https://www.freedesktop.org/software/systemd/man/latest/systemd.socket.html) units.
-   I want to shut things off afterwards too?
-   See [Understanding Systemd Socket Units By Creating Your Own QOTD Service](https://www.youtube.com/watch?v=lkeyWJiKv4A) (youtube.com)

## Misc. Nonsense

-   [`brwap`](https://man.archlinux.org/man/extra/bubblewrap/bwrap.1.en) is [Bubblewrap](https://wiki.archlinux.org/title/Bubblewrap)
-   [Net bind capability with systemd](https://serverfault.com/questions/916807/net-bind-capability-with-systemd) (serverfault.com)
-   [Extended Attributes](https://wiki.archlinux.org/title/Extended_attributes) (Arch - covers e.g. `user.xdg.origin.url`!)
-   [systemd/User](https://wiki.archlinux.org/title/Systemd/User) (wiki.archlinux.org)



# October 24th

## Generators and Neutrals
-   [Unbonded my generator and started a fire!](https://www.youtube.com/watch?v=UVjIoL7A_-E)
-   [Bonded vs Floating neutral. portable generator as power back up](https://www.youtube.com/watch?v=rhOmLIdoKNw)



# October 25th

## Beyond 24h
[a conversation in `#code` on the *Rust GameDev* Discord](https://discord.com/channels/676678179678715904/677286494033018924/1564002925750132989):

> **kpreid — 12:49 PM**<br>
> I've heard that in some places it's common to use large numbers usually thought of as out-of-range to represent late night opening hours<br>
> maybe in 24h? not sure<br>
> but either way, like "18:00-26:00" or "6 PM-14 PM"<br>
> **Lucien — 12:55 PM**<br>
> that’s really interesting, I’ve only seen the normal 24 hour clock here, like 18h00-03h00<br>
> I always call these the de facto day versus the de jure day<br>
> **kpreid — 1:02 PM**<br>
> <https://en.wikipedia.org/wiki/24-hour_clock#Times_after_24:00><br>
> <https://en.wikipedia.org/wiki/Date_and_time_notation_in_Japan#Time><br>

I really like this.  I'm considering making a clock where the 12 hour shifts are labeled: `A.M.` `P.M.` **`Z.M.`**

## Triangle >> Quads Propaganda
-   [Optimizing Triangles for a Full-screen Pass - Two tri or not two tri?](https://wallisc.github.io/rendering/2021/04/18/Fullscreen-Pass.html)
-   [GCN Execution Patterns in Full Screen Passes](https://michaldrobot.com/2014/04/01/gcn-execution-patterns-in-full-screen-passes/)



# October 26th

## Cargo Feature Documentation
Noticed [`serde_json`'s lib.rs documents features](https://lib.rs/crates/serde_json/features).
A large part of the prose appears auto-generated, but the `<blockquote>`s are parsed from [Cargo.toml comments](https://github.com/serde-rs/json/blob/8c535a260ce561eaab4111aafc29038eb9f77ff3/Cargo.toml#L57-L59).



# October 27th
-   [`blaze`](https://github.com/indexable-inc/blaze) is some vibe coded fast rust compiler
-   I've been exposed to the classic meme, [stop doing x](https://knowyourmeme.com/memes/stop-doing-math)



# October 28th

## Factorio modding
-   [`'custom-input'`](https://lua-api.factorio.com/latest/prototypes/CustomInputPrototype.html)
-   <https://github.com/JGDoerrer/multiple-characters/blob/master/data.lua>
-   <https://mods.factorio.com/mod/multiple-characters>
-   <https://lua-api.factorio.com/latest/classes/LuaPlayer.html#character>
-   <https://lua-api.factorio.com/latest/defines.html#defines.input_action>
-   <https://lua-api.factorio.com/latest/classes/LuaPlayer.html#set_shortcut_toggled>
-   <https://github.com/ickputzdirwech/Shortcuts-ick/blob/master/control.lua>
-   <https://github.com/BlackCapCoder/factorio-keyboard-mod/blob/master/utils/eventWrapper.lua>
-   <https://wiki.factorio.com/Controls>
-   [Add Lua API feature to find held inputs/keys or listen for on_input_released events](https://forums.factorio.com/viewtopic.php?t=103051) (forums.factorio.com)
-   [Allow linking custom-input to built-in hotkeys.](https://forums.factorio.com/viewtopic.php?t=53591) (forums.factorio.com)

Still all keypress based, no joy for keydown/keyup



