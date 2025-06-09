# Lab 8: Choropleth Religion Map of the World 
このラボでは、世界の国ごとの属性（GDPや宗教など）を使って、**階級区分地図 Choropleth Map** を作成します。
![alt text](<data/religion map.png>)
**階級区分地図 Choropleth Map**とは、地図上の地域（この場合は国）を、統計データに基づいて色分けした地図のことです。これにより、データの分布やパターンを視覚的に理解しやすくなります。例えば、GDPが高い国と低い国を色分けすることで、経済状況の地域差が一目でわかります。

このラボの目的は、世界の宗教分布を示す階級区分地図を作成することです。具体的には、以下の手順で進めます。

1.  **国のポリゴンデータ**：世界の国境を示すGeoJSON形式のデータが必要です。これは、地図の形状を定義するために使用します。
2.  **宗教データ**：国ごとの宗教情報を含むCSV形式のデータが必要です。
3.  **データの結合**：国のポリゴンデータと宗教データを、共通のフィールド（例えばISOコード）を使って結合します。これにより、地図上に宗教情報を表示できるようになります。

これらのデータを組み合わせることで、世界の宗教分布を視覚的に表現し、宗教と地理の関係について考察を深めることができます。

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

**Google Colab**は、ブラウザ上でPythonコードを実行できる無料のクラウド環境です。この環境を利用して、ステップ1でダウンロードした国のポリゴンデータ（`countries.geojson`）と、ステップ2でダウンロードした国別の統計データ（`countries.csv`）を結合します。

具体的には、以下の手順で進めます。

1.  **GeoPandasとPandasのインストール**：GeoJSONファイルの読み込みとデータ操作のために、`geopandas`と`pandas`というPythonライブラリをインストールします。
2.  **ファイルのアップロード**：`countries.geojson`と`countries.csv`をGoogle Colabにアップロードします。
3.  **データの読み込み**：`geopandas`を使ってGeoJSONファイルを、`pandas`を使ってCSVファイルをそれぞれ読み込みます。
4.  **データの結合**：`iso_a2`と`iso2`という共通のフィールドを使って、2つのデータを結合します。この操作により、各国のポリゴンデータに統計データが紐付けられます。
5.  **結合データの確認**：結合されたデータを確認し、必要に応じてデータのクリーニングや変換を行います。
6.  **GeoJSONファイルとして保存**：結合後のデータをGeoJSON形式で保存し、ダウンロードします。このファイルは、後のステップでMapLibreで表示するために使用します。

Google Colabを起動し、新しいノートブックを作成する手順は以下の通りです。

1.  **Google Colabへのアクセス**: ブラウザで [Google Colab](https://colab.research.google.com/) にアクセスします。Googleアカウントでのログインが必要になる場合があります。
2.  **新しいノートブックの作成**: Colabの画面が開いたら、「ファイル」メニューから「新しいノートブック」を選択します。これにより、Pythonコードを記述・実行できる新しいノートブックが開きます。

以下のPythonコードをGoogle Colabのセルに入力し、実行してください。


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

## ステップ 4：宗教の階級区分地図をColabで描画

このステップでは、`geopandas`と`matplotlib`ライブラリを使用して、国の宗教に基づいたカテゴリカルな階級区分地図をGoogle Colab上に表示します。

1.  **描画設定**: `matplotlib`を使用して、描画領域(`fig`)と軸(`ax`)を作成します。地図のサイズやタイトルを設定します。
2.  **カテゴリカルな階級区分地図の描画**: `plot_data.plot`関数を使用して、`religion`列の値に基づいて地図を色分けします。`categorical=True`とすることで、カテゴリカルデータとして扱います。`cmap`引数で色の組み合わせを指定します。ここでは、`"tab20"`というカラーマップを使用しています。
3.  **凡例の追加**: 宗教の種類に対応する凡例を手動で追加します。
4.  **地図の表示**: `plt.show()`関数を使用して、作成した地図を表示します。

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(1, figsize=(12, 8))

joined.plot(column='religion', cmap='tab20', linewidth=0.8, ax=ax, edgecolor='0.8', legend=True)

ax.set_title('Choropleth Map of Religion by Country')
ax.set_axis_off()

plt.show()
```

---
## ステップ 5：GeoJSONとして保存

このステップでは、結合されたデータ（国の形状と宗教データが紐づいたもの）をGeoJSON形式で保存します。

なぜこのステップが必要かというと、**MapLibre GL JS** は、地図のデータをGeoJSON形式で読み込むように設計されているからです。ステップ3で結合したデータは、まだGeoPandasのデータフレームとしてGoogle Colab上に存在しています。このデータをMapLibre GL JSで表示するためには、GeoJSONという特定のファイル形式に変換し、保存する必要があります。

このステップで生成される `countries_joined.geojson` ファイルは、MapLibre GL JSが読み込んで地図上に表示するための「地図の設計図」のようなものです。このファイルがあることで、`index.html` 内のJavaScriptコードが、どの国をどの色で塗り分けるか、といった情報を理解し、インタラクティブな地図として表示できるようになります。

```python
output_file = "countries_joined.geojson"
joined.to_file(output_file, driver="GeoJSON")
files.download(output_file)
```

---

## ステップ 6：MapLibre GL JSで地図を表示する

このステップでは、前のステップで作成した `countries_joined.geojson` ファイルと、提供されている `index.html` ファイルを使って、実際にブラウザ上にインタラクティブな地図を表示します。

**MapLibre GL JS** は、ブラウザ上で動作するJavaScriptライブラリで、ベクトルタイルやGeoJSONなどのデータを使って、インタラクティブな地図を高速に描画することができます。

具体的には、以下の手順で進めます。

1.  **フォルダの作成**: `lab08`という名前の新しいフォルダを作成します。このフォルダは、地図を表示するためのすべてのファイル（`index.html`、`countries_joined.geojson`など）を整理して格納するために使用します。
2.  **`index.html`ファイルの作成**: 提供されているHTMLコードをコピーし、`index.html`という名前で`lab08`フォルダ内に保存します。このHTMLファイルは、MapLibre GL JSライブラリを読み込み、地図を表示するための基本的な構造を定義しています。
3.  **`countries_joined.geojson`ファイルの配置**: ステップ5でダウンロードした`countries_joined.geojson`ファイルを、`lab08`フォルダ内に配置します。このファイルには、国の形状データと宗教データが結合された情報が含まれており、地図上に表示するデータソースとして使用されます。
4.  **ブラウザで`index.html`を開く**: `lab08`フォルダ内の`index.html`ファイルを、Google ChromeやFirefoxなどのWebブラウザで開きます。これにより、MapLibre GL JSが`countries_joined.geojson`ファイルを読み込み、地図上に宗教ごとの色分けされた国が表示されます。

このステップを完了することで、Google Colabで作成・加工したデータが、実際にインタラクティブな地図としてブラウザ上で表示され、操作できるようになります。

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


## 発展課題

### 1. ポップアップの追加

地図上の国にマウスオーバーした際に、国名、GDP、宗教などの情報をポップアップ表示するようにしましょう。

### 2. 凡例の改善

宗教の凡例をインタラクティブにし、クリックすると該当する宗教の国だけを表示するように変更してみましょう。

### 3. GDPによる色分けの追加

宗教だけでなく、GDPの値によって地図の色を変化させるレイヤーを追加し、切り替えられるようにしましょう。

### 4. モーダルウィンドウによる説明の追加

ページを開いた際に、地図の説明や操作方法をモーダルウィンドウで表示するようにしましょう。

