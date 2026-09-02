# リアルタイム通信（WebSocket / SSE）

> **カテゴリ**: 機能追加
> **前提**: React + TypeScript + TailwindCSS + アトミックデザイン
> **難易度**: ★★★★☆
> **所要時間目安**: 40〜60分

## このプロンプトの使い方

WebSocket または Server-Sent Events（SSE）を使ったリアルタイム通信機能を構築する際に使用します。チャット、通知、ライブフィード等に利用できます。

---

## プロンプト

````text
以下の仕様に従い、リアルタイム通信機能を構築してください。
既存の AGENTS.md および BLUEPRINT.md の規約に従ってください。

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 通信方式
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
【ここに方式を指定: WebSocket / Server-Sent Events (SSE)】

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 構成ファイル
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
src/
├── services/
│   └── websocket/
│       ├── WebSocketClient.ts     # WebSocket クライアントクラス
│       ├── types.ts               # メッセージ型定義
│       └── index.ts
├── hooks/
│   ├── useWebSocket.ts            # WebSocket 接続フック
│   ├── useRealtimeData.ts         # リアルタイムデータ購読フック
│   └── useChat.ts                 # チャット機能フック（オプション）
├── stores/
│   └── useRealtimeStore.ts        # リアルタイム状態管理
├── components/
│   ├── atoms/
│   │   ├── ConnectionStatus.tsx   # 接続状態バッジ
│   │   └── TypingIndicator.tsx    # 入力中インジケーター
│   ├── molecules/
│   │   ├── ChatBubble.tsx         # チャットメッセージ吹き出し
│   │   └── ChatInput.tsx          # チャット入力フォーム
│   └── organisms/
│       └── ChatWindow.tsx         # チャットウィンドウ全体
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ WebSocket クライアント仕様
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 自動再接続（指数バックオフ: 1s → 2s → 4s → 8s → 最大30s）
- 最大リトライ回数の設定（デフォルト: 10回）
- ハートビート（ping/pong、30秒間隔）
- 接続状態の管理: connecting / connected / disconnecting / disconnected / error
- メッセージの型安全な送受信（ジェネリクス）
- イベントベースのメッセージハンドリング（on / off / emit）
- 接続時の認証トークン送信

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ useWebSocket フック
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```typescript
const {
  status,         // 接続状態
  lastMessage,    // 最後に受信したメッセージ
  sendMessage,    // メッセージ送信
  connect,        // 手動接続
  disconnect,     // 手動切断
} = useWebSocket<MessageType>({
  url: 'wss://api.example.com/ws',
  onOpen: () => console.log('接続しました'),
  onMessage: (data) => handleMessage(data),
  onClose: () => console.log('切断しました'),
  onError: (error) => console.error('エラー:', error),
  reconnect: true,
  reconnectInterval: 3000,
  maxReconnectAttempts: 10,
});
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ チャット機能（オプション）
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- メッセージの送受信
- メッセージ一覧のスクロール管理（新着メッセージで自動スクロール）
- 入力中インジケーター（「◯◯さんが入力中...」）
- メッセージの送信状態（送信中 / 送信済み / 既読）
- 日付区切り表示
- リンクの自動検出

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
■ 接続状態 UI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 緑: 接続中 / 赤: 切断 / 黄: 再接続中
- 切断時のリコネクトバナー表示

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

- [API通信レイヤー設計](../03_architecture-prompts/02_api-layer.md) — HTTP API との併用
- [トースト通知システム](./07_notification-toast.md) — リアルタイム通知の表示
- [カスタムフックライブラリ](../03_architecture-prompts/03_custom-hooks-library.md) — useOnlineStatus
