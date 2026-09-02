# August 15th
-   <https://distrosea.com/>

## `windows` crate refactoring

References:
-   [`#windows-dev` (Rust Programming Language Community) discord discussion](https://discord.com/channels/273534239310479360/583054410670669833/1538261514212081676)
-   <https://github.com/microsoft/windows-rs/tree/master/docs/crates>
-   <https://github.com/microsoft/windows-rs/blob/master/docs/crates/windows-clang.md>

`*.h`
→ [`windows-clang`](https://github.com/microsoft/windows-rs/blob/master/docs/crates/windows-clang.md) → `*.rdl`
→ [`windows-rdl`](https://github.com/microsoft/windows-rs/blob/master/docs/crates/windows-rdl.md) → `*.winmd`
→ [`windows-bindgen`](https://github.com/microsoft/windows-rs/blob/master/docs/crates/windows-bindgen.md) → `*.rs`



# August 17th
-   [Implementing the Ultimate Gravity Algorithm in C++](https://www.youtube.com/watch?v=uOahsDhVZaE)
    -   <https://github.com/keyframe41/Fast-Multipole-Method>
    -   Fast Multipole Method (FMM)
    -   ~~Barnes-Hut~~
-   [The Fastest Gravity Algorithm You've Never Heard Of: Fast Multipole Method](https://www.youtube.com/watch?v=FhMftauQZqU)



# August 26th

## Nix on Steam OS

Reading this I was inspired to take another look:

> [**Redhawk** in the Rust Programming Language Community Discord](https://discord.com/channels/273534239310479360/1213974027371286618/1542371550714396703):
> <br> Nix works perfectly on steam os btw, the only problem is the insane amount of storage nix takes
> <br> 64g ssd is never enough, my / is almost full with 150g

-   <https://determinate.systems/blog/nix-on-the-steam-deck/>
-   <https://sadatdaniel.dev/2023/11/install-nix-package-manager-on-your-steam-deck/>
-   > This decision was further motivated by the fact that SteamOS now has a /nix partition.
    > <br> <https://steamcommunity.com/app/1675200/discussions/0/7529517132617926695/?l=hungarian>
-   <https://wiki.archlinux.org/title/Nix>



# August 28th

## Xbox One Chatpads
Keyboard works on windows only if using an xbox one gamepad dongle
Gamepad dongle doesn't appear compatible with SteamOS
Could I write my own Linux driver?

| Vendor        | Product   | Desc.                                                             |
| --------------| ----------| ------------------------------------------------------------------|
| [045E]        |           | **Microsoft**                                                     |
| [045E]        | 02F3      | Xbox One Chatpad                                                  |
| [045E]        | 02E6      | Xbox Wireless Adapter for Windows                                 |
| **[045E]**    | **02FE**  | **Xbox Wireless Adapter for Windows** &mdash; I have this one     |

On the steam machine, `lsusb` shows the adapter, but `mt76x2u` shows 0 uses, and it fails to power the status LED.

### References
-   <https://the-sz.com/products/usbid/index.php?v=0x045E>
-   <http://www.linux-usb.org/usb.ids>

### Code
-   [AcollaMolla/USB-driver](https://github.com/AcollaMolla/USB-driver) &mdash; sample linux driver
-   [torvalds/linux](https://github.com/torvalds/linux/tree/master) &mdash; github mirror
    -   Documentation/
        -   [input/devices/xpad.rst](https://github.com/torvalds/linux/blob/cf72cbb39da84b6f02f90c07f33b102fc10b16f0/Documentation/input/devices/xpad.rst#L170)
    -   drivers/
        -   [input/keyboard/](https://github.com/torvalds/linux/tree/548e7bcd0c5460ddcbca9600cea603ebeebf4da7/drivers/input/keyboard)
        -   [net/wireless/mediatek/mt76/mt76x2/usb.c](https://github.com/torvalds/linux/blob/cf72cbb39da84b6f02f90c07f33b102fc10b16f0/drivers/net/wireless/mediatek/mt76/mt76x2/usb.c#L26-L27) &mdash; Xbox One Wireless Adapters
        -   [usb/](https://github.com/torvalds/linux/tree/master/drivers/usb)
    -   include/
        -   [linux/usb.h](https://github.com/torvalds/linux/blob/master/include/linux/usb.h)
    -   samples/
        -   [uhid/uhid-example.c](https://github.com/torvalds/linux/blob/548e7bcd0c5460ddcbca9600cea603ebeebf4da7/samples/uhid/uhid-example.c) &mdash; Userspace emulation of a mouse + keyboard.
    -   Code Search
        -   [045E](https://github.com/search?q=repo%3Atorvalds%2Flinux+045E&type=code)
        -   [keyboard](https://github.com/search?q=repo%3Atorvalds%2Flinux+keyboard&type=code)

### Commands
```sh
lsusb
lsmod
usb-devices
usb_modeswitch
modprobe
modinfo
dmesg
bluetoothctl devices
hciconfig
hcitool
```

### Third Party
-   <https://github.com/atar-axis/xpadneo>
-   <https://github.com/Kytech/xbox360wirelesschatpad>

<!-- References -->
[045E]:     https://github.com/search?q=repo%3Atorvalds%2Flinux+045E&type=code
