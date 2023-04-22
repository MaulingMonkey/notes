# A review of alpha blending.

First, let's review a typical non-premultiplied alpha blending equation:

```hlsl
dst.rgb
    = dst.rgb * (1-src.a) // the original color, scaled down by however much the overdrawn image obscures it
    + src.rgb * src.a;    // the overdrawn image, scaled down by however transparent it is

dst.a = dst.a * (1-src.a) + 1 * src.a; // typically ignored for fully opaque window back buffers, since dst.a ≈ 1 always
```

This is fairly straightforward.

# A minor optimization

Astute observers might notice `src.rgb` is always multiplied against `src.a`.
While this is pretty cheap, in the land of per-pixel operations, even "pretty cheap" can be worth optimizing away.
If we modify our src texture once with:

```hlsl
src.rgb = src.rgb * src.a;
```

We can reuse that every single time we render with that texture by simplifying our alpha blending equation:

```diff
-dst.rgb = dst.rgb * (1-src.a) + src.rgb * src.a; // non-premultiplied alpha
+dst.rgb = dst.rgb * (1-src.a) + src.rgb;         // premultiplied alpha (assumes we did `src.rgb *= src.a;`)
```

# Non-premultiplied alpha linear interpolation is messy and wrong

When not using premultiplied alpha, what color is a fully transparent pixel?  Trick question:  **Anything with alpha=0**.  `(1,1,1,0)` ≈ `(0,0,0,0)` ≈ `(?,?,?,0)`.  However, when using typical [texture filtering](https://en.wikipedia.org/wiki/Texture_filtering#Bilinear_filtering) to sample in-between pixels through naieve component-wise linear interpolation, each of these 100% transparent pixels **blends differently**.

Say we move an image half a pixel, and end up blending a non-premultiplied transparent pixel with a color 50% via linear interpolation:

| Color                      | Transparent                      | Result |
| ---------------------------| ---------------------------------| -------|
| `0.0, 0.0, 0.0, 1.0` black | `0.0, 0.0, 0.0, 0.0` transparent | ✔️ `0.0, 0.0, 0.0, 0.5` 50% transparent black
| `0.0, 0.0, 0.0, 1.0` black | `1.0, 0.0, 0.0, 0.0` transparent | ❌ `0.5, 0.0, 0.0, 0.5` 50% transparent **dark red?!?**
| `0.0, 0.0, 0.0, 1.0` black | `1.0, 1.0, 1.0, 0.0` transparent | ❌ `0.5, 0.5, 0.5, 0.5` 50% transparent **grey?!?**
| | |
| `1.0, 1.0, 1.0, 1.0` white | `0.0, 0.0, 0.0, 0.0` transparent | ❌ `0.5, 0.5, 0.5, 0.5` 50% transparent **grey?!?**
| `1.0, 1.0, 1.0, 1.0` white | `1.0, 0.0, 0.0, 0.0` transparent | ❌ `1.0, 0.5, 0.5, 0.5` 50% transparent **light red?!?**
| `1.0, 1.0, 1.0, 1.0` white | `1.0, 1.0, 1.0, 0.0` transparent | ✔️ `1.0, 1.0, 1.0, 0.5` 50% transparent white

This can lead to subtle shimmering "halo" effects.  Note that no transparent color works with all colors.  "Overpainting" color into the fully transparent area can help some, but all pixels have 4-8 neighbors (depending on how you're counting) and it just plain doesn't seem to work all that well.