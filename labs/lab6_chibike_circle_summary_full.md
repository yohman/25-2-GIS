# Lab 5: 地図上でバッファを作って件数を数えよう

まずは Visual Studio Code を開き、先週までのプロジェクトと同じ場所に `week05` というフォルダを作成しましょう。
このフォルダに今回の `index.html` と `chibike.geojson` を保存して開発を進めます。

このラボでは、**Turf.js**という地理解析ライブラリを使って、地図上で「バッファ」（ある地点からの一定距離範囲）を描き、その範囲内に含まれるポイント（自転車盗難）をカウントする分析を行います。

Turf.jsは、JavaScript上でジオメトリの操作や空間分析（距離測定、重なり判定、統計処理など）を行うためのライブラリです。このラボでは、Turfの `circle()` 関数と `booleanPointInPolygon()` 関数を使って、バッファ分析の基本を体験します。

具体的には、千葉県の自転車盗難データを地図に表示し、地図上でクリックした場所の半径1km以内にある盗難件数を表示するアプリを作成します。

---

## ステップ1：MapLibre地図を作成し、Turf.jsを読み込む

このステップでは、MapLibreを使って地図のベース（Googleタイル）を表示し、空間演算用のTurf.jsを読み込みます。地図の表示は `#map` 要素内に行われ、中心とズームは東京駅に初期設定しています。

1. `index.html` を作成し、以下のコードを記述：

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Chiba Bike Theft Analysis</title>
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100%; height: 100vh; }
  </style>
</head>
<body>
<div id="map"></div>
<script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
<script src="https://unpkg.com/@turf/turf@6.5.0/turf.min.js"></script>
<script>
  const map = new maplibregl.Map({
    container: 'map',
    style: {
      version: 8,
      sources: {
        'google-tiles': {
          type: 'raster',
          tiles: ['https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}'],
          tileSize: 256
        }
      },
      layers: [
        {
          id: 'google-tiles-layer',
          type: 'raster',
          source: 'google-tiles',
          minzoom: 0,
          maxzoom: 19
        }
      ]
    },
    center: [139.7671, 35.6812],
    zoom: 2
  });
</script>
</body>
</html>
```

---

## ステップ2：GeoJSONデータを読み込み、地図全体にフィット

このステップでは、`chibike.geojson` を読み込み、自転車盗難のデータポイントを赤い円でマップに表示します。さらに、Turf.jsの `bbox()` 関数を使って、データの全体範囲にズームします。

* `map.addSource('bike', {...})`：GeoJSON形式の盗難データをソースとして追加します。
* `map.addLayer({ id: 'bike-layer', ... })`：データを視覚的に表示するためのレイヤー。赤い円で表現しています。
* `fetch(...).then(...)`：GeoJSONを読み込み、`turf.bbox()` で全体の範囲を取得して `map.fitBounds()` でズーム調整します。

```javascript
let allBikeTheftData = null;

map.on('load', () => {
  map.addSource('bike', {
    type: 'geojson',
    data: 'chibike.geojson'
  });

  map.addLayer({
    id: 'bike-layer',
    type: 'circle',
    source: 'bike',
    paint: {
      'circle-radius': 4,
      'circle-color': '#ff0000',
      'circle-stroke-color': '#ffffff',
      'circle-stroke-width': 1,
      'circle-opacity': 0.7
    }
  });

  fetch('chibike.geojson')
    .then(response => response.json())
    .then(data => {
      allBikeTheftData = data;
      if (data && data.features.length > 0) {
        const bounds = turf.bbox(data);
        map.fitBounds(bounds, { padding: 50 });
      }
    });
});
```

---

## ステップ3：クリックして1km円を描画・ズーム

ユーザーが地図をクリックすると、その地点を中心に半径1kmの円を描画します。`turf.circle()` を使ってGeoJSON形式のポリゴンを生成し、地図に表示します。さらに、その円の範囲にズームします。

* `turf.circle(center, radius, options)`：指定座標を中心にした円をGeoJSON形式で作成します。
* `map.getSource('circle-source').setData(circle)`：描画した円を既存のGeoJSONソースに上書きします。
* `map.fitBounds(...)`：円のバウンディングボックスをもとに、地図をその範囲にズームします。
* `circle-layer` は塗りつぶし、`circle-outline-layer` は白い2ピクセルの外枠です。

```javascript
map.addSource('circle-source', {
  type: 'geojson',
  data: { type: 'FeatureCollection', features: [] }
});

map.addLayer({
  id: 'circle-layer',
  type: 'fill',
  source: 'circle-source',
  paint: {
    'fill-color': '#007cbf',
    'fill-opacity': 0.4
  }
});

map.addLayer({
  id: 'circle-outline-layer',
  type: 'line',
  source: 'circle-source',
  paint: {
    'line-color': '#ffffff',
    'line-width': 2
  }
});

map.on('click', (e) => {
  const center = [e.lngLat.lng, e.lngLat.lat];
  const circle = turf.circle(center, 1, { steps: 64, units: 'kilometers' });
  map.getSource('circle-source').setData(circle);
  const bounds = turf.bbox(circle);
  map.fitBounds(bounds, { padding: 50 });

  // ステップ4に続く処理
});
```

---

## ステップ4：円内の盗難件数をカウントし、ポップアップで表示

このステップでは、描画された円の内部に含まれる自転車盗難データポイントの数を `turf.booleanPointInPolygon()` を使って数え、MapLibreの `Popup` で表示します。

* `turf.booleanPointInPolygon(feature, circle)`：各データポイントが円の中にあるかを判定します。
* `features.forEach(...)`：GeoJSON内のすべての点に対してループ処理。
* `Popup()`：クリックした地点に盗難件数を表示します。前のポップアップがあれば削除してから新しく作成します。

```javascript
let popup = null;

map.on('click', (e) => {
  const center = [e.lngLat.lng, e.lngLat.lat];
  const circle = turf.circle(center, 1, { steps: 64, units: 'kilometers' });
  map.getSource('circle-source').setData(circle);

  let count = 0;
  allBikeTheftData.features.forEach(f => {
    if (f.geometry.type === 'Point') {
      if (turf.booleanPointInPolygon(f, circle)) count++;
    }
  });

  if (popup) popup.remove();
  popup = new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`<h3>1km以内の盗難件数</h3><p>${count} 件</p>`)
    .addTo(map);
});
```

---

このラボを通じて、Turf.jsとMapLibreを使った簡単な空間集計（spatial query）の流れが体験できます。

---

## 発展課題（チャレンジ）

### 1. カテゴリによる色分け（例：鍵あり/鍵なし）

```javascript
'bike-layer' の paint に以下を追加：
'circle-color': [
  'match', ['get', '施錠関係'],
  '施錠した', '#008000',  // 鍵あり → 緑
  '施錠せず', '#ff0000', // 鍵なし → 赤
  '#888888'             // その他 → グレー
],
```


### 2. 情報パネルに範囲内のポイント情報を表示

```javascript
// map.on('click') 内の count を集計し、info パネルに表示
const infoPanel = document.getElementById('info');
infoPanel.innerHTML = `<strong>この円の中:</strong><br>盗難件数: ${count} 件`;
```

```html
<!-- HTMLにパネル追加 -->
<div id="info" style="position:absolute;top:10px;right:10px;background:white;padding:10px;border-radius:4px;z-index:1;"></div>
```

上記のコードを参考にしながら、自分のマップに合った独自のカスタマイズを考えて実装してみましょう。
色、ポップアップ、インフォパネルなど、アイデア次第でさまざまな表現が可能です！

---

## 提出手順

1. `week05` フォルダの内容を GitHub にコミット
2. GitHub Pages で公開設定（前回と同様）
3. 公開された URL をクラスの Padlet に投稿
