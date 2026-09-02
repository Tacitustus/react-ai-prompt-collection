# カスタムフックライブラリ

> **カテゴリ**: アーキテクチャ設計
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

プロジェクト全体で使い回せる汎用カスタムフックを一括で構築する際に使用します。個別のフックが必要な場合はリストから選んで指定することもできます。

---

## プロンプト

````text
以下の仕様に従い、汎用カスタムフックライブラリを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 出力先
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/hooks/
├── useLocalStorage.ts
├── useSessionStorage.ts
├── useDebounce.ts
├── useThrottle.ts
├── useMediaQuery.ts
├── useClickOutside.ts
├── useIntersectionObserver.ts
├── useKeyPress.ts
├── useCopyToClipboard.ts
├── useToggle.ts
├── useWindowSize.ts
├── useScrollPosition.ts
├── usePrevious.ts
├── useIsFirstRender.ts
├── useDocumentTitle.ts
├── useEventListener.ts
├── useFocusTrap.ts
├── useOnlineStatus.ts
└── index.ts                  # 全フックの一括エクスポート
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 各フックの仕様
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### 1. useLocalStorage<T>
- LocalStorage に値を保存・取得するフック
- ジェネリクスで型安全に使える
- SSR 対応（window が undefined の場合のフォールバック）
- JSON シリアライズ/デシリアライズ
- 他タブでの変更を StorageEvent で検知・同期

### 2. useSessionStorage<T>
- SessionStorage 版（useLocalStorage と同一インターフェース）

### 3. useDebounce<T>
- 値のデバウンスを行うフック
- delay（ミリ秒）を指定可能
- 検索入力のリアルタイムフィルタリング等に使用

### 4. useThrottle<T>
- 値のスロットリングを行うフック
- interval（ミリ秒）を指定可能
- スクロールイベント等の高頻度更新に使用

### 5. useMediaQuery
- CSS メディアクエリの一致状態を返すフック
- TailwindCSS のブレイクポイントとの連携を考慮
- `useIsMobile()`, `useIsTablet()`, `useIsDesktop()` のラッパーフックも作成

### 6. useClickOutside
- 指定要素の外側クリックを検知するフック
- RefObject を受け取り、外側クリック時にコールバックを実行
- ドロップダウン、モーダルの閉じ処理に使用

### 7. useIntersectionObserver
- Intersection Observer API のラッパーフック
- threshold, rootMargin をオプションで指定可能
- 無限スクロール、遅延ロード、スクロールアニメーションに使用
- entry と ref を返す

### 8. useKeyPress
- 特定のキー押下を検知するフック
- 修飾キー（Ctrl, Shift, Alt, Meta）の組み合わせ対応
- キーボードショートカットの実装に使用

### 9. useCopyToClipboard
- テキストをクリップボードにコピーするフック
- コピー成功/失敗の状態を返す
- コピー後一定時間で状態をリセット

### 10. useToggle
- boolean の切り替えフック
- toggle(), setTrue(), setFalse() を提供
- モーダルの開閉等に使用

### 11. useWindowSize
- ウィンドウサイズ（width, height）をリアルタイムに取得するフック
- リサイズイベントのデバウンス付き

### 12. useScrollPosition
- スクロール位置（x, y）を取得するフック
- スクロール方向（上/下）も返す
- ヘッダーの表示/非表示切り替え等に使用

### 13. usePrevious<T>
- 前回のレンダリング時の値を保持するフック

### 14. useIsFirstRender
- 初回レンダリングかどうかを判定するフック

### 15. useDocumentTitle
- document.title を動的に変更するフック
- アンマウント時に元のタイトルに戻すオプション

### 16. useEventListener
- addEventListener/removeEventListener の汎用ラッパー
- Window / Document / Element に対応
- 型安全なイベント型推論

### 17. useFocusTrap
- フォーカストラップを実装するフック
- モーダル、ダイアログ内でのキーボードナビゲーションに使用
- Tab / Shift+Tab でフォーカス対象をループ
- ESC でフォーカストラップを解除

### 18. useOnlineStatus
- ネットワーク接続状態を取得するフック
- online/offline イベントを監視
- オフライン時のフォールバック表示に使用

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- すべてのフックに JSDoc を記述する（@description, @param, @returns, @example）
- 日本語コメントを処理単位で記述する
- any 禁止。ジェネリクスで型安全に実装する
- すべてアロー関数
- useEffect のクリーンアップを必ず実装する（メモリリーク防止）
- テスト: 各フックに対して Vitest + renderHook でテストを作成する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| フック選択 | 不要なフックは省略可能 |
| 追加フック | useGeolocation, useOrientation, useBattery 等を追加可能 |
| テスト | 各フックのテストが不要な場合は省略 |

---

## 関連プロンプト

- [デザインシステム構築](./01_design-system.md) — コンポーネントと連携するフック
- [モーダル/ダイアログシステム](../02_feature-prompts/08_modal-dialog.md) — useFocusTrap, useClickOutside の活用
- [無限スクロール](../02_feature-prompts/09_infinite-scroll-pagination.md) — useIntersectionObserver の活用
- [検索&フィルタリング](../02_feature-prompts/12_search-filter.md) — useDebounce の活用
