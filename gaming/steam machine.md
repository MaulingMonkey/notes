# Steam Machine
Personal notes on game compatability with the [Steam Machine](https://store.steampowered.com/hardware/steammachine).
See [../../programming/platforms/steam machine.md](../programming/platforms/steam%20machine.md) for the game *developer* (and more general desktop usage?) experience.

## Audio
-   Bluetooth latency was pretty horrible at one point (400ms?)
-   Power cycling bluetooth headphones fixed it, so it's a variable latency issue.
-   Might not be Steam OS's fault.  My `Bose QC35 II`s don't always behave right on Windows either.

## Bindings of Note
-   <kbd>(Steam)</kbd> + <kbd>(X)</kbd> ➜ On screen keyboard
-   <kbd>Shift</kbd> + <kbd>Tab</kbd> (keyboard) ≈ <kbd>(Steam)</kbd> button (controller)

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
-   **Caveat:** Awkward key binding defaults outside of big picture mode.
    -   In big picture mode (boot default), they work fine &mdash; <kbd>Alt</kbd> + <kbd>F4</kbd> and <kbd>Ctrl</kbd> + <kbd>F4</kbd> activate the second and third module rows.
    -   In desktop mode, *they close the game.*  Stick to the top row, I guess?
    -   One can add `Ignore global shortcuts` `Force     v` `(●) Yes` to the `Window Rules` for `EvE Online`, resulting in:
        <table>
            <tr><th>Binding                             <th>Global Shortcut         <th>With EvE Online Focused
            <tr><td><kbd>Alt</kbd> + <kbd>Tab</kbd>     <td>Switch Windows          <td><b>None</b> (still disabled!)
            <tr><td><kbd>Alt</kbd> + <kbd>F4</kbd>      <td>Close Window            <td>Use Module (Row 2, Column 4)
            <tr><td><kbd>Ctrl</kbd> + <kbd>F4</kbd>     <td>Switch to Desktop 1     <td>Use Module (Row 3, Column 4)
            <tr><td><kbd>Shift</kbd> + <kbd>Tab</kbd>   <td>Steam Overlay           <td><b>Steam Overlay</b> (disable separately!)
        </table>
        Annoyingly, one cannot currently enable <kbd>Alt</kbd> + <kbd>Tab</kbd> while disabling the other commands.
        As an alternative, one *can* unbind <kbd>Ctrl</kbd> + <kbd>F#</kbd> from global shortcuts, although that seems ill-advised to do for <kbd>Alt</kbd> + <kbd>F4</kbd>.

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
