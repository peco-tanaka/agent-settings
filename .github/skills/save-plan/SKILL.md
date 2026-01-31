---
name: save-plan
description: 計画（plan）をmarkdownファイルとして .github/plans/ に保存・管理する。以下の場合に発動する：(1) ユーザーが計画の保存を求めたとき（「planを保存して」「計画をファイルに残して」など）、(2) /save-plan コマンドで呼び出されたとき、(3) plan modeで計画を提示した後にユーザーが承認したとき（「ok」「問題ない」など）、(4) 実装中にplanの更新・修正を求められたとき（「planを更新して」「計画を修正して」など）。計画のファイル保存・更新に関するすべてのリクエストに対応する。
---

# Save Plan - 計画ファイル保存スキル

計画（plan）を `.github/plans/` ディレクトリにmarkdownファイルとして保存・更新する。

## ワークフロー

### Step 1: ディレクトリの準備

`.github/plans/` ディレクトリが存在しない場合、作成する。

```bash
mkdir -p .github/plans
```

### Step 2: ファイル名の決定

```text
.github/plans/YYYY-MM-DD_task-name.md
```

- `YYYY-MM-DD`: 当日の日付
- `task-name`: タスクの概要をkebab-caseで簡潔に表現（英語、3-5語程度）
- 例: `.github/plans/2026-02-01_add-user-authentication.md`
- 同名ファイルが存在する場合、末尾に連番を付与する（例: `_2`）

### Step 3: planの保存

planの全文をそのままmarkdownファイルとして書き出す。見出し・リスト・コードブロックなどのフォーマットはそのまま維持する。

### Step 4: 保存完了の報告

保存したファイルパスをユーザーに報告する。

### 既存planの更新

実装中にplanの内容が変更された場合、`.github/plans/` 内の該当ファイルを最新内容で上書きする。変更前の内容はgitで追跡可能なため、ファイル内に変更履歴は残さない。

## 重要なルール

- planの内容は一切改変せず、そのまま保存する
- `.gitignore` への `.github/plans/` の追加はユーザーの判断に任せる（自動追加しない）
