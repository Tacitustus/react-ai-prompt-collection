# PR テンプレート＆レビュー規約

> **カテゴリ**: 開発ワークフロー
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★☆☆☆
> **所要時間目安**: 10〜20分

## このプロンプトの使い方

Pull Request のテンプレートとレビュー規約を整備する際に使用します。GitHub の PR テンプレート機能を活用して、一貫した PR を実現します。

---

## プロンプト

````text
以下の仕様に従い、PR テンプレートとレビュー規約を整備してください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 生成するファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### 1. `.github/pull_request_template.md`

```markdown
## 📝 概要

<!-- このPRで何を変更したかを簡潔に説明してください -->

## 🔗 関連 Issue

<!-- 関連する Issue の番号を記載してください -->
- Closes #

## 📋 変更内容

<!-- 変更内容を箇条書きで記載してください -->
-
-
-

## 🖼️ スクリーンショット

<!-- UIの変更がある場合はスクリーンショットを添付してください -->

| Before | After |
|--------|-------|
| | |

## ✅ チェックリスト

### コーディング規約
- [ ] TypeScript strict モードでエラーがない
- [ ] any 型を使用していない
- [ ] アロー関数のみ使用している
- [ ] JSDoc をコンポーネント・フック・ユーティリティに記述した
- [ ] 日本語コメントを処理単位で記述した
- [ ] アトミックデザインの階層に従って配置した

### 品質
- [ ] ESLint エラーが 0 件である（`npm run lint`）
- [ ] Prettier でフォーマット済みである
- [ ] TypeScript コンパイルエラーが 0 件である（`npx tsc --noEmit`）
- [ ] ビルドが成功する（`npm run build`）
- [ ] テストが通る（該当する場合）

### UI/UX
- [ ] レスポンシブ対応を確認した（モバイル / タブレット / デスクトップ）
- [ ] ダークモードで正しく表示される
- [ ] ローディング状態を確認した
- [ ] エラー状態を確認した
- [ ] アクセシビリティ（キーボード操作、ARIA）を確認した

### その他
- [ ] BLUEPRINT.md / AGENTS.md の規約に準拠している
- [ ] 破壊的変更がある場合は明記した
- [ ] ドキュメントを更新した（該当する場合）

## 📌 レビュアーへの注意事項

<!-- 特に見てほしいポイントや懸念事項があれば記載してください -->

## 🧪 テスト方法

<!-- このPRの変更を確認する手順を記載してください -->
1.
2.
3.
```

### 2. `.github/CODEOWNERS`

```
# コードオーナー設定
# 特定のディレクトリやファイルの変更に対して、自動的にレビュアーを割り当てる

# デフォルト: すべてのファイル
* @your-github-username

# 設定ファイル
/.github/ @your-github-username
*.config.* @your-github-username

# デザインシステム
/src/components/atoms/ @your-github-username
/src/styles/ @your-github-username
```

### 3. `.github/workflows/pr-check.yml`

PR 作成時に自動で品質チェックを実行する GitHub Actions:
```yaml
name: PR Check
on:
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npx tsc --noEmit
      - run: npm run build
      - run: npm run test:run -- --passWithNoTests
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ レビュー規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下のレビュー規約を CONTRIBUTING.md に追記すること：

### レビュー観点
1. **正確性**: 要件を満たしているか
2. **型安全性**: any の不使用、適切な型定義
3. **可読性**: コメント、命名、コード構造
4. **パフォーマンス**: 不要な再レンダリング、重い処理
5. **セキュリティ**: XSS、機密情報
6. **アクセシビリティ**: キーボード操作、ARIA
7. **テスト**: テストの網羅性

### レビューコメントのプレフィックス
- `[must]` — 必ず修正してください
- `[should]` — 修正を推奨します
- `[nit]` — 軽微な指摘（修正は任意）
- `[question]` — 質問・確認事項
- `[praise]` — 良い実装です

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い報告してください。
````

---

## 関連プロンプト

- [Gitブランチ戦略](./01_git-branching-strategy.md) — ブランチ命名規則
- [コードレビューチェックリスト](../04_quality-prompts/01_code-review-checklist.md) — レビュー観点の詳細
- [リリースプロセス](./03_release-process.md) — マージ後のリリースフロー
