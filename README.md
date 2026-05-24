# Tsukasa (tsukasa-art)

**Systems · Low-level · Web**

Systems programming to shipped web products — solo, end to end. Windows visual-novel engines running on Apple Silicon. An image processor that doesn't saturate a 2-core VPS. A secret manager cryptographically locked to hardware.

---

## Wukiyo — Windows Visual-Novel Engines on Apple Silicon

**[wukiyo.app](https://wukiyo.app) · [GitHub](https://github.com/tsukasa-art/Wukiyo)**

A production-grade compatibility stack for running Windows-only Japanese visual-novel games on Apple Silicon Mac. Forked Wine 10.0 ([wine-wukiyo](https://github.com/tsukasa-art/wine-wukiyo)) with game-specific patches — D3D9 surface hooks, GDI BitBlt hooks for save-thumbnail injection into Wine prefix. Built a Swift/SwiftUI launcher for per-title Wine prefix management, kqueue file watching, and ScreenCaptureKit window capture.

```
32-bit DX9 binary
  └─ Wine 10.0  (x86_64 · Rosetta 2)
       └─ DXVK 2.7.1  (DX9 → Vulkan)
            └─ MoltenVK  (Vulkan → Metal)
                 └─ M4 Pro GPU · macOS Tahoe
```

10+ ADV engines confirmed: BGI, KiriKiri Z, CMVS, CatSystem2, Artemis Engine, RealLive, and more.
Engine compatibility database → [wukiyo.app/compat](https://wukiyo.app/compat)

Technical series on Zenn (ongoing):
**→ [Apple SiliconのMacでWindows美少女ゲームを動かすまで全記録](https://zenn.dev/tsukasa_art/articles/mac-eroge-compat-part1)**

---

## Other Projects

### [zenpix](https://github.com/tsukasa-art/zenpix) — Image Engine for Constrained Environments · MIT

Zig/C image processing engine with Lanczos-3 resizing and AVIF/WebP encoding. Zero-copy FFI into Node.js, Bun, and Deno. Built to avoid the vCPU saturation caused by multi-threaded processors like `libvips` on 1–2 core VPS nodes. In production on this portfolio.
· [npm](https://www.npmjs.com/package/zenpix)

### [Amulet](https://github.com/tsukasa-art/amulet) — Hardware-Bound Secret Manager · MIT

CLI that encrypts secrets using Argon2id + XChaCha20-Poly1305, cryptographically bound to the local `machine_id`. Decryption fails on any other machine. Built for environments where `.env` files are readable by AI agents and scripts.
· [Releases](https://github.com/tsukasa-art/amulet/releases)

---

## Web & Shipped Products

Production web apps in TypeScript · Svelte · Astro · Vue, deployed and in daily use.
Details → [tsukasa-art.com/engineer](https://tsukasa-art.com/engineer)

---

## Contact

[tsukasa-art.com/contact](https://tsukasa-art.com/contact)
