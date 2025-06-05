# Lab 8: 世界のコロプレスマップを作ってみよう

このラボでは、世界地図のコロプレスマップ（Choropleth Map）を作成します。国ごとのポリゴンに属性情報（人口、政治体制など）を結びつけて、色分け表示を行います。

## Part 1: 世界ポリゴンの可視化と属性マップ作成

### ✅ ステップ 1: 国ポリゴン（GeoJSON）のダウンロード

1. 以下のサイトから世界の国境データ（GeoJSON形式）をダウンロードします：

   * [https://geojson.xyz/](https://geojson.xyz/)
   * `ne_110m_admin_0_countries.geojson`（もしくはより高解像度のもの）を選びます

### ✅ ステップ 2: Kepler.glで可視化してみよう

1. [https://kepler.gl/](https://kepler.gl/) を開き、GeoJSONファイルをアップロード
2. 「Add data」でポリゴンを読み込み、「Fill color」で属性を選択
3. コロプレス表示が簡単にできます

### ✅ ステップ 3: VSCodeでMapLibre地図を作成しよう

1. VSCodeで `lab8` フォルダを作成
2. 以下のような `index.html` を作成：

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>World Choropleth</title>
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet">
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
<div id="map"></div>
<script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
<script>
const map = new maplibregl.Map({
  container: 'map',
  style: 'https://basemaps.cartocdn.com/gl/positron-gl-style/style.json',
  center: [0, 20],
  zoom: 1.5
});

map.on('load', () => {
  map.addSource('world', {
    type: 'geojson',
    data: 'ne_110m_admin_0_countries.geojson'
  });

  map.addLayer({
    id: 'world-fill',
    type: 'fill',
    source: 'world',
    paint: {
      'fill-color': [
        'interpolate', ['linear'], ['get', 'POP_EST'],
        1000000, '#fee5d9',
        10000000, '#fcae91',
        50000000, '#fb6a4a',
        100000000, '#de2d26',
        1000000000, '#a50f15'
      ],
      'fill-opacity': 0.7
    }
  });
});
</script>
</body>
</html>
```

---

## Part 2: データを結合して新しいコロプレスを作ろう

### ✅ ステップ 4: 世界の政治体制データを入手

1. [https://ourworldindata.org/grapher/political-regime](https://ourworldindata.org/grapher/political-regime)
2. データをCSVでダウンロードします（`country`, `Code`, `Year`, `Regime` など）
3. 一つの年（例：2020年）だけを使ってデータをフィルタリングしておきましょう

### ✅ ステップ 5: 国コードでデータを結合する（Join）

#### 🔎 Joinとは？

異なるデータセット（この場合は地理データと属性データ）を、共通する「キー（識別子）」を使って結合する操作を「Join（ジョイン）」と呼びます。
ここでは：

* **GeoJSONの国ポリゴンデータ**には `ISO_A3` という3文字の国コードがあります。
* **CSVの属性データ**には `Code` というフィールドがあり、同じく3文字国コードです。

この2つのフィールドを使って結合します。

#### 🔧 PapaParseとは？

PapaParseは、CSVファイルをJavaScriptで簡単に読み込むことができるオープンソースのライブラリです。
CDN経由で読み込めば、クライアント側でCSVをオブジェクトとして扱うことができます。

HTML内に以下を追加して使います：

```html
<script src="https://unpkg.com/papaparse@5.4.1/papaparse.min.js"></script>
```

#### 🧪 実装コード：

以下のコードを `map.on('load')` の中に追加します：

```javascript
Promise.all([
  fetch('ne_110m_admin_0_countries.geojson').then(res => res.json()),
  fetch('regime.csv')
    .then(res => res.text())
    .then(text => Papa.parse(text, { header: true }).data)
]).then(([geojson, csvData]) => {
  // 年が2020年のデータを抽出し、CodeをキーにRegime値を保存
  const regimeMap = {};
  csvData.forEach(row => {
    if (row.Year === '2020') {
      regimeMap[row.Code] = row.Regime;
    }
  });

  // GeoJSONの各国フィーチャにRegime値を追加
  geojson.features.forEach(f => {
    f.properties.Regime = regimeMap[f.properties.ISO_A3];
  });

  // 地図に反映
  map.addSource('world', {
    type: 'geojson',
    data: geojson
  });

  map.addLayer({
    id: 'regime-fill',
    type: 'fill',
    source: 'world',
    paint: {
      'fill-color': [
        'match', ['get', 'Regime'],
        '1', '#e41a1c',  // Democracy
        '2', '#377eb8',  // Flawed
        '3', '#4daf4a',  // Hybrid
        '4', '#984ea3',  // Authoritarian
        '#cccccc'       // その他
      ],
      'fill-opacity': 0.8
    }
  });
});
```

#### 💬 解説：

* `Promise.all()` を使ってGeoJSONとCSVを同時に読み込み
* `Papa.parse` でCSVをパース（読み込み）
* `regimeMap` オブジェクトでCodeとRegimeを対応づけ
* GeoJSONの各国に `.properties.Regime` を追加
* `fill-color` を `match` によってカテゴリごとに色分け

---

このラボでは、属性データと地理データの結合、色分け方法の選定、MapLibreによる実装まで、コロプレスマップの基本を学びました。次は、自分で興味のあるテーマを見つけて、可視化してみましょう！
