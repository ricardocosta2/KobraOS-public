# KobraOS Documentation Roadmap

As KobraOS continues to evolve, the documentation is being expanded into a complete knowledge base designed not only for developers but also for AI-assisted development tools such as GitHub Copilot, ChatGPT, Claude and Gemini.

The goal is to ensure that both humans and AI assistants understand the project architecture, previous engineering decisions and reverse engineering discoveries without repeating work that has already been done.

## Planned Documentation Structure

### ✅ Chapter 1 – Project Overview
Provides a high-level overview of KobraOS, its goals, design philosophy and long-term vision.

**Key topics**
* ✅ Project mission
* ✅ Supported hardware
* ✅ Design principles
* ✅ Long-term objectives

---

### ✅ Chapter 2 – Current Architecture
Documents how every major component of KobraOS interacts.

**Key topics**
* ✅ ESP32 architecture
* ✅ Local networking
* ✅ HTTP discovery
* ✅ MQTT communication
* ⬜ AI integration *(planned — v2.0)*
* ⬜ Display subsystem *(planned — v2.0)*
* ✅ Configuration storage

---

### ✅ Chapter 3 – Reverse Engineering Knowledge
Central repository of everything discovered about the Anycubic Kobra X local protocol.

**Key topics**
* ✅ Discovery endpoints (`http://<ip>:18910/info`, `ctrlInfoUrl`)
* ✅ Authentication (MD5 signing, AES-128-CBC decryption, mutual TLS)
* ✅ MQTT broker (`mqtts://<ip>:9883`, topic prefix `anycubic/anycubicCloud/v1`)
* ✅ Packet structure (fragmented payloads, reassembly, JSON parsing)
* ✅ HTTP services (`/status`, `/health`, `/meta`, `/pause`, `/resume`, `/stop`, `/reset`)
* ⬜ Upload mechanism *(RTSP URL known, upload endpoint identified, not yet implemented)*
* ⬜ Camera interface *(RTSP URL discovered, integration planned)*
* 🔄 Firmware observations *(ongoing)*

---

### 🔄 Chapter 4 – External Research Library
Summarises relevant open-source projects and research that influenced KobraOS.

Projects currently analysed:
* ⬜ Rinkhals
* ✅ KX-Bridge *(thysson2701 — MQTT topic mapping and state parsing)*
* ⬜ CYD-Klipper
* ✅ Home Assistant LAN integrations *(grunna/ha-anycubic-kobra-x-lan, stribor/anycubic_kobrax)*
* ✅ Community reverse engineering efforts
* 🔄 Technical articles and protocol analysis

Each project includes:
* Purpose
* Useful discoveries
* Reusable concepts
* Limitations
* Licensing considerations
* Integration notes

---

### ✅ Chapter 5 – Engineering Decisions
Documents architectural decisions to avoid regressions and inconsistent implementations.

* ✅ LAN-first communication *(cloud API found to be blocked/unstable — pivoted to LAN)*
* ✅ Cloud-independent architecture
* ✅ Modular design *(wifi_manager, web_config, anycubic_discovery, anycubic_mqtt isolated)*
* ✅ Offline operation
* ✅ Low memory footprint *(ESP-IDF native, no magic libraries)*
* ✅ Backwards compatibility *(legacy save path + JSON API + captive-portal fallback)*

---

### ⬜ Chapter 6 – AI Development Context
Provides permanent context for AI coding assistants.

The objective is to prevent AI from:
* ⬜ Rewriting working modules
* ⬜ Repeating completed reverse engineering
* ⬜ Introducing unnecessary dependencies
* ⬜ Breaking established architecture

Instead, AI is encouraged to:
* ⬜ Extend existing code
* ⬜ Reuse validated solutions
* ⬜ Follow current coding patterns
* ⬜ Respect project conventions

---

### ⬜ Chapter 7 – Verified Knowledge Base
A structured database of verified protocol information.

Each discovery is classified by confidence level:
* ⬜ Verified
* ⬜ Observed
* ⬜ Hypothesized
* ⬜ Unknown

This prevents assumptions from becoming accepted facts.

---

### 🔄 Chapter 8 – Protocol Reference
A living reference of all known communication mechanisms.

* ✅ HTTP endpoints (`/info`, `/ctrl`, `/status`, `/health`, `/meta`, `/pause`, `/resume`, `/stop`, `/reset`, `/save`, `/api/save`, `/save_compat`, `/dashboard`)
* ✅ MQTT topics (`info/report`, `tempature/report`, `print/report`, `status/report`, `fan/report`, `file/report`, `multiColorBox/report`, `user/report`)
* ✅ JSON payloads *(state, temps, progress, layers, ETA, fan, light, multicolor, RTSP)*
* ✅ Print commands (pause, resume, stop)
* 🔄 Responses and error codes *(partially documented)*
* ✅ Printer states (`free`, `busy`, `finished`)

---

### 🔄 Chapter 9 – Components Status
Tracks the implementation status of every subsystem.

| Component | Status |
|---|---|
| Wi-Fi Manager | ✅ Complete |
| Discovery | ✅ Complete |
| MQTT | ✅ Complete |
| Web Config / Setup Portal | ✅ Complete |
| Local Dashboard | ✅ Complete |
| Print Control (pause/resume/stop) | ✅ Complete |
| Local Storage (NVS) | ✅ Complete |
| Health / Status / Meta endpoints | ✅ Complete |
| OTA | ⬜ Planned |
| Display | ⬜ Planned — v2.0 |
| AI Assistant | ⬜ Planned — v2.0 |
| Voice Pipeline | ⬜ Planned — v2.0 |
| Camera / Visual Detection | ⬜ Planned — v2.0 |
| Firmware Updates | ⬜ Planned — v3.0 |

---

### ⬜ Chapter 10 – Development Guidelines
Defines coding standards and engineering practices for contributors.

Topics include:
* ⬜ Code organisation
* ⬜ Memory optimisation
* ⬜ Error handling
* ⬜ Async programming
* ⬜ Testing strategy
* ⬜ Logging
* ⬜ Documentation standards

---

### ⬜ Chapter 11 – Roadmap
Maintains the long-term development plan. *(See `ROADMAP.md`)*

Future areas include:
* ⬜ Plugin system
* ⬜ Web interface
* ⬜ Desktop application
* ⬜ Mobile application
* ⬜ Multi-printer support
* ⬜ Advanced AI features
* ⬜ Predictive maintenance
* ⬜ Additional printer models

---

# Vision

The documentation is intended to become much more than a traditional developer manual.
