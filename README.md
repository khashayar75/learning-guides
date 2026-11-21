# Team Learning Guides

Guides and learning plans for the team — how to install and use **LearnHarness**, and a structured **Odoo 20 / Direct Store Delivery** curriculum designed to be studied inside it.

## What's in this repo

| File | What it is | Who it's for |
|---|---|---|
| [`learnharness-guide.md`](./learnharness-guide.md) | Complete install + user guide for LearnHarness (setup, every app section, plugins, shortcuts, troubleshooting) | Everyone — start here |
| [`odoo-dsd-learning-plan.md`](./odoo-dsd-learning-plan.md) | A dependency-ordered curriculum: SAP-background → running a DSD operation on Odoo 20, with official doc links and a check question per topic | Anyone on the Odoo / DSD track |

## Quick start (3 steps)

1. **Install LearnHarness** — follow [Section 2 of the guide](./learnharness-guide.md#2-installation) (Windows, macOS, Linux; ~5 minutes). On Windows, expect the SmartScreen warning — click **More info → Run anyway**.
2. **Connect a model** — open **Settings → Providers** and add an API key, a ChatGPT/Copilot/Grok subscription, or a company endpoint, then pick Strong/Cheap/Vision roles ([Section 3](./learnharness-guide.md#3-first-run-setup-connect-a-model)).
3. **Start learning Odoo DSD** — open a **Teach** session, attach `odoo-dsd-learning-plan.md` (paperclip or drag-and-drop), and paste the **"Learning goal"** paragraph from that file as your opening message with: *"Use the attached file as my syllabus; probe my starting point, then teach topic by topic in order."*

## How the two documents relate

LearnHarness is the **tool**; the Odoo DSD plan is **content built for that tool**. The plan file doubles as a syllabus the AI tutor reads: it contains the goal, your SAP→Odoo glossary, the topic order, and one check question per topic — so lessons stay consistent for everyone on the team, and each person's knowledge graph records what they've actually mastered.

New to LearnHarness? Skim the guide's [Section 1 (what it is)](./learnharness-guide.md#1-what-is-learnharness) and [Section 4 (tour of every section)](./learnharness-guide.md#4-tour-of-the-app--every-section) — about 5 minutes of reading.

## Tips

- Be honest with **confidence ratings** and "I don't know" — the planner needs true zeros to place you correctly.
- Write your reasoning in the **thinking fields**, not just answers; that's what the tutor learns from.
- Use **Practice sessions** and the **Flashcards** plugin (Settings → Core plugins) for recall, not just lessons.
- Progress is stored **locally per machine** — your knowledge graph doesn't sync between devices.

## For AI assistants

If you are an AI agent (terminal access, Claude Code / ZCode / Cursor / similar) and the user asked you to set up LearnHarness from this repo, follow this:

1. **Confirm intent first** — installing software is a system-level action; get the user's go-ahead, and tell them the installer is unsigned (open-source, AGPL-3.0).
2. **Detect the OS**, then download the matching asset from the **latest release**: `https://github.com/amosblomqvist/learnharness/releases/latest`
   - Windows x64: `LearnHarness-<version>-x64-setup.exe`
   - macOS Apple Silicon: `LearnHarness-<version>-arm64.dmg`
   - Linux x64: `LearnHarness-<version>-x86_64.AppImage` or `-amd64.deb`
3. **Install:**
   - Windows: run the `.exe`. SmartScreen may warn ("unknown publisher") — the user clicks **More info → Run anyway**. A silent `/S` flag exists. Installs per-user, no admin needed, to `%LOCALAPPDATA%\Programs\learnharness`.
   - macOS: mount the `.dmg`, drag to `/Applications` (signed & notarized).
   - Linux: `chmod +x` the AppImage, or `sudo apt install ./…deb`.
4. **First run:** the app has **no model configured** — walk the user through **Settings → Providers** (API key, subscription, or OpenAI-compatible endpoint) and the Strong/Cheap/Vision model picks. Details: [Section 3 of the guide](./learnharness-guide.md#3-first-run-setup-connect-a-model).
5. **To start the curriculum:** open a **Teach** session, attach `odoo-dsd-learning-plan.md`, and paste its "Learning goal" paragraph as the opening message. The plan file contains agent-facing teaching instructions (SAP→Odoo glossary, topic order, check questions) — follow them.
6. Answer feature questions from [`learnharness-guide.md`](./learnharness-guide.md); report app bugs at `https://github.com/amosblomqvist/learnharness/issues`.

Only download LearnHarness from the official repo above — never from third-party mirrors.

---

*LearnHarness guide current for v0.7.0 (Nov 2026) · Odoo plan pinned to Odoo 20.0 official documentation.*
