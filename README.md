# Tsukasa (tsukasa-art)

**Systems & UX Developer — Perceptual Engineering · Local-First Tooling · Constrained-Environment Infrastructure**

I work at the intersection of physical constraints, human perception, and system runtimes. My engineering decisions are driven by verifiable friction points: CPU contention on single-digit-vCPU nodes, the measurable gap between RGB math and human visual perception, `.env` files leaking to AI agents, Japanese tax law encoded as runtime validation logic, and 32-bit Windows DX9 engines refusing to run on Apple Silicon.

I adopt Zig, Rust, or C only when Node/Web ecosystems introduce unacceptable overhead, unpredictable latency, or concurrency ceilings. Everything else stays in high-productivity runtimes.

> イラストレーターとしての視覚認知の専門性と、低レイヤからインフラにまたがる実装を交差させ、システムや UI の境界に生じる「摩擦」を解決しています。

---

## Engineering Stance

| Constraint Class | Observed Problem | Implementation Approach |
| :--- | :--- | :--- |
| **Hardware** | `libvips`-style processors saturate 1–2 vCPU nodes under parallel transcoding | Purpose-built Zig/C engine with controlled concurrency; FFI into Node/Bun |
| **Perception** | RGB/HSL coordinates fail to model human visual discrimination | CIE $L^*a^*b^*$ color space, $\Delta E_{2000}$ distance, $k$-means clustering in perceptual space |
| **Security** | `.env` files leak to AI agents; cloud secret managers add network dependency | Hardware-bound AES: Argon2id + ChaCha20-Poly1305, cryptographically locked to OS `machine_id` |
| **Regulation** | Japanese tax thresholds ("103万円の壁") generate cognitive overhead; existing tools use server-round-trips | Complex rule sets compiled into client-side validation logic; offline-first PWA |
| **Platform** | Apple Silicon deprecates Boot Camp; DX9 visual-novel engines (BGI, Kirikiri, やねうらおSDK) are Windows-only | Wine + DXVK 2.7.1 + MoltenVK → Metal pipeline; Phase 1 runtime validation confirmed *(private R&D)* |

---

## Open-Source Projects

### 01. [Amulet](https://github.com/Tuki-Sana/amulet) — *Zero-Trace Local Secret Manager*

- **Problem:** Standard `.env` files are readable by AI coding agents and scripts. Enterprise secret managers introduce network latency and setup friction.
- **Implementation:** A CLI written in Zig. Encrypts secrets using Argon2id + ChaCha20-Poly1305, bound to the local hardware via OS `machine_id` derivation. Designed to prevent terminal history leakage and agent-accessible plaintext.
- **Stack:** Zig · `crypto.zig` (Argon2id, ChaCha20-Poly1305) · `probe_id.zig` (hardware ID derivation) · cross-platform build via `build.zig`
- [Repository](https://github.com/Tuki-Sana/amulet) · [Releases](https://github.com/Tuki-Sana/amulet/releases)

### 02. [zenpix](https://github.com/Tuki-Sana/zenpix) — *Constrained-Environment Image Engine*

- **Problem:** Multi-threaded processors (`libvips`) cause severe vCPU contention and upstream service degradation on 1–2 core VPS nodes during parallel transcoding.
- **Implementation:** A Zig/C image processing engine with bicubic resizing and AVIF/WebP encoding. Interfaces to Node.js, Bun, and Deno via zero-copy FFI. Prioritizes stable latency over maximizing raw throughput. Pre-built `libpict.dylib` confirmed operational on Apple Silicon (macOS Tahoe).
- **Stack:** Zig · C · Node.js FFI · AVIF/WebP encoding
- [npm](https://www.npmjs.com/package/zenpix) · [Repository](https://github.com/Tuki-Sana/zenpix)

### 03. [Teinte](https://github.com/Tuki-Sana/teinte) — *Perceptual Color Analysis Desktop App*

- **Problem:** Digital color pickers return mathematical coordinates. No offline tool evaluates real-world human perception, WCAG contrast accessibility, or color harmony simultaneously.
- **Implementation:** A local-first desktop application. Rust backend implements $k$-means clustering in CIE $L^*a^*b^*$ space to extract dominant color clusters. Calculates perceptual distance via $\Delta E_{2000}$. Frontend handles color theory, harmony scoring, WCAG 2.1 contrast ratios, and palette export.
- **Stack:** Tauri 2 · Vue 3 · TypeScript · Rust (`analyze.rs`, `color_theory.rs`, `harmony.rs`, `palette_match.rs`)
- [Repository](https://github.com/Tuki-Sana/teinte) · [Releases](https://github.com/Tuki-Sana/teinte/releases)

<!-- ### 04. [BaiToTime](https://github.com/Tuki-Sana/BaiToTime) — *Shift & Payroll Compliance Simulator*

- **Problem:** Japan's labor income thresholds ("103万円の壁" and related brackets) impose significant cognitive load on hourly workers. Existing tools rely on server round-trips for validation.
- **Implementation:** An offline-first PWA. Complex tax and labor law rules are compiled into client-side validation logic with real-time feedback. Full shift management: multi-type scheduling (shift, day-off, paid leave, business trip), payroll calculation, WCAG-compliant visual hierarchy. Deployed on Cloudflare Pages with D1 (SQLite) and Drizzle ORM. 10 migration cycles completed.
- **Stack:** SvelteKit 5 · Svelte 5 (runes) · Cloudflare D1 · Drizzle ORM · Better Auth · Playwright · Vitest
- [Repository](https://github.com/Tuki-Sana/BaiToTime) · [App](https://baitotime.pages.dev/) 
-->
---

## Operational Infrastructure *(Private)*

Production infrastructure and specialized research remain private for operational and security boundaries.

- **Zero-dependency deployments:** Podman Compose with static Go/Zig/Rust binaries. No daemon dependencies.
- **Asset-policy routing:** Hybrid Cloudflare R2 + self-hosted Nginx for SFW/R18 asset-class separation.
- **Full-text search:** Self-hosted PostgreSQL + PGroonga for normalized Japanese FTS without PaaS overhead.

---

## R&D: Runtime Preservation *(Private · Active)*

Investigating Windows DX9 / DirectSound visual-novel engine compatibility on Apple Silicon.

**Confirmed (2026-05-08):** BGI/Ethornell engine (*Houkago Cinderella 2*, HOOKSOFT) running on M4 Pro / macOS Tahoe 26.4.1.

```
DX9 game binary
  └─ Wine 7.7 (ARM build, Whisky distribution)
       └─ DXVK 2.7.1  (d3d9.dll override: DX9 → Vulkan)
            └─ MoltenVK  (Vulkan → Metal)
                 └─ M4 Pro GPU / macOS Tahoe 26.4.1
```

**Status:**

- [x] Title screen, scene transitions, Japanese text rendering, mouse input
- [x] Kirikiri/KAG engine: graphics + audio confirmed operational
- [ ] BGI audio: `winecoreaudio.drv` yields `ca_channel_layout_to_channel_mask: Unhandled channel 0xffffffff` on macOS Tahoe — Wine 10.0 source build in progress

**Engine coverage:** BGI/Ethornell · Kirikiri/KAG · やねうらおGameSDK 3rd · RealLive (`rlvm` route) · Circus ADV (DirectDraw path)

**Research scope:** DX9→DXVK→MoltenVK→Metal pipeline validation · CoreAudio channel-mask mismatch analysis · DRM layer identification (Sofkudenchi / ソフト電池) · Engine binary fingerprinting.

---

## Technology Stack

| Layer | Tools | Stance |
| :--- | :--- | :--- |
| **System / Native** | Zig, Rust, C, Tauri 2 | Explicit allocation, minimal binaries, targeted FFI bridges — not a default stack choice |
| **Frontend / Web** | TypeScript, Astro, SvelteKit 5, Svelte 5, Vue 3 | Minimize hydration cost, semantic HTML, reduce main-thread blocking |
| **Infrastructure** | Linux (VPS), Podman, Cloudflare Pages/R2/D1, PostgreSQL | Self-hosted, reproducible; optimized for concurrency control on low-core nodes |
| **Testing / Ops** | Playwright, Vitest, Sentry, Better Auth | Automated regression coverage; low-maintenance telemetry |
| **R&D / Compat** | Wine, DXVK, MoltenVK, Metal, Swift/SwiftUI (planned) | Platform compatibility layer research for Japanese visual-novel engine preservation |

---

## Collaboration

Available for contract-based R&D, infrastructure consulting, and technical advisory on:

- Resolving vCPU contention, runtime latency, or FFI overhead in edge/VPS environments
- Designing bridge layers between native runtimes (Zig/Rust/C) and Web ecosystems
- Translating complex perceptual or regulatory data into actionable UI logic

Contact: [tsukasa-art.com/contact](https://tsukasa-art.com/contact)
