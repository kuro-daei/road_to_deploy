# 03. Gitのブランチをマスターしよう

ブランチを使うと、メインの作業を壊さずに新機能の開発や修正を行えます。
チーム開発では必須のスキルです。この章でブランチの概念から実際の操作まで習得しましょう。

---

## 1. ブランチとは

**ブランチ（branch）** とは、コードの変更履歴を枝分かれさせる仕組みです。

### ブランチがない場合の問題

```
ブランチなし（危険）:

  main: ●─────●─────●─────●  ← 壊れたら即、本番に影響！
        A     B     C     D
                         (壊れた)
```

新機能を試していたら本番コードが壊れた、という事故が起きやすくなります。

### ブランチを使うと

```
ブランチあり（安全）:

  main:    ●─────●──────────────────●  ← 常に安定した状態
           A     B                  E（マージ）
                 │                  ▲
                 └────●────●────────┘
  feature:            C    D
                   （ここで実験的な変更）
```

- `main` ブランチは常に安定した状態を保つ
- 新機能や修正は別ブランチで作業する
- 完成したらブランチを `main` に合流（merge）する

---

## 2. 現在のブランチを確認する

```bash
git branch
```

```
* main
```

`*` が付いているのが現在いるブランチです。

リモートも含めて確認するには：

```bash
git branch -a
```

---

## 3. ブランチを作成する

### 新しいブランチを作成する

```bash
git branch feature/add-about
```

```
実行直後のイメージ:

  HEAD ──▶ main
           │
           ●─────●─────●
           A     B     C
                       └── feature/add-about  ← 同じ場所を指す（まだ main にいる）
```

ブランチ名のルール（推奨）：

| 用途 | 命名例 |
|---|---|
| 新機能追加 | `feature/機能名` |
| バグ修正 | `fix/バグ内容` |
| ドキュメント | `docs/更新内容` |
| リファクタリング | `refactor/対象` |

### ブランチを確認する

```bash
git branch
```

```
  feature/add-about
* main
```

ブランチが作成されましたが、まだ `main` にいます。

---

## 4. ブランチを切り替える

### checkout で移動する

```bash
git checkout feature/add-about
```

```
Switched to branch 'feature/add-about'
```

```
切り替え後のイメージ:

           main
           │
           ●─────●─────●
           A     B     C
                       └── feature/add-about ◀── HEAD  （ここに移動した）
```

### 作成と同時に切り替える（よく使う）

```bash
git checkout -b feature/add-about
```

または（Git 2.23以降）：

```bash
git switch -c feature/add-about
```

現在のブランチを確認します。

```bash
git branch
```

```
* feature/add-about
  main
```

---

## 5. ブランチで作業する

### ファイルを追加・編集する

```bash
echo "# About" > about.md
echo "このプロジェクトについての説明です。" >> about.md
```

### commit する

```bash
git add about.md
git commit -m "feat: Aboutページを追加"
```

この commit は `feature/add-about` ブランチにのみ記録されます。
`main` ブランチには影響しません。

```
commit 後のブランチの状態:

           main
           │
           ●─────●─────●
           A     B     C
                       └── ●─────●  ◀── HEAD (feature/add-about)
                            D     E
                         about.md  about.md
                          追加      更新
```

### main ブランチに戻って確認する

```bash
git checkout main
ls
```

```
main に戻った後:

  HEAD ──▶ main
           │
           ●─────●─────●          ← ここには about.md がない！
           A     B     C
                       └── ●─────●  (feature/add-about)
```

`about.md` が存在しないことが確認できます。ブランチが分離されています。

---

## 6. ブランチを push する

作業ブランチを GitHub に push します。

```bash
git checkout feature/add-about
git push -u origin feature/add-about
```

---

## 7. main ブランチにマージする

### main に切り替える

```bash
git checkout main
```

### マージを実行する

```bash
git merge feature/add-about
```

```
Updating abc1234..def5678
Fast-forward
 about.md | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 about.md
```

`about.md` が `main` ブランチに取り込まれました。

---

## 8. マージの種類

### Fast-forward マージ

`main` ブランチが進んでいない場合、単純にポインタを前に進めます。
履歴が一本線になります。

```
マージ前:
           main
           │
           ●─────●
           A     B
                 └── ●─────●  (feature/add-about)
                      C     D

マージ後（Fast-forward）:
                           main ◀── HEAD
                           │
           ●─────●─────●─────●
           A     B     C     D

特徴: ポインタが前に進むだけ。履歴が一本線になる（ブランチがあった痕跡は消える）
```

### マージコミット

両方のブランチが進んでいる場合、マージコミットが作成されます。

```
マージ前:
           main
           │
           ●─────●─────●
           A     B     E（main 側で別の変更があった）
                 │
                 └── ●─────●  (feature/add-about)
                      C     D

マージ後（--no-ff または両ブランチが進んでいる場合）:
                                 main ◀── HEAD
                                 │
           ●─────●─────●─────────M（マージコミット）
           A     B     E         │
                 │               │
                 └── ●─────●────┘
                      C     D

特徴: マージコミット M が作られる。ブランチがあったことが履歴に残る
```

```bash
git merge --no-ff feature/add-about
```

`--no-ff` オプションでマージコミットを強制的に作成できます。
履歴が分岐していたことが記録に残ります。

---

## 9. コンフリクト（競合）の解決

同じファイルの同じ箇所を両ブランチで変更していた場合、コンフリクトが発生します。

### コンフリクトが発生する状況

```
同じファイルの同じ行を、両ブランチで別々に変更した場合:

  main:    ●─────●─────●  ← README.md を「はじめてのリポジトリ」に変更
           A     B     E
                 │
                 └── ●─────●  (feature/add-about)
                      C     D  ← README.md を「My First Repository」に変更

マージしようとすると → どちらの変更を使えばいい？ → Git が判断できない → コンフリクト！
```

### コンフリクトが発生した場合

```bash
git merge feature/add-about
# CONFLICT (content): Merge conflict in README.md
# Automatic merge failed; fix conflicts and then commit the result.
```

### コンフリクトしたファイルを開く

```
<<<<<<< HEAD
# はじめてのリポジトリ（mainの内容）
=======
# My First Repository（featureの内容）
>>>>>>> feature/add-about
```

| マーカー | 意味 |
|---|---|
| `<<<<<<< HEAD` | 現在のブランチ（main）の内容 |
| `=======` | 区切り線 |
| `>>>>>>> feature/add-about` | マージしようとしたブランチの内容 |

### 手動で解決する

残したい内容だけ残し、マーカーを削除します。

```
# はじめてのリポジトリ
```

### 解決後に commit する

```bash
git add README.md
git commit -m "fix: マージコンフリクトを解決"
```

---

## 10. 不要なブランチを削除する

マージが完了したブランチは削除してリポジトリをきれいに保ちます。

### ローカルブランチを削除

```bash
git branch -d feature/add-about
```

```
Deleted branch feature/add-about (was def5678).
```

### リモートブランチを削除

```bash
git push origin --delete feature/add-about
```

---

## まとめ

ブランチを使った基本的な開発フロー：

```
git pull                       # 最新の main を取得しておく
git checkout -b feature/xxx    # ブランチを作成して移動
# ... ファイルを編集 ...
git add .
git commit -m "feat: xxx"      # ブランチで commit
git push -u origin feature/xxx # GitHub に push
git checkout main
git pull                       # main の最新を取得
git merge feature/xxx          # main にマージ
git push                       # main を GitHub に push
git branch -d feature/xxx      # ローカルブランチを削除
git push origin --delete feature/xxx  # リモートブランチを削除
```

次は [04_markdown.md](./04_markdown.md) で Markdown の書き方を学び、
ドキュメントをブランチ経由で push する実践を行います。
