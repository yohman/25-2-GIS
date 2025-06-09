# Lab 8: Choropleth Map of the World using GDP

このラボでは、世界の国ごとの属性（GDPなど）を使って、**階級区分地図（Choropleth Map）**を作成します。GeoJSON形式の世界地図ポリゴンと、国ごとの統計情報を結合して視覚化します。

---

## ステップ 1：GeoJSON形式の世界地図をダウンロード

**ポリゴン（行政区域）**は、地図上で色分けするための形の情報です。  
以下のリンクから世界の国境を含むGeoJSONをダウンロードします：  
- https://geojson.xyz/world.geo.json  

保存名：`countries.geojson`

🔍 **解説**：  
GeoJSONは地理データをJSON形式で表現したフォーマットです。各国の境界はFeatureオブジェクトとして格納されており、各国のコードや名前などの属性（properties）が含まれています。

---

## ステップ 2：国別の統計データをダウンロード

以下のリンクから、各国のGDPなどの情報を含むCSVファイルをダウンロードします：  
- https://simplemaps.com/data/countries  

保存名：`countries.csv`

🔍 **解説**：  
このデータには、`iso2` という2文字の国コード（例：JP、US、CN）を含む列があり、`gdp` などの指標が含まれています。

---

## ステップ 3：Google Colabを使ってデータを結合（Join）

Google Colabを使って、`countries.geojson` と `countries.csv` を `iso_a2` と `iso2` で**結合（Join）**します。

```python
!pip install geopandas

from google.colab import files
uploaded = files.upload()  # 2ファイルをアップロード

import geopandas as gpd
import pandas as pd

# GeoJSONとCSVの読み込み
gdf = gpd.read_file("countries.geojson")
df = pd.read_csv("countries.csv")

# Join（結合）
joined = gdf.merge(df, left_on='iso_a2', right_on='iso2', how='left')
```

🔍 **解説**：  
- `gpd.read_file()` は空間データ（GeoJSON）を読み込みます。  
- `merge()` によって、共通する国コードをキーにして属性を結合します。  
- `how='left'` は、GeoJSONの全てのポリゴンを保持し、CSVのデータをマッチする部分にだけ付加します。

---

## ステップ 4：ChoroplethをColab上で描画

```python
import matplotlib.pyplot as plt

# GDP列を数値型にして、NaNを除外
joined['gdp'] = pd.to_numeric(joined['gdp'], errors='coerce')
plot_data = joined.dropna(subset=['gdp'])

# Choropleth描画
fig, ax = plt.subplots(figsize=(15, 8))
plot_data.plot(
    column='gdp',
    cmap='OrRd',
    linewidth=0.5,
    ax=ax,
    edgecolor='0.8',
    legend=True,
    legend_kwds={'label': "GDP by Country", 'orientation': "horizontal"}
)
ax.set_title("Choropleth Map of GDP by Country")
ax.axis('off')
plt.show()
```

🔍 **解説**：  
- `cmap` は色の種類を表します。`OrRd` はオレンジ系のグラデーション。  
- `column='gdp'` によって色分けの対象列を指定。  
- `dropna()` によって、GDPの無い国を除外。

---

## ステップ 5：GeoJSONでエクスポート

```python
output_file = "countries_joined.geojson"
joined.to_file(output_file, driver='GeoJSON')

files.download(output_file)
```

これで、GDP情報付きのGeoJSONがダウンロードできます。

---

## ステップ 6：MapLibreで地図に表示する

VSCodeに `lab08` フォルダを作成し、以下の2ファイルを用意します：  
- `index.html`  
- `style.css`

### `index.html`（CartoDB Positronをベースマップに使用）

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Choropleth GDP Map</title>
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
  <link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
  <link rel="stylesheet" href="style.css" />
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
  map.addSource('countries', {
    type: 'geojson',
    data: 'countries_joined.geojson'
  });

  map.addLayer({
    id: 'gdp-layer',
    type: 'fill',
    source: 'countries',
    paint: {
      'fill-color': [
        'interpolate', ['linear'], ['get', 'gdp'],
        1000, '#fee5d9',
        10000, '#fcae91',
        100000, '#fb6a4a',
        1000000, '#de2d26',
        10000000, '#a50f15'
      ],
      'fill-opacity': 0.8,
      'fill-outline-color': '#ffffff'
    }
  });
});
</script>
</body>
</html>
```

### `style.css`

```css
body { margin: 0; padding: 0; }
#map { position: absolute; top: 0; bottom: 0; width: 100%; }
```

---

## 補足

- `interpolate` は値に応じて色をグラデーションする式です。  
- `['get', 'gdp']` で、GeoJSON内のgdp属性を参照します。  
- より詳細なスタイル設定には、`step` や `match` も使用可能です。

---

## おまけ課題（任意）

- 色分けの方式を変更してみよう（例：stepで分類別に）  
- gdp以外の指標（人口、面積など）で階級区分を作ってみよう  
- クリックすると国名とGDPをポップアップ表示する機能を追加してみよう
