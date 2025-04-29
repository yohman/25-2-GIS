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
      zoom: 1
    });

    // ポップアップ（吹き出し）を追加
    const popup = new maplibregl.Popup({ offset: 25 })
      .setLngLat([139.7671, 35.6812])
      .setHTML("<h3>Hello world!</h3><p>Greetings from Japan!🇯🇵</p>")
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

## 🗺️ ステップ5: ベースマップを国土地理院タイルに変更しよう！

これまで使っていたMapLibreデモスタイル（`https://demotiles.maplibre.org/style.json`）の代わりに、国土地理院（GSI）の標準地図タイルを使ってみましょう。

`index.html` のJavaScript内、`new maplibregl.Map({...})` の `style` の指定を以下のように変更します。

変更前：

```javascript
const map = new maplibregl.Map({
  container: 'map',
  style: 'https://demotiles.maplibre.org/style.json',
  center: [139.7671, 35.6812],
  zoom: 12
});
```

変更後：

```javascript
const map = new maplibregl.Map({
  container: 'map',
  style: {
    "version": 8,
    "sources": {
      "gsi-std": {
        "type": "raster",
        "tiles": [
          "https://cyberjapandata.gsi.go.jp/xyz/std/{z}/{x}/{y}.png"
        ],
        "tileSize": 256,
        "attribution": "© 国土地理院"
      }
    },
    "layers": [
      {
        "id": "gsi-std-layer",
        "type": "raster",
        "source": "gsi-std",
        "minzoom": 0,
        "maxzoom": 18
      }
    ]
  },
  center: [139.7671, 35.6812],
  zoom: 12
});
```

これで国土地理院の標準地図タイルを使った地図が表示されるようになります。

---

## ✅ チェックリスト

- [ ] `index.html` と `style.css` を正しく作成できたか？
- [ ] 地図が画面全体に表示されているか？
- [ ] 中心地点を好きな場所に変えられたか？
- [ ] ポップアップが表示されるか？
- [ ] ベースマップのURLを変えてみたか？

---


## 🧠 チャレンジ（応用課題）

以下の応用課題に挑戦して、地図をもっとカスタマイズしてみましょう！

---

### 🔄 1. 表示位置とポップアップのカスタマイズ

- 地図の中心 (`center`) を自分の好きな場所に変更してみましょう
- ポップアップ (`popup`) のテキストも自由に書き換えてください

```javascript
.setLngLat([経度, 緯度])
.setHTML("<h3>場所の名前</h3><p>ここに説明文を入れましょう。</p>")
```

---

### 📍 2. 複数のマーカーを地図に追加

以下のように複数の場所にマーカーとポップアップを追加してみましょう：

```javascript
const locations = [
  { lng: 139.7671, lat: 35.6812, name: "東京駅" },
  { lng: 135.7586, lat: 34.9855, name: "京都駅" },
  { lng: 141.3545, lat: 43.0687, name: "札幌駅" }
];

locations.forEach(loc => {
  new maplibregl.Marker()
    .setLngLat([loc.lng, loc.lat])
    .setPopup(
      new maplibregl.Popup().setHTML(`<h3>${loc.name}</h3>`)
    )
    .addTo(map);
});
```

---

### 🌍 3. すべてのマーカーが見えるように地図を調整

すべてのマーカーが見えるように `fitBounds()` を使ってみましょう：

```javascript
const bounds = new maplibregl.LngLatBounds();
locations.forEach(loc => bounds.extend([loc.lng, loc.lat]));
map.fitBounds(bounds, { padding: 50 });
```

---

### 🛣️ 4. 2地点間に線（ルート）を描画してみよう

以下のように、2地点の間に線（LineString）を描画してみましょう：

```javascript
map.on('load', () => {
  map.addSource('route', {
    type: 'geojson',
    data: {
      type: 'Feature',
      geometry: {
        type: 'LineString',
        coordinates: [
          [139.7671, 35.6812], // 東京
          [135.7586, 34.9855]  // 京都
        ]
      }
    }
  });

  map.addLayer({
    id: 'route-line',
    type: 'line',
    source: 'route',
    paint: {
      'line-color': '#ff0000',
      'line-width': 4
    }
  });
});
```

---

### 💡 その他のおすすめチャレンジ

- 地図にカスタムアイコンを使ってみよう（`Marker`に画像を指定）  
- ボタンを押すと地図が他の場所にジャンプする機能をつけてみよう  
- `style.css` を編集して、地図の下に自分のプロフィール情報を表示してみよう  
- モバイル対応のレイアウトを試してみよう（`@media` を使う）  

---

チャレンジを自由に組み合わせて、オリジナルな地図ページを作ってみましょう！🌟
