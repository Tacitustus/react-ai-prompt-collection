# デザインシステム構築

> **カテゴリ**: アーキテクチャ設計
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★☆
> **所要時間目安**: 40〜60分

## このプロンプトの使い方

プロジェクト開始時にデザイントークン（カラー、タイポグラフィ、スペーシング等）を定義し、一貫したUIを構築するための基盤を整える際に使用します。

---

## プロンプト

````text
以下の仕様に従い、プロジェクトのデザインシステムを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 目的
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
プロジェクト全体で統一されたデザインを実現するため、以下のデザインシステムを構築する：
1. TailwindCSS のカスタムテーマ設定
2. 共通のデザイントークン定義
3. 基本的な atoms コンポーネントライブラリ
4. デザインシステムのドキュメント

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. tailwind.config.ts のカスタマイズ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下のデザイントークンを tailwind.config.ts に定義すること：

### カラーパレット
- `primary`: メインカラー（5段階: 50, 100, 200, ..., 900）
- `secondary`: サブカラー（5段階）
- `accent`: アクセントカラー（5段階）
- `neutral`: ニュートラルカラー（グレー系、10段階）
- `success`, `warning`, `error`, `info`: セマンティックカラー

### タイポグラフィ
- フォントファミリー: Google Fonts から Inter をデフォルトとして設定
- フォントサイズ: TailwindCSS デフォルト + カスタム追加（display-1, display-2）

### スペーシング
- TailwindCSS デフォルトスケールに加えて、レイアウト用カスタムスペーシング

### 角丸
- `rounded-card`: カード用
- `rounded-button`: ボタン用
- `rounded-input`: フォーム入力用
- `rounded-modal`: モーダル用

### シャドウ
- `shadow-card`: カード用
- `shadow-dropdown`: ドロップダウン用
- `shadow-modal`: モーダル用

### アニメーション
- `animate-fade-in`: フェードイン
- `animate-slide-up`: 下からスライド
- `animate-scale-in`: 拡大表示
- `animate-shimmer`: スケルトンローディング用

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. グローバル CSS (src/styles/globals.css)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下を含むグローバルCSSを作成すること：
- TailwindCSS の @tailwind ディレクティブ
- CSS カスタムプロパティ（Light Mode / Dark Mode）
- スクロールバーのカスタムスタイル
- フォーカスリングのデフォルトスタイル
- カスタムアニメーションの @keyframes
- ユーティリティクラス（.glass-morphism, .gradient-primary 等）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. デザイントークン定数 (src/constants/designTokens.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TypeScript で型安全なデザイントークン定数を定義すること：
- カラーのキー定義
- ブレイクポイントの定数
- z-index のスケール定義
- トランジション設定

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. atoms コンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下の基本コンポーネントをデザインシステムの一部として作成すること：

### Button
- variant: primary / secondary / ghost / danger / outline
- size: xs / sm / md / lg / xl
- 状態: default / hover / active / focus / disabled / loading
- アイコン対応（左/右/アイコンのみ）
- fullWidth オプション

### Input
- type: text / email / password / number / search
- size: sm / md / lg
- 状態: default / focus / error / disabled
- ラベル、プレースホルダー、ヘルパーテキスト、エラーメッセージ対応
- アイコン対応（左/右）

### Badge
- variant: primary / secondary / success / warning / error / info
- size: sm / md
- dot（ドットインジケーター）対応

### Avatar
- size: xs / sm / md / lg / xl
- 画像 / イニシャル / デフォルトアイコン
- オンラインステータスインジケーター

### Card
- variant: default / outlined / elevated / glass
- padding: sm / md / lg
- hover アニメーション対応
- ヘッダー / ボディ / フッター構成

### Spinner
- size: sm / md / lg
- カラーバリアント

### Skeleton
- variant: text / circular / rectangular
- アニメーション: pulse / shimmer

### Typography
- variant: h1 / h2 / h3 / h4 / body / small / caption
- color / weight / align オプション
- truncate / clamp 対応

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. ユーティリティ関数 (src/utils/cn.ts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
clsx + tailwind-merge を組み合わせた `cn` ユーティリティ関数を作成すること。
すべてのコンポーネントでこの関数を使用して className を結合する。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- すべてのコンポーネントに JSDoc を記述する（@description, @param, @returns, @example）
- 日本語コメントを処理単位で記述する
- any 禁止、すべてアロー関数
- Props は interface で定義し、各プロパティに JSDoc コメント
- clsx/cn を使用した条件付きクラス名結合
- React.forwardRef 対応（DOM要素をラップするコンポーネント）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| カラーパレット | ブランドカラーに合わせて HSL 値を調整 |
| フォント | Inter 以外のフォント（Noto Sans JP 等）に変更可能 |
| atoms の種類 | Checkbox, Radio, Select, Textarea 等を追加 |
| Storybook | 大規模プロジェクトでは Storybook の導入も検討 |

---

## 関連プロンプト

- [UI/UX仕様書テンプレート](../05_specification-templates/03_ui-ux-specification.md) — デザインの仕様定義
- [コンポーネント仕様書テンプレート](../05_specification-templates/05_component-specification.md) — 各コンポーネントの詳細仕様
- [ダークモード切り替え](../02_feature-prompts/06_dark-mode.md) — ダークモード実装
