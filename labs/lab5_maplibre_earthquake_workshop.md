# Lab 5: MapLibre で地震データを可視化しよう

このワークショップでは、MapLibre GL JS を使って、USGSの地震APIを利用してリアルタイム地震データを地図に表示する方法を学びます。

---

## 1. HTMLと地図の基本構造

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

### ステップ1：circleレイヤーで地震を表示する（固定サイズ）

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
