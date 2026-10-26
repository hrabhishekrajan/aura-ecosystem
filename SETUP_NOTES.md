# FOSS AI Assistant Ecosystem — Setup & Plan

> Working project directory: E:\ClineDesktopApp\Cline\projects\FOSS AI Assistant Ecosystem
> Blueprint doc: `FOSS AI Assistant Ecosystem Blueprint Doc.txt` (contains the full chat session transcript).

## 1. Current state
- Phase 0–1 code exists and is committed locally.
- The current deliverable is a working local MVP plus planning documents; pilot/enterprise profiles are not scaffolded yet.

## 2. Working project name
- Current working title: "FOSS Copilot Ecosystem" — **conflicts with Microsoft "Copilot" trademark** (must be avoided per blueprint constraints).
- Proposed brand (see Section 4).

## 3. Prerequisites to start the project
Grouped by layer. All are derived from the blueprint's "Lightweight Tech Stack Architecture" and "Refined Prompt".

### 3.1 Runtime & tooling
- Bun runtime, Node.js, Next.js, Tauri (Rust + WebView), WASM runtimes (Wasmtime / Wasmer)
- Rust toolchain (Tauri / Rust backend)
- Node.js + npm/yarn, Git
- Android Studio (mobile), Xcode (iOS/macOS) — if building native mobile/desktop targets

### 3.2 AI & inference
- Local lightweight LLMs: LLaMA 3, Mistral, Phi-3
- Whisper.cpp for speech-to-text
- ONNX Runtime, OpenVINO, Apache TVM (optimized CPU/GPU/edge inference)
- API tiers: OpenAI, Anthropic, Gemini + custom paid API config

### 3.3 Multimodal & orchestration
- OpenCL + ffmpeg (vision/audio/video pipelines)
- CopilotKit, Semantic Kernel, LangChain (orchestration/generative UI)

### 3.4 Data & memory
- SQLite / LibSQL / SQLite-vec / PgLite / Turbo-vec
- LanceDB / Turbo-vec for vector search
- Litestream (streaming replication)
- GraphQL + graph memory tools
- RAG layer

### 3.5 Plugins & skills
- Pawwork-style plugin hub
- MCP / ACP integration (Model/Agent Control Protocol)
- Aura-native onboarding first; Typebot only as an isolated, legally reviewed optional profile

### 3.6 Deployment & scalability (no Docker)
- Podman, Buildah, Skopeo, Containerd, Nerdctl
- k3s (lightweight Kubernetes, multi-tenant)
- WSL2 auto-config (Windows)
- Amazon Firecracker micro-VMs
- Workerd (Cloudflare Workers, serverless)

### 3.7 Security & governance
- TPM 2.0 hooks
- RBAC + tenant isolation
- Prompt injection defense (sanitize, audit logs)
- Encryption at rest/in transit, BYOK, secrets management (OpenBao/Infisical or another reviewed manager), key rotation
- TPM attestation flows for device keys

### 3.8 UX & commercialization
- Cross-platform desktop (Tauri), mobile (React Native/Flutter), browser (Next.js)
- Multi-user login (email/mobile, OAuth2)
- Subscription tiers (SaaS monetization)

### 3.9 Hardware & OS
- CPU-first, lightweight, "old OS/devices compatible" target
- Minimum hardware specs to be defined per device class (low-end CPU vs high-end GPU)

### 3.10 Legal & licensing
- MIT / Apache licenses
- Trademark clearance (no "Copilot", "Microsoft" branding)
- License compliance scanning in CI
- GDPR/CCPA/SOC2/ISO27001 roadmap; export controls

## 4. Product name — final recommendation
**Primary: "Aura"**
- Short, assistant-like, brandable across languages and platforms.
- No trademark conflict with "Copilot" or "Microsoft".
- Works for desktop / mobile / web / backend.
- Easy to brand as "Aura AI Assistant" or "Aura Ecosphere".

Strong alternatives if you prefer a literal/descriptive name:
- **OpenDesk** — desktop-first, open-source, clean positioning.
- **Cascade** — emphasizes orchestration/automation/multi-agent flows.
- **Prism** — emphasizes multimodal (lenses) + clarity.

## 5. Initial instruction (what the blueprint says)
- **Goal:** Lightweight, multi-tenant AI assistant desktop/web/mobile replicating Copilot Personal + Enterprise.
- **Deliverables:** Autonomous setup scripts, multi-tenant onboarding flows, lightweight orchestration framework, integration with listed GitHub repos.
- **Constraints:** Lightweight/CPU-friendly/old-OS compatible; local + external APIs; MIT/Apache commercializable; no Microsoft branding.

## 6. Revised plan / next steps
1. **Confirm final name** → update branding everywhere (README, code, repos, docx/pdf).
2. **Close PRD gaps** (blueprint Section B): model lifecycle/quantization, benchmarking, observability (OpenTelemetry/Prometheus), CI/CD + reproducible builds + SBOM, secrets/KMS, offline install + auto-update, enterprise SSO (SAML/OIDC/SCIM), legal/compliance.
3. **Scaffold repo** — monorepo layout: `apps/{desktop,web,mobile}`, `packages/{core,ai,plugins,storage}`, `scripts/`, `docs/`.
4. **Build** autonomous setup script + multi-tenant onboarding + core orchestration.
5. **Integrate** listed FOSS repos (CopilotKit, OpenBot, OpenDots, OpenTag, Semantic Kernel, Agents-M365Copilot, Windows-Copilot-API, copilot-prompts, m365-copilot-proxy) with license/compliance review.
6. **Launch** in phases (desktop → web → mobile), then enterprise onboarding.

## 7. Open items to confirm before coding
- Where is the project root intended to be? (This folder currently holds only docs.)
- Preferred mobile stack: React Native or Flutter?
- Any preferred cloud provider or is it fully local/Cloudflare Workers only?
- Target "minimum viable device" (e.g., 4 GB RAM Windows 10, or Raspberry Pi)?
