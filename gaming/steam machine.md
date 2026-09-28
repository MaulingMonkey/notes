# Steam Machine
Personal notes on game compatability with the [Steam Machine](https://store.steampowered.com/hardware/steammachine).
See [../../programming/platforms/steam machine.md](../programming/platforms/steam%20machine.md) for the game *developer* (and more general desktop usage?) experience.

## Audio
-   Bluetooth latency was pretty horrible at one point (400ms?)
-   Power cycling bluetooth headphones fixed it, so it's a variable latency issue.
-   Might not be Steam OS's fault.  My `Bose QC35 II`s don't always behave right on Windows either.

## Bindings of Note
-   `(Steam)`+`(X)`: On screen keyboard
-   `Shift`+`Tab` (keyboard) ≈ `(Steam)` button (controller)

## Steam Controller
-   Decent as a gamepad.
-   Charging puck works well enough and is neato.
-   I hate the touchpads and pinky buttons.  Good experiment though!

# Games

## 💚 EvE Online
-   Didn't test using the steam controller at all.
-   Performance: Good (1080p @ 60hz.  Can also do 4k, but that dips under 60fps in some cloudy environments.  Didn't test large fleet battles or multi-account.)
-   **Caveat:** ⚠️ A bit crashy!
    -   Might be EvE Online's fault, I haven't used the windows version in a bit.
    -   Selecting Direct3D11 in the launcher seems more stable than Direct3D12, but still not perfect.
-   **Caveat:** ⚠️ Unusable key bindings outside of big picture mode.
    -   In big picture mode (boot default), they work fine &mdash; `Alt`+`F4` and `Ctrl`+`F4` activate the second and third module rows.
    -   In desktop mode, *they close the game.*  Stick to the top row, I guess?

## 💚 Factorio
-   Tested my existing game-beating save.
-   Performance: Good (4k @ 60hz, including my space age victory savegame on Gleba where my NUC struggled to perform.)
-   **Caveat:** ⚠️ Steam Controller is unusable!
    -   Perhaps I need to reset a setting somewhere? I do presumably have old profile data. Couldn't move character, buttons seemed to map to keyboard inputs?
    -   Keyboard + Mouse works fine.  Just use that.

## 💚 Satisfactory
-   Beat the game using the Steam Controller exclusively.  Works *mostly* OK.  Generic Xbox controls, nothing Steam Controller specific.
-   **Performance:** Good (1920x1080 @ 60hz, even with a game-beating factory.)
-   **Caveat:** ⚠️ Unreliable On Screen Keyboard (OSK.)
    -   Name fields don't stay reliably focused for station and vehicle names.  I sometimes have to reopen the OSK a couple of times.
    -   Name field for map markers can't be saved at all from what I can tell.
