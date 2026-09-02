# データ取得レイヤー（TanStack Query）

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

TanStack Query (React Query) を導入し、サーバー状態管理を効率的に行うためのデータ取得レイヤーを構築する際に使用します。

---

## プロンプト

````text
以下の仕様に従い、TanStack Query によるデータ取得レイヤーを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 追加パッケージ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- @tanstack/react-query
- @tanstack/react-query-devtools

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── lib/
│   └── queryClient.ts           # QueryClient の設定
├── hooks/
│   └── api/
│       ├── queryKeys.ts          # クエリキーの一元管理
│       ├── useResourceQuery.ts   # リソース取得クエリ（テンプレート）
│       ├── useResourceMutation.ts# リソース更新ミューテーション（テンプレート）
│       └── index.ts
├── providers/
│   └── QueryProvider.tsx         # QueryClientProvider ラッパー
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. QueryClient 設定 (queryClient.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000,        // 5分間はキャッシュを新鮮とみなす
      gcTime: 10 * 60 * 1000,           // 10分間キャッシュを保持
      retry: 3,                          // 最大3回リトライ
      retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
      refetchOnWindowFocus: false,       // ウィンドウフォーカス時の再取得を無効
      throwOnError: false,               // エラーを ErrorBoundary に投げない
    },
    mutations: {
      retry: 1,
    },
  },
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. クエリキー管理 (queryKeys.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
クエリキーを型安全に一元管理する:

```typescript
export const queryKeys = {
  users: {
    all: ['users'] as const,
    lists: () => [...queryKeys.users.all, 'list'] as const,
    list: (params: UserListParams) => [...queryKeys.users.lists(), params] as const,
    details: () => [...queryKeys.users.all, 'detail'] as const,
    detail: (id: string) => [...queryKeys.users.details(), id] as const,
  },
  posts: {
    all: ['posts'] as const,
    lists: () => [...queryKeys.posts.all, 'list'] as const,
    list: (params: PostListParams) => [...queryKeys.posts.lists(), params] as const,
    details: () => [...queryKeys.posts.all, 'detail'] as const,
    detail: (id: string) => [...queryKeys.posts.details(), id] as const,
  },
} as const;
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. カスタムクエリフック（テンプレート）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下のパターンでリソースごとのフックを作成すること:

### 一覧取得（ページネーション付き）
- useQuery でデータ取得
- keepPreviousData でページ遷移時のちらつきを防止
- クエリパラメータをクエリキーに含める

### 詳細取得
- useQuery + enabled オプションで条件付き実行
- プリフェッチ対応（一覧のホバー時に詳細をプリフェッチ）

### 作成ミューテーション
- useMutation + 楽観的更新（Optimistic Update）
- 成功時: キャッシュの無効化 + トースト通知
- 失敗時: ロールバック + エラートースト

### 更新ミューテーション
- useMutation + 楽観的更新
- 成功時: 該当キャッシュの更新 + トースト通知

### 削除ミューテーション
- useMutation + 楽観的更新（一覧からの即座削除）
- 確認ダイアログとの連携
- 成功時: キャッシュの無効化 + トースト通知

### 無限スクロール
- useInfiniteQuery でページネーション
- getNextPageParam でカーソルベースまたはオフセットベースに対応

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. ローディング/エラー UI パターン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TanStack Query の状態に応じた UI コンポーネントを用意:

| 状態 | UI |
|------|-----|
| isLoading (初回) | スケルトンUI |
| isFetching (バックグラウンド) | 右上にスピナー（データは表示維持） |
| isError | エラーメッセージ + リトライボタン |
| data が空 | 空状態コンポーネント |
| isSuccess | データ表示 |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. DevTools
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 開発環境でのみ ReactQueryDevtools を表示
- initialIsOpen: false で初期状態は閉じておく

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- AGENTS.md のコーディング規約をすべて遵守する
- any 禁止、レスポンス型は必ず明示
- すべてアロー関数
- JSDoc 必須（@description, @param, @returns, @example）
- 日本語コメント

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| staleTime / gcTime | データの更新頻度に合わせて調整 |
| 楽観的更新 | 厳密な整合性が必要な場合は無効化 |
| DevTools | 本番では完全に除外 |
| エラーリトライ | リトライ回数・間隔の調整 |

---

## 関連プロンプト

- [API通信レイヤー設計](../03_architecture-prompts/02_api-layer.md) — API クライアントの設計
- [無限スクロール＆ページネーション](./09_infinite-scroll-pagination.md) — useInfiniteQuery の活用
- [エラーハンドリング](./14_error-handling.md) — エラー処理パターン
