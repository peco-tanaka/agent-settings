---
name: ship
description: >
  git add → commit → push → PR作成の一連のワークフローを実行する。
  以下の場合に使用する：
  (1) ユーザーが変更をコミットしてPRを作成したいとき
  (2) 「PRを出して」「shipして」「マージリクエスト作って」などの指示
  (3) /ship コマンドで呼び出されたとき
  コミットメッセージはConventional Commits準拠のフォーマットを使用し、
  PRは作成前にユーザー確認を挟む。
---

# Ship

git add → commit → push → PR作成を一連で実行するワークフロー。

## ワークフロー

### Step 1: 変更の確認

`git status` と `git diff` で変更内容を把握する。

### Step 2: 適切な粒度でステージング

- 関連する変更のみをまとめてステージングする
- 無関係な変更が混在している場合、ユーザーに分割を提案する
- `.env`、credentials、秘密鍵など機密ファイルは除外し、警告する
- `git add -A` は使わず、ファイル名を明示的に指定する

### Step 3: コミットメッセージ作成

[assets/commit-message-format.md](assets/commit-message-format.md) を読み込み、フォーマットに従ってコミットメッセージを作成する。

- prefix は Conventional Commits に準拠（feat, fix, docs, refactor 等）
- summary は日本語で簡潔に
- 変更内容を分析し、適切な prefix を自動選択する

コミットメッセージの末尾に以下を付与する：

```
Co-Authored-By: Claude <noreply@anthropic.com>
```

### Step 4: プッシュ

- 現在のブランチをリモートにプッシュする
- トラッキングブランチが未設定の場合は `-u origin <branch>` を使用する
- main/master への直接プッシュの場合は警告し、ユーザーに確認する

### Step 5: PR作成（ユーザー確認あり）

まず「PRを作成しますか？」とユーザーに確認する。

ユーザーが承認したら、以下の情報を提示する：

- PRタイトル（70文字以内）
- PRの本文プレビュー
- ベースブランチ
- 含まれるコミット一覧

内容に問題がなければ、[assets/pr-template.md](assets/pr-template.md) のテンプレートに沿って `gh pr create` で PR を作成する。

## 注意事項

- `--force` オプションは使用しない
- `--no-verify` は使用しない（pre-commit hookを尊重する）
- hook失敗時は問題を修正して新しいコミットを作成する（`--amend` しない）
- PR作成後、URLをユーザーに返す
