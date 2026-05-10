<div align="center">

# Whisper by Remskill

### Hold a key. Talk. Text appears where your cursor is.

**Voice-to-text and real-time web answers for Windows and macOS — pasted straight into the app you're already in.**

[![Download](https://img.shields.io/badge/Download-whisper.remskill.com-blue?style=for-the-badge)](https://whisper.remskill.com/download)
[![Version](https://img.shields.io/github/v/release/Remskill/whisper?style=for-the-badge&label=Latest)](https://github.com/Remskill/whisper/releases/latest)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20Apple%20Silicon-lightgrey?style=for-the-badge)]()
[![Issues](https://img.shields.io/github/issues/Remskill/whisper?style=for-the-badge)](https://github.com/Remskill/whisper/issues)

[Website](https://whisper.remskill.com) · [Download](https://whisper.remskill.com/download) · [Pricing](https://whisper.remskill.com/pricing) · [FAQ](https://whisper.remskill.com/faq) · [Report a bug](https://github.com/Remskill/whisper/issues/new/choose)

---

Press your hotkey, speak, release. The text — transcribed locally on your machine, or via OpenAI in the cloud (your call, your API key) — lands at the cursor in any app: Word, Gmail, Slack, VS Code, Notion, Figma, your browser, anywhere.

You can also ask it a question and have a real-time, web-grounded answer pasted at the cursor instead of a transcript. No tab-switching, no copy-paste.

</div>

---

## Issues, bugs, feature requests

**File anything here:** [github.com/Remskill/whisper/issues](https://github.com/Remskill/whisper/issues)

This repo is the public bug tracker for the Whisper desktop app. The source code lives in private repos, but every issue, every feature request, every "this label is confusing" — please file it here.

For billing, account, or sales questions, email [whisper@remskill.com](mailto:whisper@remskill.com) instead — those don't belong on a public tracker.

---

## What it actually does

Three things, all triggered by the same hotkey:

1. **Dictation** — you speak, transcribed text appears at the cursor. Works in any app.
2. **Voice command + web search** *(Cloud mode only)* — you ask a question, the app searches the web via OpenAI and pastes a real, current answer. Built on the OpenAI Responses API.
3. **AI enhancement** — optional pass that fixes punctuation, formatting, tone, or rewrites with a custom preset (email draft, meeting notes, code comment style, your own).

You pick which transcription engine runs each one. There are three:

| Engine | Where it runs | Strengths | Trade-offs |
|---|---|---|---|
| **OpenAI Cloud** *(BYOK)* | OpenAI servers | Best-in-class accuracy. Web search. `gpt-4o-mini-transcribe` and `gpt-4o-transcribe`. Cloud AI enhancement via `gpt-5-mini` / `gpt-5-nano` / `gpt-5`. | Needs internet. Audio leaves your machine. You supply your own OpenAI API key — Remskill takes no cut. |
| **Parakeet** *(local)* | Your machine, pure Rust | 5–10× faster than Whisper on CPU. ~600 MB. English + 24 EU languages. | Quality is good but not Whisper-large. No translate-to-English. |
| **Whisper** *(local, 8 models)* | Your machine, pure Rust | 99 languages. Translate-to-English. Custom vocabulary, beam size, hotwords. | Slower than Parakeet. |

**There is no Python sidecar.** Local transcription runs in-process via [`transcribe-rs`](https://github.com/floneum/floneum) — pure Rust, no Python, no PyTorch, no separate runtime to install or break.

---

## Pricing — free, then optional Pro

The entire local pipeline is **free for any signed-in user**. No payment method required at signup. That includes:

- Whisper (all 8 models)
- Parakeet
- Local AI enhancement via [Ollama](https://ollama.com/)
- Hotkey, history, presets, hotwords
- Hardware acceleration, model downloads
- Settings, custom vocabulary, FAQ — everything

**Whisper Pro** ($9.99/mo · $79.99/yr · $99 lifetime) adds the Cloud surface:

- OpenAI cloud transcription
- Cloud AI enhancement (`gpt-5-mini` and friends)
- OpenAI web search via the Responses API

Pro ships with a 7-day Cloud trial when you upgrade (card required for the upgrade flow only — never at first signup). Team plans for 5 / 10 / 20 seats also available.

Stripe handles billing. [See current pricing →](https://whisper.remskill.com/pricing)

---

## How it works

1. **Press the hotkey.** Default is `Ctrl + Space` on Windows and **Right ⌥ (Right Option)** on macOS — both rebindable.
2. **Speak.** A small overlay shows you're recording.
3. **Release.** Transcribed text is pasted where your cursor is.

Under the hood: the hotkey starts an audio capture, the audio is fed to your chosen engine (Cloud, Parakeet, or Whisper), the result optionally goes through AI enhancement, then it's pasted at the cursor via OS-level clipboard handoff.

---

## Models (local)

Pick your trade-off between speed, size, and accuracy. The first run downloads the model; everything else is offline.

### English-optimized

| Model | Size | Speed | Best for |
|---|---|---|---|
| Base | ~140 MB | Fastest | Quick dictation |
| Small | ~480 MB | Fast | Voice memos |
| **Medium** *(recommended)* | ~1.5 GB | Balanced | Daily English use |
| Turbo *(distil-large-v3)* | ~1.5 GB | Fast | Large-quality at higher speed |

### Multilingual (99 languages, Whisper)

| Model | Size | Speed | Best for |
|---|---|---|---|
| Small | ~480 MB | Fast | Multi-language dictation |
| Medium | ~1.5 GB | Balanced | Multi-language daily |
| Large v3 | ~3 GB | Quality | Professional multi-language |
| Large v3 Turbo | ~1.62 GB | Fast | Large-v3 quality, faster |

### English + 24 EU languages (Parakeet)

| Model | Size | Speed | Best for |
|---|---|---|---|
| Parakeet v3 | ~600 MB | Fastest local | English + EU dictation when speed matters |

Switch models from Settings → Transcription. You can keep multiple models on disk and swap between them.

---

## Privacy — exactly what's true, no fudging

**Local mode (Parakeet or Whisper):** audio never leaves your machine. Disconnect the network and everything still works. No telemetry on transcripts. Transcriptions are stored locally in SQLite.

**Cloud mode (OpenAI):** audio goes to OpenAI's servers using your own API key under your OpenAI account, governed by [OpenAI's API data policy](https://openai.com/policies/api-data-usage-policies). API inputs are not used for training by default. Remskill servers do not see, log, or store the audio.

**Web search:** when you use the voice-command + web-search flow, your question is sent to OpenAI and the response includes web context. Transcripts of these queries are stored locally in your history unless you delete them.

[Full privacy policy →](https://whisper.remskill.com/privacy) · [Security details →](https://whisper.remskill.com/security)

---

## Platform support

| Platform | Architecture | Status | Default hotkey | Installer |
|---|---|---|---|---|
| **Windows 10/11** | x86_64 | Supported, ships today | `Ctrl + Space` | `.exe` |
| **macOS** (Apple Silicon) | arm64 | Supported, ships today | Right ⌥ | `.dmg` |
| **macOS** (Intel) | x86_64 | Deprecated — no longer built | — | — |
| **Linux** | — | Not supported | — | — |

### System requirements

- **Minimum:** 4 GB RAM, ~500 MB free + your chosen model size
- **Recommended:** 8 GB RAM. NVIDIA GPU with CUDA on Windows accelerates Whisper, but isn't required.

---

## Auto-updates

The app checks for updates automatically and shows a one-click update banner when a new version is available. Lifetime users receive all updates forever at no additional cost. Releases are published to Cloudflare R2; the desktop app verifies updates against a per-platform signed manifest.

---

## Support

- **Bugs / feature requests:** [github.com/Remskill/whisper/issues](https://github.com/Remskill/whisper/issues)
- **Billing, account, sales:** [whisper@remskill.com](mailto:whisper@remskill.com)
- **Live chat:** [whisper.remskill.com/contact](https://whisper.remskill.com/contact)
- **Security disclosures:** [security@whisper.remskill.com](mailto:security@whisper.remskill.com)
- **FAQ:** [whisper.remskill.com/faq](https://whisper.remskill.com/faq)

---

## License

Proprietary software. All rights reserved. See [Terms of Service](https://whisper.remskill.com/terms).

This repository contains the public README and release artifacts only. The desktop app source and the marketing site source live in private repositories.

---

<div align="center">

**[Download Whisper by Remskill](https://whisper.remskill.com/download)** — voice and web answers, at the cursor, in any app.

Made by [Remskill](https://remskill.com)

</div>
