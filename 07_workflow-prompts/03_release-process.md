# リリースプロセス

> **カテゴリ**: 開発ワークフロー
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★☆☆
> **所要時間目安**: 20〜30分

## このプロンプトの使い方

セマンティックバージョニング、CHANGELOG 自動生成、GitHub Releases を含むリリースプロセスを整備する際に使用します。

---

## プロンプト

````text
以下の仕様に従い、プロジェクトのリリースプロセスを整備してください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ セマンティックバージョニング
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| バージョン | 変更内容 | 例 |
|----------|---------|-----|
| MAJOR (X.0.0) | 破壊的変更、API の非互換 | 1.0.0 → 2.0.0 |
| MINOR (0.X.0) | 後方互換のある新機能 | 1.0.0 → 1.1.0 |
| PATCH (0.0.X) | バグ修正 | 1.0.0 → 1.0.1 |

コミットメッセージからバージョンを自動判定:
- `feat:` → MINOR
- `fix:` → PATCH
- `BREAKING CHANGE:` → MAJOR

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### 1. CHANGELOG.md の自動生成

CHANGELOG.md を Conventional Commits から自動生成する設定:
- `npm install -D standard-version` または `npm install -D @changesets/cli`

```json
// package.json
{
  "scripts": {
    "release": "standard-version",
    "release:minor": "standard-version --release-as minor",
    "release:major": "standard-version --release-as major",
    "release:patch": "standard-version --release-as patch"
  }
}
```

### 2. .versionrc.json（standard-version 設定）

```json
{
  "types": [
    { "type": "feat", "section": "✨ 新機能" },
    { "type": "fix", "section": "🐛 バグ修正" },
    { "type": "docs", "section": "📝 ドキュメント" },
    { "type": "style", "section": "💄 スタイル" },
    { "type": "refactor", "section": "♻️ リファクタリング" },
    { "type": "perf", "section": "⚡ パフォーマンス" },
    { "type": "test", "section": "✅ テスト" },
    { "type": "chore", "section": "🔧 その他", "hidden": true }
  ],
  "commitUrlFormat": "https://github.com/{{owner}}/{{repository}}/commit/{{hash}}",
  "compareUrlFormat": "https://github.com/{{owner}}/{{repository}}/compare/{{previousTag}}...{{currentTag}}"
}
```

### 3. GitHub Actions リリースワークフロー

`.github/workflows/release.yml`:
```yaml
name: Release
on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          generate_release_notes: true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ リリース手順書
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
以下のリリース手順を CONTRIBUTING.md に追記すること：

### 手順
1. `main` ブランチで `npm run release` を実行
   - package.json の version が自動更新される
   - CHANGELOG.md が自動生成される
   - コミット + タグが作成される
2. `git push --follow-tags` でプッシュ
3. GitHub Actions がタグを検知して自動リリース
4. GitHub Releases でリリースノートを確認

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ タスク完了時
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGENTS.md の「タスク完了時の確認事項」に従い報告してください。
````

---

## 関連プロンプト

- [Gitブランチ戦略](./01_git-branching-strategy.md) — ブランチ命名規則
- [PRテンプレート](./02_pr-review-template.md) — マージ前のチェック
