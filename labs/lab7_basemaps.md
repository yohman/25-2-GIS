# Lab 7: Basemap Tile Guide for MapLibre GL JS

![alt text](<tiles thumbs/basemaps all.png>)

➡︎ https://github.com/yohman/25-2-GIS/blob/main/labs/basemap.html

このガイドでは、MapLibre GL JSで使用できるベースマップの種類と、その使い方（コード例）を紹介します。

ベースマップは、Webマッピングプロジェクトの「視覚的な土台（visual backbone）」です。どんなベースマップを選ぶかによって、マップの雰囲気や可読性、さらにはデータの伝わり方が大きく変わります。

一般的に、ベースマップは主役であるデータレイヤーよりも**視覚的に控えめ**であるべきです。つまり、目立ちすぎず、でも情報として背景に必要な要素をしっかり支えることが求められます。

このラボでは、複数のベースマップを試しながら、それぞれの特徴と適した用途を学んでいきます。

---

### Getting Started

以下は MapLibre を使ってベースマップを表示する基本の HTML テンプレートです。  
`style: ...` の部分を、下にある各スタイルのコードに置き換えて実験してみましょう。

```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8" />
	<title>MapLibre Basemap Starter</title>
	<meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
	<link href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css" rel="stylesheet" />
	<style>
		body {
			margin: 0;
			padding: 0;
		}

		#map {
			position: absolute;
			top: 0;
			bottom: 0;
			width: 100%;
		}
	</style>
</head>

<body>
	<div id="map"></div>
	<script src="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.js"></script>
	<script>
		const map = new maplibregl.Map({
			container: 'map',
			style: 'https://basemaps.cartocdn.com/gl/positron-gl-style/style.json', // ← ここを書き換える
			center: [139.7671, 35.6812],
			zoom: 10
		});
	</script>
</body>

</html>
```

---

## 1. CartoDB Positron（ベクトル形式、Mapbox互換スタイルURL）

CartoDBのPositronは軽量で視認性の高い背景スタイル。

---

## 1-B. CartoDB Dark Matter（ダークモード対応）

黒背景に白文字で、夜間表示やカラフルなマーカーに最適。

### コード例：

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: 'https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json',
	center: [139.7671, 35.6812],
	zoom: 10
});
```

---

## 2. Stamen Tiles（Toner Lite / Terrain / Watercolor）

Stadia Mapsを経由してホスティングされている Stamen スタイルです。

### Toner Lite（グレーの輪郭）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'stamen-toner-lite': {
				type: 'raster',
				tiles: [
					'https://tiles.stadiamaps.com/tiles/stamen_toner_lite/{z}/{x}/{y}.png',
				],
				tileSize: 256,
				attribution:
					'&copy; <a href="https://stamen.com/">Stamen</a>, <a href="https://openstreetmap.org">OpenStreetMap</a>',
			},
		},
		layers: [
			{
				id: 'stamen-toner-lite-layer',
				type: 'raster',
				source: 'stamen-toner-lite',
			},
		],
	},
	center: [139.7671, 35.6812],
	zoom: 10,
});
```

### Terrain

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'stamen-terrain': {
				type: 'raster',
				tiles: [
					'https://tiles.stadiamaps.com/tiles/stamen_terrain/{z}/{x}/{y}.png'
				],
				tileSize: 256,
				attribution:
					'&copy; <a href="https://stamen.com/">Stamen</a>, <a href="https://openstreetmap.org">OpenStreetMap</a>'
			}
		},
		layers: [
			{
				id: 'stamen-terrain-layer',
				type: 'raster',
				source: 'stamen-terrain'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### Watercolor

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'stamen-watercolor': {
				type: 'raster',
				tiles: [
					'https://tiles.stadiamaps.com/tiles/stamen_watercolor/{z}/{x}/{y}.jpg'
				],
				tileSize: 256,
				attribution:
					'&copy; <a href="https://stamen.com/">Stamen</a>, <a href="https://openstreetmap.org">OpenStreetMap</a>'
			}
		},
		layers: [
			{
				id: 'stamen-watercolor-layer',
				type: 'raster',
				source: 'stamen-watercolor'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

---

## 3. GSI Tiles（国土地理院タイル）

GSIの白地図や淡色タイルは、日本国内での視認性が高くおすすめ。

### 白地図（Blank）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-blank': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/blank/{z}/{x}/{y}.png'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-blank-layer',
				type: 'raster',
				source: 'gsi-blank'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 淡色地図（Pale）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-pale': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/pale/{z}/{x}/{y}.png'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-pale-layer',
				type: 'raster',
				source: 'gsi-pale'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 航空写真（1961年～1969年）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-ort-old10': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/ort_old10/{z}/{x}/{y}.png'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-ort-old10-layer',
				type: 'raster',
				source: 'gsi-ort-old10'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 航空写真（1974年～1978年）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-gazo1': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/gazo1/{z}/{x}/{y}.jpg'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-gazo1-layer',
				type: 'raster',
				source: 'gsi-gazo1'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 航空写真（1979年～1983年）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-gazo2': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/gazo2/{z}/{x}/{y}.jpg'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-gazo2-layer',
				type: 'raster',
				source: 'gsi-gazo2'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 航空写真（1984年～1986年）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-gazo3': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/gazo3/{z}/{x}/{y}.jpg'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-gazo3-layer',
				type: 'raster',
				source: 'gsi-gazo3'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### 航空写真（1987年～1990年）

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'gsi-gazo4': {
				type: 'raster',
				tiles: [
					'https://cyberjapandata.gsi.go.jp/xyz/gazo4/{z}/{x}/{y}.jpg'
				],
				tileSize: 256,
				attribution: '地理院タイル &copy; <a href="https://www.gsi.go.jp/">国土地理院</a>'
			}
		},
		layers: [
			{
				id: 'gsi-gazo4-layer',
				type: 'raster',
				source: 'gsi-gazo4'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

## 4. Google Tiles

Googleが提供する地図タイル。

### Google Roads

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'google-roads': {
				type: 'raster',
				tiles: [
					'https://mt1.google.com/vt/lyrs=h&x={x}&y={y}&z={z}'
				],
				tileSize: 256,
				attribution: '&copy; Google'
			}
		},
		layers: [
			{
				id: 'google-roads-layer',
				type: 'raster',
				source: 'google-roads'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```


### Google Satellite Hybrid

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'google-satellite-hybrid': {
				type: 'raster',
				tiles: [
					'https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}'
				],
				tileSize: 256,
				attribution: '&copy; Google'
			}
		},
		layers: [
			{
				id: 'google-satellite-hybrid-layer',
				type: 'raster',
				source: 'google-satellite-hybrid'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### Google Satellite

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'google-satellite': {
				type: 'raster',
				tiles: [
					'https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}'
				],
				tileSize: 256,
				attribution: '&copy; Google'
			}
		},
		layers: [
			{
				id: 'google-satellite-layer',
				type: 'raster',
				source: 'google-satellite'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### Google Streets

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'google-streets': {
				type: 'raster',
				tiles: [
					'https://mt1.google.com/vt/lyrs=r&x={x}&y={y}&z={z}'
				],
				tileSize: 256,
				attribution: '&copy; Google'
			}
		},
		layers: [
			{
				id: 'google-streets-layer',
				type: 'raster',
				source: 'google-streets'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

### Google Maps

```javascript
const map = new maplibregl.Map({
	container: 'map',
	style: {
		version: 8,
		sources: {
			'google-maps': {
				type: 'raster',
				tiles: [
					'https://mt1.google.com/vt/lyrs=m&x={x}&y={y}&z={z}'
				],
				tileSize: 256,
				attribution: '&copy; Google'
			}
		},
		layers: [
			{
				id: 'google-maps-layer',
				type: 'raster',
				source: 'google-maps'
			}
		]
	},
	center: [139.7671, 35.6812],
	zoom: 10
});
```

---
## 使用上の注意：

* **CartoDBのstyle.jsonはMapbox GL互換形式** → `style: 'https://...'` で指定
* **StamenやGSI, Googleなどのラスタタイル**は、`style` にオブジェクトで `sources` と `layers` を明示する必要あり
* **GSIの歴史的航空写真タイル**は、地域によってはデータが存在しない（タイルが表示されない）場合があります。
* 複数のソースを同時に使いたい場合は、`sources` に複数定義して、`layers`で順序制御
* attribution（著作権表記）は必ず明記しましょう

---

## まとめ

| タイル名                 | タイプ          | URL形式                                    |
| -------------------- | ------------ | ---------------------------------------- |
| CartoDB Positron     | ベクトル（GLスタイル） | `style: 'https://...json'`               |
| Stamen Tiles         | ラスター（ZXY）    | `tiles: ['https://.../{z}/{x}/{y}.png']` |
| GSI（地理院）             | ラスター（ZXY）    | 同上                                       |
| Google Maps          | ラスター（ZXY）    | 同上                                       |

Webマップの背景スタイルは、可読性や雰囲気に大きく影響します。目的に応じて最適なベースマップを選びましょう！
