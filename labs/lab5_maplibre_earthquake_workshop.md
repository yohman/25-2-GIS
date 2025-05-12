# Lab 5: MapLibre で地震データを可視化しよう

このワークショップでは、MapLibre GL JS を使って、USGSの地震APIを利用してリアルタイム地震データを地図に表示する方法を学びます。

---

## 1. HTMLと地図の基本構造

このセクションでは、地図を表示するためのHTMLの基本的な構成について説明します。

* `<!DOCTYPE html>`：HTML5文書であることを宣言します。
* `<html> ... </html>`：HTML文書全体を囲むタグです。
* `<head>`：タイトルやCSS、JavaScriptなど、文書のメタ情報を定義します。

  * `<meta charset="utf-8">`：文字コードをUTF-8に指定します。
  * `<title>`：ブラウザのタブに表示されるタイトルです。
  * `<link>`：MapLibreのCSSファイルを読み込むタグです。
  * `<style>`：地図の大きさを画面いっぱいに表示するためのCSSを記述します。
* `<body>`：ブラウザに表示される本体部分です。

  * `<div id="map">`：このdivの中に地図が表示されます。
  * `<script>`：MapLibre GL JSのライブラリを読み込み、地図を表示するためのJavaScriptを記述します。

以下はそのHTMLコードです：

### MapLibreの初期化コードの解説

```javascript
const map = new maplibregl.Map({
  container: 'map',
  style: {
    version: 8,
    sources: {
      'google-tiles': {
        type: 'raster',
        tiles: [
          'https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}'
        ],
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
```

* `container: 'map'`：地図を表示するHTMLの要素ID（ここでは`<div id="map">`）を指定。
* `style`：地図のスタイルを定義するオブジェクト。

  * `sources`：地図タイルの情報を指定。Googleの衛星画像タイル（`lyrs=y`）を使っています。
  * `layers`：表示するレイヤーを設定。ここではラスタ画像（衛星写真）をそのまま表示。
* `center`：地図の初期中心座標。例では東京駅（139.7671, 35.6812）。
* `zoom`：初期のズームレベル。2は全体表示に近い。

この設定により、MapLibreを使ってGoogleのサテライト画像をベースマップとして表示することができます。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>マップと地震データ</title>
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100%; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
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

## 2. USGS地震データを使ってリアルタイム地震情報を可視化

### USGSのGeoJSONフィードとは？

* 米国地質調査所（USGS）は、世界中の地震情報をGeoJSON形式で公開しています。
* 各地震は1つの「Feature」として構成され、位置情報（geometry）と詳細情報（properties）を含みます。

以下はデータの一例です（抜粋）：

```json
{
  "type": "Feature",
  "properties": {
    "mag": 6.1,
    "place": "72 km SSE of Luganville, Vanuatu",
    "time": 1715585959240,
    "title": "M 6.1 - 72 km SSE of Luganville"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [167.2764, -16.8291, 10.0] // 経度, 緯度, 深さ
  },
  "id": "us7000kgyv"
}
```

#### よく使うフィールドの意味

* `properties.mag`: マグニチュード（例：6.1）
* `properties.place`: 地震発生場所の説明（例："SSE of Luganville"）
* `properties.time`: 発生時刻（Unix時間）
* `geometry.coordinates`: `[経度, 緯度, 深さ]`

MapLibreでは次のように使います：

```javascript
const coords = e.features[0].geometry.coordinates;
const props = e.features[0].properties;

popup.setHTML(`<strong>${props.place}</strong><br>Magnitude: ${props.mag}`);
```

### ステップ1：circleレイヤーで地震を表示する（固定サイズ）

このステップでは、地震データをGeoJSON形式で読み込み、すべての地震に対して固定サイズの赤い円を表示します。MapLibreの`circle`レイヤーは、シンプルな丸印でポイントデータを表現するためによく使われます。

* `circle-radius`: 円の半径を指定します（ここでは固定で6ピクセル）
* `circle-color`: 円の色（赤）
* `circle-opacity`: 不透明度（0.7 = 少し透過）

```javascript
map.on('load', () => {
  map.addSource('earthquakes', {
    type: 'geojson',
    data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/4.5_week.geojson'
  });

  map.addLayer({
    id: 'earthquakes-layer',
    type: 'circle',
    source: 'earthquakes',
    paint: {
      'circle-radius': 6,
      'circle-color': '#ff0000',
      'circle-opacity': 0.7
    }
  });
});
```

### ステップ2：マグニチュードに応じて円サイズを変化させる（interpolate）

```javascript
map.on('load', () => {
  map.addSource('earthquakes', {
    type: 'geojson',
    data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/4.5_week.geojson'
  });

  map.addLayer({
    id: 'earthquakes-layer',
    type: 'circle',
    source: 'earthquakes',
    paint: {
      'circle-radius': ['interpolate', ['linear'], ['get', 'mag'], 4.5, 4, 7, 50],
      'circle-color': '#ff0000',
      'circle-opacity': 0.7
    }
  });
});
```

#### 解説：circle-radius の仕組み

```javascript
'circle-radius': ['interpolate', ['linear'], ['get', 'mag'], 4.5, 4, 7, 50]
```

* `interpolate`: 数値に応じて滑らかに変化させる
* `linear`: 線形スケーリング
* `['get', 'mag']`: 地震の `mag` プロパティを取得
* `4.5, 4`: M4.5 の地震 → 半径 4
* `7, 50`: M7 の地震 → 半径 50

---

### ステップ3：ポップアップ（ホバーで情報表示）を追加

このステップでは、マウスを地震ポイントの上に乗せたとき（`mouseenter`）に、詳細情報を表示するポップアップを出し、マウスが離れたとき（`mouseleave`）にポップアップを非表示にします。

* `map.on('mouseenter', ...)`：レイヤー上にマウスが来たらイベント発生
* `map.on('mouseleave', ...)`：マウスがレイヤーから離れたときにイベント発生
* `Popup().setLngLat().setHTML().addTo(map)`：指定位置に吹き出しを表示
* `popup.remove()`：ポップアップを閉じる

```javascript
let popup;

map.on('mouseenter', 'earthquakes-layer', (e) => {
  const coords = e.features[0].geometry.coordinates;
  const props = e.features[0].properties;
  popup = new maplibregl.Popup({ closeOnClick: false })
    .setLngLat([coords[0], coords[1]])
    .setHTML(`<strong>${props.place}</strong><br>Magnitude: ${props.mag}`)
    .addTo(map);
  map.getCanvas().style.cursor = 'pointer';
});

map.on('mouseleave', 'earthquakes-layer', () => {
  map.getCanvas().style.cursor = '';
  if (popup) {
    popup.remove();
    popup = null;
  }
});
```

---

## 応用アイデア

* 地震データをカテゴリごとに色分け表示する
* マグニチュードに応じてアニメーションを加える
* 地震情報の詳細をサイドパネルで表示する

---

## 発展チャレンジ（オプション）

* 地震の一覧を表示する**サイドパネル**を作成し、地図と連動するようにする
* 地震の発生時間を軸にした**タイムライン可視化**を追加する（スライダーなど）
* サイドパネルの地震名をクリックすると、地図上でポップアップを開くようにする

---

## ワークショップ課題

1. 上記のコードをコピーして、自分の GitHub Pages またはローカルで動かしてみましょう
2. 地震のスタイルや中心位置、ズームなどを自由にカスタマイズしてみましょう
3. 発展チャレンジにもぜひ挑戦してみましょう
