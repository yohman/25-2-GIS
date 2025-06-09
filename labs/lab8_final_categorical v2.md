# Lab 8: Choropleth Map of the World using GDP

このラボでは、世界の国ごとの属性（GDPや宗教など）を使って、**階級区分地図（Choropleth Map）**を作成します。

---

## ステップ 1：GeoJSON形式の世界地図をダウンロード

以下のリンクから世界の国境を含むGeoJSONをダウンロードします：

- https://geojson.xyz/world.geo.json

保存名：`countries.geojson`

---

## ステップ 2：国別の統計データをダウンロード

以下のリンクから、各国のGDPや宗教の情報を含むCSVファイルをダウンロードします：

- https://simplemaps.com/data/countries

保存名：`countries.csv`

---

## ステップ 3：Google Colabでデータを結合

```python
!pip install geopandas
from google.colab import files
uploaded = files.upload()

import geopandas as gpd
import pandas as pd

gdf = gpd.read_file("countries.geojson")
df = pd.read_csv("countries.csv")

joined = gdf.merge(df, left_on="iso_a2", right_on="iso2", how="left")
```

---

## ステップ 4：ChoroplethをColabで描画

```python
import matplotlib.pyplot as plt

joined["gdp"] = pd.to_numeric(joined["gdp"], errors="coerce")
plot_data = joined.dropna(subset=["gdp"])

fig, ax = plt.subplots(figsize=(15, 8))
plot_data.plot(
		column="gdp",
		cmap="OrRd",
		linewidth=0.5,
		ax=ax,
		edgecolor="0.8",
		legend=True,
		legend_kwds={"label": "GDP by Country", "orientation": "horizontal"}
)
ax.set_title("Choropleth Map of GDP by Country")
ax.axis("off")
plt.show()
```

---

## ステップ 5：GeoJSONとして保存

```python
output_file = "countries_joined.geojson"
joined.to_file(output_file, driver="GeoJSON")
files.download(output_file)
```

---

## ステップ 6：MapLibreで表示する

`lab08` フォルダを作り、以下の `index.html` を作成：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Choropleth GDP & Religion Map</title>
		<meta
			name="viewport"
			content="initial-scale=1,maximum-scale=1,user-scalable=no"
		/>
		<link
			href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css"
			rel="stylesheet"
		/>
		<style>
			body { margin: 0; padding: 0; }
			#map { position: absolute; top: 0; bottom: 0; width: 100%; }
			.legend { position: absolute; bottom: 30px; left: 10px; background: white; padding: 10px; font-family: sans-serif; font-size: 12px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.2); border-radius: 4px; }
			.legend-item { display: flex; align-items: center; margin-bottom: 5px; }
			.legend-color { width: 15px; height: 15px; margin-right: 5px; }
		</style>
	</head>
	<body>
		<div id="map"></div>
		<div class="legend" id="legend">
			<div><strong>Major Religion</strong></div>
			<div class="legend-item">
				<div class="legend-color" style="background: #1f78b4"></div>Islam
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #e31a1c"></div>Christianity
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #33a02c"></div>Buddhism
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #ff7f00"></div>Hinduism
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #6a3d9a"></div>Judaism
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #b15928"></div>No Religion
			</div>
			<div class="legend-item">
				<div class="legend-color" style="background: #d9d9d9"></div>Unknown
			</div>
		</div>

		<script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
		<script>
			// マップの初期化
			const map = new maplibregl.Map({
				container: "map",
				style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
				center: [0, 20],
				zoom: 1.5,
			});

			// マップがロードされた後の処理
			map.on("load", () => {
				// データソースの追加
				map.addSource("countries", {
					type: "geojson",
					data: "countries_joined.geojson",
				});

				// レイヤーの追加
				map.addLayer({
					id: "religion-layer",
					type: "fill",
					source: "countries",
					paint: {
						"fill-color": [
							"match",
							["get", "religion"],
							"Islam", "#1f78b4",
							"Christianity", "#e31a1c",
							"Buddhism", "#33a02c",
							"Hinduism", "#ff7f00",
							"Judaism", "#6a3d9a",
							"No Religion", "#b15928",
							"#d9d9d9"
						],

						"fill-opacity": 0.8,
						"fill-outline-color": "#ffffff",
					},
				});
			});
		</script>
	</body>
</html>
```

---

これで、GDPや宗教ごとの階級区分地図が完成です。