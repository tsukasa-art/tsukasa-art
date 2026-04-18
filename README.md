# tsukasa

イラストレーター / Web エンジニア。
ポートフォリオサイトや業務系 Web アプリを設計・開発・運用しています。VPS でのサーバー構築から認証・DB 設計まで手がけており、デザインと実装の両面からプロダクトを作ります。

技術スタックとプロジェクトの説明は下記のとおりです。より詳しい紹介はポートフォリオの [Engineer](https://tsukasa-art.com/about/engineer) にまとめています。

---

## スキル

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Astro](https://img.shields.io/badge/Astro-FF5D01?style=flat-square&logo=astro&logoColor=white)
![Svelte](https://img.shields.io/badge/Svelte-FF3E00?style=flat-square&logo=svelte&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=next.js&logoColor=white)
![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![SolidJS](https://img.shields.io/badge/SolidJS-2C4F7C?style=flat-square&logo=solid&logoColor=white)
![Tauri](https://img.shields.io/badge/Tauri-24C8D8?style=flat-square&logo=tauri&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white)
![Zig](https://img.shields.io/badge/Zig-F7A41D?style=flat-square&logo=zig&logoColor=white)
![C](https://img.shields.io/badge/C-00599C?style=flat-square&logo=c&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat-square&logo=tailwind-css&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-1572B6?style=flat-square&logo=css3&logoColor=white)
![Sass](https://img.shields.io/badge/Sass-CC6699?style=flat-square&logo=sass&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=flat-square&logo=cloudflare&logoColor=white)
![Bun](https://img.shields.io/badge/Bun-000000?style=flat-square&logo=bun&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=flat-square&logo=vitest&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![Sentry](https://img.shields.io/badge/Sentry-362D59?style=flat-square&logo=sentry&logoColor=white)

- **フロント**: TypeScript, JavaScript, Astro, Svelte 5, SolidJS, React, Next.js, Vue.js, CSS, SCSS, Tailwind CSS
- **バック・データ**: Node.js, Bun, PostgreSQL, Drizzle ORM, Zod, Supabase, IndexedDB
- **インフラ**: VPS (Linux), Podman Compose, Nginx, Cloudflare（CDN / R2）
- **認証**: カスタムセッション・パスワードハッシュ（公開プロフィールではライブラリ名を割愛）
- **監視・テスト**: Sentry, Vitest, Playwright
- **ツール**: Git, Bun, rclone（B2 オフサイトバックアップ）
- **デスクトップ（個人）**: Tauri 2, Rust（画像解析・ファイル I/O など）
- **ネイティブ・画像（npm）**: Zig, C — 自作 OSS [**zigpix**](https://www.npmjs.com/package/zigpix) のコア実装。デコード・リサイズ・AVIF / WebP エンコードなどを Node.js / Bun / Deno（ネイティブ FFI）から利用可能。ブラウザ向けは [**zigpix-wasm**](https://www.npmjs.com/package/zigpix-wasm)（WebAssembly）

---

## 主な成果物

| 成果物                   | 説明                                                                              | リンク                                                                                                               |
| ------------------------ | --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **HP**                   | Astro + Svelte 5, VPS 自前運用（本番 Bun）, 作品管理・カスタム認証・Sentry 監視・Cloudflare R2・B2 オフサイトバックアップ。画像トランスコードは自作 zigpix | [tsukasa-art.com](https://tsukasa-art.com)                                                                           |
| **zigpix**               | 自作 npm 画像処理ライブラリ。Zig/C のネイティブ実装を Node.js / Bun / Deno（FFI）から利用し、デコード・リサイズ・AVIF/WebP エンコードなどを提供（ブラウザ向けは zigpix-wasm） | [npm](https://www.npmjs.com/package/zigpix) · [GitHub](https://github.com/Tuki-Sana/zig-pix)                         |
| **サロンレジデモ**       | 美容室向け料金計算レジの**公開デモ**。本番（Svelte 5）相当の UX を **Vanilla JS** で再実装。Vite, Tailwind CSS, IndexedDB, PWA, 日次レポート | [リポジトリ](https://github.com/Tuki-Sana/salon-register-demo) · [デモ](https://salon-register-demo.pages.dev/login) |
| **画像メタデータ解析**   | 画像を外部に送らずローカルで色・EXIF・配色指標などを表示。Tauri 2 + Vue 3 + TypeScript、解析は Rust。用語集・技術ドキュメント・GitHub Releases（macOS Apple Silicon 向け DMG） | [リポジトリ](https://github.com/Tuki-Sana/image-data-analyzer) · [Releases](https://github.com/Tuki-Sana/image-data-analyzer/releases) |
| **JAN Sync**             | 小売棚前向け JAN 管理 PWA。スキャン・バーコード一括生成・横断検索。**CSV/TSV エクスポート**（列プリセット・区切り・個数の行展開・表計算向け JAN 列）。連続スキャン・棚卸しモード・読取時のバイブ/短音。Vitest で出力ロジックを検証。SolidJS, TypeScript, Tailwind v4, IndexedDB, Cloudflare Pages, PWA（Workbox） | [リポジトリ](https://github.com/Tuki-Sana/jan-sync) · [アプリ](https://jan-sync.pages.dev/) |
| **テトリス風ゲーム**     | Vanilla JS + Canvas。難易度・HOLD・BGM切替・ライト/ダークテーマ・PWA。PC/モバイル | [リポジトリ](https://github.com/Tuki-Sana/tet-js) · [プレイ](https://tuki-sana.github.io/tet-js/)                    |

---

## 連絡

お問い合わせは [ポートフォリオのお問い合わせフォーム](https://tsukasa-art.com/contact) からお願いします。
