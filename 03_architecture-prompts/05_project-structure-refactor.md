# ディレクトリ構成リファクタリング

> **カテゴリ**: アーキテクチャ設計
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★☆
> **所要時間目安**: 45〜90分

## このプロンプトの使い方

プロジェクトが成長して、レイヤーベースの構成（components/, hooks/, utils/ 等）では管理しきれなくなった場合に、機能ベース（Feature-Based）のディレクトリ構成へ移行する際に使用します。

---

## プロンプト

````text
以下の仕様に従い、プロジェクトのディレクトリ構成を機能ベース（Feature-Based）にリファクタリングしてください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リファクタリング前（レイヤーベース）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── components/
│   ├── atoms/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── ...
│   ├── molecules/
│   ├── organisms/
│   ├── templates/
│   └── pages/
├── hooks/
│   ├── useAuth.ts
│   ├── useUsers.ts
│   └── ...
├── stores/
│   ├── useAuthStore.ts
│   └── ...
├── services/
│   ├── authService.ts
│   └── ...
├── types/
│   ├── User.ts
│   └── ...
└── utils/
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リファクタリング後（ハイブリッド構成）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
アトミックデザインの共有コンポーネントは維持しつつ、機能単位でコードをまとめる：

```
src/
├── components/                 # 共有コンポーネント（アトミックデザイン）
│   ├── atoms/                  # ← 変更なし: プロジェクト共通の最小パーツ
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   └── index.ts
│   │   ├── Input/
│   │   └── ...
│   ├── molecules/
│   ├── organisms/              # 共有 organisms のみ（Header, Footer 等）
│   └── templates/              # レイアウトテンプレート
│
├── features/                   # 機能単位のモジュール
│   ├── auth/                   # 認証機能
│   │   ├── components/         # 認証固有のコンポーネント
│   │   │   ├── LoginForm.tsx
│   │   │   ├── RegisterForm.tsx
│   │   │   └── ProtectedRoute.tsx
│   │   ├── hooks/
│   │   │   ├── useAuth.ts
│   │   │   └── useLogin.ts
│   │   ├── stores/
│   │   │   └── useAuthStore.ts
│   │   ├── services/
│   │   │   └── authService.ts
│   │   ├── types/
│   │   │   └── auth.ts
│   │   └── index.ts            # パブリック API（外部に公開するもの）
│   │
│   ├── dashboard/              # ダッシュボード機能
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── types/
│   │   └── index.ts
│   │
│   ├── users/                  # ユーザー管理機能
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── types/
│   │   └── index.ts
│   │
│   └── settings/               # 設定機能
│       ├── components/
│       └── index.ts
│
├── pages/                      # ルーティング対象のページ
│   ├── HomePage.tsx
│   ├── LoginPage.tsx
│   ├── DashboardPage.tsx
│   └── NotFoundPage.tsx
│
├── hooks/                      # 共有カスタムフック（機能横断）
│   ├── useDebounce.ts
│   ├── useLocalStorage.ts
│   └── ...
│
├── stores/                     # 共有ストア（機能横断）
│   ├── useThemeStore.ts
│   ├── useUIStore.ts
│   └── ...
│
├── services/                   # 共有サービス（API クライアント等）
│   └── api/
│       ├── client.ts
│       └── interceptors.ts
│
├── utils/                      # 共有ユーティリティ
├── types/                      # 共有型定義
├── styles/                     # グローバルスタイル
├── constants/                  # 共有定数
├── assets/                     # 静的アセット
├── App.tsx
├── main.tsx
└── vite-env.d.ts
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リファクタリングのルール
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### 1. 機能モジュールの原則
- 各 feature/ フォルダは自己完結的であること
- feature の内部構造は自由だが、外部への公開は index.ts のみ
- feature 間の直接参照は禁止。共有コンポーネント/フック経由で連携する

### 2. 共有 vs 機能固有の判断基準
| 条件 | 配置先 |
|------|--------|
| 2つ以上の機能で使用される | `src/components/`, `src/hooks/`, `src/utils/` |
| 1つの機能でしか使用されない | `src/features/<feature>/` 内 |
| ページコンポーネント | `src/pages/` |
| レイアウトテンプレート | `src/components/templates/` |

### 3. インポートパス規約
```typescript
// ✅ 共有コンポーネントは @/ エイリアスで
import { Button } from '@/components/atoms/Button';

// ✅ feature の公開 API は index.ts 経由で
import { LoginForm, useAuth } from '@/features/auth';

// ❌ feature の内部ファイルを直接参照しない
import { LoginForm } from '@/features/auth/components/LoginForm';
```

### 4. パスエイリアスの追加
tsconfig.json と vite.config.ts に以下のエイリアスを追加する：
```json
{
  "@/*": ["src/*"],
  "@/features/*": ["src/features/*"],
  "@/components/*": ["src/components/*"]
}
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 移行手順
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. 新しいディレクトリ構造を作成する
2. 機能ごとにファイルを移動する
3. index.ts でパブリック API を定義する
4. インポートパスをすべて更新する
5. ESLint でインポート順序を確認する
6. TypeScript コンパイルエラーが 0 件であることを確認する
7. ビルドが成功することを確認する
8. すべてのテストが通ることを確認する

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ コーディング規約
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- AGENTS.md のコーディング規約をすべて遵守する
- 既存のコメント・JSDoc はすべて保持する
- 機能変更は一切行わない（純粋なリファクタリング）
- Git のコミットは段階的に行う（ディレクトリ作成 → ファイル移動 → インポート修正）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い、すべてのチェックを実施して報告してください。
特に以下を追加確認すること：
- 機能変更がないこと（リファクタリングのみであること）
- feature 間の直接参照がないこと
- すべてのインポートパスが正しいこと
````

---

## カスタマイズポイント

| 箇所 | 説明 |
|------|------|
| feature 一覧 | プロジェクトの機能に合わせて feature を定義 |
| 共有の粒度 | 小規模プロジェクトでは feature 分割を最小限に |
| 段階的移行 | 一度にすべてを移行せず、新機能から順次適用も可能 |

---

## 関連プロンプト

- [技術設計書テンプレート](../05_specification-templates/02_technical-design.md) — ディレクトリ構成の設計
- [コードレビューチェックリスト](../04_quality-prompts/01_code-review-checklist.md) — リファクタリング後のレビュー
- [レガシーコード刷新](../06_refactoring-prompts/01_legacy-modernization.md) — 併用可能なリファクタリング
