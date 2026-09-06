# React + Vite + TailwindCSS テンプレートプロジェクト作成用プロンプト集

> 各デプロイ先ごとに1プロジェクト。Antigravityに以下のプロンプトをそのまま渡してください。

---

## 目次

1. [GitHub Pages 向け](#1-github-pages-向け)
2. [Vercel 向け](#2-vercel-向け)
3. [Netlify 向け](#3-netlify-向け)
4. [Cloudflare Pages 向け](#4-cloudflare-pages-向け)
5. [Firebase Hosting 向け](#5-firebase-hosting-向け)

---

## 1. GitHub Pages 向け

````text
以下の仕様に従い、React + Vite + TailwindCSS のひな形プロジェクトを作成してください。
このプロジェクトは「AIエージェントがwebアプリを開発する際に毎回使い回すテンプレート」です。
デプロイ先は GitHub Pages です。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 技術スタック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- React 18+（TypeScript 必須）
- Vite（最新安定版）
- TailwindCSS v3+
- React Router v6+（GitHub Pages の SPA 対応として 404.html リダイレクトハックを含む）
- 状態管理: Zustand（必要最小限のストア雛形のみ）
- アイコン: lucide-react

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ディレクトリ構成（アトミックデザイン）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── .agents/
│   ├── AGENTS.md              # AIエージェントへの行動指示書
│   └── skills/                # 必要に応じてスキル定義
├── .github/
│   └── workflows/
│       └── deploy.yml         # GitHub Pages 自動デプロイ用 GitHub Actions
├── public/
│   ├── 404.html               # SPA用リダイレクト（GitHub Pages対応）
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── atoms/             # ボタン、入力、ラベル等の最小単位
│   │   ├── molecules/         # atoms を組み合わせた小コンポーネント
│   │   ├── organisms/         # molecules を組み合わせたセクション
│   │   ├── templates/         # ページレイアウト
│   │   └── pages/             # ルーティング対象のページ
│   ├── hooks/                 # カスタムフック
│   ├── stores/                # Zustand ストア
│   ├── utils/                 # 汎用ユーティリティ関数
│   ├── types/                 # 型定義ファイル
│   ├── styles/                # グローバルCSS・TailwindCSS設定
│   ├── constants/             # 定数定義
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── scripts/
│   └── setup.mjs              # プロジェクト初期化スクリプト
├── BLUEPRINT.md               # プロジェクト仕様書（後述）
├── .eslintrc.cjs              # ESLint 設定
├── .prettierrc                # Prettier 設定
├── .vscode/
│   └── settings.json          # ctrl+S でフォーマッター自動実行の設定
├── tailwind.config.ts
├── postcss.config.cjs
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── package.json
└── README.md
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ AGENTS.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の全項目を AGENTS.md に記述すること：

### コーディング規約
- TypeScript 厳格モード（strict: true）
- any 型の使用は一切禁止。unknown + 型ガードを用いること
- 関数はすべてアロー関数で定義する（export const MyComponent = () => {} 形式）
- コメントは処理単位で非常に細かく親切に日本語で書く
  例: // ユーザー一覧を取得し、アクティブなユーザーのみをフィルタリングする
- JSDoc は以下の単位で必ず記述する：
  - すべてのコンポーネント（@description, @param, @returns, @example）
  - すべてのカスタムフック（@description, @returns, @example）
  - すべての汎用ユーティリティ関数（@description, @param, @returns）
  - 型定義（@description）
- インポート順序: React → 外部ライブラリ → 内部モジュール（絶対パス） → 相対パス → 型 → スタイル
- パスエイリアス（@/）を使用する

### デザイン規約
- アトミックデザインの5階層（atoms / molecules / organisms / templates / pages）を厳守する
- TailwindCSS のユーティリティクラスを優先し、カスタムCSSは最小限にする
- ダークモード対応を考慮した設計にする（TailwindCSS の dark: プレフィックス）
- レスポンシブデザインを前提とする（モバイルファースト）
- モダンで洗練されたデザイン: グラスモーフィズム、グラデーション、マイクロアニメーションを適切に活用する
- カラーパレットは tailwind.config.ts で一元管理する

### ファイル命名規約
- コンポーネント: PascalCase（例: UserCard.tsx）
- フック: camelCase、use プレフィックス（例: useAuth.ts）
- ユーティリティ: camelCase（例: formatDate.ts）
- 型定義: PascalCase（例: User.ts）
- 定数: UPPER_SNAKE_CASE のエクスポート名、camelCase のファイル名

### テスト規約
- 新規コンポーネントには必ず基本的なレンダリングテストを書くこと（Vitest + React Testing Library）

### Git規約
- コミットメッセージは Conventional Commits に従う（feat:, fix:, docs:, style:, refactor:, test:, chore:）
- 日本語の説明を括弧内に添える（例: feat: add login form (ログインフォームを追加)）

### タスク完了時の確認事項（必須）
タスクが完了したら、以下を必ず実施すること：
1. BLUEPRINT.md を再読し、仕様との乖離がないことを確認する
2. AGENTS.md を再読し、コーディング規約・デザイン規約・命名規約をすべて守れていることを確認する
3. ESLint / Prettier エラーが 0 件であることを確認する（npm run lint で確認）
4. TypeScript コンパイルエラーが 0 件であることを確認する（npx tsc --noEmit で確認）
5. ビルドが成功することを確認する（npm run build で確認）
6. 以下の形式で報告する：
   ```
   ## ✅ タスク完了報告
   ### 修正内容
   - （変更点を箇条書き）

   ### 仕様書チェック（BLUEPRINT.md）
   - ✅ / ❌ 各要件の適合状況

   ### コーディング規約チェック（AGENTS.md）
   - ✅ any 未使用
   - ✅ アロー関数のみ使用
   - ✅ JSDoc 記述済み
   - ✅ 日本語コメント記述済み
   - ✅ アトミックデザイン準拠
   - ✅ ESLint エラー 0 件
   - ✅ Prettier フォーマット済み
   - ✅ TypeScript コンパイルエラー 0 件
   - ✅ ビルド成功
   ```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ BLUEPRINT.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT.md にはプロジェクト仕様のテンプレートを記述する。以下の構成とすること：

```markdown
# プロジェクト仕様書

## 概要
（このプロジェクトの目的・概要を記述する）

## 技術スタック
- React 18+ (TypeScript)
- Vite
- TailwindCSS v3+
- React Router v6+
- Zustand
- デプロイ先: GitHub Pages

## 機能要件
（機能ごとに箇条書きで記述する）

## 非機能要件
- レスポンシブデザイン（モバイルファースト）
- ダークモード対応
- Lighthouse パフォーマンススコア 90 以上を目標
- アクセシビリティ（WCAG 2.1 AA 準拠を目標）

## ページ構成
（ページ一覧と各ページの概要を記述する）

## デザイン方針
- モダンで洗練されたUI
- グラスモーフィズム、グラデーション、マイクロアニメーションの活用
- 統一感のあるカラーパレット

## API 仕様
（外部APIを使用する場合にエンドポイント一覧を記述する）

## 更新履歴
| 日付 | 版 | 内容 |
|------|-----|------|
| YYYY-MM-DD | 1.0 | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ESLint / Prettier 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ESLint: @typescript-eslint, eslint-plugin-react, eslint-plugin-react-hooks, eslint-plugin-jsx-a11y を導入
- any を error レベルで禁止するルールを設定
- Prettier: シングルクォート、セミコロンあり、末尾カンマ all、タブ幅 2
- eslint-config-prettier で競合を解消
- .vscode/settings.json に editor.formatOnSave: true, editor.defaultFormatter: esbenp.prettier-vscode, editor.codeActionsOnSave の source.fixAll.eslint: "explicit" を設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CI/CD（GitHub Actions → GitHub Pages）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
.github/workflows/deploy.yml を作成し、以下を実現すること：
- トリガー: main ブランチへの push
- ジョブ:
  1. checkout
  2. Node.js セットアップ（v20）
  3. npm ci
  4. npm run lint（ESLint チェック）
  5. npx tsc --noEmit（型チェック）
  6. npm run build
  7. actions/upload-pages-artifact でビルド成果物をアップロード
  8. actions/deploy-pages でデプロイ
- vite.config.ts の base オプションをリポジトリ名に合わせて設定可能にしておく（環境変数 or コメントで説明）
- GitHub リポジトリの Settings > Pages > Source を "GitHub Actions" に設定する旨を README に記載

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ サンプルコンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テンプレートとして以下のサンプルを含めること：
- atoms: Button.tsx（variant, size, disabled 対応）、Input.tsx
- molecules: SearchBar.tsx（Input + Button の組み合わせ）
- organisms: Header.tsx（ナビゲーション含む）
- templates: DefaultLayout.tsx（Header + main + footer のレイアウト）
- pages: HomePage.tsx（ウェルカムページ、グラデーション背景 + グラスモーフィズムカード）

すべてのサンプルコンポーネントは上記のコーディング規約（JSDoc、日本語コメント、アロー関数、any禁止）を完全に遵守すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ その他
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **プロジェクト初期化スクリプト (`scripts/setup.mjs`)** を作成すること。
  - 実行時に新しいプロジェクト名を受け取るか入力させ、`package.json` の `name`、`index.html` の `<title>`、および `BLUEPRINT.md` や `README.md` 内のプロジェクト名を自動で一括置換する Node.js スクリプト。
  - `package.json` の `scripts` に `"setup": "node scripts/setup.mjs"` を追加する。
- README.md には以下の内容を記述すること：
  - このリポジトリが **GitHub の Public template リポジトリ** として利用されることを想定している旨。
  - セットアップ手順（1. `Use this template` でリポジトリ作成 → 2. `git clone` → 3. `npm install` → 4. `npm run setup` でプロジェクト情報の一括更新）。
  - 開発コマンド一覧、デプロイ手順。
- .gitignore を適切に設定する（node_modules, dist, .env 等）
- パスエイリアス（@/ → src/）を vite.config.ts と tsconfig.json の両方で設定する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 2. Vercel 向け

````text
以下の仕様に従い、React + Vite + TailwindCSS のひな形プロジェクトを作成してください。
このプロジェクトは「AIエージェントがwebアプリを開発する際に毎回使い回すテンプレート」です。
デプロイ先は Vercel です。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 技術スタック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- React 18+（TypeScript 必須）
- Vite（最新安定版）
- TailwindCSS v3+
- React Router v6+
- 状態管理: Zustand（必要最小限のストア雛形のみ）
- アイコン: lucide-react
- Vercel CLI（開発時のプレビュー用、devDependencies に含む）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ディレクトリ構成（アトミックデザイン）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── .agents/
│   ├── AGENTS.md              # AIエージェントへの行動指示書
│   └── skills/                # 必要に応じてスキル定義
├── .github/
│   └── workflows/
│       └── ci.yml             # CI 用 GitHub Actions（lint / type-check / build）
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── atoms/
│   │   ├── molecules/
│   │   ├── organisms/
│   │   ├── templates/
│   │   └── pages/
│   ├── hooks/
│   ├── stores/
│   ├── utils/
│   ├── types/
│   ├── styles/
│   ├── constants/
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── scripts/
│   └── setup.mjs              # プロジェクト初期化スクリプト
├── BLUEPRINT.md               # プロジェクト仕様書
├── vercel.json                # Vercel 設定（SPA リライト等）
├── .eslintrc.cjs
├── .prettierrc
├── .vscode/
│   └── settings.json
├── tailwind.config.ts
├── postcss.config.cjs
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── package.json
└── README.md
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ AGENTS.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の全項目を AGENTS.md に記述すること：

### コーディング規約
- TypeScript 厳格モード（strict: true）
- any 型の使用は一切禁止。unknown + 型ガードを用いること
- 関数はすべてアロー関数で定義する（export const MyComponent = () => {} 形式）
- コメントは処理単位で非常に細かく親切に日本語で書く
  例: // ユーザー一覧を取得し、アクティブなユーザーのみをフィルタリングする
- JSDoc は以下の単位で必ず記述する：
  - すべてのコンポーネント（@description, @param, @returns, @example）
  - すべてのカスタムフック（@description, @returns, @example）
  - すべての汎用ユーティリティ関数（@description, @param, @returns）
  - 型定義（@description）
- インポート順序: React → 外部ライブラリ → 内部モジュール（絶対パス） → 相対パス → 型 → スタイル
- パスエイリアス（@/）を使用する

### デザイン規約
- アトミックデザインの5階層（atoms / molecules / organisms / templates / pages）を厳守する
- TailwindCSS のユーティリティクラスを優先し、カスタムCSSは最小限にする
- ダークモード対応を考慮した設計にする（TailwindCSS の dark: プレフィックス）
- レスポンシブデザインを前提とする（モバイルファースト）
- モダンで洗練されたデザイン: グラスモーフィズム、グラデーション、マイクロアニメーションを適切に活用する
- カラーパレットは tailwind.config.ts で一元管理する

### ファイル命名規約
- コンポーネント: PascalCase（例: UserCard.tsx）
- フック: camelCase、use プレフィックス（例: useAuth.ts）
- ユーティリティ: camelCase（例: formatDate.ts）
- 型定義: PascalCase（例: User.ts）
- 定数: UPPER_SNAKE_CASE のエクスポート名、camelCase のファイル名

### テスト規約
- 新規コンポーネントには必ず基本的なレンダリングテストを書くこと（Vitest + React Testing Library）

### Git規約
- コミットメッセージは Conventional Commits に従う（feat:, fix:, docs:, style:, refactor:, test:, chore:）
- 日本語の説明を括弧内に添える（例: feat: add login form (ログインフォームを追加)）

### タスク完了時の確認事項（必須）
タスクが完了したら、以下を必ず実施すること：
1. BLUEPRINT.md を再読し、仕様との乖離がないことを確認する
2. AGENTS.md を再読し、コーディング規約・デザイン規約・命名規約をすべて守れていることを確認する
3. ESLint / Prettier エラーが 0 件であることを確認する（npm run lint で確認）
4. TypeScript コンパイルエラーが 0 件であることを確認する（npx tsc --noEmit で確認）
5. ビルドが成功することを確認する（npm run build で確認）
6. 以下の形式で報告する：
   ```
   ## ✅ タスク完了報告
   ### 修正内容
   - （変更点を箇条書き）

   ### 仕様書チェック（BLUEPRINT.md）
   - ✅ / ❌ 各要件の適合状況

   ### コーディング規約チェック（AGENTS.md）
   - ✅ any 未使用
   - ✅ アロー関数のみ使用
   - ✅ JSDoc 記述済み
   - ✅ 日本語コメント記述済み
   - ✅ アトミックデザイン準拠
   - ✅ ESLint エラー 0 件
   - ✅ Prettier フォーマット済み
   - ✅ TypeScript コンパイルエラー 0 件
   - ✅ ビルド成功
   ```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ BLUEPRINT.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT.md にはプロジェクト仕様のテンプレートを記述する。以下の構成とすること：

```markdown
# プロジェクト仕様書

## 概要
（このプロジェクトの目的・概要を記述する）

## 技術スタック
- React 18+ (TypeScript)
- Vite
- TailwindCSS v3+
- React Router v6+
- Zustand
- デプロイ先: Vercel

## 機能要件
（機能ごとに箇条書きで記述する）

## 非機能要件
- レスポンシブデザイン（モバイルファースト）
- ダークモード対応
- Lighthouse パフォーマンススコア 90 以上を目標
- アクセシビリティ（WCAG 2.1 AA 準拠を目標）

## ページ構成
（ページ一覧と各ページの概要を記述する）

## デザイン方針
- モダンで洗練されたUI
- グラスモーフィズム、グラデーション、マイクロアニメーションの活用
- 統一感のあるカラーパレット

## API 仕様
（外部APIを使用する場合にエンドポイント一覧を記述する）

## 更新履歴
| 日付 | 版 | 内容 |
|------|-----|------|
| YYYY-MM-DD | 1.0 | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ESLint / Prettier 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ESLint: @typescript-eslint, eslint-plugin-react, eslint-plugin-react-hooks, eslint-plugin-jsx-a11y を導入
- any を error レベルで禁止するルールを設定
- Prettier: シングルクォート、セミコロンあり、末尾カンマ all、タブ幅 2
- eslint-config-prettier で競合を解消
- .vscode/settings.json に editor.formatOnSave: true, editor.defaultFormatter: esbenp.prettier-vscode, editor.codeActionsOnSave の source.fixAll.eslint: "explicit" を設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ Vercel 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
vercel.json を作成し、以下を設定すること：
- SPA 用リライトルール（すべてのパスを /index.html にリライト）
- ヘッダー設定（キャッシュ制御など基本的なもの）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CI/CD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
### GitHub Actions（CI: .github/workflows/ci.yml）
- トリガー: main ブランチへの push および pull_request
- ジョブ:
  1. checkout
  2. Node.js セットアップ（v20）
  3. npm ci
  4. npm run lint（ESLint チェック）
  5. npx tsc --noEmit（型チェック）
  6. npm run build（ビルド確認）

### Vercel 自動デプロイ
- Vercel はリポジトリ連携により main ブランチへの push で自動デプロイされる
- README.md に Vercel プロジェクトの連携手順を記載する：
  1. Vercel にログイン
  2. "Import Project" から GitHub リポジトリを選択
  3. Framework Preset: Vite を選択
  4. デプロイ

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ サンプルコンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テンプレートとして以下のサンプルを含めること：
- atoms: Button.tsx（variant, size, disabled 対応）、Input.tsx
- molecules: SearchBar.tsx（Input + Button の組み合わせ）
- organisms: Header.tsx（ナビゲーション含む）
- templates: DefaultLayout.tsx（Header + main + footer のレイアウト）
- pages: HomePage.tsx（ウェルカムページ、グラデーション背景 + グラスモーフィズムカード）

すべてのサンプルコンポーネントは上記のコーディング規約（JSDoc、日本語コメント、アロー関数、any禁止）を完全に遵守すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ その他
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **プロジェクト初期化スクリプト (`scripts/setup.mjs`)** を作成すること。
  - 実行時に新しいプロジェクト名を受け取るか入力させ、`package.json` の `name`、`index.html` の `<title>`、および `BLUEPRINT.md` や `README.md` 内のプロジェクト名を自動で一括置換する Node.js スクリプト。
  - `package.json` の `scripts` に `"setup": "node scripts/setup.mjs"` を追加する。
- README.md には以下の内容を記述すること：
  - このリポジトリが **GitHub の Public template リポジトリ** として利用されることを想定している旨。
  - セットアップ手順（1. `Use this template` でリポジトリ作成 → 2. `git clone` → 3. `npm install` → 4. `npm run setup` でプロジェクト情報の一括更新）。
  - 開発コマンド一覧、デプロイ手順。
- .gitignore を適切に設定する（node_modules, dist, .env, .vercel 等）
- パスエイリアス（@/ → src/）を vite.config.ts と tsconfig.json の両方で設定する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 3. Netlify 向け

````text
以下の仕様に従い、React + Vite + TailwindCSS のひな形プロジェクトを作成してください。
このプロジェクトは「AIエージェントがwebアプリを開発する際に毎回使い回すテンプレート」です。
デプロイ先は Netlify です。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 技術スタック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- React 18+（TypeScript 必須）
- Vite（最新安定版）
- TailwindCSS v3+
- React Router v6+
- 状態管理: Zustand（必要最小限のストア雛形のみ）
- アイコン: lucide-react

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ディレクトリ構成（アトミックデザイン）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── .agents/
│   ├── AGENTS.md              # AIエージェントへの行動指示書
│   └── skills/                # 必要に応じてスキル定義
├── .github/
│   └── workflows/
│       └── ci.yml             # CI 用 GitHub Actions（lint / type-check / build）
├── public/
│   ├── _redirects              # Netlify SPA リダイレクト設定
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── atoms/
│   │   ├── molecules/
│   │   ├── organisms/
│   │   ├── templates/
│   │   └── pages/
│   ├── hooks/
│   ├── stores/
│   ├── utils/
│   ├── types/
│   ├── styles/
│   ├── constants/
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── scripts/
│   └── setup.mjs              # プロジェクト初期化スクリプト
├── BLUEPRINT.md               # プロジェクト仕様書
├── netlify.toml               # Netlify 設定
├── .eslintrc.cjs
├── .prettierrc
├── .vscode/
│   └── settings.json
├── tailwind.config.ts
├── postcss.config.cjs
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── package.json
└── README.md
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ AGENTS.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の全項目を AGENTS.md に記述すること：

### コーディング規約
- TypeScript 厳格モード（strict: true）
- any 型の使用は一切禁止。unknown + 型ガードを用いること
- 関数はすべてアロー関数で定義する（export const MyComponent = () => {} 形式）
- コメントは処理単位で非常に細かく親切に日本語で書く
  例: // ユーザー一覧を取得し、アクティブなユーザーのみをフィルタリングする
- JSDoc は以下の単位で必ず記述する：
  - すべてのコンポーネント（@description, @param, @returns, @example）
  - すべてのカスタムフック（@description, @returns, @example）
  - すべての汎用ユーティリティ関数（@description, @param, @returns）
  - 型定義（@description）
- インポート順序: React → 外部ライブラリ → 内部モジュール（絶対パス） → 相対パス → 型 → スタイル
- パスエイリアス（@/）を使用する

### デザイン規約
- アトミックデザインの5階層（atoms / molecules / organisms / templates / pages）を厳守する
- TailwindCSS のユーティリティクラスを優先し、カスタムCSSは最小限にする
- ダークモード対応を考慮した設計にする（TailwindCSS の dark: プレフィックス）
- レスポンシブデザインを前提とする（モバイルファースト）
- モダンで洗練されたデザイン: グラスモーフィズム、グラデーション、マイクロアニメーションを適切に活用する
- カラーパレットは tailwind.config.ts で一元管理する

### ファイル命名規約
- コンポーネント: PascalCase（例: UserCard.tsx）
- フック: camelCase、use プレフィックス（例: useAuth.ts）
- ユーティリティ: camelCase（例: formatDate.ts）
- 型定義: PascalCase（例: User.ts）
- 定数: UPPER_SNAKE_CASE のエクスポート名、camelCase のファイル名

### テスト規約
- 新規コンポーネントには必ず基本的なレンダリングテストを書くこと（Vitest + React Testing Library）

### Git規約
- コミットメッセージは Conventional Commits に従う（feat:, fix:, docs:, style:, refactor:, test:, chore:）
- 日本語の説明を括弧内に添える（例: feat: add login form (ログインフォームを追加)）

### タスク完了時の確認事項（必須）
タスクが完了したら、以下を必ず実施すること：
1. BLUEPRINT.md を再読し、仕様との乖離がないことを確認する
2. AGENTS.md を再読し、コーディング規約・デザイン規約・命名規約をすべて守れていることを確認する
3. ESLint / Prettier エラーが 0 件であることを確認する（npm run lint で確認）
4. TypeScript コンパイルエラーが 0 件であることを確認する（npx tsc --noEmit で確認）
5. ビルドが成功することを確認する（npm run build で確認）
6. 以下の形式で報告する：
   ```
   ## ✅ タスク完了報告
   ### 修正内容
   - （変更点を箇条書き）

   ### 仕様書チェック（BLUEPRINT.md）
   - ✅ / ❌ 各要件の適合状況

   ### コーディング規約チェック（AGENTS.md）
   - ✅ any 未使用
   - ✅ アロー関数のみ使用
   - ✅ JSDoc 記述済み
   - ✅ 日本語コメント記述済み
   - ✅ アトミックデザイン準拠
   - ✅ ESLint エラー 0 件
   - ✅ Prettier フォーマット済み
   - ✅ TypeScript コンパイルエラー 0 件
   - ✅ ビルド成功
   ```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ BLUEPRINT.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT.md にはプロジェクト仕様のテンプレートを記述する。以下の構成とすること：

```markdown
# プロジェクト仕様書

## 概要
（このプロジェクトの目的・概要を記述する）

## 技術スタック
- React 18+ (TypeScript)
- Vite
- TailwindCSS v3+
- React Router v6+
- Zustand
- デプロイ先: Netlify

## 機能要件
（機能ごとに箇条書きで記述する）

## 非機能要件
- レスポンシブデザイン（モバイルファースト）
- ダークモード対応
- Lighthouse パフォーマンススコア 90 以上を目標
- アクセシビリティ（WCAG 2.1 AA 準拠を目標）

## ページ構成
（ページ一覧と各ページの概要を記述する）

## デザイン方針
- モダンで洗練されたUI
- グラスモーフィズム、グラデーション、マイクロアニメーションの活用
- 統一感のあるカラーパレット

## API 仕様
（外部APIを使用する場合にエンドポイント一覧を記述する）

## 更新履歴
| 日付 | 版 | 内容 |
|------|-----|------|
| YYYY-MM-DD | 1.0 | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ESLint / Prettier 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ESLint: @typescript-eslint, eslint-plugin-react, eslint-plugin-react-hooks, eslint-plugin-jsx-a11y を導入
- any を error レベルで禁止するルールを設定
- Prettier: シングルクォート、セミコロンあり、末尾カンマ all、タブ幅 2
- eslint-config-prettier で競合を解消
- .vscode/settings.json に editor.formatOnSave: true, editor.defaultFormatter: esbenp.prettier-vscode, editor.codeActionsOnSave の source.fixAll.eslint: "explicit" を設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ Netlify 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
### netlify.toml
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### public/_redirects
```
/*    /index.html   200
```
（netlify.toml と _redirects の両方で SPA リダイレクトを設定し、確実に動作するようにする）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CI/CD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
### GitHub Actions（CI: .github/workflows/ci.yml）
- トリガー: main ブランチへの push および pull_request
- ジョブ:
  1. checkout
  2. Node.js セットアップ（v20）
  3. npm ci
  4. npm run lint（ESLint チェック）
  5. npx tsc --noEmit（型チェック）
  6. npm run build（ビルド確認）

### Netlify 自動デプロイ
- Netlify はリポジトリ連携により main ブランチへの push で自動デプロイされる
- README.md に Netlify プロジェクトの連携手順を記載する：
  1. Netlify にログイン
  2. "Add new site" > "Import an existing project" から GitHub リポジトリを選択
  3. Build command: npm run build
  4. Publish directory: dist
  5. デプロイ

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ サンプルコンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テンプレートとして以下のサンプルを含めること：
- atoms: Button.tsx（variant, size, disabled 対応）、Input.tsx
- molecules: SearchBar.tsx（Input + Button の組み合わせ）
- organisms: Header.tsx（ナビゲーション含む）
- templates: DefaultLayout.tsx（Header + main + footer のレイアウト）
- pages: HomePage.tsx（ウェルカムページ、グラデーション背景 + グラスモーフィズムカード）

すべてのサンプルコンポーネントは上記のコーディング規約（JSDoc、日本語コメント、アロー関数、any禁止）を完全に遵守すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ その他
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **プロジェクト初期化スクリプト (`scripts/setup.mjs`)** を作成すること。
  - 実行時に新しいプロジェクト名を受け取るか入力させ、`package.json` の `name`、`index.html` の `<title>`、および `BLUEPRINT.md` や `README.md` 内のプロジェクト名を自動で一括置換する Node.js スクリプト。
  - `package.json` の `scripts` に `"setup": "node scripts/setup.mjs"` を追加する。
- README.md には以下の内容を記述すること：
  - このリポジトリが **GitHub の Public template リポジトリ** として利用されることを想定している旨。
  - セットアップ手順（1. `Use this template` でリポジトリ作成 → 2. `git clone` → 3. `npm install` → 4. `npm run setup` でプロジェクト情報の一括更新）。
  - 開発コマンド一覧、デプロイ手順。
- .gitignore を適切に設定する（node_modules, dist, .env, .netlify 等）
- パスエイリアス（@/ → src/）を vite.config.ts と tsconfig.json の両方で設定する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 4. Cloudflare Pages 向け

````text
以下の仕様に従い、React + Vite + TailwindCSS のひな形プロジェクトを作成してください。
このプロジェクトは「AIエージェントがwebアプリを開発する際に毎回使い回すテンプレート」です。
デプロイ先は Cloudflare Pages です。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 技術スタック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- React 18+（TypeScript 必須）
- Vite（最新安定版）
- TailwindCSS v3+
- React Router v6+
- 状態管理: Zustand（必要最小限のストア雛形のみ）
- アイコン: lucide-react
- Wrangler CLI（開発時のプレビュー用、devDependencies に含む）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ディレクトリ構成（アトミックデザイン）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── .agents/
│   ├── AGENTS.md              # AIエージェントへの行動指示書
│   └── skills/                # 必要に応じてスキル定義
├── .github/
│   └── workflows/
│       └── deploy.yml         # Cloudflare Pages 自動デプロイ用 GitHub Actions
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── atoms/
│   │   ├── molecules/
│   │   ├── organisms/
│   │   ├── templates/
│   │   └── pages/
│   ├── hooks/
│   ├── stores/
│   ├── utils/
│   ├── types/
│   ├── styles/
│   ├── constants/
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── scripts/
│   └── setup.mjs              # プロジェクト初期化スクリプト
├── BLUEPRINT.md               # プロジェクト仕様書
├── .eslintrc.cjs
├── .prettierrc
├── .vscode/
│   └── settings.json
├── tailwind.config.ts
├── postcss.config.cjs
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── wrangler.toml              # Cloudflare Pages 設定（オプション）
├── package.json
└── README.md
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ AGENTS.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の全項目を AGENTS.md に記述すること：

### コーディング規約
- TypeScript 厳格モード（strict: true）
- any 型の使用は一切禁止。unknown + 型ガードを用いること
- 関数はすべてアロー関数で定義する（export const MyComponent = () => {} 形式）
- コメントは処理単位で非常に細かく親切に日本語で書く
  例: // ユーザー一覧を取得し、アクティブなユーザーのみをフィルタリングする
- JSDoc は以下の単位で必ず記述する：
  - すべてのコンポーネント（@description, @param, @returns, @example）
  - すべてのカスタムフック（@description, @returns, @example）
  - すべての汎用ユーティリティ関数（@description, @param, @returns）
  - 型定義（@description）
- インポート順序: React → 外部ライブラリ → 内部モジュール（絶対パス） → 相対パス → 型 → スタイル
- パスエイリアス（@/）を使用する

### デザイン規約
- アトミックデザインの5階層（atoms / molecules / organisms / templates / pages）を厳守する
- TailwindCSS のユーティリティクラスを優先し、カスタムCSSは最小限にする
- ダークモード対応を考慮した設計にする（TailwindCSS の dark: プレフィックス）
- レスポンシブデザインを前提とする（モバイルファースト）
- モダンで洗練されたデザイン: グラスモーフィズム、グラデーション、マイクロアニメーションを適切に活用する
- カラーパレットは tailwind.config.ts で一元管理する

### ファイル命名規約
- コンポーネント: PascalCase（例: UserCard.tsx）
- フック: camelCase、use プレフィックス（例: useAuth.ts）
- ユーティリティ: camelCase（例: formatDate.ts）
- 型定義: PascalCase（例: User.ts）
- 定数: UPPER_SNAKE_CASE のエクスポート名、camelCase のファイル名

### テスト規約
- 新規コンポーネントには必ず基本的なレンダリングテストを書くこと（Vitest + React Testing Library）

### Git規約
- コミットメッセージは Conventional Commits に従う（feat:, fix:, docs:, style:, refactor:, test:, chore:）
- 日本語の説明を括弧内に添える（例: feat: add login form (ログインフォームを追加)）

### タスク完了時の確認事項（必須）
タスクが完了したら、以下を必ず実施すること：
1. BLUEPRINT.md を再読し、仕様との乖離がないことを確認する
2. AGENTS.md を再読し、コーディング規約・デザイン規約・命名規約をすべて守れていることを確認する
3. ESLint / Prettier エラーが 0 件であることを確認する（npm run lint で確認）
4. TypeScript コンパイルエラーが 0 件であることを確認する（npx tsc --noEmit で確認）
5. ビルドが成功することを確認する（npm run build で確認）
6. 以下の形式で報告する：
   ```
   ## ✅ タスク完了報告
   ### 修正内容
   - （変更点を箇条書き）

   ### 仕様書チェック（BLUEPRINT.md）
   - ✅ / ❌ 各要件の適合状況

   ### コーディング規約チェック（AGENTS.md）
   - ✅ any 未使用
   - ✅ アロー関数のみ使用
   - ✅ JSDoc 記述済み
   - ✅ 日本語コメント記述済み
   - ✅ アトミックデザイン準拠
   - ✅ ESLint エラー 0 件
   - ✅ Prettier フォーマット済み
   - ✅ TypeScript コンパイルエラー 0 件
   - ✅ ビルド成功
   ```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ BLUEPRINT.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT.md にはプロジェクト仕様のテンプレートを記述する。以下の構成とすること：

```markdown
# プロジェクト仕様書

## 概要
（このプロジェクトの目的・概要を記述する）

## 技術スタック
- React 18+ (TypeScript)
- Vite
- TailwindCSS v3+
- React Router v6+
- Zustand
- デプロイ先: Cloudflare Pages

## 機能要件
（機能ごとに箇条書きで記述する）

## 非機能要件
- レスポンシブデザイン（モバイルファースト）
- ダークモード対応
- Lighthouse パフォーマンススコア 90 以上を目標
- アクセシビリティ（WCAG 2.1 AA 準拠を目標）

## ページ構成
（ページ一覧と各ページの概要を記述する）

## デザイン方針
- モダンで洗練されたUI
- グラスモーフィズム、グラデーション、マイクロアニメーションの活用
- 統一感のあるカラーパレット

## API 仕様
（外部APIを使用する場合にエンドポイント一覧を記述する）

## 更新履歴
| 日付 | 版 | 内容 |
|------|-----|------|
| YYYY-MM-DD | 1.0 | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ESLint / Prettier 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ESLint: @typescript-eslint, eslint-plugin-react, eslint-plugin-react-hooks, eslint-plugin-jsx-a11y を導入
- any を error レベルで禁止するルールを設定
- Prettier: シングルクォート、セミコロンあり、末尾カンマ all、タブ幅 2
- eslint-config-prettier で競合を解消
- .vscode/settings.json に editor.formatOnSave: true, editor.defaultFormatter: esbenp.prettier-vscode, editor.codeActionsOnSave の source.fixAll.eslint: "explicit" を設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CI/CD（GitHub Actions → Cloudflare Pages）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
.github/workflows/deploy.yml を作成し、以下を実現すること：
- トリガー: main ブランチへの push
- ジョブ:
  1. checkout
  2. Node.js セットアップ（v20）
  3. npm ci
  4. npm run lint（ESLint チェック）
  5. npx tsc --noEmit（型チェック）
  6. npm run build
  7. cloudflare/wrangler-action@v3 を使用して Cloudflare Pages にデプロイ
- 必要なシークレット:
  - CLOUDFLARE_API_TOKEN
  - CLOUDFLARE_ACCOUNT_ID
- README.md に以下を記載：
  1. Cloudflare ダッシュボードで API トークンを作成する手順
  2. GitHub リポジトリの Settings > Secrets に上記シークレットを登録する手順
  3. Cloudflare Pages プロジェクトの作成手順

もしくは、Cloudflare Pages のダッシュボードからリポジトリを直接連携する方法も README に併記すること：
  1. Cloudflare ダッシュボードにログイン
  2. Pages > "Create a project" > "Connect to Git"
  3. GitHub リポジトリを選択
  4. Build command: npm run build
  5. Build output directory: dist

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ SPA ルーティング対応
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cloudflare Pages の SPA 対応として、public/_routes.json を作成する：
```json
{
  "version": 1,
  "include": ["/*"],
  "exclude": ["/assets/*"]
}
```
また、public/_redirects も併用する：
```
/*    /index.html   200
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ サンプルコンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テンプレートとして以下のサンプルを含めること：
- atoms: Button.tsx（variant, size, disabled 対応）、Input.tsx
- molecules: SearchBar.tsx（Input + Button の組み合わせ）
- organisms: Header.tsx（ナビゲーション含む）
- templates: DefaultLayout.tsx（Header + main + footer のレイアウト）
- pages: HomePage.tsx（ウェルカムページ、グラデーション背景 + グラスモーフィズムカード）

すべてのサンプルコンポーネントは上記のコーディング規約（JSDoc、日本語コメント、アロー関数、any禁止）を完全に遵守すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ その他
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **プロジェクト初期化スクリプト (`scripts/setup.mjs`)** を作成すること。
  - 実行時に新しいプロジェクト名を受け取るか入力させ、`package.json` の `name`、`index.html` の `<title>`、および `BLUEPRINT.md` や `README.md` 内のプロジェクト名を自動で一括置換する Node.js スクリプト。
  - `package.json` の `scripts` に `"setup": "node scripts/setup.mjs"` を追加する。
- README.md には以下の内容を記述すること：
  - このリポジトリが **GitHub の Public template リポジトリ** として利用されることを想定している旨。
  - セットアップ手順（1. `Use this template` でリポジトリ作成 → 2. `git clone` → 3. `npm install` → 4. `npm run setup` でプロジェクト情報の一括更新）。
  - 開発コマンド一覧、デプロイ手順。
- .gitignore を適切に設定する（node_modules, dist, .env, .wrangler 等）
- パスエイリアス（@/ → src/）を vite.config.ts と tsconfig.json の両方で設定する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 5. Firebase Hosting 向け

````text
以下の仕様に従い、React + Vite + TailwindCSS のひな形プロジェクトを作成してください。
このプロジェクトは「AIエージェントがwebアプリを開発する際に毎回使い回すテンプレート」です。
デプロイ先は Firebase Hosting です。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 技術スタック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- React 18+（TypeScript 必須）
- Vite（最新安定版）
- TailwindCSS v3+
- React Router v6+
- 状態管理: Zustand（必要最小限のストア雛形のみ）
- アイコン: lucide-react
- Firebase CLI（開発時のプレビュー用、devDependencies に含む）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ディレクトリ構成（アトミックデザイン）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
project-root/
├── .agents/
│   ├── AGENTS.md              # AIエージェントへの行動指示書
│   └── skills/                # 必要に応じてスキル定義
├── .github/
│   └── workflows/
│       ├── firebase-hosting-merge.yml         # main ブランチマージ時の本番デプロイ
│       └── firebase-hosting-pull-request.yml  # PRごとのプレビューチャンネルデプロイ
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── atoms/
│   │   ├── molecules/
│   │   ├── organisms/
│   │   ├── templates/
│   │   └── pages/
│   ├── hooks/
│   ├── stores/
│   ├── utils/
│   ├── types/
│   ├── styles/
│   ├── constants/
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── scripts/
│   └── setup.mjs              # プロジェクト初期化スクリプト
├── BLUEPRINT.md               # プロジェクト仕様書
├── firebase.json              # Firebase Hosting 設定
├── .firebaserc                # Firebase プロジェクトID紐付け設定
├── .eslintrc.cjs
├── .prettierrc
├── .vscode/
│   └── settings.json
├── tailwind.config.ts
├── postcss.config.cjs
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── package.json
└── README.md
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ AGENTS.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の全項目を AGENTS.md に記述すること：

### コーディング規約
- TypeScript 厳格モード（strict: true）
- any 型の使用は一切禁止。unknown + 型ガードを用いること
- 関数はすべてアロー関数で定義する（export const MyComponent = () => {} 形式）
- コメントは処理単位で非常に細かく親切に日本語で書く
  例: // ユーザー一覧を取得し、アクティブなユーザーのみをフィルタリングする
- JSDoc は以下の単位で必ず記述する：
  - すべてのコンポーネント（@description, @param, @returns, @example）
  - すべてのカスタムフック（@description, @returns, @example）
  - すべての汎用ユーティリティ関数（@description, @param, @returns）
  - 型定義（@description）
- インポート順序: React → 外部ライブラリ → 内部モジュール（絶対パス） → 相対パス → 型 → スタイル
- パスエイリアス（@/）を使用する

### デザイン規約
- アトミックデザインの5階層（atoms / molecules / organisms / templates / pages）を厳守する
- TailwindCSS のユーティリティクラスを優先し、カスタムCSSは最小限にする
- ダークモード対応を考慮した設計にする（TailwindCSS の dark: プレフィックス）
- レスポンシブデザインを前提とする（モバイルファースト）
- モダンで洗練されたデザイン: グラスモーフィズム、グラデーション、マイクロアニメーションを適切に活用する
- カラーパレットは tailwind.config.ts で一元管理する

### ファイル命名規約
- コンポーネント: PascalCase（例: UserCard.tsx）
- フック: camelCase、use プレフィックス（例: useAuth.ts）
- ユーティリティ: camelCase（例: formatDate.ts）
- 型定義: PascalCase（例: User.ts）
- 定数: UPPER_SNAKE_CASE のエクスポート名、camelCase のファイル名

### テスト規約
- 新規コンポーネントには必ず基本的なレンダリングテストを書くこと（Vitest + React Testing Library）

### Git規約
- コミットメッセージは Conventional Commits に従う（feat:, fix:, docs:, style:, refactor:, test:, chore:）
- 日本語の説明を括弧内に添える（例: feat: add login form (ログインフォームを追加)）

### タスク完了時の確認事項（必須）
タスクが完了したら、以下を必ず実施すること：
1. BLUEPRINT.md を再読し、仕様との乖離がないことを確認する
2. AGENTS.md を再読し、コーディング規約・デザイン規約・命名規約をすべて守れていることを確認する
3. ESLint / Prettier エラーが 0 件であることを確認する（npm run lint で確認）
4. TypeScript コンパイルエラーが 0 件であることを確認する（npx tsc --noEmit で確認）
5. ビルドが成功することを確認する（npm run build で確認）
6. 以下の形式で報告する：
   ```
   ## ✅ タスク完了報告
   ### 修正内容
   - （変更点を箇条書き）

   ### 仕様書チェック（BLUEPRINT.md）
   - ✅ / ❌ 各要件の適合状況

   ### コーディング規約チェック（AGENTS.md）
   - ✅ any 未使用
   - ✅ アロー関数のみ使用
   - ✅ JSDoc 記述済み
   - ✅ 日本語コメント記述済み
   - ✅ アトミックデザイン準拠
   - ✅ ESLint エラー 0 件
   - ✅ Prettier フォーマット済み
   - ✅ TypeScript コンパイルエラー 0 件
   - ✅ ビルド成功
   ```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ BLUEPRINT.md の内容
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT.md にはプロジェクト仕様のテンプレートを記述する。以下の構成とすること：

```markdown
# プロジェクト仕様書

## 概要
（このプロジェクトの目的・概要を記述する）

## 技術スタック
- React 18+ (TypeScript)
- Vite
- TailwindCSS v3+
- React Router v6+
- Zustand
- デプロイ先: Firebase Hosting

## 機能要件
（機能ごとに箇条書きで記述する）

## 非機能要件
- レスポンシブデザイン（モバイルファースト）
- ダークモード対応
- Lighthouse パフォーマンススコア 90 以上を目標
- アクセシビリティ（WCAG 2.1 AA 準拠を目標）

## ページ構成
（ページ一覧と各ページの概要を記述する）

## デザイン方針
- モダンで洗練されたUI
- グラスモーフィズム、グラデーション、マイクロアニメーションの活用
- 統一感のあるカラーパレット

## API 仕様
（外部APIを使用する場合にエンドポイント一覧を記述する）

## 更新履歴
| 日付 | 版 | 内容 |
|------|-----|------|
| YYYY-MM-DD | 1.0 | 初版作成 |
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ESLint / Prettier 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ESLint: @typescript-eslint, eslint-plugin-react, eslint-plugin-react-hooks, eslint-plugin-jsx-a11y を導入
- any を error レベルで禁止するルールを設定
- Prettier: シングルクォート、セミコロンあり、末尾カンマ all、タブ幅 2
- eslint-config-prettier で競合を解消
- .vscode/settings.json に editor.formatOnSave: true, editor.defaultFormatter: esbenp.prettier-vscode, editor.codeActionsOnSave の source.fixAll.eslint: "explicit" を設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ Firebase 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
### firebase.json
```json
{
  "hosting": {
    "public": "dist",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
※ Vite のビルド出力先（dist）をそのまま Hosting の公開ディレクトリに指定するだけで
  SPA向けのリライトも `rewrites` 一箇所で完結する（Netlify や Cloudflare Pages のように
  `_redirects` 等の別ファイルを追加で用意する必要がない）。

### .firebaserc
```json
{
  "projects": {
    "default": "<FIREBASE_PROJECT_ID>"
  }
}
```
`<FIREBASE_PROJECT_ID>` の部分は実際のFirebaseプロジェクトIDに置き換える旨をREADMEに明記すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CI/CD（GitHub Actions → Firebase Hosting）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Firebase公式のGitHub連携パターンに沿って、2つのワークフローを作成すること。

### .github/workflows/firebase-hosting-merge.yml（本番デプロイ）
- トリガー: `main` ブランチへの push
- ジョブ:
  1. checkout
  2. Node.js セットアップ（v20）
  3. npm ci
  4. npm run lint（ESLint チェック）
  5. npx tsc --noEmit（型チェック）
  6. npm run build
  7. `FirebaseExtended/action-hosting-deploy@v0` を使用し、`channelId: live` でデプロイ

### .github/workflows/firebase-hosting-pull-request.yml（プレビューデプロイ）
- トリガー: `pull_request`
- ジョブ: 上記と同様のビルド手順のあと、`FirebaseExtended/action-hosting-deploy@v0` を
  `channelId` を指定せずに実行し、PRごとのプレビューチャンネルURLを自動発行する
  （アクションが自動でPRにプレビューURLをコメントする）

### 必要なSecrets
- `FIREBASE_SERVICE_ACCOUNT_<PROJECT_ID>`（Firebaseサービスアカウントの認証情報JSON。
  ローカルで `firebase init hosting:github` を実行すると自動生成・登録される）
- `GITHUB_TOKEN`（GitHub Actionsが自動的に提供するため追加設定不要）

README.md には以下の2通りのセットアップ方法を併記すること：
1. ローカルで `firebase init hosting:github` を実行し、上記2ワークフローとサービス
   アカウントのSecrets登録を自動で行わせる方法（推奨・簡単）
2. 上記のワークフローファイルを手動で配置し、Firebaseコンソールでサービスアカウントを
   発行してSecretsに手動登録する方法

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ サンプルコンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
テンプレートとして以下のサンプルを含めること：
- atoms: Button.tsx（variant, size, disabled 対応）、Input.tsx
- molecules: SearchBar.tsx（Input + Button の組み合わせ）
- organisms: Header.tsx（ナビゲーション含む）
- templates: DefaultLayout.tsx（Header + main + footer のレイアウト）
- pages: HomePage.tsx（ウェルカムページ、グラデーション背景 + グラスモーフィズムカード）

すべてのサンプルコンポーネントは上記のコーディング規約（JSDoc、日本語コメント、アロー関数、any禁止）を完全に遵守すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ その他
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **プロジェクト初期化スクリプト (`scripts/setup.mjs`)** を作成すること。
  - 実行時に新しいプロジェクト名を受け取るか入力させ、`package.json` の `name`、`index.html` の `<title>`、および `BLUEPRINT.md` や `README.md` 内のプロジェクト名を自動で一括置換する Node.js スクリプト。
  - `package.json` の `scripts` に `"setup": "node scripts/setup.mjs"` を追加する。
- README.md には以下の内容を記述すること：
  - このリポジトリが **GitHub の Public template リポジトリ** として利用されることを想定している旨。
  - セットアップ手順（1. `Use this template` でリポジトリ作成 → 2. `git clone` → 3. `npm install` → 4. `npm run setup` でプロジェクト情報の一括更新）。
  - 開発コマンド一覧、デプロイ手順。
- .gitignore を適切に設定する（node_modules, dist, .env, .firebase 等）
- パスエイリアス（@/ → src/）を vite.config.ts と tsconfig.json の両方で設定する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 付録: 各サービスの差分まとめ

| 観点 | GitHub Pages | Vercel | Netlify | Cloudflare Pages | Firebase Hosting |
|------|-------------|--------|---------|-----------------|-------------------|
| SPA対応 | `404.html` リダイレクトハック | `vercel.json` リライト | `netlify.toml` + `_redirects` | `_routes.json` + `_redirects` | `firebase.json` の `rewrites` 一箇所のみで完結（追加ファイル不要） |
| デプロイ方法 | GitHub Actions (`actions/deploy-pages`) | リポジトリ連携（自動） | リポジトリ連携（自動） | GitHub Actions (`wrangler-action`) or リポジトリ連携 | GitHub Actions（`FirebaseExtended/action-hosting-deploy`）。`firebase init hosting:github` で自動セットアップ可 |
| CI | deploy.yml に統合 | 別途 ci.yml | 別途 ci.yml | deploy.yml に統合 | `firebase-hosting-merge.yml` と `firebase-hosting-pull-request.yml` の2ファイルに分離 |
| 設定ファイル | なし | `vercel.json` | `netlify.toml` | `wrangler.toml`（オプション） | `firebase.json`, `.firebaserc` |
| vite.config.ts の `base` | リポジトリ名に応じて変更が必要 | デフォルト (`/`) | デフォルト (`/`) | デフォルト (`/`) | デフォルト (`/`) |
| シークレット | 不要（GITHUB_TOKEN 自動） | 不要（連携で自動） | 不要（連携で自動） | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` | `FIREBASE_SERVICE_ACCOUNT_<PROJECT_ID>`（`firebase init hosting:github` で自動登録可） |

他の4サービスとの一番の違いは、**PRごとのプレビュー環境が最初から2本立てのワークフローとして公式サポートされている**点です（Vercel/Netlifyはダッシュボード連携で自動的にプレビューが作られますが、Firebaseの場合はGitHub Actions側に明示的に2つ目のワークフローを持つ構成が公式に案内されています）。

---

## 使い方

1. 使いたいデプロイ先のプロンプトをコピーする
2. 新しいフォルダを作成し、Antigravity（またはお好みのAIエージェント）でそのフォルダを開く
3. プロンプトを貼り付けて実行する
4. テンプレートが生成されたら、BLUEPRINT.md の「概要」「機能要件」「ページ構成」を実際のプロジェクトに合わせて書き換える
5. 以降の開発タスクでは、AIエージェントが AGENTS.md と BLUEPRINT.md を参照しながら規約に沿った開発を行う
