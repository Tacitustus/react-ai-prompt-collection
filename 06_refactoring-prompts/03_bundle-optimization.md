# バンドルサイズ最適化

> **カテゴリ**: リファクタリング
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 20〜40分

## このプロンプトの使い方

ビルドサイズが大きくなった場合に、バンドルサイズの分析と最適化を行う際に使用します。

---

## プロンプト

````text
以下の手順に従い、プロジェクトのバンドルサイズを分析・最適化してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. バンドル分析ツールの導入
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
rollup-plugin-visualizer を devDependencies に追加し、vite.config.ts に設定する:
```typescript
import { visualizer } from 'rollup-plugin-visualizer';

export default defineConfig({
  plugins: [
    visualizer({
      open: true,
      filename: 'bundle-analysis.html',
      gzipSize: true,
      brotliSize: true,
    }),
  ],
});
```

npm run build を実行し、bundle-analysis.html を確認する。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. 分析と改善
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
バンドル分析の結果に基づき、以下の観点で改善する：

### 大きな依存関係の軽量化
| ライブラリ | 代替候補 | サイズ比較 |
|----------|---------|-----------|
| moment.js | dayjs / date-fns | 230KB → 2KB / 6KB |
| lodash (全体) | lodash-es (個別) | 70KB → 必要分のみ |
| axios | ky / ofetch | 30KB → 10KB |
| chart.js | recharts (部分インポート) | 60KB → 必要分のみ |

### Tree-shaking の確認
- named export が使われているか（default export は Tree-shaking しにくい）
- sideEffects: false が package.json に設定されているか
- 不要なポリフィルが含まれていないか

### 不要な依存関係の除去
- npm ls --depth=0 で依存関係一覧を確認
- 使用していないパッケージを npm uninstall で削除
- dependencies と devDependencies の分類が正しいか確認

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. 動的インポート
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
初回ロードに不要な機能を動的インポートに切り替える:
- ページコンポーネント → React.lazy
- 重いライブラリ → dynamic import()
- 管理者専用機能 → 条件付きロード

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. Vite ビルド設定の最適化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
          ui: ['lucide-react', 'clsx', 'tailwind-merge'],
          query: ['@tanstack/react-query'],
        },
      },
    },
    target: 'esnext',
    minify: 'esbuild',
    sourcemap: false,       // 本番ではソースマップ無効
    cssCodeSplit: true,     // CSS コード分割
    chunkSizeWarningLimit: 500,
  },
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. TailwindCSS の最適化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- content 設定が正しいか確認（未使用のクラスが PurgeCSS で除去されるか）
- 大量の safelist が設定されていないか確認

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 報告形式
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
## 📦 バンドルサイズ最適化結果

### 最適化前
| チャンク | サイズ (gzip) |
|---------|-------------|
| 合計 | XXX KB |

### 最適化後
| チャンク | サイズ (gzip) | 削減率 |
|---------|-------------|--------|
| 合計 | XXX KB | -XX% |

### 実施した施策
1. （施策と効果）
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い報告してください。
````

---

## 関連プロンプト

- [パフォーマンス監査](../04_quality-prompts/02_performance-audit.md) — パフォーマンス全般
- [パフォーマンス最適化](./02_performance-optimization.md) — React 固有の最適化
