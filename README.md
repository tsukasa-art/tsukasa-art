# Tsukasa (tsukasa-art)

**Engineer & Creative — Systems Programming · Visual Design · Windows→macOS Compatibility**

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
| **Platform** | Apple Silicon has no Boot Camp; 10+ DX9 Windows-only visual-novel engines (BGI, KiriKiri Z, CMVS, CatSystem2, Artemis, RealLive…) needed on macOS | Wine 10.0 (x86_64 via Rosetta 2) + DXVK + MoltenVK → Metal; all major ADV engines confirmed; Swift launcher (Wukiyo) + Wine fork (sikarugir-wine) in development |

---

## Open-Source Projects

### 01. [Amulet](https://github.com/Tuki-Sana/amulet) — *Zero-Trace Local Secret Manager*

- **Problem:** Standard `.env` files are readable by AI coding agents and scripts. Enterprise secret managers introduce network latency and setup friction.
- **Implementation:** A CLI written in Zig. Encrypts secrets using Argon2id + ChaCha20-Poly1305, bound to the local hardware via OS `machine_id` derivation. Designed to prevent terminal history leakage and agent-accessible plaintext.
- **Stack:** Zig · `crypto.zig` (Argon2id, ChaCha20-Poly1305) · `probe_id.zig` (hardware ID derivation) · cross-platform build via `build.zig`
- [Repository](https://github.com/Tuki-Sana/amulet) · [Releases](https://github.com/Tuki-Sana/amulet/releases)

### 02. [zenpix](https://github.com/Tuki-Sana/zenpix) — *Constrained-Environment Image Engine*

- **Problem:** Multi-threaded processors (`libvips`) cause severe vCPU contention and upstream service degradation on 1–2 core VPS nodes during parallel transcoding.
- **Implementation:** A Zig/C image processing engine with Lanczos-3 resizing and AVIF/WebP encoding. Interfaces to Node.js, Bun, and Deno via zero-copy FFI. Prioritizes stable latency over maximizing raw throughput. Pre-built `libpict.dylib` confirmed operational on Apple Silicon (macOS Tahoe).
- **Stack:** Zig · C · Node.js FFI · AVIF/WebP encoding
- [npm](https://www.npmjs.com/package/zenpix) · [Repository](https://github.com/Tuki-Sana/zenpix)

### 03. [Teinte](https://github.com/Tuki-Sana/teinte) — *Perceptual Color Analysis Desktop App*

- **Problem:** Digital color pickers return RGB coordinates that fail to model human visual discrimination. No offline desktop tool simultaneously evaluates perceptual color distance ($\Delta E_{2000}$), WCAG contrast accessibility, and color harmony — without uploading images to a server. A Rust-only immediate-mode GUI prototype (`egui`) confirmed the color algorithms, but lacked the layout control needed for a multi-panel palette interface.
- **Implementation:** Tauri 2 with Vue 3 handling UI precision via HTML/CSS, delegating only the computationally heavy tasks ($k$-means in $L^*a^*b^*$ space, $\Delta E_{2000}$ distance) to a focused Rust backend.
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

## R&D: Windows Visual-Novel Compatibility on Apple Silicon *(Active · Public)*

Validating and documenting a production-grade compatibility stack for Japanese Windows-only visual-novel engines on Apple Silicon. Research published as a 5-part technical series on Zenn.

**→ [Apple SiliconのMacでWindows美少女ゲームを動かすまで全記録（2026年）](https://zenn.dev/tsukasa_art/articles/mac-eroge-compat-part1)**

**Confirmed stack (2026-05-16):**

```
32-bit Windows DX9 binary
  └─ Wine 10.0  (x86_64, source build)
       └─ Rosetta 2  (x86_64 → ARM64 translation, M4 Pro)
            └─ DXVK 2.7.1  (DX9 → Vulkan)
                 └─ MoltenVK  (Vulkan → Metal)
                      └─ M4 Pro GPU / macOS Tahoe
```

**Engine coverage (10+ engines confirmed):**

| Engine | Status | Notes |
| :--- | :--- | :--- |
| BGI/Ethornell | ✅ Full | HOOKSOFT titles (Houkago Cinderella series) |
| Kirikiri/KAG | ✅ Full | |
| KiriKiri Z 2013 | ✅ Full | XP3 direct patch · extrans.dll swap · LAV Filters |
| CMVS (Purple software) | ✅ Full | cmvs32.exe required; cmvs64 crashes on Vulkan 1.3 |
| CatSystem2 | ✅ Full | wined3d required; waitd3dcommand=0 + disablemovie=1 |
| Artemis Engine | ✅ Full | wined3d recommended |
| RealLive | ✅ Full | No virtual desktop needed under Wine 10.0 |
| GIGA ADV | ✅ Full | |
| marmalade | ✅ Full | |
| Circus ADV | ✅ Full | DirectDraw path |
| やねうらおGameSDK 3rd | ⚠️ Partial | DRM (ソフト電池 / sdsys64.dll) blocks some titles |

**In development:**
- `sikarugir-wine` — Wine 10.0 fork: D3D9 surface + GDI BitBlt hooks for save-thumbnail injection into macOS
- `Wukiyo` — Swift/SwiftUI launcher: per-title Wine prefix management · kqueue file watch · ScreenCaptureKit thumbnail capture

**Research scope:** DX9→DXVK→MoltenVK→Metal pipeline · XP3 archive reverse-engineering · KiriKiri Z KAG3 script patching · DirectShow/LAV Filters integration · Apple amfid/allow-jit constraints on Wine arm64 · Rosetta 2 x86_64 translation limits

---

## Technology Stack

| Layer | Tools | Stance |
| :--- | :--- | :--- |
| **Native / Systems (Zig)** | Zig, C | Primary implementation language. Explicit allocation, SIMD (NEON/SSE2), cross-platform FFI, cryptographic primitives — written from scratch in two production libraries. |
| **Native / Systems (Rust)** | Rust, Tauri 2 | Used for Tauri desktop backends (algorithmic computation: k-means, ΔE₂₀₀₀, color theory). Not a headline specialization. |
| **Frontend / Web** | TypeScript, Astro, SvelteKit 5, Svelte 5, Vue 3 | Minimize hydration cost, semantic HTML, reduce main-thread blocking. |
| **Infrastructure** | Linux (VPS), Podman, Cloudflare Pages/R2/D1, PostgreSQL | Self-hosted, reproducible; optimized for concurrency control on low-core nodes. |
| **Testing / Ops** | Playwright, Vitest, Sentry, Better Auth | Automated regression coverage; low-maintenance telemetry. |
| **R&D / Compat** | Wine 10.0, DXVK, MoltenVK, Metal, Swift/SwiftUI, C | Windows→macOS compatibility layer: 10+ ADV engines confirmed; Wine fork (d3d9 hooks) + Swift launcher in development. |

---

## Collaboration

Available for contract-based R&D, infrastructure consulting, and technical advisory on:

- Resolving vCPU contention, runtime latency, or FFI overhead in edge/VPS environments
- Designing bridge layers between native runtimes (Zig/Rust/C) and Web ecosystems
- Translating complex perceptual or regulatory data into actionable UI logic

Contact: [tsukasa-art.com/contact](https://tsukasa-art.com/contact)
