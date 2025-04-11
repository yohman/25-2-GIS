# 🗺️ Lab 3: はじめての MapLibre 地図を作ろう！

このラボでは、**MapLibre GL JS** ライブラリを使って、Web上にインタラクティブな地図を表示します。

---

## 🌐 MapLibreとは？

**MapLibre** は、オープンソースのJavaScriptライブラリで、モダンな地図をWebブラウザ上で表示・操作できます。Mapboxの代替として世界中で使われています。

---

## 📁 ステップ1: プロジェクトフォルダを作成

`Week03` という新しいフォルダを作り、その中に以下のファイルを用意：

```
Week03/
├── index.html
├── style.css
```

---

## 📝 ステップ2: HTMLファイルのコードを書く

`index.html` に以下のコードを貼り付けましょう：

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>My First MapLibre Map</title>
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>

  <script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
  <script>
    const map = new maplibregl.Map({
      container: 'map',
      style: 'https://demotiles.maplibre.org/style.json', // ベースマップ
      center: [139.7671, 35.6812], // 東京駅 (経度, 緯度)
      zoom: 12
    });

    // ポップアップ（吹き出し）を追加
    const popup = new maplibregl.Popup({ offset: 25 })
      .setLngLat([139.7671, 35.6812])
      .setHTML("<h3>東京駅</h3><p>ここが東京駅です！</p>")
      .addTo(map);
  </script>
</body>
</html>
```

---

## 🎨 ステップ3: CSSで地図を全画面に表示する

`style.css` に以下のコードを入力：

```css
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}

#map {
  width: 100%;
  height: 100%;
}
```

これでブラウザの画面いっぱいに地図が表示されます！

---

## 🌍 ステップ4: 中心位置とズームを変更する

`center: [経度, 緯度]` を変更すれば、好きな場所を表示できます：

例：

- 京都駅 → `[135.7586, 34.9855]`
- 札幌駅 → `[141.3545, 43.0687]`
- ロサンゼルス → `[-118.2437, 34.0522]`

`zoom` の数値を上げると拡大、下げると広域表示になります（例： `zoom: 14` は市街地、`zoom: 4` は国全体）

---

## 🗺️ ステップ5: ベースマップの変更

`style: "URL"` を変更することで、ベースマップのデザインを変えられます。

いくつかの例：

- MapLibre公式スタイル（デフォルト）  
  `https://demotiles.maplibre.org/style.json`

- Maptiler（要APIキー）  
  `https://api.maptiler.com/maps/streets/style.json?key=あなたのAPIキー`

- OpenStreetMap系タイルを自作スタイルに組み込むことも可能

---

## ✅ チェックリスト

- [ ] `index.html` と `style.css` を正しく作成できたか？
- [ ] 地図が画面全体に表示されているか？
- [ ] 中心地点を好きな場所に変えられたか？
- [ ] ポップアップが表示されるか？
- [ ] ベースマップのURLを変えてみたか？

---

次回は、地図上に**マーカー**や**ジオJSONデータ**を追加して、よりインタラクティブな地図を作っていきます！
