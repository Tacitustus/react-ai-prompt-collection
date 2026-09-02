# API 通信レイヤー設計

> **カテゴリ**: アーキテクチャ設計
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

バックエンドAPIと通信するための共通レイヤーを構築する際に使用します。axios または fetch のラッパーを作成し、認証トークンの自動付与、エラーハンドリング、リトライロジック等を一元管理します。

---

## プロンプト

````text
以下の仕様に従い、API 通信レイヤーを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── services/
│   ├── api/
│   │   ├── client.ts          # API クライアント（axios インスタンス）
│   │   ├── interceptors.ts    # リクエスト/レスポンス インターセプター
│   │   ├── endpoints.ts       # エンドポイント定数定義
│   │   └── index.ts           # エクスポート
│   ├── auth.service.ts        # 認証関連 API
│   ├── user.service.ts        # ユーザー関連 API
│   └── index.ts               # サービス一括エクスポート
├── types/
│   ├── api.ts                 # API 共通型（レスポンス、エラー、ページネーション）
│   └── models/                # ドメインモデル型
│       ├── User.ts
│       └── index.ts
└── hooks/
    └── api/                   # API 関連カスタムフック（TanStack Query ラッパー）
        ├── useAuth.ts
        ├── useUsers.ts
        └── index.ts
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. API クライアント (client.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- axios を使用した API クライアントインスタンスを作成する
- ベースURL は環境変数 `VITE_API_BASE_URL` から取得する
- タイムアウト: 10秒
- デフォルトヘッダー: Content-Type: application/json

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. インターセプター (interceptors.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
### リクエストインターセプター
- 認証トークン（Bearer Token）を Authorization ヘッダーに自動付与
- トークンは Zustand ストアから取得する
- リクエストログの出力（開発環境のみ）

### レスポンスインターセプター
- 401 エラー時: トークンリフレッシュを試行 → 失敗時はログアウト + ログイン画面へリダイレクト
- 403 エラー時: 権限エラーとしてハンドリング
- 500 エラー時: エラートースト通知
- ネットワークエラー時: オフライン状態の通知
- レスポンスログの出力（開発環境のみ）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. エンドポイント定数 (endpoints.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
export const API_ENDPOINTS = {
  AUTH: {
    LOGIN: '/auth/login',
    REGISTER: '/auth/register',
    REFRESH: '/auth/refresh',
    LOGOUT: '/auth/logout',
  },
  USERS: {
    ME: '/users/me',
    BY_ID: (id: string) => `/users/${id}`,
  },
  // ...リソースごとに追加
} as const;
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. 共通型定義 (types/api.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- `ApiResponse<T>`: 成功レスポンスの汎用型
- `ApiError`: エラーレスポンス型
- `PaginationMeta`: ページネーションメタ情報
- `PaginatedResponse<T>`: ページネーション付きレスポンス
- `QueryParams`: 共通クエリパラメータ型（page, perPage, sort, order, search）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. サービス関数
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
各リソースごとにサービス関数を作成する。例:

```typescript
// auth.service.ts
export const authService = {
  login: (credentials: LoginRequest): Promise<ApiResponse<LoginResponse>> => ...
  register: (data: RegisterRequest): Promise<ApiResponse<LoginResponse>> => ...
  refresh: (refreshToken: string): Promise<ApiResponse<RefreshResponse>> => ...
  logout: (): Promise<void> => ...
};
```

- すべてのサービス関数に JSDoc を記述する
- リクエスト型とレスポンス型を明示する（any 禁止）
- エラーは try-catch せず、呼び出し元（TanStack Query のフック）に委譲する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 6. TanStack Query フック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
各サービス関数に対応する TanStack Query のカスタムフックを作成する：
- `useLogin`: ログインミューテーション
- `useRegister`: 登録ミューテーション
- `useCurrentUser`: 現在のユーザー情報クエリ
- クエリキーは定数化して管理する（queryKeys.ts）
- staleTime, gcTime を適切に設定する
- onSuccess, onError コールバックでトースト通知を行う

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 7. モック (開発用)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 環境変数 `VITE_USE_MOCK=true` の場合に MSW (Mock Service Worker) を使用する
- 最低限のモックハンドラーを用意する（ログイン、ユーザー取得）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- any 禁止。レスポンス型は必ず明示的に定義する
- すべてアロー関数
- JSDoc 必須（@description, @param, @returns）
- 日本語コメントを処理単位で記述
- エラー型は unknown で受けて型ガードで処理する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| HTTP ライブラリ | axios 以外（ky, ofetch）に変更可能 |
| 認証方式 | Cookie 認証の場合はインターセプターを変更 |
| モック | MSW 以外（json-server, Mirage.js）に変更可能 |
| リトライ | TanStack Query の retry 設定を調整 |

---

## 関連プロンプト

- [API仕様書テンプレート](../05_specification-templates/04_api-specification.md) — API仕様の定義
- [データ取得レイヤー](../02_feature-prompts/03_data-fetching.md) — TanStack Query の詳細設定
- [認証機能](../02_feature-prompts/01_authentication.md) — 認証フローの実装
- [エラーハンドリング](../02_feature-prompts/14_error-handling.md) — エラー処理の実装
