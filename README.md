# VoidPub

Public cape list for the [Void](https://github.com/Lonecard/VoidClient) Minecraft client. Edit these files to change what other Void players see. Changes reach players within about 10 minutes, the next time their launcher syncs (GitHub's raw-file cache can add up to 5 minutes).

| File | What it is |
|------|-----------|
| `capes.txt` | Who wears which cape: `player = cape_id` per line (player = name or UUID) |
| `users.txt` | Void users, shown with a small V next to their nametag |
| `capes/index.json` | Every cape: its id, file, and for animated capes the frame count and timing |
| `capes/*.png` | The textures |

## Adding a cape

1. Put the PNG into `capes/`. File names: lowercase letters, digits, `-` and `_`, ending in `.png`.
   - **Static:** 64x32, or an HD multiple with the same 2:1 layout (128x64, 256x128, 512x256, 1024x512, 2048x1024). Same layout as vanilla capes.
   - **Animated:** a vertical frame sheet, frame 0 on top, each frame below the previous one. Each frame 2:1, at most 512x256; at most 128 frames. The Void launcher's Cosmetics tab turns a GIF or animated PNG into this format: import it there, then copy the result from `%APPDATA%\.void\cosmetics\custom\`.
   - At most 2 MB per file.
2. Add it to `capes/index.json`:
   ```json
   "my_cape": { "file": "my_cape.png" }
   "my_anim": { "file": "my_anim.png", "frames": 12, "frameMs": 80 }
   ```
   Optional: `"sha1": "<sha1 of the file>"` (the launcher then refuses a file that doesn't match), and `"delays": [..]` with one delay in ms per frame for uneven timing (20-2000 ms each).
3. Give it to players in `capes.txt`.

## Rules the launcher enforces

- Downloads only from `https://raw.githubusercontent.com/`, only the files listed above, only PNGs that match the cape layout and size limits. Anything else is skipped and logged.
- Only players listed in `capes.txt` get a Void cape; everyone else looks vanilla.
- A player's own cape choice in their launcher always wins over this list for themselves.
- Profiles in vanilla mode never download anything.

## About names and UUIDs

Names are matched case-insensitively and UUIDs exactly; a UUID entry wins over a name entry. Names are **not** proof of identity: on offline-mode servers anyone can join with any name, and names can change. Use UUIDs for Microsoft accounts. An offline account's UUID is derived from its name (`UUID.nameUUIDFromBytes("OfflinePlayer:<name>")`), so for offline players a name entry is just as good.
