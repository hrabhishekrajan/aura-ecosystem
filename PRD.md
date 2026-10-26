# Aura — FOSS AI Assistant Ecosystem (MVP)

A lightweight, local-first FOSS AI assistant MVP; the multi-tenant pilot is a later profile. **Local-model-first**, CPU/Vulkan-friendly, and designed to run on older hardware with a **zero host footprint**. This is the **Phase 0–1 MVP**: an orchestrator that serves a local LLM (via llama.cpp) through an OpenAI-compatible API with a streaming web chat UI and SQLite-backed conversations.

> Working product name: **Aura**. See `PRD.md` for full product requirements, phased roadmap, and timeline.

## What's in the MVP

- **Inference Router** (`services/orchestrator/src/router.ts`) — spawns and health-checks `llama-server`, proxies OpenAI-compatible `/v1/chat/completions`, manages model lifecycle.
- **Model catalog** (`services/orchestrator/src/catalog.ts` + `models/MANIFEST.json`) — loads/validates local models with license + provenance metadata.
- **Storage** (`services/orchestrator/src/storage.ts`) — conversations + messages in SQLite via Bun's built-in `bun:sqlite` (no external package).
- **HTTP API + web UI** (`services/orchestrator/src/server.ts`, `public/`) — REST endpoints + streaming SSE chat + a clean chat interface.

## Local models (verified on disk)

| Model | Size | Use |
|---|---|---|
| Qwen3.5-0.8B-Q4_K_M | 508 MB | **Default chat** |
| Qwen3.5-0.8B-Q8_0 | 795 MB | Higher-quality 0.8B |
| Gemma-3-1B-it-Q4_K_M | 769 MB | Text + image |
| MiniCPM5-2B-Q4_K_M | 1.49 GB | Best vision |
| whisper tiny.en | 74 MB | Speech-to-text |
| kokoro (int8) | 88 MB | Text-to-speech |

Models live on the portable drive at `E:\ClineDesktopApp\Cline\models`. On the Vega 8 iGPU, **quantized models only** — FP16 is slower than CPU here (llama.cpp #5486).

## Requirements

- **Bun** (1.4.2+) — already on the drive at `E:\ClineDesktopApp\Cline\tools\bun\bun.exe`.
- **llama.cpp** (build 11346 with Vulkan) — already at `E:\ClineDesktopApp\Cline\tools\llamacpp\bin`.
- No compiler, no Docker, no system Python required for the MVP.

## Run

```bat
start.bat
```

Then open **http://127.0.0.1:8787** in your browser.

Or via Bun directly:

```bat
bun run services\orchestrator\src\index.ts
```

## Verify

```bat
scripts\smoke-test.ps1
```

This boots the orchestrator, checks `/api/health` and `/api/models`, sends a chat request, and prints the streamed reply.

## API

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/health` | Orchestrator + router status |
| GET | `/api/models` | Model catalog |
| GET | `/api/conversations` | List conversations |
| POST | `/api/conversations` | Create `{ title, modelId }` |
| GET | `/api/conversations/:id` | Conversation + messages |
| POST | `/api/chat` | `{ message?, messages?, modelId?, conversationId? }` → SSE stream |

## Configuration (env vars)

| Var | Default | Meaning |
|---|---|---|
| `AURA_PORT` | `8787` | Orchestrator HTTP port |
| `AURA_LLAMA_PORT` | `8800` | llama-server port |
| `AURA_HOST` | `127.0.0.1` | Bind host |

## Roadmap status

- [x] **Phase 0** — Foundation: monorepo, model manifest, router, storage, health/models API.
- [x] **Phase 1** — MVP local chat: streaming chat UI, conversation history, model switching.
- [ ] **Phase 2** — Voice (whisper/kokoro) + Vision (Gemma/MiniCPM).
- [ ] **Phase 3** — Plugins/skills (MCP/ACP), RAG.
- [ ] **Phase 4** — Multi-tenancy, SSO, billing (pilot profile; not a claim that the laptop serves 100 concurrent generations).
- [ ] **Phase 5** — Deployment/packaging/observability.
- [ ] **Phase 6** — Enterprise compliance & hardening.

## License

Apache-2.0. Model licenses vary — see `models/MANIFEST.json`; some (Gemma, MiniCPM) require review before commercial redistribution.
