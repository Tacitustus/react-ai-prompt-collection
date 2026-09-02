# ダークモード切り替え

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★☆☆☆
> **所要時間目安**: 20〜30分

## このプロンプトの使い方

TailwindCSS の dark: プレフィックスを使ったダークモード切り替え機能を実装する際に使用します。

---

## プロンプト

````text
以下の仕様に従い、ダークモード切り替え機能を実装してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 実装する機能
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. **3段階テーマ切り替え**: Light / Dark / System（OS設定連動）
2. **永続化**: 選択したテーマを localStorage に保存
3. **OS 設定との連動**: System 選択時に prefers-color-scheme を監視
4. **トグルボタン**: アイコンアニメーション付き切り替えUI
5. **ちらつき防止**: ページロード時のテーマ未適用状態（FOUC）を防止

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── stores/
│   └── useThemeStore.ts         # テーマ状態管理（Zustand + persist）
├── hooks/
│   └── useTheme.ts              # テーマ操作フック
├── components/
│   └── atoms/
│       └── ThemeToggle.tsx       # テーマ切り替えボタン
├── styles/
│   └── globals.css              # CSS カスタムプロパティ（Light/Dark）
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ TailwindCSS 設定
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tailwind.config.ts で darkMode: 'class' を設定する。
`<html>` 要素に `dark` クラスを動的に付与/削除して切り替える。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ FOUC 防止
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
index.html の `<head>` 内に以下のインラインスクリプトを配置：
```html
<script>
  (function() {
    const stored = localStorage.getItem('theme-storage');
    const theme = stored ? JSON.parse(stored).state.theme : 'system';
    const isDark = theme === 'dark' || 
      (theme === 'system' && window.matchMedia('(prefers-color-scheme: dark)').matches);
    if (isDark) document.documentElement.classList.add('dark');
  })();
</script>
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ ThemeToggle コンポーネント
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Sun / Moon / Monitor アイコン（lucide-react）
- Light → Dark → System の順で切り替え（クリックで巡回）
- アイコン切り替え時の回転アニメーション
- ツールチップで現在のテーマを表示
- アクセシビリティ: aria-label で「テーマを切り替え」

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ CSS カスタムプロパティ（オプション）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TailwindCSS の dark: プレフィックスに加えて、CSS カスタムプロパティでも管理：
```css
:root {
  --color-bg-primary: theme('colors.white');
  --color-text-primary: theme('colors.slate.900');
}
.dark {
  --color-bg-primary: theme('colors.slate.900');
  --color-text-primary: theme('colors.slate.50');
}
```

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

- [デザインシステム構築](../03_architecture-prompts/01_design-system.md) — カラーパレットの Light/Dark 定義
- [状態管理パターン集](../03_architecture-prompts/04_state-management-patterns.md) — テーマストア設計
