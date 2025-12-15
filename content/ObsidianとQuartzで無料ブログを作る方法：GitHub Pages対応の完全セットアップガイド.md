---
title: "ObsidianとQuartzで無料ブログを作る方法：GitHub Pages対応の完全セットアップガイド"
date: 2025-01-15
tags:
  - Obsidian
  - Quartz
  - ブログ
  - 技術
  - GitHub Pages
  - 静的サイトジェネレーター
draft: false
---

# ObsidianとQuartzで無料ブログを作る方法：GitHub Pages対応の完全セットアップガイド

「ローカルで書いてコマンド一発でデプロイ」——そんな理想的なブログ執筆環境を実現する方法をご紹介します。**Obsidian**と**Quartz**を組み合わせることで、Markdownで書いたメモがそのまま美しいブログとして公開できるようになります。しかも**完全無料**で、GitHub Pagesを使って独自ドメインでの公開も可能です。

## なぜObsidianとQuartzの組み合わせなのか？

従来のブログ執筆では、以下のような課題がありました：

- ブラウザ上で書く必要があり、オフラインで作業できない
- エディタの機能が限られている
- 記事間のリンクが弱い
- ローカルで管理できない
- 有料プラットフォームが多い

**Obsidian**と**Quartz**の組み合わせなら、これらの課題をすべて解決できます。Obsidianで快適に執筆し、Quartzで自動的にブログとして公開する——まさに「第二の脳」から「公開ブログ」への最短ルートです。

## Obsidianとは？その役割と使い方

**Obsidian**は、Markdownベースの知識管理ツールです。以下のような特徴があります：

| 役割 | ツール | 特徴 |
|------|--------|------|
| 同期 | iCloud | MacとiPhoneを全自動でつなぐ |
| 保存/履歴 | GitHub | 過去のバージョンに戻せる・消失リスクゼロ |
| 執筆（脳） | Cursor | AI補完、マルチカーソル、強力な編集機能 |
| 閲覧/整理 | Obsidian | リンクの繋がり、モバイルでの確認、グラフビュー |

Obsidianの最大の魅力は、**WikiLink**（`[[リンク]]`）による記事間のつながりを可視化できることです。知識が有機的に結びつき、新しいアイデアが生まれやすくなります。

### Obsidianでの記事の書き方

ブログ用の記事は、`70_Output/Blog`フォルダに配置します。記事の先頭には、以下のような**フロントマター**（メタデータ）を追加してください：

```markdown
---
title: "記事タイトル"
date: 2025-01-15
tags:
  - タグ1
  - タグ2
draft: false
---

記事の本文...
```

ObsidianのWikiLink（`[[リンク]]`）は、Quartzによって自動的に通常のMarkdownリンクに変換されるため、そのまま使用できます。

## Quartzとは？静的サイトジェネレーターとしての特徴

**Quartz**は、Markdownファイルから静的サイトを生成するツールです。以下のような特徴があります：

- **高速**: 静的サイトなので、読み込みが速い
- **シンプル**: Markdownファイルをそのまま使用できる
- **カスタマイズ可能**: テーマやプラグインで自由に拡張できる
- **無料でホスティング可能**: GitHub Pagesで無料公開できる
- **SEOに強い**: 静的サイトなので検索エンジンに優しい

## Quartzのセットアップ手順

### 前提条件

- Node.js（v18以上）がインストールされていること
- Gitがインストールされていること
- GitHubアカウント（無料で作成可能）

### 1. Quartzプロジェクトの初期化

**重要**: Quartzプロジェクトは**vaultの外**に配置してください。以下の理由があります：

- ビルドファイル（`node_modules`, `dist`など）が大量に生成されるため、iCloud Driveの同期が重くなる
- Obsidianのパフォーマンスに影響する可能性がある
- Git管理がしやすくなる

#### 推奨配置場所

**`/dev/quartz-blog`**に配置します。

**避けるべき場所**:
- ❌ iCloud Drive内（同期が重くなる）
- ❌ vault内（Obsidianのパフォーマンスに影響）

#### 方法A: GitHubテンプレートを使用（推奨）

```bash
# プロジェクト用のディレクトリを作成
mkdir -p /dev/quartz-blog
cd /dev/quartz-blog

# GitHubテンプレートからリポジトリを作成
# ブラウザで https://github.com/jackyzha0/quartz にアクセス
# 「Use this template」→「Create a new repository」を選択
# その後、作成したリポジトリをクローン：
git clone https://github.com/あなたのユーザー名/quartz-blog.git .
```

**リポジトリの公開設定について**:

- **Public（公開）**: 
  - ✅ GitHub Pagesが無料で使える（推奨）
  - ✅ ソースコードが公開される（設定ファイルなど）
  - ✅ コミュニティからの貢献を受けやすい
  - **ブログを公開する予定ならこちらを推奨**

- **Private（非公開）**:
  - ⚠️ GitHub Pagesを使う場合、有料プラン（GitHub Pro以上）が必要
  - ✅ ソースコードが非公開
  - ✅ 内部ドキュメントやプライベートなメモに適している

**推奨**: ブログを公開する予定であれば**Public**を選択してください。GitHub Pagesが無料で使えます。

#### 方法B: 直接クローン

```bash
# プロジェクト用のディレクトリを作成
mkdir -p /dev/quartz-blog
cd /dev/quartz-blog

# Quartzリポジトリをクローン
git clone https://github.com/jackyzha0/quartz.git .
```

#### 依存関係のインストール

どちらの方法でも、以下のコマンドで依存関係をインストールします：

```bash
npm install
```

### 2. コンテンツディレクトリの設定

Quartzの設定ファイル`quartz.config.ts`を編集して、コンテンツディレクトリをvaultの`70_Output/Blog`に設定します。

**`quartz.config.ts`は、Quartzプロジェクトのルートディレクトリ（`/dev/quartz-blog`）にあります。**

`quartz.config.ts`を開き、`source`セクションの`contentDir`を以下のように変更：

```typescript
const config: QuartzConfig = {
  // ... 他の設定 ...
  source: {
    contentDir: "/Users/kenshintanaka/Library/Mobile Documents/iCloud~md~obsidian/Documents/MyVault/70_Output/Blog",
    // ... 他の設定 ...
  },
  // ... 他の設定 ...
}
```

**注意**: パス内のスペースや特殊文字（`~`など）はそのまま使用できますが、絶対パスを指定してください。

### 3. ビルドとプレビュー

```bash
# 開発サーバーを起動（プレビュー）
npm run start

# 本番用ビルド
npm run build
```

ブラウザで`http://localhost:1313`にアクセスしてプレビューを確認できます。

### 4. GitHub Pagesへのデプロイ

#### GitHub Pagesを使用する場合

1. GitHubにリポジトリを作成
2. 以下のコマンドでデプロイ：

```bash
npm run deploy
```

#### 独自ドメインを設定する場合

1. `quartz/static`フォルダ内に`CNAME`ファイルを作成（中身はドメイン名のみ）
2. DNS設定（お名前.com等）でGitHubのAレコード4つと、www用のCNAMEを設定
3. `quartz.config.ts`の`baseUrl`を独自ドメインに変更

## 日々の運用方法

Obsidianの`70_Output/Blog`フォルダで記事を書いた後、**Quartzのフォルダ（terminal）で**以下のワンライナーを実行します。

> [!IMPORTANT] 更新コマンド
> Symlink（ショートカット）ではなく実ファイルをコピーして送る必要があるため、必ずこのコマンドを使います。
> （Obsidian上のgitプラグインではなく、Cursorのターミナルで行います）

```bash
rm -rf content && cp -r "/Users/kenshintanaka/Library/Mobile Documents/iCloud~md~obsidian/Documents/MyVault/70_Output/Blog" content && git add . && git commit -m "New post" && git push
```

このコマンドを実行すると、以下のことが自動的に行われます：

1. 既存の`content`フォルダを削除
2. Obsidianの`70_Output/Blog`フォルダを`content`にコピー
3. Gitに変更を追加
4. コミット
5. GitHubにプッシュ
6. GitHub Actionsが自動的にデプロイを実行

## よくあるトラブルと解決策

### Gitの権限エラー（403 Permission denied）

**症状**: `git push`したらPermission deniedになった。

**原因**: 送信先（remote origin）がQuartz公式リポジトリに向いていた。

**解決**: リモートURLを自分のリポジトリに変更。

```bash
git remote set-url origin <自分のリポジトリURL>
```

### GitHub Actionsが動かない

**症状**: Pushしてもデプロイが始まらない。「Deploy Quartz Site」がない。

**原因**: `.github/workflows/deploy.yml`が存在しない、またはGitHub Pagesの設定が無効だった。

**解決**:
1. `deploy.yml`を作成（コードは公式準拠）
2. GitHub Settings > Actions > GeneralでRead and write permissionsを付与
3. GitHub Settings > PagesでSourceをGitHub Actionsに変更

### サイトではなくXMLが表示される / 404エラー

**症状**: アクセスするとRSSフィードが表示される。

**原因**: トップページとなる`index.md`が空っぽだったため、HTMLが生成されなかった。

**解決**: `index.md`にタイトルや本文を追記してプッシュ。

### nothing to commit（更新されない）

**症状**: Obsidianを更新したのにGitが変更を検知しない。

**原因**: `ln -s`で作ったシンボリックリンクの実体ファイルを、Gitは追跡してくれないため。

**解決**: リンク運用をやめ、`cp -r`コマンドでフォルダごとコピーする運用（上記のワンライナー）に変更。

## まとめ

ObsidianとQuartzの組み合わせにより、以下のような理想的なブログ執筆環境が実現できます：

- ✅ ローカルで快適に執筆できる（Obsidian）
- ✅ 記事間のつながりを可視化できる（WikiLink）
- ✅ コマンド一発でデプロイできる（Quartz）
- ✅ 完全無料でホスティングできる（GitHub Pages）
- ✅ 独自ドメインで公開できる
- ✅ SEOに強い静的サイト

「第二の脳」としてのObsidianと、公開ブログとしてのQuartz——この組み合わせで、知識管理と情報発信の両方を効率的に行えるようになります。初心者でも、このガイドに従えば30分程度でセットアップが完了します。

## 参考リンク

- [Quartz公式ドキュメント](https://quartz.jzhao.xyz/)
- [Quartz GitHub](https://github.com/jackyzha0/quartz)

