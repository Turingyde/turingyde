# Turingyde

**Local-first, AI-native CRM for independent professionals**

![Windows](https://img.shields.io/badge/Windows-0078D4?logo=windows&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-000000?logo=apple&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![License](https://img.shields.io/badge/License-Proprietary-555555)
[![Website](https://img.shields.io/badge/Website-turingyde.com-7C3AED)](https://www.turingyde.com)

---

## What is Turingyde?

Turingyde is a CRM that runs entirely on your machine. All data is stored locally in IndexedDB via RxDB — no accounts, no cloud database, no SaaS subscription. You connect your own AI API keys (Gemini, OpenAI, Claude, or a local Ollama instance) and the LLM talks directly to your data. One-time purchase. Lifetime updates.

---

## Philosophy

There is no cloud — only other people's computers. Most CRMs are SaaS businesses in disguise: your data is their leverage, and your subscription is their revenue model. Turingyde inverts this. The application installs on your machine, your contacts live in your browser's IndexedDB, and AI inference uses keys you control. If the company disappeared tomorrow, your data and your tool would still work.

---

## Architecture

```
Frontend:    React 19 · Vite · Tailwind CSS v4 · Framer Motion
Local DB:    RxDB v14 (IndexedDB)
Server:      Express.js · better-sqlite3 (license management only)
Desktop:     Electron
AI:          Gemini · OpenAI · Claude · Ollama (user-supplied keys)
Voice:       Whisper (default) · Google Voice API
Sync:        CouchDB replication (self-hosted or managed)
Backup:      Google Drive · OneDrive (one-click export)
Licensing:   Ed25519 asymmetric signing (offline verification)
Payments:    Stripe
E2E Tests:   Playwright (14 passing)
```

The Express server handles license activation and Stripe webhooks. It does not store CRM data. The embedded server runs locally inside Electron — requests from the React frontend never leave the device during normal operation.

---

## Features

**Contacts & Organizations**
- Full contact records with tags, custom fields, and archive
- Organization linking and org-level activity tracking
- Fuzzy duplicate detection: pre-LLM check at input + post-LLM safety net
- CSV and VCF import with AI-assisted field mapping

**Deals**
- Kanban pipeline with drag-and-drop stage management
- Deal-to-contact linking, value tracking, close date

**Activities & Email**
- Per-contact activity feed (notes, emails, calls)
- Gmail OAuth and manual SMTP with HTML templates (Markdown-to-HTML)
- Email open tracking via pixel relay — AI can answer "did they open it?"

**AI Interface**
- Natural language queries against your full contact + deal database
- Voice input via Whisper or Google Voice API — tap, speak, done
- Contextual prompt injection: CRM state is passed to the LLM per query, not stored externally
- AI writes notes, creates contacts, moves deals, and drafts emails on command

**Automation**
- Trigger/condition/action chains with branching logic
- Server-side execution engine runs automations 24/7 (no app required to be open)

**Backup & Sync**
- One-click JSON export to Google Drive or OneDrive
- Live CouchDB replication (self-hosted or managed tier)

---

## AI / BYOK

Turingyde supports Gemini, OpenAI, Claude, and Ollama. You enter your API key in settings — the app calls the provider directly from the embedded server. No intermediary, no markup on tokens. A RAG context snapshot of your CRM data is injected per query so the AI has access to your contacts and deals without storing anything externally.

Recommended baseline: `gemini-2.5-flash` (low latency, low cost). Works equally well with GPT-4-class models or a local Mistral/LLaMA instance via Ollama.

---

## Data Model

All primary data (contacts, deals, activities, automations) lives in IndexedDB on the user's device, managed by RxDB v14. The schema uses Ed25519-signed collection versioning — bumping a version triggers an identity migration on upgrade; no data is ever dropped or silently discarded.

Optional sync via CouchDB (bring your own instance, or subscribe to the managed tier). Export snapshots as JSON at any time from Settings.

---

## Licensing

| | |
|---|---|
| Price | $79 early adopter / $99 standard (one-time) |
| Devices | 1 desktop + 1 mobile per license |
| Updates | Lifetime |
| Verification | Ed25519 signed token, verified offline |
| Refund | 30 days, no questions |
| Key format | `TUR-XXXX-XXXX-XXXX` |

License keys are opaque lookup tokens. Validation uses a signed JWT verified client-side against an embedded public key — no network call required after initial activation.

---

## Platforms

| Platform | Installer |
|---|---|
| Windows | NSIS installer (`.exe`) |
| macOS | App bundle in `.zip` (unsigned — right-click to open on first launch) |
| Linux | AppImage + `.deb` |

Download links at [turingyde.com](https://www.turingyde.com).

---

## Roadmap

- [ ] Mobile app (iOS + Android, linked to existing desktop license)
- [ ] Managed CouchDB sync service
- [ ] Google Voice API as alternative STT provider
- [ ] Enterprise audit logs
- [ ] Collaborative workspaces

---

## Links

- **Website & purchase**: [turingyde.com](https://www.turingyde.com)
- **Report a bug or request a feature**: [Issues](https://github.com/turingyde/turingyde/issues)
