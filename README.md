# 🗺️ Almora Block Boundary Dashboard

A single-file, dependency-free web dashboard that visualises the administrative block boundaries of Almora district, Uttarakhand, built from `Block-Boundary.geojson`.

## 📌 Overview

The dashboard shows the 11 development blocks of Almora district on an interactive map, alongside summary statistics, a ranked area chart and a searchable details table. Everything is linked: selecting a block on the map, in the bar chart or in the table highlights it in all three.

## ✨ Features

- 📊 **KPI cards**: block count, total area, largest block, smallest block, average block area
- 🧭 **Interactive block map**: SVG rendering with labels, hover info and click-to-select
- 📈 **Area ranking**: horizontal bar chart of blocks sorted by area
- 🔍 **Details table**: search by block name, sort by any column, share of district area
- 📱 **Responsive layout** that works on desktop and mobile
- 🌗 **Light and dark themes**, following the system setting
- 🔒 **No external requests**: no libraries, CDNs or map tiles, so it works offline

## 🗂️ Data

| Field        | Type   | Description                         |
| ------------ | ------ | ----------------------------------- |
| `Name`       | string | Block name (e.g. Lamgara, Dhaula Devi) |
| `Distt_Name` | string | District name                       |
| `Area`       | number | Block area (treated as sq km)       |

- 📍 **Source file:** `Block-Boundary.geojson` (FeatureCollection, 11 MultiPolygon features, CRS84 / WGS 84)
- 💾 **Embedded copy:** boundary geometry is embedded in the HTML as a JavaScript object (`const D = {...}`), with coordinates rounded to 4 decimal places (about 10 m) to reduce file size. It is a display copy, not a survey-grade dataset. Use the original GeoJSON for analysis.

## 📁 Project structure

```
.
├── index.html     # the dashboard (HTML + CSS + JS + embedded data)
└── README.md
```

Rename `almora-block-dashboard.html` to `index.html` when deploying.

## 🚀 Running locally

No build step is needed. Either open the file directly in a browser, or serve it:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## 🌐 Deployment

Because it is a static page, it can be hosted anywhere:

- ☁️ **Netlify / Vercel:** drag and drop the folder, or connect the repository
- 🐙 **GitHub Pages:** push `index.html` and enable Pages on the branch
- 🖥️ **Any static web server:** copy the file

## ⚙️ How it works

1. The GeoJSON features are read from the embedded `D` object.
2. Longitude and latitude are projected to screen coordinates with an equirectangular projection, with longitude scaled by the cosine of the mid-latitude to limit distortion.
3. Each block is drawn as an SVG `<path>`, with labels placed at bounding-box centres.
4. KPIs, bars and table rows are computed from the `Area` property. Share is each block's area divided by the sum of all block areas.
5. Selection state is kept in one variable (`sel`), and `pick(name)` updates the map, bars and table together.

## 🔄 Updating the data

To refresh the dashboard with a new or edited GeoJSON:

1. Keep the property names `Name`, `Distt_Name` and `Area`, or update the references in the script.
2. Optionally simplify and round the geometry (for example with `mapshaper` or `ogr2ogr -simplify`) to keep the file small.
3. Replace the contents of `const D = ...` in the HTML with `{"features":[...]}`, where each feature has `properties` and a `MultiPolygon` `geometry`. Single `Polygon` features need to be wrapped as MultiPolygon.

## 🎨 Customisation

- 🎨 **Colours and theme:** edit the CSS variables at the top of the `<style>` block (`--ac` for the accent colour, `--hi` for the highlight colour).
- 📐 **Map size:** change the `W` constant in the script.
- 📏 **Units:** update the "km²" and "sq km" labels if your `Area` field uses a different unit.

## ⚠️ Limitations

- No basemap: the map shows boundaries only, with no roads, terrain or satellite imagery.
- Geometry is simplified for display, so edges are generalised.
- The equirectangular projection is suitable for a district-sized area but not for precise measurement.
- Area values come from the source attribute table. They are not recomputed from the geometry.

## 🧰 Tech

Vanilla HTML, CSS and JavaScript with inline SVG. No dependencies.

## 📄 License

Add a licence of your choice (for example MIT) and confirm that the source boundary data may be redistributed.
