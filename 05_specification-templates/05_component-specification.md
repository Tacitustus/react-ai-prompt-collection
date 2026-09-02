# コンポーネント仕様書テンプレート

> **カテゴリ**: 仕様書テンプレート
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★☆☆☆
> **所要時間目安**: 15〜30分（1コンポーネントあたり）

## このプロンプトの使い方

新しいコンポーネントを設計する際に、AIエージェントにこのテンプレートを渡してコンポーネント仕様を定義してもらいます。仕様書をもとに実装することで、一貫性のあるコンポーネントが作成されます。

---

## プロンプト

````text
以下のテンプレートに従い、コンポーネント仕様書を作成してください。
対象コンポーネントごとにセクションを作成し、COMPONENT_SPEC.md として保存してください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コンポーネント仕様書テンプレート
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```markdown
# コンポーネント仕様書

## [コンポーネント名]

### 基本情報

| 項目 | 内容 |
|------|------|
| コンポーネント名 | `ComponentName` |
| ファイルパス | `src/components/atoms/ComponentName.tsx` |
| アトミックデザイン階層 | atom / molecule / organism / template / page |
| 責務 | （このコンポーネントの役割を1文で記述） |

### Props 定義

```typescript
/**
 * @description ComponentName の Props 型定義
 */
interface ComponentNameProps {
  /** バリアント（見た目の種類） */
  variant?: 'primary' | 'secondary' | 'ghost' | 'danger';
  /** サイズ */
  size?: 'sm' | 'md' | 'lg';
  /** 無効状態 */
  disabled?: boolean;
  /** ローディング状態 */
  isLoading?: boolean;
  /** クリックイベントハンドラー */
  onClick?: () => void;
  /** 子要素 */
  children: React.ReactNode;
  /** 追加の className */
  className?: string;
}
```

### バリエーション一覧

| バリアント | 見た目 | 用途 |
|----------|--------|------|
| primary | 青背景 + 白テキスト | CTA、主要アクション |
| secondary | 透明背景 + 青テキスト + 青ボーダー | 補助アクション |
| ghost | 透明背景 + グレーテキスト | テキストボタン |
| danger | 赤背景 + 白テキスト | 削除等の危険操作 |

### 状態遷移

| 状態 | 条件 | 見た目の変化 |
|------|------|------------|
| default | 初期状態 | 通常表示 |
| hover | マウスオーバー | 明るさ+5%, scale(1.02) |
| active | クリック中 | scale(0.98) |
| focus | Tab キーでフォーカス | ring-2 ring-blue-500 ring-offset-2 |
| disabled | disabled=true | opacity-50, cursor-not-allowed, イベント無効 |
| loading | isLoading=true | スピナー表示、テキスト非表示、イベント無効 |

### アクセシビリティ要件

| 要件 | 対応方法 |
|------|---------|
| キーボード操作 | Enter / Space でクリックイベント発火 |
| フォーカスインジケーター | ring-2 で視覚的に明示 |
| disabled 時 | aria-disabled="true" を設定 |
| loading 時 | aria-busy="true" を設定 |
| role | button（HTML button 要素使用で自動設定） |

### 使用例

```tsx
// 基本使用
<Button variant="primary" size="md" onClick={handleClick}>
  保存する
</Button>

// ローディング状態
<Button variant="primary" isLoading>
  保存中...
</Button>

// 無効状態
<Button variant="secondary" disabled>
  送信不可
</Button>

// 危険操作
<Button variant="danger" onClick={handleDelete}>
  削除する
</Button>
```

### テスト要件

| テストケース | 種別 | 確認事項 |
|------------|------|---------|
| レンダリング | 単体 | 各 variant で正しくレンダリングされること |
| クリックイベント | 単体 | onClick が呼ばれること |
| disabled 状態 | 単体 | disabled 時に onClick が呼ばれないこと |
| loading 状態 | 単体 | loading 時にスピナーが表示されること |
| アクセシビリティ | 単体 | aria 属性が正しく設定されること |
| キーボード操作 | 結合 | Enter/Space で動作すること |

### 依存関係

| 依存先 | 種別 | 用途 |
|--------|------|------|
| React | 外部 | コンポーネントフレームワーク |
| clsx | 外部 | className の条件付き結合 |
| lucide-react | 外部 | ローディングアイコン (Loader2) |

---

（次のコンポーネント...）
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 作成時の注意事項
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Props は TypeScript の interface で定義し、各プロパティに JSDoc コメントを付けること
- バリエーションをすべて列挙し、各バリエーションの視覚的な違いと用途を明記すること
- すべての状態（default, hover, active, focus, disabled, loading, error 等）を定義すること
- アクセシビリティ要件を必ず含めること
- 使用例（コードサンプル）を含めること
- テスト要件を含めること
- 完成後、AGENTS.md の規約に照らし合わせてセルフチェックを行い、結果を報告する
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| Props 定義 | コンポーネントの目的に合わせてプロパティを変更 |
| バリエーション | ブランドガイドラインに合わせて追加・削除 |
| アクセシビリティ | WAI-ARIA のデザインパターンに沿って要件を追加 |
| テスト要件 | プロジェクトのテスト方針に合わせて調整 |

---

## 関連プロンプト

- [UI/UX仕様書テンプレート](./03_ui-ux-specification.md) — 画面全体の設計
- [デザインシステム構築](../03_architecture-prompts/01_design-system.md) — デザイントークンの定義
- [テスト戦略](../04_quality-prompts/05_testing-strategy.md) — テスト方針の策定
