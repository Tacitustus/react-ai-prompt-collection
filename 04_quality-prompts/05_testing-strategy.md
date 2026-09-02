# テスト戦略

> **カテゴリ**: 品質管理
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★☆
> **所要時間目安**: 45〜90分

## このプロンプトの使い方

プロジェクトのテスト基盤を構築し、テスト戦略を策定・実装する際に使用します。Vitest（単体/結合）+ Playwright（E2E）の構成です。

---

## プロンプト

````text
以下の仕様に従い、テスト基盤を構築し、テスト戦略に基づくテストを実装してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 追加パッケージ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- vitest
- @testing-library/react
- @testing-library/jest-dom
- @testing-library/user-event
- jsdom
- @playwright/test（E2E テスト用）
- msw（API モック）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── vitest.config.ts                # Vitest 設定
├── playwright.config.ts            # Playwright 設定
├── src/
│   ├── test/
│   │   ├── setup.ts                # テストセットアップ（jest-dom 拡張等）
│   │   ├── utils.tsx               # テストユーティリティ（カスタム render 等）
│   │   ├── mocks/
│   │   │   ├── handlers.ts         # MSW ハンドラー
│   │   │   └── server.ts           # MSW サーバー設定
│   │   └── fixtures/               # テストデータ
│   │       ├── users.ts
│   │       └── posts.ts
│   ├── components/
│   │   ├── atoms/
│   │   │   └── Button/
│   │   │       ├── Button.tsx
│   │   │       └── Button.test.tsx  # 単体テスト（コロケーション）
│   │   └── ...
│   └── hooks/
│       └── useDebounce.test.ts      # フックテスト
├── e2e/                             # E2E テスト
│   ├── auth.spec.ts
│   ├── navigation.spec.ts
│   └── fixtures/
│       └── test-data.ts
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ テストピラミッド
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| レベル | ツール | 対象 | カバレッジ目標 | 実行タイミング |
|--------|------|------|-------------|------------|
| 単体 | Vitest | ユーティリティ関数、カスタムフック | 80% | pre-commit |
| コンポーネント | Vitest + RTL | atoms, molecules | 70% | pre-commit |
| 結合 | Vitest + RTL + MSW | organisms, pages | 60% | CI |
| E2E | Playwright | 主要ユーザーフロー | 主要フロー 100% | CI (deploy前) |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. カスタム render 関数
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テストで共通して必要なプロバイダーをラップする:
```typescript
// test/utils.tsx
const AllProviders = ({ children }) => (
  <QueryClientProvider client={queryClient}>
    <BrowserRouter>
      {children}
    </BrowserRouter>
  </QueryClientProvider>
);

const customRender = (ui, options) =>
  render(ui, { wrapper: AllProviders, ...options });
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. 単体テストのパターン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ユーティリティ関数
```typescript
describe('formatDate', () => {
  it('ISO文字列を日本語形式に変換する', () => { ... });
  it('null の場合は空文字を返す', () => { ... });
  it('不正な日付文字列の場合はエラーをスローする', () => { ... });
});
```

### カスタムフック
```typescript
describe('useDebounce', () => {
  it('指定した遅延後に値が更新される', async () => { ... });
  it('遅延中に値が変わった場合はリセットされる', () => { ... });
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. コンポーネントテストのパターン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
describe('Button', () => {
  it('正しくレンダリングされる', () => { ... });
  it('クリックイベントが発火する', async () => { ... });
  it('disabled の場合はクリックできない', async () => { ... });
  it('loading の場合はスピナーが表示される', () => { ... });
  it('各 variant で正しいスタイルが適用される', () => { ... });
  it('アクセシビリティ属性が正しく設定される', () => { ... });
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. E2E テストのパターン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
// e2e/auth.spec.ts
test.describe('認証フロー', () => {
  test('ログインが成功する', async ({ page }) => {
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'user@example.com');
    await page.fill('[data-testid="password"]', 'password123');
    await page.click('[data-testid="login-button"]');
    await expect(page).toHaveURL('/dashboard');
  });
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. package.json スクリプト
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage",
    "test:ui": "vitest --ui",
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui"
  }
}
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 6. CI 統合
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GitHub Actions に以下を追加:
- 単体/結合テスト: npm run test:run
- カバレッジレポート: npm run test:coverage
- E2E テスト: npx playwright test

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- テストファイルは対象ファイルと同じディレクトリに配置（コロケーション）
- describe でグルーピング、it/test で個別ケース
- テスト名は日本語で記述
- data-testid を使用して要素を特定（E2E テスト用）
- any 禁止、アロー関数

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
テスト実行結果（通過/失敗件数、カバレッジ）も報告すること。
````

---

## 関連プロンプト

- [コードレビューチェックリスト](./01_code-review-checklist.md) — テスト品質のレビュー
- [API通信レイヤー設計](../03_architecture-prompts/02_api-layer.md) — MSW によるモック
