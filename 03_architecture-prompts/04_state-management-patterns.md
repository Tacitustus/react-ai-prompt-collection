# 状態管理パターン集

> **カテゴリ**: アーキテクチャ設計
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

プロジェクトの状態管理戦略を決定し、適切なパターンを実装する際に使用します。AIエージェントに渡すことで、プロジェクトに最適な状態管理のストア設計・実装を行えます。

---

## プロンプト

````text
以下の仕様に従い、プロジェクトの状態管理を設計・実装してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 状態の分類と管理方法
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

状態は以下の5種類に分類し、それぞれ適切な手法で管理する：

| 分類 | 管理方法 | 例 |
|------|---------|-----|
| サーバー状態 | TanStack Query | API レスポンスデータ（ユーザー一覧、投稿一覧等） |
| グローバルUI状態 | Zustand | テーマ、認証状態、サイドバー開閉、通知 |
| ローカルUI状態 | useState / useReducer | フォーム入力値、モーダル開閉、タブ選択 |
| URL状態 | React Router (useSearchParams) | ページ番号、検索キーワード、フィルター条件 |
| 永続化状態 | Zustand + persist ミドルウェア | テーマ設定、言語設定、同意状態 |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/stores/
├── useThemeStore.ts       # テーマ管理（永続化あり）
├── useAuthStore.ts        # 認証状態管理
├── useUIStore.ts          # グローバルUI状態（サイドバー、モーダル等）
├── useNotificationStore.ts # トースト通知管理
└── index.ts               # 一括エクスポート
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. テーマストア (useThemeStore.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
interface ThemeStore {
  /** 現在のテーマ */
  theme: 'light' | 'dark' | 'system';
  /** 実効テーマ（system の場合は OS 設定から解決） */
  resolvedTheme: 'light' | 'dark';
  /** テーマを変更する */
  setTheme: (theme: ThemeStore['theme']) => void;
}
```
- `persist` ミドルウェアで localStorage に永続化
- `system` 選択時は `matchMedia('(prefers-color-scheme: dark)')` を監視
- HTML の `<html>` 要素に `dark` クラスを付与/削除

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. 認証ストア (useAuthStore.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
interface AuthStore {
  /** 現在のユーザー情報 */
  user: User | null;
  /** アクセストークン */
  accessToken: string | null;
  /** リフレッシュトークン */
  refreshToken: string | null;
  /** 認証済みかどうか */
  isAuthenticated: boolean;
  /** 認証状態の初期化中かどうか */
  isInitializing: boolean;
  /** ログイン処理 */
  login: (response: LoginResponse) => void;
  /** ログアウト処理 */
  logout: () => void;
  /** トークン更新 */
  updateToken: (accessToken: string) => void;
  /** 認証状態の初期化 */
  initialize: () => Promise<void>;
}
```
- `persist` ミドルウェアでトークンを安全に保存
- `partialize` でストアの一部のみ永続化（user と token のみ）
- `initialize` でアプリ起動時にトークンの有効性を確認

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. UI ストア (useUIStore.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
interface UIStore {
  /** サイドバーの開閉状態 */
  isSidebarOpen: boolean;
  /** サイドバーの開閉を切り替え */
  toggleSidebar: () => void;
  /** グローバルローディング状態 */
  isGlobalLoading: boolean;
  /** グローバルローディングを設定 */
  setGlobalLoading: (loading: boolean) => void;
  /** コマンドパレットの開閉状態 */
  isCommandPaletteOpen: boolean;
  /** コマンドパレットの開閉を切り替え */
  toggleCommandPalette: () => void;
}
```
- 永続化不要（セッション内のみ有効）
- モバイルでは初期状態でサイドバーを閉じる

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. 通知ストア (useNotificationStore.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
type NotificationType = 'success' | 'error' | 'warning' | 'info';

interface Notification {
  id: string;
  type: NotificationType;
  title: string;
  message?: string;
  duration?: number;
}

interface NotificationStore {
  notifications: Notification[];
  addNotification: (notification: Omit<Notification, 'id'>) => void;
  removeNotification: (id: string) => void;
  clearAll: () => void;
}
```
- ID は nanoid で自動生成
- duration（デフォルト: 5000ms）経過後に自動削除
- 最大表示数を制限（例: 5件）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. Zustand ベストプラクティス
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下のベストプラクティスに従って実装すること：

### Selector パターン
- 不要な再レンダリングを防ぐため、必要な値のみを selector で取得する
- `useShallow` を活用してオブジェクトの浅い比較を行う

```typescript
// ❌ 悪い例: ストア全体をサブスクライブ
const store = useThemeStore();

// ✅ 良い例: 必要な値のみをサブスクライブ
const theme = useThemeStore((state) => state.theme);
const setTheme = useThemeStore((state) => state.setTheme);
```

### ストア分割
- 責務ごとにストアを分割する（1ストア = 1責務）
- ストア間の依存は最小限にする

### ミドルウェア
- `devtools`: 開発時のデバッグ（Redux DevTools 連携）
- `persist`: 永続化が必要なストアのみ
- `immer`: ネストが深い状態の更新時（オプション）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 6. URL 状態管理
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
検索パラメータやフィルター条件は URL に保存し、ブックマーク・共有可能にする。

```typescript
// カスタムフック: useQueryParams
// - useSearchParams のラッパー
// - 型安全なクエリパラメータの取得・設定
// - デフォルト値の指定
// - 配列パラメータの対応
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- any 禁止。ストアの型を interface で定義する
- すべてアロー関数
- JSDoc 必須
- 日本語コメントを処理単位で記述
- ストア作成関数には create の型引数を明示する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| ストア追加 | カート、お気に入り等のドメイン固有ストアを追加 |
| 永続化先 | localStorage 以外（IndexedDB、Cookie）に変更可能 |
| ミドルウェア | immer を追加してイミュータブル更新を簡素化 |

---

## 関連プロンプト

- [技術設計書テンプレート](../05_specification-templates/02_technical-design.md) — 状態管理設計の全体像
- [ダークモード切り替え](../02_feature-prompts/06_dark-mode.md) — テーマストアの実装
- [認証機能](../02_feature-prompts/01_authentication.md) — 認証ストアの実装
- [トースト通知システム](../02_feature-prompts/07_notification-toast.md) — 通知ストアの実装
