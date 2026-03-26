<p align="center">
  <img src="https://raw.githubusercontent.com/SpoolSense/spoolsense_scanner/main/docs/spoolsense-logo.png" width="180" alt="SpoolSense">
</p>

<h3 align="center">Open-source filament intelligence for 3D printers</h3>

<p align="center">
  Tap a spool. Everything else happens automatically.
</p>

---

## What is SpoolSense?

SpoolSense is an open-source ecosystem for automatic NFC-based filament tracking. Tap an NFC tag on your spool and your printer knows exactly what's loaded — material, color, weight, temperatures, and more — without typing a thing.

Built for Klipper printers — single toolhead, multi-tool (klipper-toolchanger), or AFC (BoxTurtle/NightOwl). Works with Spoolman and Home Assistant out of the box. Slicer integration publishes spool data so Orca Slicer auto-populates tool colors, materials, and temps.

Designed for multi-platform — the publisher architecture means adding support for new printer platforms (Prusa, Bambu, Elegoo) requires one new file.

```
Tap NFC tag on spool
        ↓
ESP32 scanner reads tag (OpenPrintTag / TigerTag / OpenTag3D / NTAG)
        ↓
Publishes decoded spool data via MQTT
        ↓
SpoolSense middleware activates spool in Klipper + syncs Spoolman
        ↓
LED shows filament color → Slicer auto-populates tool info
```

**Shared scanner modes** — one scanner for all AFC lanes (`afc_stage`) or all toolheads (`toolhead_stage`). Scan a spool, load a lane or pick up a tool — data auto-assigns. No dedicated scanner per lane/tool required.

---

## Repositories

| Repo | What it is |
|------|-----------|
| [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner) | ESP32 firmware — reads and writes NFC tags, built-in web UI for tag reading/writing/config/NFC+ registration, publishes to MQTT and Home Assistant |
| [spoolsense_middleware](https://github.com/SpoolSense/spoolsense_middleware) | Python middleware — runs on a Pi, bridges the scanner to Spoolman, Klipper, AFC/BoxTurtle, klipper-toolchanger, and slicers via extensible publisher architecture |
| [spoolsense-installer](https://github.com/SpoolSense/spoolsense-installer) | Interactive CLI installer — flashes firmware, configures middleware, sets up Spoolman extra fields in one command |

---

## Getting Started

1. **Hardware** — ESP32-WROOM or ESP32-S3-Zero + PN5180 NFC module. Full BOM in the [scanner README](https://github.com/SpoolSense/spoolsense_scanner#hardware-needed).
2. **Install** — Run the installer on your Pi with the ESP32 connected via USB:
   ```bash
   curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh -o /tmp/install.sh && bash /tmp/install.sh
   ```
3. **Get your Scanner ID** — Open `http://spoolsense.local` and copy the Device ID from the landing page.
4. **Write some tags** — Use the built-in web UI at `spoolsense.local` to write filament data to NFC tags. Or use NFC+ to register plain NTAG tags in Spoolman without writing any data to the tag.
5. **Scan and print** — Tap a tagged spool on the scanner. Spoolman, Klipper, your slicer, and your front end update automatically.

---

## Tag Format Support

| Format | Read | Write | Notes |
|--------|------|-------|-------|
| OpenPrintTag (ISO15693) | ✅ | ✅ | Full CBOR payload — material, color, weight, temperatures, remaining tracking |
| TigerTag (ISO14443A) | ✅ | ✅ | Binary format — material, brand, color, weight, temperatures |
| OpenTag3D (ISO14443A) | ✅ | ✅ | NDEF binary — material, color, weight, density, extended fields |
| Bambu Lab (MIFARE Classic) | ✅ | — | UID detection only (encrypted, no data access) |
| Plain NFC (NTAG215, etc.) | ✅ | — | UID-only — register in Spoolman via NFC+ web page, no data on tag |

---

## Help Wanted: CAD / Enclosure Design

No printable cases exist yet for the current hardware. If you design for 3D printing, contributions are very welcome.

**Designs needed:**

- **Standalone scanner case (ESP32-WROOM-32 + AITRIP PN5180)** — wall or desk mount with NFC placement area on top
- **Standalone scanner case (ESP32-S3-Zero + AITRIP PN5180)** — same, smaller footprint
- **BoxTurtle AFC lane mount** — positions the PN5180 antenna correctly for tags on loaded spools

[Open an issue](https://github.com/SpoolSense/spoolsense_scanner/issues) in the scanner repo with photos and files.

---

## Community

- **Issues & bugs** — open an issue in the relevant repo
- **Ideas & discussion** — [scanner issues](https://github.com/SpoolSense/spoolsense_scanner/issues) or [middleware issues](https://github.com/SpoolSense/spoolsense_middleware/issues)
- **Contributing** — PRs welcome. See the contributing section in each repo's README.

---

<p align="center">
  <sub>Star the repos if you find it useful.</sub>
</p>
