# Loaders

The actual asset to be loaded in a proper gamedev asset loading pipeline can be suprisingly complicated.

### Input: Path/ID

Typically you have some kind of path or ID representing an asset to load that game assets might use to refer to each other.
This can be a path like `some/game/asset.png` or perhaps some other form of string ID.
This often doesn't have a 1:1 correspondance to the actual *built* files on-disk that said resource might actually be loaded from!

### Input: Locale & Region

Locale can modify what actual assets are loaded in several ways.
Obviously, text files full of localized text should load different contents when on `en-US` vs `es-MX`.
Textures with text embedded into them might also vary by locale.
To comply with censorship rules, games in some regions might change some of their effects to avoid blood, drugs, nudity, etc.
Intro sequences might show ESRB ratings logos in one region and CERO ratings logos in another.

### Input: "MIME"

Some loaders might want merely metadata about an image for layout calculation purpouses (width/height/???).
A windows build might want DXT-compressed textures, a console might want PNG-compressed textures if it has dedicated decompression hardware.
Perhaps high-definition HDR textures are only downloaded on consoles with 4k HDR TVs connected.
Android APKs might bundle different shaders for vulkan-enabled phones vs gles-enabled phones.
iOS apps might bundle different textures for retina devices vs non-retina.
An editor might want to load the editable asset on disk, whereas the game might want to load a preprocessed and prebaked version of the same asset.

### Input: Debug

Resources like shaders have dev variants that are easier to debug.
Textures might have id-stamped variants.
Text might have "localization ID replacement" variants.

### Input: Controller?

Might load a different resource for `ui/menu/next.png` based on if the last input was keyboard vs gamepad vs mouse.
On the other hand, trying to handle that at the resource loader level might be... awkward.

### Input: LOD

Textures might be loadable at different LODs.  Might preload a low resolution texture, then stream in a high resolution texture as the graphics pipeline requests higher and higher mip levels.

### Input: Device Handle

Textures and shaders are often specific to a given device handle and must be reloaded on device resets, renderer backend switches (d3d9 -> d3d11), etc.
