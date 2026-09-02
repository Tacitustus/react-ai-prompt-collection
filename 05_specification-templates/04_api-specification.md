# API 仕様書テンプレート

> **カテゴリ**: 仕様書テンプレート
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 20〜40分

## このプロンプトの使い方

バックエンドAPIと連携するフロントエンドを開発する際に、API仕様を明文化するために使用します。バックエンドのAPIドキュメント（Swagger/OpenAPI等）がある場合はそれを参照して渡すと、より正確な仕様書が生成されます。

---

## プロンプト

````text
以下のテンプレートに従い、このプロジェクトで使用するAPIの仕様書を作成してください。
API_SPEC.md として保存してください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ API 仕様書テンプレート
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```markdown
# API 仕様書

## 1. 概要

### 1.1 ベース情報

| 項目 | 内容 |
|------|------|
| ベースURL（開発） | `http://localhost:3001/api/v1` |
| ベースURL（本番） | `https://api.example.com/v1` |
| 環境変数 | `VITE_API_BASE_URL` |
| 通信プロトコル | HTTPS (本番) / HTTP (開発) |
| データ形式 | JSON |
| 文字コード | UTF-8 |

### 1.2 認証方式

| 方式 | 詳細 |
|------|------|
| 種別 | Bearer Token (JWT) |
| ヘッダー | `Authorization: Bearer <token>` |
| トークン取得 | POST /auth/login |
| トークン有効期限 | 1時間 |
| リフレッシュ | POST /auth/refresh |

### 1.3 共通ヘッダー

| ヘッダー | 値 | 必須 |
|---------|-----|------|
| Content-Type | application/json | ✅ |
| Authorization | Bearer \<token\> | 認証が必要なエンドポイントのみ |
| Accept-Language | ja / en | ❌（デフォルト: ja） |

---

## 2. 共通レスポンス形式

### 2.1 成功レスポンス

```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

### 2.2 エラーレスポンス

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "入力内容に誤りがあります",
    "details": [
      {
        "field": "email",
        "message": "有効なメールアドレスを入力してください"
      }
    ]
  }
}
```

### 2.3 ステータスコード

| コード | 意味 | フロントエンド対応 |
|--------|------|-----------------|
| 200 | 成功 | データ表示 |
| 201 | 作成成功 | 成功トースト + リダイレクト |
| 204 | 削除成功 | 成功トースト + リスト更新 |
| 400 | リクエスト不正 | エラートースト |
| 401 | 認証エラー | ログイン画面へリダイレクト |
| 403 | 権限不足 | 403エラーページ |
| 404 | リソース未検出 | 404エラーページ |
| 422 | バリデーションエラー | フォームにエラー表示 |
| 429 | レート制限超過 | リトライ（Retry-After ヘッダー参照） |
| 500 | サーバーエラー | エラートースト + リトライボタン |

---

## 3. エンドポイント詳細

### 3.1 認証 (Auth)

#### POST /auth/login

ユーザーログイン

**リクエスト**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**レスポンス（200）**
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "dGhpcyBpcyBhIHJlZnJl...",
    "expiresIn": 3600,
    "user": {
      "id": "usr_abc123",
      "email": "user@example.com",
      "name": "田中太郎",
      "avatarUrl": "https://..."
    }
  }
}
```

**エラー（401）**
```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "メールアドレスまたはパスワードが正しくありません"
  }
}
```

**TypeScript 型定義**
```typescript
/** ログインリクエストの型定義 */
interface LoginRequest {
  email: string;
  password: string;
}

/** ログインレスポンスの型定義 */
interface LoginResponse {
  accessToken: string;
  refreshToken: string;
  expiresIn: number;
  user: User;
}
```

---

#### POST /auth/register

新規ユーザー登録

**リクエスト**
```json
{
  "email": "user@example.com",
  "password": "password123",
  "name": "田中太郎"
}
```

**レスポンス（201）**
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "dGhpcyBpcyBhIHJlZnJl...",
    "expiresIn": 3600,
    "user": {
      "id": "usr_abc123",
      "email": "user@example.com",
      "name": "田中太郎",
      "avatarUrl": null
    }
  }
}
```

---

#### POST /auth/refresh

トークンリフレッシュ

**リクエスト**
```json
{
  "refreshToken": "dGhpcyBpcyBhIHJlZnJl..."
}
```

**レスポンス（200）**
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "expiresIn": 3600
  }
}
```

---

#### POST /auth/logout

ログアウト（認証必要）

**レスポンス（204）**: No Content

---

### 3.2 ユーザー (Users)

#### GET /users/me

自分のプロフィール取得（認証必要）

**レスポンス（200）**
```json
{
  "success": true,
  "data": {
    "id": "usr_abc123",
    "email": "user@example.com",
    "name": "田中太郎",
    "avatarUrl": "https://...",
    "createdAt": "2025-01-01T00:00:00Z",
    "updatedAt": "2025-06-01T12:00:00Z"
  }
}
```

---

### 3.3 リソース (Resources) — テンプレート

#### GET /resources

リソース一覧取得

**クエリパラメータ**

| パラメータ | 型 | 必須 | デフォルト | 説明 |
|----------|-----|------|-----------|------|
| page | number | ❌ | 1 | ページ番号 |
| perPage | number | ❌ | 20 | 1ページあたりの件数 |
| sort | string | ❌ | createdAt | ソートフィールド |
| order | 'asc' \| 'desc' | ❌ | desc | ソート順 |
| search | string | ❌ | - | 検索キーワード |
| status | string | ❌ | - | ステータスフィルター |

**レスポンス（200）**
```json
{
  "success": true,
  "data": [
    {
      "id": "res_001",
      "title": "サンプルリソース",
      "description": "説明文",
      "status": "active",
      "createdAt": "2025-01-01T00:00:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

---

#### GET /resources/:id

リソース詳細取得

**パスパラメータ**

| パラメータ | 型 | 説明 |
|----------|-----|------|
| id | string | リソースID |

---

#### POST /resources

リソース新規作成（認証必要）

**リクエスト**
```json
{
  "title": "新しいリソース",
  "description": "説明文"
}
```

**レスポンス（201）**

---

#### PATCH /resources/:id

リソース更新（認証必要）

**リクエスト**
```json
{
  "title": "更新後のタイトル"
}
```

**レスポンス（200）**

---

#### DELETE /resources/:id

リソース削除（認証必要）

**レスポンス（204）**: No Content

---

## 4. フロントエンド型定義一覧

```typescript
// === 共通型 ===

/** APIの共通レスポンス型 */
interface ApiResponse<T> {
  success: boolean;
  data: T;
  meta?: PaginationMeta;
}

/** ページネーションメタ情報 */
interface PaginationMeta {
  page: number;
  perPage: number;
  total: number;
  totalPages: number;
}

/** APIエラーレスポンス */
interface ApiError {
  code: string;
  message: string;
  details?: ApiErrorDetail[];
}

/** バリデーションエラー詳細 */
interface ApiErrorDetail {
  field: string;
  message: string;
}

// === ドメイン型 ===

/** ユーザー */
interface User {
  id: string;
  email: string;
  name: string;
  avatarUrl: string | null;
  createdAt: string;
  updatedAt: string;
}

/** リソース */
interface Resource {
  id: string;
  title: string;
  description: string;
  status: 'active' | 'inactive' | 'draft';
  createdAt: string;
}
```

---

## 5. レート制限

| エンドポイント | 制限 | ウィンドウ |
|-------------|------|-----------|
| POST /auth/login | 5回 | 1分 |
| POST /auth/register | 3回 | 1分 |
| その他 | 100回 | 1分 |

---

## 6. 更新履歴

| 日付 | 版 | 更新者 | 内容 |
|------|-----|--------|------|
| YYYY-MM-DD | 1.0 | | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 作成時の注意事項
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- すべてのエンドポイントにリクエスト/レスポンス例を含めること
- TypeScript の型定義を必ず含めること（any 禁止）
- エラーレスポンスのパターンをすべて列挙すること
- フロントエンド側の対応方針（どの画面でどう表示するか）も記述すること
- ページネーション対応のエンドポイントはクエリパラメータを明示すること
- 完成後、AGENTS.md の規約に照らし合わせてセルフチェックを行い、結果を報告する
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| 認証方式 | JWT以外（Cookie、OAuth等）を使う場合は変更 |
| レスポンス形式 | バックエンドの形式に合わせて調整 |
| エンドポイント | 実際のリソースに合わせてセクション 3 を追加 |
| レート制限 | バックエンドの設定に合わせて調整 |

---

## 関連プロンプト

- [技術設計書テンプレート](./02_technical-design.md) — API設計の全体像
- [データ取得レイヤー](../02_feature-prompts/03_data-fetching.md) — TanStack Query による実装
- [API通信レイヤー設計](../03_architecture-prompts/02_api-layer.md) — axios/fetch ラッパーの実装
- [エラーハンドリング](../02_feature-prompts/14_error-handling.md) — エラー処理の実装
