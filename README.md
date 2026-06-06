# Player ESP — NeoForge 1.21.1 Client Mod

A fully client-side ESP mod for Minecraft Java 1.21.1 with NeoForge 21.1.227.  
Tracks player coordinates in real-time, persists last-known locations when they go offline, and displays everything in a sleek hold-to-show HUD.

---

## Features

- **Hold-to-show overlay** — tap and hold a configurable keybind (default: `.`) to pop up the ESP panel. Release to hide it with a smooth fade animation.
- **Online players** — shows live XYZ coordinates, dimension (OW/Nether/End), distance in meters, and health bar.
- **Offline players** — stores last known position at logout time, shows how long ago they were seen.
- **Fully configurable settings screen** — press `,` to open the ESP settings at any time (in-game, not just in pause menu).
- **Sort modes** — sort by Status (online first), Name, Distance, or Last Seen.
- **Compact mode** — single-line rows for more players on screen.
- **Color presets** — Forest, Ice, Blood, Amber, or full custom RGB per status type.
- **Overlay position presets** — Top-Left, Top-Right, Bottom-Left, or drag via config.
- **Flash notifications** — brief on-screen notice when players join or leave.
- **Config persists** — settings saved to `config/playeresp.json` between sessions.

---

## Installation

1. Install [NeoForge 21.1.227](https://neoforged.net/) for Minecraft 1.21.1.
2. Drop `playeresp-1.21.1-1.0.0.jar` into your `.minecraft/mods/` folder.
3. Launch the game. The mod is client-side only — no server installation needed.

---

## Building from Source

**Requirements:** JDK 21, internet connection (Gradle downloads NeoForge MDK)

```bash
# Clone / extract the project
cd playeresp/

# Generate Gradle wrapper if missing
gradle wrapper

# Build
./gradlew build

# Jar will be at:
# build/libs/playeresp-1.21.1-1.0.0.jar
```

On Windows use `gradlew.bat build`.

---

## Keybinds

| Action | Default Key | Configurable? |
|---|---|---|
| Show ESP Overlay (hold) | `.` (period) | ✅ Options > Controls > Player ESP |
| Open Settings | `,` (comma) | ✅ Options > Controls > Player ESP |

---

## Settings Screen

Press `,` in-game to open the settings screen.

### Display Options
| Setting | Description |
|---|---|
| Show Coordinates | Show XYZ coords for each player |
| Show Dimension | Show OW / Nether / End tag |
| Show Distance | Show distance in meters (online only) |
| Show Health | Show current health with color coding |
| Show Last Seen | Show "Xm ago" time for offline players |
| Show Offline Players | Include offline players in the list |
| Show Player Count | Show online/offline count in header |
| Compact Mode | Single-line rows (more players on screen) |
| Flash on Join/Leave | Brief on-screen notification |

### Behavior & Style
| Setting | Description |
|---|---|
| Sort Mode | STATUS / NAME / DISTANCE / LAST_SEEN |
| Opacity | Overlay background opacity (30–100%) |
| Max Players Shown | Cap at 10–100 players |
| Color Presets | Forest / Ice / Blood / Amber |
| Position Presets | Snap overlay to screen corners |

---

## Config File

Located at `.minecraft/config/playeresp.json`. Edit manually for fine-grained control:

```json
{
  "overlayKey": "key.keyboard.period",
  "settingsKey": "key.keyboard.comma",
  "overlayX": 0.02,
  "overlayY": 0.10,
  "overlayWidth": 320,
  "overlayOpacity": 0.88,
  "colorOnlineR": 80, "colorOnlineG": 255, "colorOnlineB": 120,
  "colorOfflineR": 180, "colorOfflineG": 80, "colorOfflineB": 80,
  "colorUnknownR": 160, "colorUnknownG": 160, "colorUnknownB": 160,
  "colorHeaderR": 255, "colorHeaderG": 200, "colorHeaderB": 60,
  "colorBgR": 5, "colorBgG": 5, "colorBgB": 15,
  "showCoordinates": true,
  "showDimension": true,
  "showDistance": true,
  "showHealth": true,
  "showLastSeen": true,
  "showOfflinePlayers": true,
  "showOnlineCount": true,
  "sortMode": "STATUS",
  "compactMode": false,
  "maxPlayersShown": 50,
  "flashOnPlayerJoin": true
}
```

---

## How It Works

- Every 10 ticks (~0.5s), the mod scans:
  1. All `Player` entities visible in the current dimension (live XYZ, health, distance)
  2. The tab-list (`PlayerInfo`) for any players not in render range
- When a player goes offline (disappears from both), their last position is frozen and marked with a timestamp.
- All data lives in `PlayerDataStore` (in-memory, cleared on server disconnect).
- The overlay is rendered via `RenderGuiEvent.Post` and only draws while the keybind is held.

---

## Limitations

- **Cross-dimension tracking**: Players in different dimensions won't have live XYZ (they won't appear as entities) — they'll show as their last known coordinates until they're in the same dimension. Tab-list presence still marks them online.
- **Render distance**: Player entities are only tracked if within your render distance. The tab-list supplements this but doesn't provide coords.
- **Servers with anti-cheat**: Some servers hide player positions from the tab-list or entity packets. This mod only reads data the client legitimately receives.

---

## License

MIT — use, modify, redistribute freely.
