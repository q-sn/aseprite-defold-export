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
3. Frame PNGs are written next to the **.aseprite source file** (e.g.
   `<sprite-folder>/bee_0.png`) and referenced by project path. The atlas itself
   may live anywhere in the same Defold project.

Re-exporting the same sprite replaces only that sprite's frames and animations;
everything else already in the atlas is preserved. This is what lets you build one
shared atlas for a whole biome / enemy set by exporting each sprite into it.

## Tags become animations

A sprite without tags exports as a single animation named after the sprite. Each tag
becomes an animation named `<sprite>_<tag>`. Play it in Defold with:

```lua
sprite.play_flipbook(url, "bee_walk")
```

## Animation speed (FPS)

Speed is taken from your **Aseprite frame durations** — you set it once in Aseprite
and it flows into the atlas. The exported `fps` of a tag is:

```
fps = 1000 / average_frame_duration_ms   (averaged over the tag's frames)
```

So set each frame's duration (ms) on the Aseprite timeline:

- all frames `100 ms` → `fps: 10`; `50 ms` → `20`; `~83 ms` → `12`
- to get exactly **N** fps, set each frame to **`1000 / N` ms**
- keep a tag's frames uniform, otherwise you get the average

There is no separate fps setting in Defold — it comes entirely from Aseprite.

## Playback

The Defold playback of each animation is built from two tag properties in Aseprite:

- **Direction** → forward / backward / ping-pong
  (`Forward`, `Reverse`, `Ping-Pong`; `Ping-Pong Reverse` falls back to Ping-Pong)
- **Loop vs once** → from the tag's **User Data** (`once` / `loop` / `none`); if the
  user data is empty it is taken from the tag's **Repeat** count (`1` → once,
  ∞ / other → loop). User Data overrides Repeat.

| tag direction | loop source | Defold playback |
|---|---|---|
| Forward | Repeat ∞ or user data `loop` | `PLAYBACK_LOOP_FORWARD` |
| Forward | Repeat `1` or user data `once` | `PLAYBACK_ONCE_FORWARD` |
| Reverse | loop / once | `PLAYBACK_LOOP_BACKWARD` / `PLAYBACK_ONCE_BACKWARD` |
| Ping-Pong | loop / once | `PLAYBACK_LOOP_PINGPONG` / `PLAYBACK_ONCE_PINGPONG` |
| any | user data `none` | `PLAYBACK_NONE` |

A brand-new tag (Repeat ∞, no user data) exports as `PLAYBACK_LOOP_FORWARD`.

## Building from source

`aseprite-defold-export.lua` is the source of truth; `ci/release.py` preprocesses it into
`extension.lua` and packs the `.aseprite-extension`. Install the resulting file via
`Aseprite → Edit → Preferences → Extensions → Add Extension`.
