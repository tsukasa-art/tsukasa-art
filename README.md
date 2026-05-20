# Tsukasa (tsukasa-art)

**Engineer & Creative — Systems Programming · Visual Design · Windows→macOS Compatibility**

Every project starts from something that wasn't working. The layer — systems, native, or web — follows the problem.

---

## Engineering Stance

| Constraint Class | Observed Problem | Implementation Approach |
| :--- | :--- | :--- |
| **Hardware** | `libvips`-style processors saturate 1–2 vCPU nodes under parallel transcoding | Purpose-built Zig/C engine with controlled concurrency; FFI into Node/Bun |
| **Perception** | RGB/HSL coordinates fail to model human visual discrimination | CIE $L^*a^*b^*$ color space, $\Delta E_{2000}$ distance, $k$-means clustering in perceptual space |
| **Security** | `.env` files leak to AI agents; cloud secret managers add network dependency | Hardware-bound AES: Argon2id + ChaCha20-Poly1305, cryptographically locked to OS `machine_id` |
| **Platform** | Apple Silicon has no Boot Camp; 10+ DX9 Windows-only visual-novel engines (BGI, KiriKiri Z, CMVS, CatSystem2, Artemis, RealLive…) needed on macOS | Wine 10.0 (x86_64 via Rosetta 2) + DXVK + MoltenVK → Metal; all major ADV engines confirmed; Swift launcher (Wukiyo) + Wine fork (wine-wukiyo) in development |

---

## Projects

### 01. [Amulet](https://github.com/Tuki-Sana/amulet) — *Zero-Trace Local Secret Manager* · MIT

- **Problem:** Standard `.env` files are readable by AI coding agents and scripts. Enterprise secret managers introduce network latency and setup friction.
- **Implementation:** A CLI written in Zig. Encrypts secrets using Argon2id + ChaCha20-Poly1305, bound to the local hardware via OS `machine_id` derivation. Designed to prevent terminal history leakage and agent-accessible plaintext.
- **Stack:** Zig · `crypto.zig` (Argon2id, ChaCha20-Poly1305) · `probe_id.zig` (hardware ID derivation) · cross-platform build via `build.zig`
- [Repository](https://github.com/Tuki-Sana/amulet) · [Releases](https://github.com/Tuki-Sana/amulet/releases)

### 02. [zenpix](https://github.com/Tuki-Sana/zenpix) — *Constrained-Environment Image Engine* · MIT

- **Problem:** Multi-threaded processors (`libvips`) cause severe vCPU contention and upstream service degradation on 1–2 core VPS nodes during parallel transcoding.
- **Implementation:** A Zig/C image processing engine with Lanczos-3 resizing and AVIF/WebP encoding. Interfaces to Node.js, Bun, and Deno via zero-copy FFI. Prioritizes stable latency over maximizing raw throughput. Pre-built `libpict.dylib` confirmed operational on Apple Silicon (macOS Tahoe).
- **Stack:** Zig · C · Node.js FFI · AVIF/WebP encoding
- [npm](https://www.npmjs.com/package/zenpix) · [Repository](https://github.com/Tuki-Sana/zenpix)

### 03. [Teinte](https://github.com/Tuki-Sana/teinte) — *Perceptual Color Analysis Desktop App* · Proprietary

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

## R&D: Windows Visual-Novel Compatibility on Apple Silicon *(Active · Public)*

Validating and documenting a production-grade compatibility stack for Japanese Windows-only visual-novel engines on Apple Silicon. Research published as an ongoing technical series on Zenn.

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
- `wine-wukiyo` — Wine 10.0 fork: `macdrv_GetImage` hook in `winemac.drv` for Metal-rendered window capture; save-thumbnail injection
- `Wukiyo` — Swift/SwiftUI launcher: per-title Wine prefix management · kqueue file watch · ScreenCaptureKit thumbnail capture

**Research scope:** DX9→DXVK→MoltenVK→Metal pipeline · XP3 archive reverse-engineering · KiriKiri Z KAG3 script patching · DirectShow/LAV Filters integration · Apple amfid/allow-jit constraints on Wine arm64 · Rosetta 2 x86_64 translation limits

---

## Collaboration

Available for contract-based R&D, infrastructure consulting, and technical advisory on:

- Resolving vCPU contention, runtime latency, or FFI overhead in edge/VPS environments
- Designing bridge layers between native runtimes (Zig/Rust/C) and Web ecosystems
- Translating complex perceptual or regulatory data into actionable UI logic

Contact: [tsukasa-art.com/contact](https://tsukasa-art.com/contact)
