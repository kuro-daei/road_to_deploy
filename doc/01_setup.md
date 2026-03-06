# 01. 環境構築

WSL・VS Code・Git・GitHub を使える状態にするための初期設定手順です。
この章を終えると、ローカルで書いたコードを GitHub に送る準備が整います。

---

## 1. WSL のインストール

WSL (Windows Subsystem for Linux) は、Windows 上で Linux 環境を動かす仕組みです。

### インストール手順

1. **PowerShell を管理者として起動する**

   スタートメニューで「PowerShell」を右クリック →「管理者として実行」

2. **WSL をインストールする**

   ```powershell
   wsl --install
   ```

   既定で Ubuntu がインストールされます。

3. **PC を再起動する**

4. **Ubuntu を起動する**

   スタートメニューから「Ubuntu」を起動し、初回のユーザー名・パスワードを設定します。

   ```
   Enter new UNIX username: yourname
   New password: ********
   ```

   > パスワード入力時は画面に何も表示されませんが、正しく入力されています。

### WSL が起動できているか確認

```bash
uname -a
```

`Linux` から始まる文字列が表示されれば成功です。

---

## 2. VS Code のインストール

VS Code (Visual Studio Code) はコードを書くためのエディタです。

### インストール手順

1. [https://code.visualstudio.com/](https://code.visualstudio.com/) にアクセス
2. 「Download for Windows」をクリックしてインストーラを取得
3. インストーラを実行し、デフォルト設定のままインストール

### WSL 拡張機能の追加

VS Code で WSL 環境のファイルを編集するには拡張機能が必要です。

1. VS Code を起動
2. 左側の拡張機能アイコン（四角が4つ並んだアイコン）をクリック
3. 検索欄に `WSL` と入力
4. 「WSL」（Microsoft 製）をインストール

### VS Code を WSL から起動する

Ubuntu のターミナルで以下を実行すると、VS Code が WSL モードで起動します。

```bash
code .
```

---

## 3. Git のインストールと初期設定

Git はファイルの変更履歴を管理するツールです。

### Git のインストール

Ubuntu には標準で Git が入っていることが多いですが、念のため確認・インストールします。

```bash
sudo apt update
sudo apt install git -y
```

バージョンを確認します。

```bash
git --version
# git version 2.x.x
```

### ユーザー情報の設定

Git のコミット（変更の記録）に表示される名前とメールアドレスを設定します。
GitHub のアカウント情報と合わせておくと便利です。

```bash
git config --global user.name "Your Name"
git config --global user.email "your@example.com"
```

設定を確認します。

```bash
git config --list
```

### デフォルトブランチ名の設定

現在の標準は `main` です。以下で統一します。

```bash
git config --global init.defaultBranch main
```

---

## 4. GitHub アカウントの作成と SSH 設定

### GitHub アカウント作成

1. [https://github.com](https://github.com) にアクセス
2. 「Sign up」からアカウントを作成

### SSH キーの生成

SSH キーを使うと、GitHub へのアクセス時にパスワードを毎回入力せずに済みます。

```bash
ssh-keygen -t ed25519 -C "your@example.com"
```

質問が表示されますが、すべて Enter キーを押してデフォルトのまま進めてください。

```
Enter file in which to save the key (/home/yourname/.ssh/id_ed25519): [Enter]
Enter passphrase (empty for no passphrase): [Enter]
Enter same passphrase again: [Enter]
```

### 公開鍵を GitHub に登録する

生成された公開鍵を表示します。

```bash
cat ~/.ssh/id_ed25519.pub
```

表示された内容（`ssh-ed25519 AAAA...` から始まる1行）をすべてコピーします。

1. GitHub にログイン
2. 右上のアイコン →「Settings」
3. 左メニュー →「SSH and GPG keys」
4. 「New SSH key」をクリック
5. Title に任意の名前（例: `WSL Ubuntu`）、Key に公開鍵を貼り付けて保存

### SSH 接続をテストする

```bash
ssh -T git@github.com
```

以下のように表示されれば成功です。

```
Hi yourname! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## まとめ

この章で設定した内容：

| 項目 | 確認コマンド |
|---|---|
| WSL / Ubuntu | `uname -a` |
| VS Code (WSL モード) | `code .` で起動 |
| Git | `git --version` |
| Git ユーザー設定 | `git config --list` |
| GitHub SSH 接続 | `ssh -T git@github.com` |

次は [02_github_start.md](./02_github_start.md) でリポジトリを作成し、
最初の commit と push を行います。
