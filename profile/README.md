<p align="center">
  <img src="https://raw.githubusercontent.com/SpoolSense/spoolsense_scanner/main/docs/spoolsense-logo.png" width="180" alt="SpoolSense">
</p>

<h3 align="center">Open-source filament intelligence for 3D printers</h3>

<p align="center">
  Tap a spool. Everything else happens automatically.
</p>

---

## What is SpoolSense?

SpoolSense is an open-source ecosystem for automatic NFC-based filament tracking. Tap an NFC tag on your spool and your printer knows exactly what's loaded — material, color, weight, and more — without typing a thing.

Built for Voron/Klipper printers with single, multi-toolhead, or AFC (Box Turtle) setups. Works with Spoolman and Home Assistant out of the box.

```
Tap NFC tag on spool
        ↓
ESP32 scanner reads tag (OpenPrintTag / NTAG215 / OpenTag3D)
        ↓
Publishes decoded spool data via MQTT
        ↓
SpoolSense middleware updates Spoolman + Moonraker
        ↓
LED flashes → holds filament color
        ↓
Mainsail / Fluidd shows active spool per toolhead
```

---

## Repositories

| Repo | What it is |
|------|-----------|
| [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner) | ESP32 firmware — reads and writes NFC tags, serves a built-in web tag writer, publishes to MQTT and Home Assistant |
| [SpoolSense](https://github.com/SpoolSense/SpoolSense) | Python middleware — runs on a Pi, bridges the scanner to Spoolman, Moonraker, AFC/Box Turtle, and Klipper |

---

## Getting Started

1. **Hardware** — ESP32-WROOM or ESP32-S3-Zero + PN5180 NFC module. Full BOM in the [scanner README](https://github.com/SpoolSense/spoolsense_scanner#hardware).
2. **Flash the scanner** — configure `UserConfig.h` and flash with PlatformIO. Serves a tag writer UI at `spoolsense.local`.
3. **Run the middleware** — clone [SpoolSense](https://github.com/SpoolSense/SpoolSense), drop in your config, and run on any Pi on your network.

> An interactive installer that does all of this in one command is on the roadmap.

---

## Tag Format Support

| Format | Read | Write | Notes |
|--------|------|-------|-------|
| OpenPrintTag (ICODE SLIX2) | ✅ | ✅ | Full CBOR payload — material, color, weight, UUID |
| NTAG215 (ISO14443A) | ✅ | — | UID-only, Spoolman lookup via middleware |
| OpenTag3D | 🔜 | 🔜 | Planned |

---

## Community

- **Issues & bugs** — open an issue in the relevant repo
- **Ideas & discussion** — [spoolsense_scanner issues](https://github.com/SpoolSense/spoolsense_scanner/issues) or [SpoolSense issues](https://github.com/SpoolSense/SpoolSense/issues)
- **Contributing** — PRs welcome. See the contributing section in each repo's README.

---

<p align="center">
  <sub>Alpha software. Works great, actively evolving. Star the repos if you find it useful.</sub>
</p>
