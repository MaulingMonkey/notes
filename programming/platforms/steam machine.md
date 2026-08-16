# Steam Controller
-   Decent as a gamepad
-   Charging puck works well enough and is neato
-   I hate the touchpads and pinky buttons, good experiment though.



# Steam Machine
Acquired a steam machine w/ steam controller.

## Audio
-   Bluetooth latency was pretty horrible at one point (400ms?)
-   Power cycling bluetooth headphones fixed it, so it's a variable latency issue.
-   Might not be Steam OS's fault.  My `Bose QC35 II`s don't always behave right on Windows either.

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

-   <code>sudo [pacman] -S base base-devel code git rustup</code> &mdash; Install various packages:
    -   `base` - ...already installed?
    -   `base-devel` - misc. packages including gcc, linkers
    -   `code` - My preferred editor, [Visual Studio Code].  Slightly awkward (no VSC icon, must be launched from VM) but UI shows up in SteamOS just fine otherwise, and has minimal friction when working within said VM.
    -   `git` - My preferred version control system (also used by ≈everyone else.)
    -   `rustup` - My preferred tool for installing/managing rust-lang installations.

-   <code>rustup toolchain install stable</code> &mdash; Installs `rustc`, `cargo`, etc.

-   <code>exit</code> then <code>[distrobox] enter dev</code> again &mdash; so your shell has `~/.cargo/bin` in it's `${PATH}`?

-   <code>cargo new hello-world</code> &mdash; Create a test project

-   <code>code hello-world</code> &mdash; Open said test project in Visual Studio Code

-   <code>cargo run</code> (in Visual Studio Code) &mdash; Test build tools on test project

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



## Game: Satisfactory
-   Steam Controller works mostly OK.  Generic XB1 style controls, nothing Steam Controller specific.
    **However:** I can't figure out how to get map marker names to stick when using the on-screen keyboard.
-   Performance good (1920x1080 @ 60hz?)

## Game: Factorio
Out of the box:
-   **Steam Controller unusable** (perhaps I need to reset a setting somewhere? I do presumably have old profile data. Couldn't move character, buttons seemed to map to keyboard inputs?)
-   Keyboard + Mouse works fine.  Just use that.
-   Performance good (1920x1080 @ 60hz?  Including my space age victory savegame on Gleba where my NUC struggled to perform.)



<!-- References -->
[distrobox]:            https://wiki.archlinux.org/title/Distrobox
[pacman]:               https://wiki.archlinux.org/title/Pacman
[pacstrap]:             https://wiki.archlinux.org/title/Pacstrap
[Visual Studio Code]:   https://code.visualstudio.com/
