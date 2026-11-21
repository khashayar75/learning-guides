# LearnHarness — Installation & User Guide

A practical guide for installing and using **LearnHarness**, an AI-powered desktop app for structured learning. Share it freely with your team.

---

## 1. What is LearnHarness?

LearnHarness is an open-source "agent harness for learning." Instead of a chatbot you poke with questions, it runs an AI tutor that:

1. **Measures** — finds where your understanding of a topic actually ends, using your answers, confidence, and reasoning.
2. **Plans** — builds a dependency map from that starting point to your goal. The plan is visible and adapts as your understanding shifts.
3. **Teaches** — each step builds on concepts you already know: predict → explanation → understanding checks. Confusions get repaired before anything is built on top of them.
4. **Practices** — separate practice sessions give you varied problems that require recall without cues; problems and hints adapt to your reasoning.

The goal: a topic should feel **obviously true, not merely familiar** when you're done.

It keeps a persistent **knowledge graph** of what you understand — propositions and how they connect — so future sessions continue where you left off instead of starting from zero.

> License note: LearnHarness is AGPL-3.0. Using the app is unrestricted; reuse of its source must respect the license.

---

## 2. Installation

Download the latest release from **[learnharness.ai](https://learnharness.ai)** or **[GitHub Releases](https://github.com/amosblomqvist/learnharness/releases)**.

### Windows (x64)

1. Download `LearnHarness-<version>-x64-setup.exe`.
2. Double-click to run it. The installer is **not code-signed**, so Windows SmartScreen may show "Windows protected your PC".
   - Click **More info → Run anyway**.
3. The install is per-user (no admin rights needed) and lands in `C:\Users\<you>\AppData\Local\Programs\learnharness`.
4. Launch **LearnHarness** from the Start menu.

### macOS (Apple Silicon)

1. Download the `.dmg` (signed and notarized).
2. Open it and drag LearnHarness to **Applications**.

### Linux (x64)

- Make the `.AppImage` executable (`chmod +x`) and run it, **or**
- Install the `.deb` package.

---

## 3. First-run setup: connect a model

The app ships with **no model configured**. Open **Settings → Providers** (`Ctrl+,`) and set up one of:

| Option | Notes |
|---|---|
| **Provider API key** | OpenAI, Anthropic, etc. — you pay the provider's normal rates. |
| **Subscription connection** | ChatGPT, GitHub Copilot, or Grok accounts. These use your subscription; they never silently fall back to API-key billing. |
| **Claude Code CLI** | Uses a separately installed and authenticated Claude Code CLI on your machine. |
| **Custom endpoint** | Any single OpenAI-compatible server — including a local model server you run yourself. The app does not install or download models. |

Then pick three roles under **Settings**:

- **Strong** — used for new sessions (the main tutor brain)
- **Cheap** — used for background work
- **Vision** — used when images need understanding

You can switch the model per session (`Ctrl+K`); per-session picks don't change the background roles.

### Optional extras

- **Web search** — Settings → Providers → Search: a key for Tavily, Brave, Exa, or Firecrawl (Ollama's option uses its hosted service). URLs can still be fetched without a search key.
- **Dictation** — Settings → Providers → Dictation: a Deepgram key (needs "Member" role or higher on the Deepgram project), then pick a language (Multilingual available). Talk with `Ctrl+D`; `Esc` discards the pending transcript.
- **Cost meter** — a live estimate of API costs for known providers (subscriptions aren't metered).

---

## 4. Tour of the app — every section

### 4.1 Sessions sidebar (`Ctrl+B`)

Your session list lives on the left.

- **New session** — `Ctrl+N`; rename with `F2`.
- **Folders** — organize sessions into shared sidebar folders.
- **Search** — `Ctrl+Shift+F` to search across sessions.
- Everything saves locally, including compacted context for long sessions.
- Appearance options (Settings): compact list, adjustable transcript width.

The two session types:

### 4.2 Teach sessions

The main learning experience.

1. **Describe your goal** — e.g. "I want to really understand how HTTPS prevents eavesdropping."
2. **Answer the probe questions** — the agent figures out what you already know. Use the thinking fields (type or dictate) to explain *your reasoning*, not just the answer; mark your **confidence** honestly — it's self-reported and feeds the planner.
3. **Review the plan** — you get a visible dependency map from your level to the goal. It adapts as your understanding shifts.
4. **Work the lesson** — each step: predict → explanation → understanding check. Attach PDFs or photos, ask for visual explanations, image markups with arrows and labels, or interactive demonstrations.
5. **Go deeper safely** — open a **side conversation** (a floating, dockable follow-up window) for tangents without submitting an answer or advancing the lesson. The exchange is marked as a helper conversation the tutor knows about; use the separate **Cite** action to pull a conclusion back into the main thread.

### 4.3 Practice sessions

For testing recall.

1. Start a **Practice** session on a topic (or from a finished lesson).
2. Work the problem in the **thinking field** — write out your reasoning. Problems and hints adapt to how you reason.
3. Submit your thinking, then **reveal the committed solution**.
4. **Next problem** to continue. No cues while you work — that's the point.

### 4.4 Thinking fields & confidence

Throughout the app, answer boxes come with:

- A **thinking area** — your actual reasoning, typed or dictated. This is what the agent learns from; short correct answers teach it almost nothing.
- **Confidence** — self-reported, per answer. Say "I don't know" when you don't; the planner needs true zeros to place you correctly.
- `Shift+Tab` cycles the thinking level; `Ctrl+T` toggles all thinking display in the session.

### 4.5 Question dock

Understanding checks appear in the expanded question dock:

- `↑`/`↓` walk the answer rows, `Space` picks
- `←`/`→` adjust quiz confidence (except on "I don't know")
- `Tab` toggles between the thinking field and the answer rows
- **Other…** — free-text answer when none of the options fit

### 4.6 Knowledge Graph (`Ctrl+G`) and Chains

Your long-term memory.

- The graph stores **propositions** (ideas worth remembering) and **chains** (how they connect), each linked back to the session and source it came from. It stores *ideas and connections, not ratings of your ability*.
- **Chains view** — walk dependency trees: `→` expands a row, `←` returns to the parent, `Space` toggles rows, `Ctrl+F` searches.
- Future sessions read the graph instead of starting fresh.
- **Extract files…** — feed existing material straight into the graph: Files/Folder picker accepts `.txt`, `.md`, `.markdown`, `.rst`, `.org`, `.adoc`, `.tex`, `.pdf`. Limits: 25 MiB per file, up to three extractions in flight, UTF-8 only; PDFs are converted locally without OCR (image-only pages are skipped). Extraction is model-based, so it needs a working model.
- You can import notes, and filter the graph by session folder.

### 4.7 Files & attachments

- Attach with the **paperclip (Files / Folder)** or drag-and-drop.
- Files are **not uploaded** — the message carries a local path; agents read them under read-only guards when relevant.
- Images and PDF figures can be displayed, cropped, and annotated with arrows/labels; annotations are saved as session-owned snapshots.

### 4.8 Session stats & cost

The context/cost row under a session shows live token/context usage and estimated cost (for known API pricing). Long sessions get **compacted** automatically — old turns are summarized so the session can continue.

---

## 5. Core plugins

Off by default. Enable individually in **Settings → Core plugins**.

| Plugin | What it gives you |
|---|---|
| **Flashcards** | Generate cards from propositions or PDF/text, organized into decks, scheduled with FSRS-6 spaced repetition. |
| **Coding Questions** | Syntax-highlighted editor with starter code, tests, and model solutions. *Runs code locally with your permissions — it is not sandboxed.* |
| **Drawing Pad** | Pressure-sensitive pen, eraser, undo. A drawing attaches to your next answer — sketch a diagram or derivation. |
| **Rich Code Blocks** | Static rendering of SMILES (chemistry), ABC (music), and TikZ/CircuitikZ/PGFPlots diagrams in lesson text. |

---

## 6. Community plugins

Settings also has a **Community plugins** section: a local plugin directory with discovery/refresh, and a downloadable authoring skill with a type-only SDK. Plugins can add views, settings, tools, agent instructions, question types, and background tasks.

> Community plugins are trusted local code — they are **not sandboxed**. Only install plugins you trust.

---

## 7. Appearance

- **11 built-in themes** (Catppuccin, Rosé Pine, Gruvbox, Ivory, Pastel, …), full light-theme support.
- Color customization, JSON theme import/export.
- Separate code-ligatures toggle (off by default).

---

## 8. Keyboard shortcuts

`Mod` = `⌘` on macOS, `Ctrl` on Windows/Linux.

| Action | Shortcut |
|---|---|
| Toggle dictation | `Mod+D` |
| Open settings | `Mod+,` |
| Model selector | `Mod+K` |
| Cycle thinking level | `Shift+Tab` |
| Toggle sessions sidebar | `Mod+B` |
| Search sessions | `Mod+Shift+F` |
| Graph view | `Mod+G` |
| New session | `Mod+N` |
| Rename current session | `F2` |
| Toggle all thinking in session | `Mod+T` |
| Find in session / search graph & chains | `Mod+F` |
| Cheat sheet | `?` |
| Submit | `Enter` / `Mod+Enter` |
| Newline | `Shift+Enter` |
| Dismiss / close / cancel running turn | `Esc` |

All bindings are rebindable under **Settings → Keyboard**.

---

## 9. Data, privacy, and updates

- **Everything is stored locally**:
  - Windows: `C:\Users\<you>\AppData\Roaming\LearnHarness\data`
  - macOS: `~/Library/Application Support/LearnHarness/data`
  - Linux: equivalent XDG path
- **API keys are stored in plaintext** beside that data. Treat the machine accordingly.
- *Local storage ≠ local-only processing:* your model provider, research services, and dictation service can receive lesson content. If that matters for your material, use a provider/policy you control.
- **Updates** arrive in-app on all platforms (Settings → General). If an update ever fails to install silently, just download the latest installer from GitHub Releases and run it over the old version — your data is untouched.

---

## 10. Troubleshooting

| Symptom | Fix |
|---|---|
| SmartScreen blocks the Windows installer | More info → Run anyway (the installer is unsigned, not malicious). |
| "403" when using dictation | Deepgram key needs Member role or higher on the project. |
| A file won't extract into the graph | Check: ≤ 25 MiB, supported extension, UTF-8 (a binary/zero byte in the first 8 KiB is rejected), max 3 extractions at once. Image-only PDFs need OCR first — not supported. |
| No model / can't start sessions | Settings → Providers — no model is configured out of the box. Pick Strong/Cheap/Vision roles after adding one. |
| App updated but version looks old | Fully quit and reopen; check Settings → General for the update channel status. |

---

*Guide written for LearnHarness v0.7.0 (November 2026). Sources: official README, getting-started and keyboard docs at [github.com/amosblomqvist/learnharness](https://github.com/amosblomqvist/learnharness).*
