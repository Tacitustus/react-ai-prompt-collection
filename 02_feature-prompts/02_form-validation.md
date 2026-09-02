# フォーム＆バリデーション

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 30〜45分

## このプロンプトの使い方

React Hook Form + Zod を使った型安全なフォームシステムを構築する際に使用します。再利用可能なフォームコンポーネントとバリデーションスキーマを作成します。

---

## プロンプト

````text
以下の仕様に従い、React Hook Form + Zod によるフォームシステムを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 追加パッケージ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- react-hook-form
- @hookform/resolvers
- zod

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── components/
│   ├── atoms/
│   │   ├── FormInput.tsx         # テキスト入力（RHF 連携）
│   │   ├── FormTextarea.tsx      # テキストエリア（RHF 連携）
│   │   ├── FormSelect.tsx        # セレクトボックス（RHF 連携）
│   │   ├── FormCheckbox.tsx      # チェックボックス（RHF 連携）
│   │   ├── FormRadioGroup.tsx    # ラジオグループ（RHF 連携）
│   │   ├── FormSwitch.tsx        # トグルスイッチ（RHF 連携）
│   │   ├── FormDatePicker.tsx    # 日付選択（RHF 連携）
│   │   ├── FormFileInput.tsx     # ファイル入力（RHF 連携）
│   │   ├── FormErrorMessage.tsx  # エラーメッセージ表示
│   │   └── FormLabel.tsx         # ラベル（必須マーク対応）
│   └── molecules/
│       ├── FormField.tsx         # Label + Input + Error の組み合わせ
│       └── FormSection.tsx       # フォームセクション（見出し + 説明 + フィールド群）
├── hooks/
│   └── useFormWithSchema.ts      # RHF + Zod の統合フック
├── utils/
│   └── validation/
│       ├── schemas.ts            # 共通バリデーションスキーマ
│       ├── messages.ts           # バリデーションメッセージ（日本語）
│       └── rules.ts              # カスタムバリデーションルール
└── types/
    └── form.ts                   # フォーム関連型定義
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 各コンポーネントの仕様
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### FormInput (atoms)
- React Hook Form の Controller / register と連携
- Props: name, control, label, placeholder, type, disabled, helperText
- エラー状態: 赤いボーダー + エラーメッセージ表示
- フォーカス状態: 青いリング
- アイコン対応（左/右）
- パスワード表示/非表示トグル（type="password" 時）

### FormField (molecules)
- FormLabel + FormInput + FormErrorMessage を組み合わせたコンポーネント
- 必須マーク（*）の自動表示
- ヘルパーテキスト対応

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ バリデーションスキーマ（Zod）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
日本語のエラーメッセージで共通バリデーションを定義：

```typescript
// schemas.ts - 例
export const emailSchema = z
  .string()
  .min(1, 'メールアドレスを入力してください')
  .email('有効なメールアドレスを入力してください');

export const passwordSchema = z
  .string()
  .min(8, 'パスワードは8文字以上で入力してください')
  .regex(/[A-Z]/, '大文字を1文字以上含めてください')
  .regex(/[a-z]/, '小文字を1文字以上含めてください')
  .regex(/[0-9]/, '数字を1文字以上含めてください');

export const requiredString = (fieldName: string) =>
  z.string().min(1, `${fieldName}を入力してください`);
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ マルチステップフォーム対応
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
複数ステップに分割されたフォームのためのユーティリティ：
- useMultiStepForm フック
  - currentStep, totalSteps, next, prev, goTo, isFirstStep, isLastStep
- ステップごとのバリデーション（各ステップで部分バリデーション）
- プログレスバー表示
- ステップ間のデータ保持

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ UI デザイン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- フォーカス時のリングアニメーション
- バリデーションエラーのリアルタイム表示（onChange モード）
- 送信時のローディング状態
- 成功時のフィードバック（トースト通知）
- ダークモード対応

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- AGENTS.md のコーディング規約をすべて遵守する
- any 禁止。Zod の z.infer<typeof schema> で型推論する
- すべてアロー関数
- JSDoc 必須
- 日本語コメント
- ジェネリクスを活用して型安全なフォームコンポーネントを作成する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| バリデーションメッセージ | 言語・トーンを調整 |
| フォーム要素 | 必要なフォーム要素を追加・削除 |
| 非同期バリデーション | メールの重複チェック等 |
| ファイルアップロード | ファイル入力が不要なら省略 |

---

## 関連プロンプト

- [認証機能](./01_authentication.md) — ログイン/登録フォームの実装
- [ファイルアップロード](./10_file-upload.md) — ファイル入力フィールドの詳細
- [デザインシステム構築](../03_architecture-prompts/01_design-system.md) — フォーム要素のデザイン定義
