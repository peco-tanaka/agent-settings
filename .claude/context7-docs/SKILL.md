---
name: context7-docs
description: ライブラリやAPIの実装前にContext7 MCPサーバーで最新ドキュメントを参照するワークフロー。以下の場合に自動発動する：(1) 外部ライブラリやフレームワークを使ったコード実装、(2) APIクライアントやSDKの利用、(3) パッケージのインストールや設定、(4) 既存の依存ライブラリのアップデート対応。手動で /context7 コマンドでも呼び出し可能。
---

# Context7 Docs - ライブラリ/APIドキュメント参照スキル

ライブラリやAPIの実装コードを書く**前に**、必ずContext7 MCPサーバーを使って最新ドキュメントを参照する。これにより、古い情報や誤った使い方を防ぎ、正確な実装を行う。

## ワークフロー

### Step 1: ライブラリIDの解決

`resolve-library-id` を呼び出し、対象ライブラリのContext7互換IDを取得する。

```
Tool: mcp__plugin_context7_context7__resolve-library-id
Parameters:
  - libraryName: ライブラリ名（例: "react", "express", "prisma"）
  - query: ユーザーの実装タスクの内容
```

- 複数の候補が返される場合、Code Snippet数が多く、Benchmark Scoreが高いものを選択する
- 見つからない場合は、別名やパッケージ名で再検索する（最大3回まで）

### Step 2: ドキュメントの参照

取得したライブラリIDで `query-docs` を呼び出し、実装に必要なドキュメントを取得する。

```
Tool: mcp__plugin_context7_context7__query-docs
Parameters:
  - libraryId: Step 1で取得したID（例: "/vercel/next.js"）
  - query: 実装したい機能に関する具体的な質問
```

- queryは具体的に記述する（悪い例: "auth"、良い例: "How to set up JWT authentication middleware"）
- 1つの質問で不十分な場合、異なるqueryで追加呼び出しする（最大3回まで）

### Step 3: 実装

ドキュメントで確認した内容に基づいてコードを実装する。

- ドキュメントのコード例やAPIシグネチャに従う
- 非推奨（deprecated）のAPIを避ける
- ドキュメントで推奨されているパターンを採用する

## 重要なルール

- **実装コードを書く前に必ずStep 1→2を実行する**。ドキュメント参照をスキップしない
- 複数ライブラリを使う場合、それぞれについてStep 1→2を実行する
- resolve-library-id と query-docs はそれぞれ**1質問あたり最大5回**までの呼び出しに制限する
- ユーザーがライブラリIDを直接指定した場合（"/org/project"形式）、Step 1をスキップしてStep 2から開始する
