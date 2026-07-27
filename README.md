<div align="center">
  <img src="KobraOS.png" alt="KobraOS Logo" width="180"/>

  # KobraOS

  **Smarter Control. Better Prints. More Possibilities.**

  [![Status](https://img.shields.io/badge/status-active%20development-22d17a?style=flat-square&labelColor=0a1628)](.)
  [![Platform](https://img.shields.io/badge/platform-Anycubic%20Kobra%20X-1a8fff?style=flat-square&labelColor=0a1628)](.)
  [![Hardware](https://img.shields.io/badge/hardware-ESP32--S3-1a8fff?style=flat-square&labelColor=0a1628)](.)
  [![License](https://img.shields.io/badge/license-Open%20Source-22d17a?style=flat-square&labelColor=0a1628)](.)
  [![Cloud](https://img.shields.io/badge/cloud%20dependency-none-22d17a?style=flat-square&labelColor=0a1628)](.)

  *An AI-powered companion system integrated directly into the Anycubic Kobra X —*
  *no cloud, no subscriptions, no limits.*

</div>

---

## What is KobraOS?

KobraOS turns a standard Anycubic Kobra X into a smart, AI-aware machine.

Imagine asking your printer out loud: **"How's the print going?"** and having it answer with real live data — temperature, progress, time remaining, whether something looks wrong. Not through a phone app. Through a local voice assistant mounted directly on the printer, that sees and knows what's happening in real time.

That's what this project is building — a **Jarvis for your 3D printer**.

No Anycubic account required at runtime. No internet connection required. Everything runs locally on a small ESP32-S3 board attached to the printer.

---

## Current Status

### ✅ Done — tested on real hardware

| Feature | Details |
|---|---|
| **Wi-Fi setup portal** | Device creates its own `KobraOS-Setup` AP (WPA2-protected) on first boot — no code editing needed |
| **3-step setup wizard** | Premium UI with client-side validation and a live MQTT mini-dashboard (Wi-Fi → Printer IP → MQTT) |
| **Multiple save paths** | `POST /save` (form), `POST /api/save` (JSON API), `GET /save_compat` (captive-portal fallback) |
| **Local LAN discovery** | Automatically discovers and decrypts printer MQTT credentials over LAN — no Anycubic cloud |
| **Encrypted local MQTT** | Mutual TLS connection to printer's local broker — zero cloud dependency |
| **MQTT reconnect resilience** | Progressive backoff, Wi-Fi-aware fast retry scheduling, reconnect event history |
| **Initial MQTT sync** | Requests printer snapshot immediately after subscribe — `/status` populated on connect |
| **Real-time temperature** | Nozzle and bed temperature updates every ~1 second during a print |
| **Full print state** | Layer number, percentage, time remaining (ETA), filename, print status |
| **Actuator telemetry** | Fan speed, light state, multicolor box status, RTSP stream URL when available |
| **Local status API** | `/status` endpoint with full printer telemetry and MQTT reconnect diagnostics |
| **Live dashboard** | `/dashboard` — real-time telemetry with quick control actions, color-coded health badge, live uptime |
| **Health endpoint** | `/health` — lightweight readiness check (Wi-Fi, MQTT, AP mode, uptime) |
| **Firmware metadata** | `/meta` — app name/version, IDF version, build date/time, ELF hash |
| **Diagnostics export** | "Copy Diagnostics JSON" button — combined `/status` + `/health` + `/meta` for support sharing |
| **Print controls** | `/pause`, `/resume`, `/stop` accessible from the AP portal |
| **Factory reset** | `/reset` endpoint to clear all saved credentials |
| **NVS persistence** | All settings (Wi-Fi, printer IP, MQTT credentials) stored securely on-device |

### 🔵 In progress

- Touch screen interface showing real-time printer status
- External I2S microphone (positioned away from fan noise)
- Speaker for voice responses
- AI integration for natural language interaction ("How's the print going?")
- Camera for visual failure detection (spaghetti effect, detachment, bad first layer)
- Full Home Assistant integration
- Broader dashboard UI beyond the setup portal

### ⭐ End goal

- **"How's the print going?"** — voice reply with real live data
- Auto-alert for spaghetti effect, detachment, or bad first layer
- Print history and learning from past failures
- Home Assistant / Alexa notifications
- Suggested fixes after a failed print
- 100% local, private, open source — forever free for the Kobra X community

---

## How it works (plain English)

1. **First boot:** The ESP32-S3 creates a Wi-Fi network called `KobraOS-Setup`. You connect from your phone and follow a 3-step wizard — enter your home Wi-Fi, your printer's local IP (found in the printer menu under Network → LAN Mode), and confirm.

2. **Auto-discovery:** On the next boot, the device connects to your home network and automatically talks to the printer over LAN to decrypt its local MQTT credentials. No Anycubic account, no cloud, no manual configuration.

3. **Live connection:** The device connects to the printer's local MQTT broker (encrypted, mutual TLS) and immediately requests a full status snapshot. From that point it receives real-time data — temperatures, print progress, layer count, ETA, fan and light state.

4. **Local dashboard:** Open `http://<device-ip>/dashboard` from any browser on your network to see real-time telemetry and control the printer (pause, resume, stop).

5. **(Coming soon)** You ask a question by voice. The microphone picks it up, an AI processes it using the live printer data, and the speaker answers.

---

## Hardware

| Component | Role |
|---|---|
| ESP32-S3 (AI-BOX-1.28) | Main controller — 16MB flash, 8MB PSRAM |
| Anycubic Kobra X | The printer being monitored and controlled |
| Touch display *(planned)* | Real-time status screen |
| I2S microphone *(planned)* | Voice input — external, away from fan noise |
| Speaker *(planned)* | Voice output for AI responses |
| Camera *(planned)* | Visual failure detection |

---

## Tech stack

- **Firmware:** C++ with ESP-IDF 5.3.1 via PlatformIO
- **Communication:** Local MQTT over mutual TLS (no cloud)
- **Protocol:** Reverse-engineered Kobra X LAN protocol (AES-128-CBC, MD5 signing)
- **Storage:** NVS (non-volatile storage) for all credentials and settings
- **Local API:** HTTP endpoints (`/status`, `/dashboard`, `/health`, `/meta`, `/pause`, `/resume`, `/stop`)
- **AI layer:** *(planned)* Local or API-based LLM for natural language
- **Home automation:** *(planned)* Home Assistant integration

---

## Why no cloud?

The Anycubic Cloud API is undocumented, unstable, and has been actively blocking community reverse-engineering projects. The Kobra X has an official **LAN Mode** (Settings → Network → LAN Mode) that exposes a local MQTT broker — KobraOS uses this instead.

This means:
- Works without internet
- Works if Anycubic shuts down their servers
- Faster response times (local network vs. cloud round-trip)
- Your printer data stays on your network, always

---

## Acknowledgements

Protocol findings and parsing inspiration from the [KX-Bridge project](https://github.com/thysson2701/KX-Bridge) by thysson2701. Thanks to thysson2701 and the KX-Bridge community for the reverse-engineering work that helped shape KobraOS's MQTT state mapping.

---

## Support this project

If you find this useful or want to see it finished, any contribution helps keep the project moving.

**☕ [Support on Ko-fi](https://ko-fi.com/ricardokcosta) · 🌟 Star this repo · 📢 Share with the Kobra X community**

---

<div align="center">
  <sub>Built with ❤️ for the Anycubic Kobra X community · Ricardo Da Costa · 2026</sub>
</div>
