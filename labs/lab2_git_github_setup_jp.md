# 🧪 Lab 2: GitとGitHubを使ってプロジェクトを管理しよう！

---

## 🔍 このラボで学ぶこと

1. Gitをインストールする  
2. GitHubアカウントを作成する  
3. GitHubにリポジトリを作る（名前は `gis`）  
4. VS CodeでGitHubと接続する（ユーザー名・メールアドレスの設定）  
5. リポジトリをクローンして、Lab 1のファイルを追加  
6. 変更をコミットする  

---

## 🧰 ステップ1: Gitをインストールしよう

**Git** はファイルの履歴を管理するツールです。複数人の共同作業にも便利です。

1. Git公式サイトにアクセス 👉 https://git-scm.com  
2. OSにあったバージョンをダウンロードし、指示に従ってインストール  

インストール後、ターミナル（またはコマンドプロンプト）で以下を入力して確認：

```bash
git --version
```

---

## 🧑‍💻 ステップ2: GitHubアカウントを作成しよう

**GitHub** は、Gitで管理されたコードをクラウド上で保存・共有するサービスです。世界中の開発者が使っています。

1. GitHubにアクセス 👉 https://github.com  
2. アカウントを作成（メールアドレスとパスワードを入力）  
3. サインインしたら、右上の「＋」ボタンから **New repository** を選ぶ  
4. リポジトリ名に `gis` と入力  
5. 「Add a README file」に ✅ を入れる  
6. 「Create repository」をクリック  

---

## 💻 ステップ3: VS CodeでGitHubと連携する

1. VS Code を開いて、**新しいウィンドウ**を開く  
2. ターミナルを開く（`Ctrl + Shift + ~` または メニューから「ターミナル → 新しいターミナル」）  
3. Gitの設定を行う（GitHubとリンクするため）：

```bash
git config --global user.name "GitHubのユーザー名"
git config --global user.email "GitHubに登録したメールアドレス"
```

---

## 📦 ステップ4: リポジトリをクローンする

GitHubの `gis` リポジトリページで **「Code」→「HTTPS」** のURLをコピー  
VS Codeのターミナルで以下を入力：

```bash
git clone https://github.com/your-username/gis.git
```

※ `your-username` は自分のGitHubユーザー名に置き換えてください。

---

## 📁 ステップ5: Week01フォルダを追加する

1. Lab 1で作成した `Week01` フォルダとその中の `index.html`, `me.jpg` をコピー  
2. クローンした `gis` フォルダの中に `Week01` フォルダとして貼り付ける  

---

## 💾 ステップ6: 変更をコミットする

**コミット（commit）** とは、変更内容を記録することです。

1. ターミナルで以下を実行：

```bash
cd gis
git add .
git commit -m "Add Week01 folder with hello page"
```

- `git add .` はすべての変更をステージに追加  
- `git commit -m "メッセージ"` でその変更を記録  

---

これでGitとGitHubを使って、Lab 1の成果物をバージョン管理できるようになりました！✨  
次回は変更をGitHubに**プッシュ（push）**する方法も学びます。

