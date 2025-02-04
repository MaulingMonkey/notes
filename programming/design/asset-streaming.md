# Asset Streaming

## Rendering Fallback Options

-   Option 1: Don't render at all, fade in when renderable.  I've used this for e.g. minimap icons, avatars, etc.
-   Option 2: Preload a low resolution version, stream in high level versions later.
-   Option 3: Hard stall.  Lamest.  Simplest.

## Textures

-   Option 1: Recreate the texture handle at higher resolution.
    -   Pro: Simplest
    -   Con: Redundant upload work
-   Option 2: Partially initialize the texture handle on a per-mip basis, control used mips with e.g. `{Min,Max}LOD` sampler settings.
    -   Pro: Simpleish
    -   Con: Might not be able to unload afterwards
-   Option 3: [Megatextures](https://www.adriancourreges.com/blog/2016/09/09/doom-2016-graphics-study/)?
    -   Pro: ???
    -   Con: NEAREST-only filtering for mips / no trilinear interpolation?
-   Option 4: [Tiled resources](https://learn.microsoft.com/en-us/windows/win32/direct3d11/tiled-resources) or equivalent.
    -   Pro: [No wasted memory?](https://discord.com/channels/186813135263367169/476778332436824075/1336331941510778891)
    -   Con: ???
