# モーダル／ダイアログシステム

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 25〜40分

## このプロンプトの使い方

アクセシビリティ対応のモーダル／ダイアログシステムを構築する際に使用します。確認ダイアログ、フォームモーダル、フルスクリーンモーダル等を含みます。

---

## プロンプト

````text
以下の仕様に従い、モーダル/ダイアログシステムを構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── components/
│   ├── atoms/
│   │   ├── Overlay.tsx            # 背景オーバーレイ
│   │   └── DialogCloseButton.tsx  # 閉じるボタン
│   ├── molecules/
│   │   ├── Modal.tsx              # 汎用モーダル
│   │   ├── ConfirmDialog.tsx      # 確認ダイアログ
│   │   ├── AlertDialog.tsx        # アラートダイアログ
│   │   └── DrawerDialog.tsx       # ドロワー（サイドパネル）
│   └── organisms/
│       └── ModalProvider.tsx      # モーダルのポータルルート
├── hooks/
│   ├── useModal.ts                # モーダル操作フック
│   ├── useFocusTrap.ts            # フォーカストラップ
│   └── useConfirmDialog.ts        # 確認ダイアログフック（Promise ベース）
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ Modal コンポーネント仕様
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Props
- isOpen: boolean（表示状態）
- onClose: () => void（閉じるハンドラー）
- title?: string（モーダルタイトル）
- size?: 'sm' | 'md' | 'lg' | 'xl' | 'full'
- closeOnOverlayClick?: boolean（オーバーレイクリックで閉じるか、デフォルト: true）
- closeOnEscape?: boolean（ESC キーで閉じるか、デフォルト: true）
- showCloseButton?: boolean（×ボタン表示、デフォルト: true）
- children: React.ReactNode

### サイズ
| サイズ | 最大幅 | 用途 |
|--------|--------|------|
| sm | max-w-sm (384px) | 確認ダイアログ |
| md | max-w-md (448px) | フォームモーダル |
| lg | max-w-lg (512px) | コンテンツモーダル |
| xl | max-w-xl (576px) | 大きなコンテンツ |
| full | max-w-full | フルスクリーン |

### アニメーション
- オーバーレイ: opacity 0→0.5, 200ms
- モーダル: scale(0.95→1) + opacity(0→1), 300ms, cubic-bezier(0.34, 1.56, 0.64, 1)
- 閉じる: 逆再生

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ConfirmDialog 仕様
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Promise ベースの確認ダイアログ:

```typescript
const { confirm } = useConfirmDialog();

const handleDelete = async () => {
  const ok = await confirm({
    title: '削除の確認',
    message: 'このアイテムを削除しますか？この操作は取り消せません。',
    confirmLabel: '削除する',
    cancelLabel: 'キャンセル',
    variant: 'danger',
  });
  if (ok) { /* 削除処理 */ }
};
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ アクセシビリティ
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- role="dialog" + aria-modal="true"
- aria-labelledby でタイトルを参照
- aria-describedby でコンテンツを参照
- フォーカストラップ: Tab/Shift+Tab でモーダル内のみ移動
- 初回フォーカス: モーダル内の最初のフォーカス可能要素
- ESC で閉じる
- body のスクロールをロック（overflow: hidden）
- React Portal で DOM ツリーの最上位にレンダリング

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ デザイン
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- グラスモーフィズムのモーダル背景
- オーバーレイ: 黒の半透明 + backdrop-blur
- ダークモード対応
- レスポンシブ: モバイルではフルスクリーンまたは下部スライドイン

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- AGENTS.md のコーディング規約をすべて遵守する
- any 禁止、アロー関数、JSDoc、日本語コメント

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
````

---

## 関連プロンプト

- [カスタムフックライブラリ](../03_architecture-prompts/03_custom-hooks-library.md) — useFocusTrap, useClickOutside
- [アクセシビリティ監査](../04_quality-prompts/03_accessibility-audit.md) — モーダルの a11y チェック
