<div align="center">

# 🪶 slim-context

### Keep the architecture. Drop the bloat. Stop paying the **Claude Code Token Tax.**

[![License: MIT](https://img.shields.io/badge/License-MIT-6c5ce7.svg)](./LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-D97757.svg)](#-installation)
[![Dependencies](https://img.shields.io/badge/Dependencies-0-brightgreen.svg)](#-installation)
[![Estimated Savings](https://img.shields.io/badge/Estimated_Savings-30--50%25-22c55e.svg)](#-how-it-works)

A single-file skill that cuts context-window burn by an estimated **30–50%** in large repositories — no hooks, no servers, no config.

[🚀 Installation](#-installation) · [🧪 Smoke Test](#-60-second-smoke-test) · [🎬 Example Usage](#-example-usage) · [🧠 How It Works](#-how-it-works) · [❓ FAQ](#-faq)

</div>

---

## 💸 The problem: the *Token Tax*

Modern repositories are heavy. Build logs, dependency trees, coverage reports, compiled artifacts,
vendored utilities — thousands of lines that Claude Code will happily load into its context window
**and bill you for, token by token**.

The math is brutal:

| Bloat source | Typical size | ≈ Tokens burned |
| --- | --- | --- |
| One verbose build log | ~300 KB | ~75,000 |
| A dependency-tree dump | ~120 KB | ~30,000 |
| A coverage report | ~200 KB | ~50,000 |
| **A single runaway log file** | **2 MB** | **~500,000 — 2.5× a full 200K context window** |

*(Back-of-envelope estimate: ~4 characters per token.)*

That's the *Token Tax*: sessions that degrade, truncate, or auto-compact at the worst possible
moment — and bills that scale with **noise** instead of engineering value.

## ✨ The solution

`slim-context` turns Claude Code into a disciplined context optimizer:

- 🔍 **Context Auditing** — inventories everything currently consuming the attention window (active files, loaded files, the file tree) and classifies it as *essential*, *structural*, or *bloat*.
- 🧬 **Interface-only Preservation** — before any file is dropped, it is compressed to a 1–2 line summary of its public interface or function signatures. The session keeps its **structural memory** without paying for the raw text.
- 🛡️ **Budget Protection** — targets heavy dependencies, compiled build artifacts, `.log` output dumps, test coverage files, and deeply nested utilities that have zero impact on the feature being edited.
- 🧹 **Conversation Hygiene** — tells you exactly when to run the native `/compact` command to flush stale conversation loops after a slim-down.

> 📌 **Non-destructive by design.** `slim-context` only reorganizes what the session keeps *in active attention*. It never deletes, moves, or modifies a single file on disk.

### How it compares

| Command | What it does | What it costs you |
| --- | --- | --- |
| `/clear` | Wipes the entire session | You lose the architecture along with the noise |
| `/compact` | Summarizes conversation history | Bloat gets summarized too — noise survives |
| `/slim-context` | Drops bloat, preserves interfaces | Nothing — pure signal gain |

## 🚀 Installation

Pick the direct script one-liner that matches your operating system terminal shell to
install `slim-context` globally in under 10 seconds:

### 🌐 Linux / macOS (Bash/Zsh)

```bash
mkdir -p ~/.claude/skills/slim-context && curl -fsSL https://raw.githubusercontent.com/HSF237/slim-context/main/.claude/skills/slim-context/SKILL.md -o ~/.claude/skills/slim-context/SKILL.md
```

### 🪟 Windows (PowerShell)

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills\slim-context" | Out-Null; Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/HSF237/slim-context/main/.claude/skills/slim-context/SKILL.md" -OutFile "$HOME\.claude\skills\slim-context\SKILL.md"
```

Restart your Claude Code session and run `/slim-context`. That's it — nothing to configure,
no dependency to install, no hook to register.

## 🧪 60-second smoke test

This repo ships a deliberately bloated demo log —
[`examples/dummy_bloat_file.log`](./examples/dummy_bloat_file.log), 250+ lines of synthetic
npm / webpack / coverage noise:

```bash
cp examples/dummy_bloat_file.log /path/to/your-project/build-debug.log
```

Now, inside Claude Code, ask it to read `build-debug.log`, then run `/slim-context` — and watch a
few thousand tokens vanish from active focus.

## 🎬 Example usage

You're mid-refactor in a 400-file monorepo and the session is getting sluggish:

```text
> /slim-context
```

Claude Code audits the session and replies:

```text
📉 Estimated Tokens Saved: 41%
🗑️ Dropped from Active Focus:
   • node_modules/** → replaced with 1-line interface stubs for 12 packages
   • build-debug.log → 266 lines compressed to a single summary line
   • coverage/lcov-report/** → generated artifact, excluded
   • dist/**/*.map → compiled output, zero impact on this feature
💡 Retained Architecture Map: The auth refactor touches src/auth/session.ts and
   src/middleware/token.ts, so both stay in full. The public signatures of
   src/db/pool.ts and src/utils/jwt.ts are preserved as one-liners — the complete
   structural scope this task needs.

🧹 Tip: your conversation history is long — run /compact now to flush stale
   memory loops and lock in the savings.
```

The session is now lean, the architecture is intact, and the next `/compact` has far less noise
to summarize.

## 🧠 How it works

`/slim-context` runs a four-stage pipeline defined entirely in
[`SKILL.md`](./.claude/skills/slim-context/SKILL.md) — plain markdown, zero runtime:

| Stage | Action |
| --- | --- |
| 1️⃣ **Repository Audit** | Scan active files, files loaded into memory, and the file tree to inventory what is actually consuming the window. |
| 2️⃣ **Isolate Token Bloat** | Classify heavy dependencies, build artifacts, `.log` dumps, coverage files, and nested utilities unrelated to the current task. |
| 3️⃣ **Draft a Compaction Plan** | Summarize each file's public interface in 1–2 lines to preserve structural memory, then drop the raw text from active focus. |
| 4️⃣ **Enforce Conversation Hygiene** | Recommend the native `/compact` command when history is long, flushing stale memory loops. |

No hooks. No servers. No settings. One prompt file doing what a good senior engineer does
instinctively: **protect the working set.**

## 📁 Repository structure

```text
slim-context/
├── .claude/
│   └── skills/
│       └── slim-context/
│           └── SKILL.md            # the entire skill — zero dependencies
├── examples/
│   └── dummy_bloat_file.log        # 250+ lines of synthetic bloat for demos
├── LICENSE                         # MIT
└── README.md                       # you are here
```

## ❓ FAQ

**Does it delete my files?**
No. `slim-context` only changes what the session keeps *in active attention*. Disk contents are
never touched — that's the Prime Directive in [`SKILL.md`](./.claude/skills/slim-context/SKILL.md).

**Is this a replacement for `/compact`?**
No — it's the perfect warm-up act. `/compact` summarizes conversation history; `slim-context`
first strips repo bloat so compaction has less noise to summarize. Run `/slim-context`, then
`/compact`.

**Will it forget my architecture?**
No. Interface-only Preservation guarantees that every dropped file leaves behind a 1–2 line
summary of its public interface or signatures. Structural memory survives the slim-down.

**Does it work with any repo, any size?**
Yes. It's a plain markdown skill — no runtime, no dependencies, nothing to configure. It shines
in large repositories where bloat dominates the window; tiny repos see smaller wins.

**How much does it actually save?**
It depends on how much bloat is in attention. Logs, artifacts, and dependency dumps are the
biggest wins. The skill reports an estimate on every run — see
[`examples/dummy_bloat_file.log`](./examples/dummy_bloat_file.log) for a worst-case demo.

## 🤝 Contributing

Issues and pull requests are welcome — especially improvements to the classification rules in
[`SKILL.md`](./.claude/skills/slim-context/SKILL.md). House rules:

- ⚡ Keep the skill **single-file and zero-dependency**
- 🛡️ Keep it **non-destructive** — attention only, never disk
- 🧬 Keep dropped context recoverable via interface summaries

## 📄 License

Released under the [MIT License](./LICENSE). If `slim-context` saves you real money, a ⭐ is the
cheapest way to say thanks.

---

<div align="center">

**Built as a submission for the *Claude for Open Source* program.**

*Every token you don't burn is a token you don't pay for.* 🪶

</div>
