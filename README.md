# 🚀 React AI Prompt Collection

**React + TypeScript + TailwindCSS** によるWebアプリ開発を **AIエージェント（Antigravity, Cursor, GitHub Copilot 等）** で効率的に行うための、プロンプト・仕様書テンプレートのコレクションです。

---

## 📖 このリポジトリについて

Webアプリ開発のあらゆるフェーズで使える **コピペ即利用可能** なプロンプトと仕様書テンプレートを収録しています。

### 想定する技術スタック

| 技術 | バージョン |
|------|-----------|
| React | 18+ |
| TypeScript | 5+ (strict mode) |
| Vite | 最新安定版 |
| TailwindCSS | v3+ |
| React Router | v6+ |
| Zustand | 最新安定版 |

### 想定するコーディング規約

- **TypeScript strict** モード必須、`any` 禁止
- **アロー関数** のみ使用
- **JSDoc** をコンポーネント・フック・ユーティリティに必須記述
- **日本語コメント** を処理単位で細かく記述
- **アトミックデザイン**（atoms / molecules / organisms / templates / pages）

---

## 📁 コレクション一覧

### 1. 🏗️ [プロジェクトテンプレート](./01_project-templates/)

新規プロジェクトの雛形をデプロイ先ごとに構築するプロンプト。

| ファイル | 内容 |
|---------|------|
| [deploy-templates.md](./01_project-templates/deploy-templates.md) | GitHub Pages / Vercel / Netlify / Cloudflare Pages / Firebase Hosting 向けテンプレート |

---

### 2. ✨ [機能追加プロンプト](./02_feature-prompts/)

既存プロジェクトに機能を追加する際のプロンプト集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [authentication.md](./02_feature-prompts/01_authentication.md) | 認証機能（Firebase / Supabase / Auth0） |
| 02 | [form-validation.md](./02_feature-prompts/02_form-validation.md) | フォーム＆バリデーション（React Hook Form + Zod） |
| 03 | [data-fetching.md](./02_feature-prompts/03_data-fetching.md) | データ取得レイヤー（TanStack Query） |
| 04 | [i18n.md](./02_feature-prompts/04_i18n.md) | 国際化対応（react-i18next） |
| 05 | [pwa.md](./02_feature-prompts/05_pwa.md) | PWA化（vite-plugin-pwa） |
| 06 | [dark-mode.md](./02_feature-prompts/06_dark-mode.md) | ダークモード切り替え |
| 07 | [notification-toast.md](./02_feature-prompts/07_notification-toast.md) | トースト通知システム |
| 08 | [modal-dialog.md](./02_feature-prompts/08_modal-dialog.md) | モーダル／ダイアログシステム |
| 09 | [infinite-scroll-pagination.md](./02_feature-prompts/09_infinite-scroll-pagination.md) | 無限スクロール＆ページネーション |
| 10 | [file-upload.md](./02_feature-prompts/10_file-upload.md) | ファイルアップロード（D&D対応） |
| 11 | [realtime-websocket.md](./02_feature-prompts/11_realtime-websocket.md) | リアルタイム通信（WebSocket / SSE） |
| 12 | [search-filter.md](./02_feature-prompts/12_search-filter.md) | 検索＆フィルタリング |
| 13 | [dashboard-layout.md](./02_feature-prompts/13_dashboard-layout.md) | ダッシュボードレイアウト |
| 14 | [error-handling.md](./02_feature-prompts/14_error-handling.md) | エラーハンドリング（ErrorBoundary） |

---

### 3. 🏛️ [アーキテクチャ設計プロンプト](./03_architecture-prompts/)

プロジェクトの基盤設計に関するプロンプト集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [design-system.md](./03_architecture-prompts/01_design-system.md) | デザインシステム構築 |
| 02 | [api-layer.md](./03_architecture-prompts/02_api-layer.md) | API通信レイヤー設計 |
| 03 | [custom-hooks-library.md](./03_architecture-prompts/03_custom-hooks-library.md) | カスタムフックライブラリ |
| 04 | [state-management-patterns.md](./03_architecture-prompts/04_state-management-patterns.md) | 状態管理パターン集 |
| 05 | [project-structure-refactor.md](./03_architecture-prompts/05_project-structure-refactor.md) | ディレクトリ構成リファクタリング |

---

### 4. ✅ [品質管理プロンプト](./04_quality-prompts/)

コード品質を担保するためのチェック・監査プロンプト集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [code-review-checklist.md](./04_quality-prompts/01_code-review-checklist.md) | コードレビューチェックリスト |
| 02 | [performance-audit.md](./04_quality-prompts/02_performance-audit.md) | パフォーマンス監査 |
| 03 | [accessibility-audit.md](./04_quality-prompts/03_accessibility-audit.md) | アクセシビリティ監査（WCAG 2.1 AA） |
| 04 | [security-audit.md](./04_quality-prompts/04_security-audit.md) | セキュリティ監査 |
| 05 | [testing-strategy.md](./04_quality-prompts/05_testing-strategy.md) | テスト戦略（単体/結合/E2E） |

---

### 5. 📋 [仕様書テンプレート](./05_specification-templates/)

プロジェクトの各フェーズで使う仕様書のテンプレート集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [requirements-definition.md](./05_specification-templates/01_requirements-definition.md) | 要件定義書テンプレート |
| 02 | [technical-design.md](./05_specification-templates/02_technical-design.md) | 技術設計書テンプレート |
| 03 | [ui-ux-specification.md](./05_specification-templates/03_ui-ux-specification.md) | UI/UX仕様書テンプレート |
| 04 | [api-specification.md](./05_specification-templates/04_api-specification.md) | API仕様書テンプレート |
| 05 | [component-specification.md](./05_specification-templates/05_component-specification.md) | コンポーネント仕様書テンプレート |
| 06 | [database-schema.md](./05_specification-templates/06_database-schema.md) | DB設計書テンプレート |

---

### 6. 🔧 [リファクタリングプロンプト](./06_refactoring-prompts/)

既存コードの改善をAIに依頼する際のプロンプト集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [legacy-modernization.md](./06_refactoring-prompts/01_legacy-modernization.md) | レガシーコード刷新 |
| 02 | [performance-optimization.md](./06_refactoring-prompts/02_performance-optimization.md) | パフォーマンス最適化 |
| 03 | [bundle-optimization.md](./06_refactoring-prompts/03_bundle-optimization.md) | バンドルサイズ最適化 |

---

### 7. 🔄 [開発ワークフロープロンプト](./07_workflow-prompts/)

チーム開発・運用に関するプロンプト集。

| # | ファイル | 内容 |
|---|---------|------|
| 01 | [git-branching-strategy.md](./07_workflow-prompts/01_git-branching-strategy.md) | Gitブランチ戦略 |
| 02 | [pr-review-template.md](./07_workflow-prompts/02_pr-review-template.md) | PRテンプレート＆レビュー規約 |
| 03 | [release-process.md](./07_workflow-prompts/03_release-process.md) | リリースプロセス |
| 04 | [incident-response.md](./07_workflow-prompts/04_incident-response.md) | 障害対応フロー |

---

## 🎯 使い方

### 基本的な使い方

1. 使いたいプロンプトの `.md` ファイルを開く
2. `## プロンプト` セクション内のテキストをコピーする
3. AIエージェント（Antigravity, Cursor, GitHub Copilot 等）に貼り付けて実行する

### テンプレートリポジトリの作成

1. `01_project-templates/deploy-templates.md` からデプロイ先に合ったテンプレートを選ぶ
2. AIエージェントで新規フォルダを開き、プロンプトを実行する
3. 生成されたプロジェクトを GitHub に Push し、Settings から「Template repository」を有効化する（以降、このリポジトリを開発のベースとして使い回します）

### テンプレートを利用した新規プロジェクト開発

1. 作成したテンプレートリポジトリのページで「Use this template」をクリックし、新しいリポジトリを作成する
2. 新しいリポジトリをローカルに `git clone` する
3. `npm install` を実行する
4. `npm run setup` を実行し、プロンプトに従って新しいプロジェクト名を入力する（各種設定ファイル内のプロジェクト情報が自動置換されます）
5. 生成された `BLUEPRINT.md` にプロジェクト固有の情報を記入し、開発を開始する

### 既存プロジェクトへの機能追加

1. `02_feature-prompts/` から必要な機能のプロンプトを選ぶ
2. 「カスタマイズポイント」を参考にプロジェクトに合わせて調整する
3. AIエージェントに貼り付けて実行する

### 品質チェック

1. 開発の節目で `04_quality-prompts/` のプロンプトを実行する
2. AIエージェントが問題を検出した場合は、修正提案に従って対応する

---

## 📝 コントリビューション

プロンプトの改善提案や新しいカテゴリの追加は、Issue または Pull Request でお気軽にどうぞ。

### プロンプト追加時の規約

すべてのプロンプトは以下の統一フォーマットに従ってください：

```markdown
# [タイトル]

> **カテゴリ**: 機能追加 / アーキテクチャ / 品質管理 / 仕様書 / リファクタリング / ワークフロー
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★☆☆☆☆ 〜 ★★★★★
> **所要時間目安**: AIエージェントの実行時間目安

## このプロンプトの使い方
## プロンプト
## カスタマイズポイント
## 関連プロンプト
```

---

## 📜 ライセンス

[MIT License](./LICENSE)
