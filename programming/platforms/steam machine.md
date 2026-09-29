# Steam Machine

## Audio, Bindings, Games, Steam Controller
-   See [../../gaming/steam machine.md](../../gaming/steam%20machine.md)

## Network Shares
-   SteamOS uses systemd unit files.  Fortunately, cavemen like me who don't know systemd can still use their old tools.
-   Overlay FS and preinstalled `cifs-utils` means I can mount shares via `/etc/fstab` in [the usual manner](../../software/mount%20(linux).md)
-   If for some reason you want to (ab)use [kio-fuse] after you've e.g. already configured Dolphin to cache your credentials\:
    ```text
    $ dbus-send --session --print-reply --type=method_call                      \
        --dest=org.kde.KIOFuse /org/kde/KIOFuse org.kde.KIOFuse.VFS.mountUrl    \
        string:smb://user@nas1.local/all
    method return [...]
        string "/run/user/1000/kio-fuse-dxhmTP/smb/user@nas1.local/all"
    ```

## Developer

### Option 1: `distrobox`
This seems to be the option valve themselves recommend.  <code>[distrobox]</code> comes pre-installed:
```text
(deck@steamdeck ~)$ which distrobox
/usr/bin/distrobox
```
Here are the commands I start with as a rust-lang developer:
-   <code>passwd</code> &mdash; Sets the password for the default `deck` user.

-   <code>[distrobox] create dev --image archlinux</code><br>
    Create a VM named `dev`.  SteamOS is based off of Arch, so I choose it to minimize distro churn.
    <code>--image ghcr.io/linuxserver/steamos:latest</code> is an alternative mentioned on the internet,
    but ghcr.io seems like a random third party - at the very least, I haven't vetted them myself.

-   <code>[distrobox] enter dev</code> &mdash; Switch to running commands inside the VM.

-   <code>sudo [pacman] -Syu</code> &mdash; Update existing packages and whatnot? ...skippable?

-   <code>sudo [pacman] -S base base-devel code git keepass rustup</code> &mdash; Install various packages:
    -   `base` - ...already installed?
    -   `base-devel` - misc. packages including gcc, linkers
    -   `code` - My preferred editor, [Visual Studio Code].  Slightly awkward (no VSC icon, must be launched from VM) but UI shows up in SteamOS just fine otherwise, and has minimal friction when working within said VM.
    -   `git` - My preferred version control system (also used by ≈everyone else.)
    -   `keepass` - My preferred password manager
    -   `rustup` - My preferred tool for installing/managing rust-lang installations.

-   <code>[rustup] toolchain install stable</code> &mdash; Installs `rustc`, `cargo`, etc.

-   <code>[distrobox-export] --app code</code> &mdash; Export a shortcut to the host.  Searching for "Code" in the launcher will show "Code - OSS (on dev)" which can be launched, pinned to the taskbar, etc.

-   <code>exit</code> then <code>[distrobox] enter dev</code> again &mdash; so your shell has `~/.cargo/bin` in it's `${PATH}`?

-   <code>[cargo] new hello-world</code> &mdash; Create a test project

-   <code>[code] hello-world</code> &mdash; Open said test project in Visual Studio Code

-   <code>[cargo] run</code> (in Visual Studio Code) &mdash; Test build tools on test project

-   Profit?

### Option 2: Make root filesystem read+write

This has multiple drawbacks:
-   SteamOS updates will wipe installed packages and other changes [unless otherwise configured](https://steamcommunity.com/app/1675200/discussions/1/4633734546101122629/).
-   Defeats the whole immutable base concept.
-   Discouraged by Valve themselves.

```text
(deck@steamdeck ~)$ sudo steamos-devmode enable
[sudo] password for deck:

SteamOS Developer Mode

Important: This will allow potentially breaking changes to the root filesystem.
  This is meant for developers and technical users who know what they are doing.
  Changes to the root filesystem will be overwritten by the next SteamOS update.

Developers:
  - Consider packaging your application with flatpak, rather than
    invoking/requiring this script.  This is a much better (and safer) experience
    for users
  - Consider building your package in the Holo container images with
    distrobox/toolbox

? Are you sure you wish to enable developer mode? [y/N]
```

(This is also where I get the sense that valve recommends distrobox instead.)

### Option 3: Make a chroot environment by hand
Possibly useful back when <code>[distrobox]</code> wasn't preinstalled on steam decks?
-   By hand with <code>[pacman]</code> only:    <https://gist.github.com/b-n/dd0595f2370706e7e1866fdd8d0c7d80>
-   By using <code>[pacstrap]</code>:           <https://bbs.archlinux.org/viewtopic.php?pid=2076047#p2076047>

### Visual Studio Code Flatpak
I avoid this.
-   Installing [Visual Studio Code] through "Discover" (SteamOS Desktop's package management UI) will install this flatpak.
-   It includes some dev tools by default (`gcc`, `python`, etc.)
-   Flatpaks run in their own VM, orthogonal to [distrobox], adding friction if you want to run build commands in the context of the host, or in the context of some [distrobox] VM.
-   It does **not** include <code>[pacman]</code>.  Possibly because the flatpak is based off a non-Arch distro?

## Rough Edges &amp; Annoyances
-   The Steam Machine boots to Big Screen mode.  Fine for gaming, annoying for dev.
    -   Configurable, so this might be fine?
        -   <code>steamos-session-select <span style="opacity: 25%">\[gamescope \| plasma \| plasma-wayland \| plasma-wayland-persistent \| plasma-x11-persistent\]</span></code>
        -   <code>steamosctl set-default-login-mode <span style="opacity: 25%">\[desktop | game\]</span></code>
    -   OTOH I was reading posts where people couldn't switch to game mode if they configured things to boot to desktop mode?
-   Window Tiling
    -   By holding `Shift` when moving a window, I can snap it to preconfigured tile positions.
    -   `Meta`+`T` allows configuring said tile positions.  Annoyingly, these are separately configured per virtual desktop.
    -   I should probably just use [MouseTiler](https://www.youtube.com/watch?v=mhZDxNQiFSQ) ([github](https://github.com/rxappdev/MouseTiler)?), but I'm lazy about auditing third party code.
-   On Windows I'd drag images from Browser -> Explorer.  On Linux, similar doesn't seem to work.  I also bricked a Chrome window trying to save an image.
-   On Windows I'd pin recent folders in e.g. VS Code to the taskbar.  On Linux, I can't seem to pin, and shortcuts into distrobox vscode don't even show recent files.
-   I keep forgetting to check if the preinstalled readonly portion of steamos has enough packages for my needs.  So far I've assumed:
    -   I would need to install my own package manager or vm without root.  (But <code>[distrobox]</code> was preinstalled!)
    -   I would need to resort to shenannigans to mount my NAS.  (But `cifs-utils` was preinstalled and `/etc/fstab` is editable via overlay!)
-   "readonly root" feels slightly silly when I can edit all of `/etc` which seems just as potentially ruinous.



<!-- References -->
[code]:                 https://code.visualstudio.com/docs/configure/command-line
[cargo]:                https://doc.rust-lang.org/cargo/
[distrobox]:            https://wiki.archlinux.org/title/Distrobox
[distrobox-export]:     https://distrobox.it/usage/distrobox-export/
[kio-fuse]:             https://github.com/KDE/kio-fuse/blob/master/README.md
[pacman]:               https://wiki.archlinux.org/title/Pacman
[pacstrap]:             https://wiki.archlinux.org/title/Pacstrap
[rustc]:                https://doc.rust-lang.org/rustc/
[rustup]:               https://rust-lang.github.io/rustup/
[Visual Studio Code]:   https://code.visualstudio.com/
