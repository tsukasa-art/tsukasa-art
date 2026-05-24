# 月村つかさ（tsukasa-art）

**システム · 低レイヤー · Web**

低レイヤーから Web まで、一人で完結させます。Apple Silicon で動く Windows 美少女ゲーム。2コア VPS を詰まらせない画像処理エンジン。ハードウェアに暗号学的に縛られたシークレット管理 CLI。

---

## Wukiyo — Apple Silicon で Windows 美少女ゲームを動かす

**[wukiyo.app](https://wukiyo.app) · [GitHub](https://github.com/tsukasa-art/Wukiyo)**

Apple Silicon Mac 上で Windows 専用の美少女ゲームを動かすための互換スタック。Wine 10.0 をフォーク（[wine-wukiyo](https://github.com/tsukasa-art/wine-wukiyo)）し、ゲーム向けパッチ（D3D9 サーフェスフック・GDI BitBlt フックによるセーブサムネイル注入）を実装。Swift/SwiftUI 製ランチャーで、タイトルごとの Wine prefix 管理・kqueue ファイル監視・ScreenCaptureKit ウィンドウキャプチャを提供します。

```
32-bit DX9 バイナリ
  └─ Wine 10.0（x86_64 · Rosetta 2）
       └─ DXVK 2.7.1（DX9 → Vulkan）
            └─ MoltenVK（Vulkan → Metal）
                 └─ M4 Pro GPU · macOS Tahoe
```

BGI・KiriKiri Z・CMVS・CatSystem2・Artemis Engine・RealLive など主要 ADV エンジン 10 種以上の動作を確認済み。
動作確認済みタイトル一覧 → [wukiyo.app/compat](https://wukiyo.app/compat)

Zenn 技術連載（公開中）:
**→ [Apple SiliconのMacでWindows美少女ゲームを動かすまで全記録](https://zenn.dev/tsukasa_art/articles/mac-eroge-compat-part1)**

---

## その他のプロジェクト

### [zenpix](https://github.com/tsukasa-art/zenpix) — 制約環境向け画像処理エンジン · MIT

Zig/C 製の画像処理エンジン。Lanczos-3 リサイズ・AVIF/WebP エンコードを実装し、Node.js・Bun・Deno へのゼロコピー FFI を提供。`libvips` のような並列処理が 1〜2 コア VPS で引き起こす vCPU 飽和を回避するために設計。本番環境（このポートフォリオ）でも使用中。
· [npm](https://www.npmjs.com/package/zenpix)

### [Amulet](https://github.com/tsukasa-art/amulet) — ハードウェアバインド型シークレット管理 CLI · MIT

Argon2id + XChaCha20-Poly1305 でシークレットを暗号化し、OS の `machine_id` に暗号学的に紐付ける CLI。別のマシンにコピーしても復号できません。AI エージェントが `.env` を読める時代に向けて設計しました。
· [リリース](https://github.com/tsukasa-art/amulet/releases)

---

## Web・本番稼働プロダクト

TypeScript · Svelte · Astro · Vue で構築した Web アプリを複数本番稼働中。
詳細 → [tsukasa-art.com/engineer](https://tsukasa-art.com/engineer)

---

## お問い合わせ

[tsukasa-art.com/contact](https://tsukasa-art.com/contact)
