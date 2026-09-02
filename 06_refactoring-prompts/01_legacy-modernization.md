# レガシーコード刷新

> **カテゴリ**: リファクタリング
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★★
> **所要時間目安**: 60〜120分（対象コード量による）

## このプロンプトの使い方

JavaScript → TypeScript、Class Component → 関数コンポーネント、CSS Modules → TailwindCSS 等のレガシーコードの現代化を行う際に使用します。対象ファイルを指定して渡してください。

---

## プロンプト

````text
以下のガイドラインに従い、指定されたレガシーコードをモダンな実装にリファクタリングしてください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。
機能は一切変更せず、内部実装のみを更新すること。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リファクタリング対象
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
【ここに対象ファイル/ディレクトリを指定する】

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 1. JavaScript → TypeScript
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- .js / .jsx → .ts / .tsx にリネーム
- すべての変数・関数・Props に型を追加
- any は一切使用しない（unknown + 型ガード）
- interface で Props 型を定義（type でも可だが一貫性を持たせる）
- 暗黙の any を排除（noImplicitAny: true）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 2. Class Component → 関数コンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- class → アロー関数コンポーネント
- this.state → useState
- componentDidMount → useEffect（空依存配列）
- componentDidUpdate → useEffect（依存配列付き）
- componentWillUnmount → useEffect のクリーンアップ関数
- shouldComponentUpdate → React.memo
- this.props → Props の分割代入
- render() → 直接 JSX を return

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 3. CSS Modules / styled-components → TailwindCSS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- CSS Module ファイルを削除し、TailwindCSS ユーティリティクラスに置換
- styled-components のスタイル定義を TailwindCSS に移行
- カスタム CSS が必要な場合は globals.css にユーティリティクラスとして定義
- cn ユーティリティ（clsx + tailwind-merge）で条件付きクラス名を管理

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 4. 状態管理の現代化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Redux → Zustand への移行
- Context API の過剰使用 → Zustand または TanStack Query
- prop drilling → Zustand store で直接取得

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 5. API 通信の現代化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- fetch / axios の直接使用 → TanStack Query フックに置換
- useEffect 内の直接 fetch → useQuery / useMutation
- ローディング・エラー状態の手動管理 → TanStack Query の自動管理

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 6. その他の現代化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- var → const / let
- function 宣言 → アロー関数
- Promise チェーン → async/await
- require → import
- moment.js → date-fns / dayjs
- lodash（全体インポート）→ 個別インポートまたは代替実装
- JSDoc コメントの追加
- 日本語コメントの追加

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リファクタリング手順
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. 対象コードの現状分析・依存関係の把握
2. テストが存在する場合は事前に実行して結果を記録
3. ファイルごとにリファクタリングを実施
4. ESLint / TypeScript コンパイルエラーの解消
5. 既存テストの通過を確認
6. ビルド成功を確認

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 重要な制約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- **機能変更は一切行わない**
- 既存のテストがすべて通ることを確認する
- コミットはファイル単位で段階的に行う

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
追加で以下も報告すること：
- リファクタリング前後のコード行数の比較
- 削除したファイル一覧（CSS Module ファイル等）
- 型エラー 0 件であることの確認
````

---

## 関連プロンプト

- [ディレクトリ構成リファクタリング](../03_architecture-prompts/05_project-structure-refactor.md) — 構成の移行
- [パフォーマンス最適化](./02_performance-optimization.md) — リファクタリング後の最適化
- [テスト戦略](../04_quality-prompts/05_testing-strategy.md) — テストの追加
