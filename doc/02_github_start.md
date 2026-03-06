# 02. GitHubをはじめよう

この章では、GitHub にリポジトリを作成し、ローカルの変更を commit・push する
基本的なワークフローを学びます。

---

## 1. リポジトリとは

**リポジトリ（repository）** とは、プロジェクトのファイルとその変更履歴をまとめて保管する場所です。

- **ローカルリポジトリ**: 自分の PC（WSL）上にあるリポジトリ
- **リモートリポジトリ**: GitHub 上にあるリポジトリ

```
┌─────────────────────────── ローカル PC ───────────────────────────┐    ┌─────── GitHub ────────┐
│                                                                   │    │                       │
│  作業ディレクトリ  ──git add──▶  ステージング  ──git commit──▶  ローカルRepo  ──git push──▶  リモートRepo  │
│  (編集中のファイル)              (commitの予告)    (確定した履歴)              ◀──git pull──  (origin)  │
│                                                                   │    │                       │
└───────────────────────────────────────────────────────────────────┘    └───────────────────────┘
```

Git では「作業ディレクトリ → ステージング → ローカルリポジトリ → リモートリポジトリ」という 4 段階でファイルが管理されます。

---

## 2. GitHub でリポジトリを作成する

1. GitHub にログインし、右上の「+」→「New repository」をクリック
2. 以下を設定します

   | 項目 | 設定値 |
   |---|---|
   | Repository name | `my-first-repo`（任意） |
   | Description | 任意 |
   | Public / Private | どちらでも可 |
   | Initialize this repository | **チェックしない** |

3. 「Create repository」をクリック

   > 「Initialize this repository」にチェックを入れると README が自動作成されますが、
   > ここではローカルから push するためチェックしません。

---

## 3. ローカルリポジトリを作成する

WSL のターミナルで作業します。

### プロジェクトフォルダを作成

```bash
mkdir ~/my-first-repo
cd ~/my-first-repo
```

### Git リポジトリを初期化

```bash
git init
```

```
Initialized empty Git repository in /home/yourname/my-first-repo/.git/
```

`.git` という隠しフォルダが作成され、Git の管理が始まります。

---

## 4. ファイルを作成して commit する

### ファイルを作成する

```bash
echo "# はじめてのリポジトリ" > README.md
```

VS Code で編集する場合：

```bash
code .
```

### 状態を確認する

```bash
git status
```

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```

`Untracked files` は、Git がまだ管理していないファイルです。

### ファイルをステージングする（git add）

commit する前に、変更内容を「ステージングエリア」に追加します。

```
┌──────────────────┐  git add   ┌──────────────────┐  git commit  ┌──────────────────┐
│  作業ディレクトリ │ ─────────▶ │  ステージング     │ ───────────▶ │  ローカルRepo    │
│                  │            │                  │              │                  │
│  README.md ✏️  │            │  README.md ✅   │              │  ● abc1234       │
│  （編集済み）    │            │  （commitに含む） │              │  （確定した履歴） │
└──────────────────┘            └──────────────────┘              └──────────────────┘
```

```bash
git add README.md
```

すべてのファイルをまとめてステージングするには：

```bash
git add .
```

再度 `git status` を確認します。

```
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

### commit する（git commit）

ステージングした変更を記録します。`-m` の後にコミットメッセージを書きます。

```bash
git commit -m "feat: READMEを追加"
```

```
[main (root-commit) abc1234] feat: READMEを追加
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

**コミットメッセージのコツ**

- 何をしたかを簡潔に書く
- 日本語でも英語でも OK
- 例: `feat: ログイン機能を追加`, `fix: タイポを修正`, `docs: READMEを更新`

---

## 5. GitHub に push する

### リモートリポジトリを登録する

GitHub で作成したリポジトリの URL を登録します。

```bash
git remote add origin git@github.com:yourname/my-first-repo.git
```

`yourname` は自分の GitHub ユーザー名に置き換えてください。

登録を確認します。

```bash
git remote -v
```

```
origin  git@github.com:yourname/my-first-repo.git (fetch)
origin  git@github.com:yourname/my-first-repo.git (push)
```

### push する（git push）

ローカルの変更を GitHub に送ります。

```bash
git push -u origin main
```

初回は `-u origin main` を付けることで、次回以降は `git push` だけで済みます。

```
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 245 bytes | 245.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To git@github.com:yourname/my-first-repo.git
 * [new branch]      main -> main
branch 'main' set up to track 'remote branch 'main' from 'origin'.
```

GitHub のリポジトリページをブラウザで開くと、ファイルが表示されています。

---

## 6. 変更を加えて再 push する

### ファイルを編集する

README.md に行を追加します。

```bash
echo "はじめての Git & GitHub" >> README.md
```

### 変更を確認する

```bash
git diff
```

```diff
diff --git a/README.md b/README.md
index xxx..yyy 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
 # はじめてのリポジトリ
+はじめての Git & GitHub
```

### add → commit → push

```bash
git add README.md
git commit -m "docs: 説明文を追加"
git push
```

---

## よくあるエラーと対処法

### `Permission denied (publickey)`

SSH キーが GitHub に登録されていない可能性があります。
[01_setup.md](./01_setup.md) の SSH 設定手順を再確認してください。

### `error: remote origin already exists`

すでにリモートが登録されています。URL を変更するには：

```bash
git remote set-url origin git@github.com:yourname/my-first-repo.git
```

---

## まとめ

基本的なワークフローは以下の繰り返しです。

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       Git の基本ワークフロー                                │
│                                                                             │
│  ① ファイルを編集                                                            │
│         │                                                                   │
│         ▼                                                                   │
│  ② git add .        作業ディレクトリ ──▶ ステージング                       │
│         │                                                                   │
│         ▼                                                                   │
│  ③ git commit -m "" ステージング ──▶ ローカルリポジトリ（変更を記録）        │
│         │                                                                   │
│         ▼                                                                   │
│  ④ git push         ローカルリポジトリ ──▶ GitHub（リモートに送る）          │
│                                                                             │
│  ① → ② → ③ → ④ を繰り返す                                                  │
└─────────────────────────────────────────────────────────────────────────────┘
```

次は [03_git_branch.md](./03_git_branch.md) でブランチを使った開発フローを学びます。
