# AGENTS.md 運用ガイドライン

> [5 tips for writing better custom instructions for Copilot](https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/) をベースに、AGENTS.md の運用方針と構成をまとめたもの。

---

## 1. AGENTS.md の運用方針

AGENTS.md は、AIコーディングエージェントにリポジトリの文脈を伝えるファイルである。新メンバーのオンボーディングと同じ発想で、「何を作っているか」「どう作っているか」「何を守るべきか」を明文化する。

### 基本原則

- **完璧を求めず、まず書く** — 書き方に唯一の正解はない。不完全でも、何もないよりはるかに効果的。目的は「エージェントが期待する回答にたどり着く確率を高める」こと
- **継続的に育てる** — 一度書いたら終わりではなく、プロジェクトの進化に合わせて更新する。エージェントがミスをしたときが改善の最良のタイミング
- **2ページ以内に収める** — プロジェクト全体に適用できる内容に絞る。ファイルタイプ固有のルールは `.instructions` ファイルに分離する

### 配置ルール

| 配置場所                 | 用途                                                       |
| ------------------------ | ---------------------------------------------------------- |
| リポジトリルート         | プロジェクト全体への指示                                   |
| サブディレクトリ         | パッケージ/モジュール固有の指示（最も近いファイルが優先）  |
| `.instructions` ファイル | 特定ファイルパターン向け（`*.jsx`, `/tests/test_*.py` 等） |

### 目標

1. プロジェクト構成と技術スタックを文書化する
2. 確立されたプラクティスを踏襲させる
3. コマンド実行やビルドの失敗を最小限に抑える

---

## 2. AGENTS.md の構成

以下の5セクションは「要件」ではなく「推奨」だが、含めることで提案品質が大幅に向上する。

### 2.1 Project Overview（プロジェクト概要）

プロジェクトの「エレベーターピッチ」を冒頭に置く。**何を作っているか・誰が使うか・主要機能**を数文で簡潔に記載する。

```markdown
Contoso Companions は、ペット養子縁組機関を支援するWebサイト。
機関は拠点・ペット・イベントを管理でき、里親候補はペット検索・機関発見・申請提出ができる。
```

### 2.2 Tech Stack（技術スタック）

使用技術を**バージョン付き**でリストアップする。「Reactプロジェクト」ではなく「React 18 + TypeScript + Vite + Tailwind CSS」のように具体的に書く。コマンドにはフラグも含める。

```markdown
- Backend: Python 3.12 / Flask
- Frontend: Astro + Svelte
- Database: PostgreSQL 16 / SQLAlchemy
- Testing: pytest -v
- Build: npm run build
```

### 2.3 Coding Guidelines（コーディングガイドライン）

Tech Stack セクションとは分離して持つことを推奨。多くの規約は言語横断で共通し、分けた方が可読性・メンテナンス性が上がる。抽象的な説明より**実際のコード例**を示す方が効果的。

```markdown
### Python
- PEP 8 準拠、型ヒント必須、docstring 必須

### TypeScript
- const をデフォルト、Props には型定義必須
```

### 2.4 Structure（プロジェクト構成）

ディレクトリツリーと各ディレクトリの役割を示す。エージェントがファイル探索やコード配置先を正しく判断できるようになる。

```
├── server/           # Flask バックエンド
│   ├── models/       # SQLAlchemy ORM モデル
│   ├── routes/       # API エンドポイント
│   └── tests/        # ユニットテスト
├── client/src/       # Astro / Svelte フロントエンド
└── scripts/          # 開発・デプロイスクリプト
```

### 2.5 Resources（リソース・ツール）

ビルド・環境セットアップのスクリプト、設定ファイル、外部ドキュメントを網羅する。エージェントの探索時間とコマンド失敗を削減できる。

```markdown
| コマンド         | 説明                 |
| ---------------- | -------------------- |
| npm run dev      | 開発サーバー起動     |
| npm run build    | プロダクションビルド |
| pytest -v        | テスト実行           |
| flask db upgrade | DBマイグレーション   |
```

---

## 補足

### 境界の設定（Boundaries）

3段階で境界を設定すると、破壊的なミスを防げる。

- **Always** — 確認不要で常に実行: テスト実行、命名規則の遵守
- **Ask First** — 人間の承認が必要: DBスキーマ変更、依存追加、CI/CD設定変更
- **Never** — 絶対禁止: シークレットのコミット、vendor/の編集、テストの無断削除

### Bonus Tip — エージェントに書かせる

エージェント自身にリポジトリを分析させ、AGENTS.md のドラフトを生成させることで、ゼロから書く負担を軽減できる。

---

## 参考資料

- [5 tips for writing better custom instructions for Copilot](https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/)
- [How to write a great agents.md: Lessons from over 2,500 repositories](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)
- [AGENTS.md — Official Specification](https://agents.md/)