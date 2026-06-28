### Atlas output & naming

Aseprite-Defold-Export exports into a single Defold `.atlas`:

- Pick the atlas in the **atlas** field of the export dialog (existing → merge, or a
  new name in any folder inside your Defold project).
- Frame PNGs go to a subfolder next to the atlas, named after the sprite
  (`<atlas-folder>/<sprite>/<sprite>_<...>.png`).
- Animations are named `<sprite>_<tag>` (or just `<sprite>` when the sprite has no
  tags), so several sprites can live in one shared atlas without name clashes.
- Re-exporting a sprite replaces only that sprite's entries; the rest of the atlas
  is left untouched.

Call animations in Defold with `sprite.play_flipbook(url, "<sprite>_<tag>")`.

### FPS

Aseprite counts milliseconds per frame, for each frame.

Whereas Defold counts frame per second, across all animation frames.

| aseprite avg (ms/f) | defold fps (f/s) |
| ------------------- | ---------------- |
| 500 ms              | 2 fps            |
| 100 ms              | 10 fps           |
| 69 ms               | 14 fps           |

### Playback

A tag with no user data **loops** by default (unless its Aseprite repeat count is 1).
Set the tag's user data to `once`, `loop` or `none` to force the behaviour:

| ase: direction    | ase: userdata | defold playback |
| ----------------- | ------------- | --------------- |
| Forward           | loop          | Loop Forward    |
| Reverse           | loop          | Loop Backward   |
| Ping-pong         | loop          | Loop Ping Pong  |
| Forward           | once          | Once Forward    |
| Reverse           | once          | Once Backward   |
| Ping-pong         | once          | Once Ping Pong  |
| Ping-pong Reverse | once / loop   | Ping Pong (no reverse equivalent in Defold) |
| Forward           | none          | None            |
| Reverse           | none          | None            |
| Ping-pong         | none          | None            |

### Lua module (optional)

The **generate lua module** option writes a `.lua` next to the atlas listing the
exported animation names, so you can reference them as constants from scripts instead
of typing animation strings by hand.
