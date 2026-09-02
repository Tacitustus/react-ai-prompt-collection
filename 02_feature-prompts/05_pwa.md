# PWA 化

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

vite-plugin-pwa を使ってプロジェクトを PWA（Progressive Web App）化する際に使用します。オフラインサポート、インストールプロンプト、プッシュ通知を実装します。

---

## プロンプト

````text
以下の仕様に従い、vite-plugin-pwa を導入してプロジェクトをPWA化してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 追加パッケージ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- vite-plugin-pwa
- workbox-window（Service Worker の登録・更新管理）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 実装する機能
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### 1. Service Worker
- Workbox を使用した自動生成
- キャッシュ戦略:
  - 静的アセット（JS/CSS/画像）: Cache First
  - API レスポンス: Network First（フォールバックキャッシュ）
  - フォント: Cache First（長期キャッシュ）
- バックグラウンド同期（オフライン時のリクエストをキューイング）

### 2. Web App Manifest
- アプリ名、短縮名
- テーマカラー、背景色
- アイコン（192x192, 512x512）
- display: standalone
- start_url: /
- スクリーンショット（オプション）

### 3. インストールプロンプト
- beforeinstallprompt イベントのハンドリング
- カスタムインストールバナー（organisms）
- 「ホーム画面に追加」ボタン
- 閉じた場合は一定期間表示しない（localStorage で制御）

### 4. アプリ更新通知
- Service Worker 更新検知時にトースト通知
- 「更新して再読み込み」ボタン
- skipWaiting + clients.claim で即座に更新

### 5. オフライン対応
- オフライン時のフォールバックページ
- ネットワーク状態の表示（useOnlineStatus フックと連携）
- オフライン時の操作制限

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── components/
│   ├── organisms/
│   │   ├── InstallPrompt.tsx     # インストールバナー
│   │   └── UpdateNotification.tsx # 更新通知
│   └── atoms/
│       └── OfflineBadge.tsx       # オフラインバッジ
├── hooks/
│   ├── usePWAInstall.ts          # インストールフック
│   ├── useServiceWorker.ts       # SW 更新管理フック
│   └── useOnlineStatus.ts        # ネットワーク状態フック
├── sw.ts                          # Service Worker カスタム設定（必要に応じて）
public/
├── manifest.webmanifest           # Web App Manifest（自動生成 or 手動）
├── icons/
│   ├── icon-192x192.png
│   ├── icon-512x512.png
│   └── maskable-icon-512x512.png
└── offline.html                   # オフラインフォールバックページ
vite.config.ts                     # VitePWA プラグイン設定
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ vite.config.ts の PWA 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
import { VitePWA } from 'vite-plugin-pwa';

export default defineConfig({
  plugins: [
    VitePWA({
      registerType: 'prompt',
      includeAssets: ['favicon.svg', 'icons/*.png'],
      manifest: {
        name: 'アプリ名',
        short_name: '短縮名',
        theme_color: '#3B82F6',
        background_color: '#0F172A',
        display: 'standalone',
        icons: [
          { src: '/icons/icon-192x192.png', sizes: '192x192', type: 'image/png' },
          { src: '/icons/icon-512x512.png', sizes: '512x512', type: 'image/png' },
          { src: '/icons/maskable-icon-512x512.png', sizes: '512x512', type: 'image/png', purpose: 'maskable' },
        ],
      },
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg,woff2}'],
        runtimeCaching: [
          {
            urlPattern: /^https:\/\/api\..*/i,
            handler: 'NetworkFirst',
            options: {
              cacheName: 'api-cache',
              expiration: { maxEntries: 100, maxAgeSeconds: 60 * 60 * 24 },
            },
          },
        ],
      },
    }),
  ],
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- AGENTS.md のコーディング規約をすべて遵守する
- any 禁止、アロー関数、JSDoc、日本語コメント

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
Lighthouse の PWA チェックも実施し、結果を報告すること。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| キャッシュ戦略 | API の更新頻度に合わせて調整 |
| プッシュ通知 | Firebase Cloud Messaging 等と連携する場合は追記 |
| アイコン | ブランドに合わせたアイコンを用意 |

---

## 関連プロンプト

- [カスタムフックライブラリ](../03_architecture-prompts/03_custom-hooks-library.md) — useOnlineStatus フック
- [トースト通知システム](./07_notification-toast.md) — 更新通知の表示
- [パフォーマンス監査](../04_quality-prompts/02_performance-audit.md) — Lighthouse PWA チェック
