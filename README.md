# Aseprite-Defold-Export (atlas fork)

Aseprite → Defold integration. Exports `.aseprite` sprites straight into a Defold
**atlas** (`.atlas`), turning Aseprite tags into Defold flipbook animations.

This fork is **atlas-only** and adds:

- a **file picker** to choose the target atlas (existing or new) — no hand-typed paths;
- **merging** into an existing atlas, so many sprites can share one atlas (fewer draw calls);
- correct, project-relative image paths on **Windows** (forward slashes, project root auto-detected);
- animations named `<sprite>_<tag>` so names never collide in a shared atlas.

## Export

1. In Aseprite: `File → Export to Defold`.
2. **atlas**: browse to pick an existing `.atlas` to merge into, or type a new name
   in any folder **inside your Defold project** (the folder containing `game.project`
   is detected automatically).
3. Frames are written to a subfolder next to the atlas, named after the sprite
   (e.g. `<atlas-folder>/bee/bee_0.png`) and referenced by project path
   (`/assets/.../bee/bee_0.png`).

Re-exporting the same sprite replaces only that sprite's frames and animations;
everything else already in the atlas is preserved. This is what lets you build one
shared atlas for a whole biome / enemy set by exporting each sprite into it.

## Tags become animations

A sprite without tags exports as a single animation named after the sprite. Each tag
becomes an animation named `<sprite>_<tag>`. Play it in Defold with:

```lua
sprite.play_flipbook(url, "bee_walk")
```

## Looping

A tag **loops by default**. To play it once, set the tag's **user data** to `once`
(or set the tag's repeat count to 1 in Aseprite). See [USAGE.md](USAGE.md) for the
full direction + user-data → Defold playback mapping and the FPS conversion.

## Building from source

`aseprite-defold-export.lua` is the source of truth; `ci/release.py` preprocesses it into
`extension.lua` and packs the `.aseprite-extension`. Install the resulting file via
`Aseprite → Edit → Preferences → Extensions → Add Extension`.
