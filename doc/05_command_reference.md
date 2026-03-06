# 05. コマンドリファレンス

Git・WSL でよく使うコマンドの早見表です。
作業中に「あのコマンドなんだっけ？」と思ったときに参照してください。

---

## Git 基本操作

### リポジトリの初期化・クローン

| コマンド | 説明 |
|---|---|
| `git init` | カレントディレクトリを Git リポジトリとして初期化 |
| `git clone <URL>` | リモートリポジトリをローカルにコピー |
| `git clone <URL> <dir>` | 指定ディレクトリ名でクローン |

```bash
# 例
git init
git clone git@github.com:yourname/repo.git
git clone git@github.com:yourname/repo.git my-project
```

---

### 状態確認

| コマンド | 説明 |
|---|---|
| `git status` | 変更ファイルの状態を確認 |
| `git diff` | 未ステージの変更内容を表示 |
| `git diff --staged` | ステージ済みの変更内容を表示 |
| `git log` | コミット履歴を表示 |
| `git log --oneline` | 1行形式でコミット履歴を表示 |
| `git log --graph` | ブランチのグラフ付きで履歴表示 |

```bash
# よく使う組み合わせ
git log --oneline --graph --all
```

---

### ステージング（add）

| コマンド | 説明 |
|---|---|
| `git add <file>` | 指定ファイルをステージング |
| `git add .` | すべての変更ファイルをステージング |
| `git add -p` | 変更を対話的に選んでステージング |
| `git restore --staged <file>` | ステージングを取り消す |

```bash
git add README.md
git add .
git restore --staged README.md
```

---

### コミット（commit）

| コマンド | 説明 |
|---|---|
| `git commit -m "<message>"` | メッセージを付けてコミット |
| `git commit --amend` | 直前のコミットを修正（メッセージや内容）⚠️ push 済みには使わない |
| `git revert <hash>` | 指定コミットを打ち消すコミットを作成 |

```bash
git commit -m "feat: ログイン機能を追加"
git commit --amend -m "feat: ログイン機能を実装"
```

**コミットメッセージの prefix（推奨）**

| prefix | 用途 |
|---|---|
| `feat:` | 新機能 |
| `fix:` | バグ修正 |
| `docs:` | ドキュメント |
| `refactor:` | リファクタリング |
| `test:` | テスト |
| `chore:` | ビルド・設定変更 |

---

### リモート操作（push / pull / fetch）

| コマンド | 説明 |
|---|---|
| `git remote add origin <URL>` | リモートを登録 |
| `git remote -v` | 登録済みリモートを確認 |
| `git remote set-url origin <URL>` | リモートURLを変更 |
| `git push origin <branch>` | ブランチをリモートに送る |
| `git push -u origin <branch>` | 上流ブランチを設定しながら push |
| `git push` | 上流ブランチへ push（設定済みの場合） |
| `git pull` | リモートの変更をローカルに取り込む |
| `git fetch` | リモートの情報を取得（マージしない） |

```bash
git remote add origin git@github.com:yourname/repo.git
git push -u origin main
git pull
git fetch origin
```

---

## ブランチ操作

| コマンド | 説明 |
|---|---|
| `git branch` | ローカルブランチ一覧 |
| `git branch -a` | リモート含む全ブランチ一覧 |
| `git branch <name>` | ブランチを作成 |
| `git branch -d <name>` | ブランチを削除（マージ済み） |
| `git branch -D <name>` | ブランチを強制削除 |
| `git checkout <branch>` | ブランチを切り替え |
| `git checkout -b <branch>` | ブランチを作成して切り替え |
| `git switch <branch>` | ブランチを切り替え（推奨） |
| `git switch -c <branch>` | ブランチを作成して切り替え（推奨） |
| `git merge <branch>` | 指定ブランチを現在のブランチにマージ |
| `git merge --no-ff <branch>` | マージコミットを作成してマージ |

```bash
git checkout -b feature/new-feature
# 作業...
git checkout main
git merge feature/new-feature
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

---

## 変更の取り消し

| コマンド | 説明 |
|---|---|
| `git restore <file>` | 未ステージの変更を取り消す |
| `git restore --staged <file>` | ステージングを取り消す |
| `git reset HEAD~1` | 直前のコミットを取り消す（変更は残る） |
| `git reset --hard HEAD~1` | 直前のコミットを完全に取り消す（変更も消える）|

> `git reset --hard` は変更が消えるため慎重に使用してください。

---

## Git の設定

| コマンド | 説明 |
|---|---|
| `git config --global user.name "<name>"` | ユーザー名を設定 |
| `git config --global user.email "<email>"` | メールアドレスを設定 |
| `git config --global init.defaultBranch main` | デフォルトブランチを `main` に設定 |
| `git config --list` | 設定一覧を表示 |

---

## WSL / Linux 基本コマンド

| コマンド | 説明 |
|---|---|
| `pwd` | 現在のディレクトリを表示 |
| `ls` | ファイル・ディレクトリ一覧 |
| `ls -la` | 隠しファイルも含めた詳細一覧 |
| `cd <dir>` | ディレクトリを移動 |
| `cd ..` | 1つ上のディレクトリに移動 |
| `cd ~` | ホームディレクトリに移動 |
| `mkdir <dir>` | ディレクトリを作成 |
| `touch <file>` | 空ファイルを作成 |
| `cat <file>` | ファイルの内容を表示 |
| `echo "<text>" > <file>` | テキストをファイルに書き込む（上書き） |
| `echo "<text>" >> <file>` | テキストをファイルに追記 |
| `rm <file>` | ファイルを削除 |
| `rm -rf <dir>` | ディレクトリを再帰的に削除（注意）|
| `code .` | VS Code をカレントディレクトリで開く |

---

## よくある操作の手順まとめ

### はじめて push するまで

```bash
# リポジトリ初期化
git init
git remote add origin git@github.com:yourname/repo.git

# ファイルを追加して commit
git add .
git commit -m "feat: 初回コミット"

# push
git push -u origin main
```

### 日常的な作業フロー

```bash
git pull                              # 最新を取得
git checkout -b feature/xxx           # ブランチを作成
# ... 編集 ...
git add .
git commit -m "feat: xxxを追加"
git push -u origin feature/xxx        # push
git checkout main
git pull                              # main の最新を取得
git merge feature/xxx                 # マージ
git push                              # main を push
git branch -d feature/xxx             # ローカルブランチ削除
git push origin --delete feature/xxx  # リモートブランチ削除
```

### コンフリクトを解決する

```bash
git merge feature/xxx
# CONFLICT が発生
# ファイルを編集して <<<, ===, >>> を削除
git add .
git commit -m "fix: マージコンフリクトを解決"
```

---

## SSH 関連

| コマンド | 説明 |
|---|---|
| `ssh-keygen -t ed25519 -C "<email>"` | SSH キーを生成 |
| `cat ~/.ssh/id_ed25519.pub` | 公開鍵を表示 |
| `ssh -T git@github.com` | GitHub への SSH 接続テスト |
