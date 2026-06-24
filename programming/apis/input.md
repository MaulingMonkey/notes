# Cross-Platform Controller APIs

## SDL
-   Native cross platform
-   Steam patches it up for bugfixes, steam controller support, etc?
-   Probably the most popular and widely used PAL
-   What I tell myself I "should" use (even though I don't)
-   <https://www.libsdl.org/>
-   <https://wiki.libsdl.org/SDL2/Installation#steamos>

## SMFL
-   Native cross platform
-   Does anyone actually use this?
-   <https://en.wikipedia.org/wiki/Simple_and_Fast_Multimedia_Library>
-   <https://www.sfml-dev.org/>

## Browser Gamepad API
-   Browser nonsense
-   <https://developer.mozilla.org/en-US/docs/Web/API/Gamepad_API>

## mmk.gamepad
-   Browser nonsense
-   Unmaintained
-   <https://github.com/MaulingMonkey/mmk.gamepad>



# Microsoft (Windows) Controller APIs

## DirectInput
-   Old/deprecated
-   Supports joysticks which need manual calibration (software deadzones and centering)
-   Xbox 360 controllers don't seperately list left and right triggers with default drivers (how silly)
-   <https://en.wikipedia.org/wiki/DirectInput>

## XInput
-   Better Xbox 360 Controller support
-   Doesn't really handle old joysticks
-   No trigger rumble for Xbox One controllers

## RawInput
-   Ignores windows-configured centering/deadzones
-   Provides device handles for mouse/keyboard if you want to do crazy shit like manually interpret keyboard input for multiple seperately configured locales, potentially useful for e.g. xbox 360 chatpads, which are basically hardcoded to `es-MX` locale.
-   <https://learn.microsoft.com/en-us/windows/win32/inputdev/raw-input>

## Windows.Gaming.Input
-   UWP C++/CX COM nonsense
-   Trigger rumble
-   Might also support non-xinput controllers?
-   <https://learn.microsoft.com/en-us/uwp/api/windows.gaming.input>

## GameInput
-   <https://learn.microsoft.com/en-us/xbox/gdk/docs/features/common/input/overviews/input-overview>
