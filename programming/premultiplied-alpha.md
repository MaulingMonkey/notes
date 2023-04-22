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
