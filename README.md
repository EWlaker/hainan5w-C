# 海南島 五萬分一 · 中研院 RCHSS 典藏

**在线浏览：** <https://ewlaker.github.io/hainan5w-C/>

![preview](https://img.shields.io/badge/图幅-101%20幅-2e6fb7) ![tiles](https://img.shields.io/badge/瓦片-z5--z15-2e8b57) ![crs](https://img.shields.io/badge/EPSG-3857-c8781e)

---

## 这是什么

C 套海南島五万分一地形图（101 幅）的**在线瓦片版**：
原始扫描件经纠斜、内图廓裁切、按接合表定位、统一配准后，切成 XYZ 瓦片，
可直接在浏览器里看，也可以作为图层接进 QGIS / ArcGIS 与天地图、卫星影像叠加。

本套幅数最全，与 A 套同名图可逐格对照。

## 技术规格

| 项 | 值 |
|---|---|
| 投影 | EPSG:3857（Web 墨卡托） |
| 分幅 | 1:50,000，格网 13.18′ × 9.75′（名义分幅，EPSG:4326 表达） |
| 瓦片 | 256 × 256 PNG→WebP（q85） |
| 层级 | **z5 – z15**（z15 ≈ 4.77 m/px，原图 4.3 m/px） |
| 编号 | Google / XYZ 标准：原点左上，Y 向下（`{z}/{x}/{y}`） |
| 透明 | 空白图幅/海域为 alpha=0，可叠在底图上 |
| 接合表 | `sheets_index.geojson`，101 格，含图名与各套图号 |

## 在 GIS 里用

### QGIS（3.22+）

图层 → 浏览器面板 → **XYZ Tiles → 右键 New Connection**：

| 字段 | 填 |
|---|---|
| Name | 海南島五萬分一 C 套 |
| URL | `https://ewlaker.github.io/hainan5w-C/xyz_C_webp/{z}/{x}/{y}.webp` |
| Min zoom | 5 |
| Max zoom | 15 |

### ArcGIS

ArcGIS 的 Web 瓦片图层**不支持 WebP**，需要 PNG 版（另行提供），
或在 ArcGIS Pro 3.x 里用 `Add Data → Data From Path` 读 COG 文件。

## 目录结构

```
index.html            查看页（Leaflet，底图可切换天地图/OSM）
sheets_index.geojson  图幅接合表（101 格，含图名与图号）
404.html              缺失瓦片返回 404（避免页面兜底成 HTML）
xyz_C_webp/          瓦片，12 列 × 13 行，共 65,648 个文件
  {z}/{x}/{y}.webp
```

## 数据来源与版权

扫描件来自公开的档案典藏，本仓库只做**裁切、纠斜、配准、切片**，不改变原图内容：

中研院 RCHSS「地圖數位典藏整合查詢系統」之廣東省陸地測量局《海南島五萬分一》，101 幅，圖幅代號 `map_tfri_topographicmap_50k_J04936–J05036`。

图上坐标是**名义分幅**（民国图廓按 WGS84 表达），适合浏览、对照、定位，
不宜作为测绘控制或精密量测的底图（绝对残差约 ±0.05–0.09°，即 ±5–10 km 量级）。

仅供研究与学习使用；转载、再分发请遵循原典藏机构的条款。


## 更新

瓦片由本地工程重新生成后整批推送：

```bash
gdal2tiles -p mercator -z 5-15 --xyz --tiledriver WEBP -r average 源.tif xyz_C_webp
git add -A && git commit -m "更新瓦片" && git push
```
