<p align="center">
  <img src="assets/logo.png" width="250">
</p>

<h1 align="center">KobraOS</h1>

<p align="center">
  AI Companion for Anycubic Kobra X
</p>

Firmware for an ESP32-S3 companion to the Anycubic Kobra X 3D printer.

This repository implements the local firmware layer for KobraOS, focusing on printer discovery, MQTT credential extraction, and real-time print-state monitoring. Current behavior is centered on local LAN communication with the printer, not on Anycubic Cloud APIs.

## Project status

✅ Implemented:
- Wi-Fi setup portal via `KobraOS-Setup` AP (WPA2-protected)
- premium 3-step setup wizard UI with client-side validation and live MQTT mini-dashboard (Wi-Fi, printer IP, MQTT step)
- NVS persistence for Wi-Fi credentials, printer IP, and MQTT credentials
- resilient setup save pipeline with three paths:
  - `POST /save` (legacy form submit)
  - `POST /api/save` (JSON API submit)
  - `GET /save_compat` (captive-portal compatibility fallback)
- local LAN discovery of the Anycubic Kobra X using `http://<printer_ip>:18910/info`
- decryption of printer discovery payloads and extraction of local MQTT credentials
- MQTT TLS connection to the printer's local broker
- MQTT reconnect resilience with progressive backoff and Wi-Fi-aware fast retry scheduling
- initial MQTT sync requests after subscribe so `/status` is populated immediately after connect
- reassembly of fragmented MQTT payloads and parsing of temperature/print state plus fan/light/video/multicolor report messages, with fallback parsing for payload variants
- local status API at `/status` with printer telemetry and MQTT reconnect diagnostics
- dedicated live dashboard at `/dashboard` for real-time telemetry and quick local controls
- lightweight health endpoint at `/health` for readiness checks (Wi-Fi, MQTT, AP mode, uptime)
- printer control endpoints for `/pause`, `/resume`, and `/stop` from the AP configuration server
- runtime-validated setup flow: credentials saved from AP portal, reboot performed, discovery + MQTT connection confirmed in serial logs
- runtime-validated `/status` in STA mode with populated printer metadata, progress, last file, temperatures, ETA, and actuator status fields

⚠️ Still in progress:
- local web UI beyond configuration
- broader dashboard display for printer status beyond the setup portal mini-dashboard
- full Home Assistant integration or other automation frontend

## Overview

The firmware boot flow is:
1. start Wi-Fi manager
2. attempt STA connection with saved credentials
3. if STA fails, publish `KobraOS-Setup` AP for device setup
4. in STA mode, if MQTT credentials are missing, discover the printer on the LAN
5. connect to the printer's MQTT broker and subscribe to state topics

The codebase is intentionally narrow: it focuses on connectivity, discovery, credential storage, and parsing printer state.

## Hardware

- **MCU:** ESP32-S3
- **Framework:** ESP-IDF via PlatformIO
- **Build config:** `platformio.ini`

## What this repo does now

- `src/wifi_manager.cpp` handles STA/AP mode and NVS Wi-Fi storage.
- `src/web_config.cpp` serves the setup and local control/status HTTP surface, supports JSON + legacy save paths, and includes captive-portal-safe fallback routes.
- `src/anycubic_discovery.cpp` performs local LAN discovery and decrypts MQTT credentials.
- `src/anycubic_mqtt.cpp` connects to the printer's MQTT broker, subscribes to topics, logs printer state, and publishes print control commands.
- `src/anycubic_mqtt.cpp` now also schedules reconnect attempts with progressive backoff, short retry windows when Wi-Fi is not yet restored, requests an initial printer snapshot after subscribe, and exposes reconnect telemetry plus richer runtime fields (ETA, fan, light, multicolor, RTSP when available) in `/status`.
- `src/web_config.cpp` exposes AP-mode setup and controls, including pause/resume/stop and factory reset (`/reset`), and now serves a standalone dashboard at `/dashboard` with live status polling and quick control actions.

## What is not part of this repo yet

- any production-ready cloud integration
- a final user-facing display/UI
- a complete Home Assistant integration bundle
- full token refresh or Anycubic Cloud authentication flow

## Repository structure

```
KobraOS/
├── README.md
├── ARCHITECTURE.md   # main architecture document
├── ROADMAP.md
├── CONTRIBUTING.md
├── COPILOT.md
├── LICENSE
└── docs/
```

Implementation folders and firmware sources remain in `src/`, `include/`, `assets/`, `data/`, and project configuration files at repository root.

## Quick context for maintainers

If you want the architecture overview and current boot flow in one place, read `REPO_CONTEXT.md`.

## Acknowledgements

This project uses protocol findings and parsing inspiration from the external KX-Bridge project by thysson2701. The reference implementation at `kx_bridge_temp/kobrax_client.py` is based on the KX-Bridge work available from the GitHub repository:

- https://github.com/thysson2701/KX-Bridge

Thanks to thysson2701 and the KX-Bridge community for the reverse-engineering work that helped shape KobraOS's MQTT state mapping.

## License

See `LICENSE`.
