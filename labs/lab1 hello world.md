# 💻 ラボ: HTMLで自己紹介ページを作ろう！

このラボでは、以下のことを学びます：

1. **Visual Studio Code (VS Code)** をインストールする  
2. シンプルな **HTML** ファイルを作成する  
3. **自分の写真と自己紹介** をページに追加する  
4. **Live Server 拡張機能** を使ってブラウザでページを表示する  

---

## 🧰 ステップ1: Visual Studio Code をインストール

1. 公式サイトにアクセス:  
   👉 https://code.visualstudio.com  
2. お使いのOSに合った **ダウンロード** ボタンをクリック  
3. ダウンロードしたファイルを開いて、指示に従ってインストール  
4. インストール後、**Visual Studio Code** を起動  

---

## 📁 ステップ2: プロジェクトフォルダの作成

1. コンピュータ上で新しいフォルダを作成し、名前を `Week01` にする  
2. VS Code を開く  
3. **ファイル → フォルダーを開く** を選び、作成した `Week01` フォルダを開く  

---

## 📝 ステップ3: HTMLファイルを作成

1. VS Codeで **新しいファイル** を作成する  
2. ファイル名を `index.html` として保存  
3. 以下のコードを入力：

```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello from [あなたの名前]</title>
</head>
<body>
  <h1>こんにちは、[あなたの名前]です！</h1>
  <p>これは私の最初のウェブページです。プログラミングや音楽、バスケットボールが好きです。</p>

  <img src="me.jpg" width="200" alt="私の写真">
</body>
</html>
```

---

## 🖼️ ステップ4: 自分の写真を追加

1. 自分の写真（自撮りでもイラストでもOK）を1枚用意  
2. ファイル名を `me.jpg` に変更  
3. その写真を `Week01` フォルダの中にある `index.html` と同じ場所に保存  

フォルダ構成はこうなります：

```
Week01/
├── index.html
└── me.jpg
```

---

## 🌍 ステップ5: ブラウザでページを表示する（Live Server を使用）

1. VS Code左側の「拡張機能」アイコン（四つのブロック）をクリック  
2. 検索ボックスに「Live Server」と入力し、**Live Server（Ritwick Dey 作）** をインストール  
3. `index.html` ファイルを開いた状態で、右下の **「Go Live」** ボタンをクリック  
4. ブラウザが自動で開き、あなたのページが表示される！ 🎉

---

## ✅ チャレンジ

- `<img>` タグの `width="300"` や `height="300"` を使って画像サイズを変えてみよう  
- `<p>` タグを追加して、もっと自分のことを紹介してみよう  

---

## 💡 ヒント

- 編集後はこまめに保存しよう（Windows: Ctrl+S、Mac: Cmd+S）  
- 画像ファイルは必ず `index.html` と同じフォルダに入れる  
- ファイル名は半角英数字、スペースや記号は避けよう  

---


---

## 🔗 追加チャレンジ

### 1. お気に入りのサイトにリンクを追加してみよう

以下のように `<a>` タグを使うとリンクを作成できます：

```html
<p>私の好きなサイトは <a href="https://www.youtube.com" target="_blank">YouTube</a> です。</p>
```

- `href` 属性でリンク先URLを指定します  
- `target="_blank"` を追加すると、新しいタブで開きます  

---

### 2. 地図を埋め込んでみよう（Google マップ）

学校や好きな場所の地図をページに表示するには、Googleマップの埋め込みコードを使います：

1. [Google マップ](https://www.google.co.jp/maps) にアクセス  
2. 場所を検索して、メニューから **「地図を共有または埋め込む」** を選ぶ  
3. 「地図を埋め込む」タブの `<iframe>` コードをコピーして、HTMLに貼り付けます

例：

```html
<iframe src="https://www.google.com/maps/embed?..." width="300" height="200" style="border:0;" allowfullscreen="" loading="lazy"></iframe>
```

---

### 3. 背景色や文字色を変えてみよう（CSS）

HTMLの中にスタイルを追加して、見た目を変えることができます：

```html
<body style="background-color: lightblue; color: darkblue;">
```

- `background-color` で背景色
- `color` で文字の色が変えられます

---

### 4. 箇条書きで趣味を紹介してみよう

```html
<h2>私の趣味</h2>
<ul>
  <li>サッカー</li>
  <li>プログラミング</li>
  <li>アニメ鑑賞</li>
</ul>
```

---

たくさん試して、自分らしいページを作ってみましょう！🌟

---

## 🎯 最終成果物のゴール

このラボの最終的な目標は、次のような構成を持つ自分紹介ページを作ることです：

---

### ✅ ページ構成例

```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello from [あなたの名前]</title>
</head>
<body style="background-color: #f0f8ff; color: #333;">
  <h1>こんにちは、[あなたの名前]です！</h1>

  <img src="me.jpg" width="200" alt="私の写真">

  <p>はじめまして！私は〇〇です。ウェブ開発に興味があり、音楽を聴いたりバスケットボールをするのが好きです。</p>

  <h2>好きなラーメン屋さん</h2>
  <ul>
    <li>一蘭</li>
    <li>天下一品</li>
    <li>中本</li>
  </ul>

  <h2>趣味</h2>
  <ul>
    <li>プログラミング</li>
    <li>音楽鑑賞</li>
    <li>バスケットボール</li>
  </ul>

  <h2>おすすめのサイト</h2>
  <p>私は <a href="https://www.youtube.com" target="_blank">YouTube</a> をよく見ます。</p>

  <h2>好きな場所（Googleマップ埋め込み）</h2>
  <iframe src="https://www.google.com/maps/embed?..." width="300" height="200" style="border:0;" allowfullscreen="" loading="lazy"></iframe>

</body>
</html>
```

---

## 📝 チェックリスト

- [ ] 自分の名前が `<title>` と `<h1>` に表示されている  
- [ ] 自分の写真が表示されている (`me.jpg`)  
- [ ] 自己紹介文が `<p>` タグで書かれている  
- [ ] 好きなもの（例：ラーメン屋）のリストが `<ul>` と `<li>` で書かれている  
- [ ] 趣味のリストも表示されている  
- [ ] おすすめのウェブサイトにリンクがある  
- [ ] Googleマップの埋め込みがある  

---

このページを完成させて、"Week01" フォルダに保存しておきましょう！✨

---

## 💾 ステップ6: 自動保存（Auto Save）をオンにしよう

コードを書いていると、保存し忘れて変更が失われることがあります。  
**VS Codeの「自動保存」機能を使えば安心です！**

### 🔧 Auto Save の設定方法

1. VS Code の上部メニューから **「ファイル」→「自動保存」** をクリック  
   ✅ チェックマークがついていればOK！

または、設定画面からも変更できます：

1. 左下の歯車アイコン → **「設定」** をクリック  
2. 検索バーに `auto save` と入力  
3. 「Auto Save」を `afterDelay` に設定（一定時間後に自動で保存）

これでコードを書いたら自動で保存されるようになります 💡
