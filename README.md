# 🗺️ GEOSINT — Geographic OSINT analysis

**GEOSINT** is a fully client-side **geographic intelligence OSINT tool** — a
**standalone web page in a single HTML file**. From a **location** (address, place,
postal code, GPS coordinates or a **click on the map**), it automatically gathers the
available public information and produces a sourced, mapped and exportable **OSINT
report**.

> No server, no database, no authentication, no installation: open `index.html` and
> everything runs in the browser.

---

## 🎚️ Collection modes

A **Collection scope** selector lets you choose which sources are queried during
generation (the choice is stored in the browser):

| Mode | What is collected |
|---|---|
| 🛰️ **Satellite + map** | Satellite imagery (Esri, dated **Esri Wayback**, **Yandex**) **+** maps (OpenTopoMap) and external links. No OSM collection — fast. |
| 🌐 **Everything** | Full: imagery + maps **+** OpenStreetMap/Overpass data (road & rail network, transit, POIs, environment, derived **access**, relief, cities) **+** nearby Wikipedia and **geolocated photos** (Wikimedia Commons). |

The interface automatically hides the maps that are not relevant to the chosen mode.

## 📋 "Batch" mode (list of places)

A **Batch** tab lets you process a **list of places** at once:

- a **table** where each row has a **free-form name** (the name of the generated folder)
  and a **location** (address, place, or "lat, lon" coordinates);
- quick import by **pasting** a list (`Name | location`, one per line);
- a **Verify** button that geocodes and confirms each location (Nominatim);
- a **⚙ Settings** button exposing **all outputs of the main page** to include per place:
  - **images** (one per checked zoom, multiple sizes): Satellite (Esri), OpenTopoMap,
    standard OSM, Yandex satellite, dated imagery (Wayback);
  - **data & report**: `donnees.json`, GeoJSON, CSV, HTML report, PDF report (text);
- a choice of **zooms (sizes)** to generate (one image per zoom);
- generation downloads **for each place a folder named after it** containing the checked
  files.

Delivery of your choice: a single **ZIP** containing one subfolder per place, or direct
writing into a **folder on disk** (File System Access API, compatible browsers). The ZIP
and the PDF are generated in **pure JavaScript**, with no dependency.

## 📐 OSM object extraction (area, perimeter, coordinates)

A dedicated tool queries **Overpass** over the map's visible extent from a simple **type**
(OSM key/value, e.g. `landuse=residential`, `building`, `natural=water`). Generated query:

```overpassql
[out:json][timeout:25];
// full geometry (geom)
nwr["landuse"="residential"]({{bbox}});
out geom;
```

For each object found, the tool computes and shows: **area**, **perimeter**,
**dimensions** (width × height), number of vertices, and gives the **perimeter
coordinates** (copy per object or globally). **GeoJSON** and **CSV** exports; objects are
also drawn on the map.

## 🌾 "Landuse layer" tool (`outil-landuse.html`)

A standalone page dedicated to **creating a landuse (land cover) layer** over a **precise
area**, with a **Positron** basemap (© CARTO). It is the first building block of a
parametrable map oriented toward research at a given spot (not a "default" overview).

- **Precise area**: place search (Nominatim), editable `bbox` extent, "View → bbox"
  button, or **drawing** a rectangle in 2 clicks.
- **Parametrable landuse types**: an *Agriculture* group (fields, orchards, vineyards,
  meadows, greenhouses…) and an *Other* group (forest, residential, industrial…), plus
  extra OSM `key=value` pairs.
- **GeoJSON generation** via Overpass: for each polygon (way + multipolygon relation) the
  tool computes **area**, **perimeter**, dimensions, vertex count, **all coordinates**,
  and reads the **EN / FR / IT / DE** names + the **original name**. A **size class
  (1→5)** is assigned based on parametrable thresholds. **GeoJSON** and **CSV** export.
- **5-size color code** (accessible sequential ColorBrewer YlGn palette): small fields
  stay **hidden in the wide view** and **appear on zoom** (minimum appearance zoom
  adjustable per size). Alternative "by type" coloring.
- **Water stations**: fixed points (~15 large stations), editable GeoJSON + template.
- **Your coordinates**: adding custom **GeoJSON** layers (points, lines, polygons), pasted
  or imported.
- **GeoJSON cleaner**: upload a GeoJSON containing *all columns*; the tool **removes the
  unnecessary columns**, keeps only what's needed (type, multilingual names, id) and
  **recomputes the area and the perimeter** from the geometry (coordinates preserved).
  Export of the cleaned GeoJSON + CSV.

The produced layer is **independent and reusable** elsewhere (Maputnik, QGIS, another
MapLibre style).

## 🗺️ Positron / Maputnik map (`carte-landuse.html`)

A parametrable **MapLibre GL** map, editable in **Maputnik**, that **consumes the layer**
produced by `outil-landuse.html` (or fetches it itself via Overpass). It is the "research
at a given spot" map based on the **Positron** basemap.

- **Landuse layer**: load the tool's GeoJSON, or fetch the landuse over the area. Rendered
  with the **5-size color code** driven by zoom (small fields revealed on zoom), coloring
  by size or by type; field names at high zoom.
- **Customized Positron basemap**:
  - **country names hidden** (borders kept);
  - **reinforced borders** (thicker / higher contrast);
  - **small villages / hamlets hidden**, **large cities prioritized**;
  - **motorway + (secondary ~80 km/h) road reinforced**, minor roads decluttered;
  - **large rivers reinforced**;
  - **relief / topography** optional (*hillshade* from a DEM source).
- **Water stations** (fixed points ~15) and **custom GeoJSON layers**.
- **Style export** of the MapLibre style (`.json`) to open and edit it in **Maputnik**.

## ✨ Features

- **Multi-mode search**: address / place / postal code (Nominatim geocoding), direct GPS
  coordinates, or **a click on the interactive map**.
- **Leaflet mapping** with several basemaps:
  - OpenStreetMap
  - OpenTopoMap (relief)
  - **Esri World Imagery** satellite imagery
  - **Yandex** satellite tiles
- **Full coordinate conversions**: decimal, **DMS**, **UTM**, **MGRS**, **Plus Code**,
  **GeoHash**.
- **Environment & accessibility** via Overpass/OSM: distance to the nearest **motorway**,
  main roads, **train station** and **airport**.
- **Real-time weather & climate** (**Open-Meteo**).
- **External OSINT links** automatically generated to the main verification and imagery
  tools: Google Maps / Earth, Bing, Apple Maps, Yandex, Mapillary, KartaView, Wikimapia,
  what3words, SunCalc, Windy, Geoportail, Geoportail Switzerland (map.geo.admin.ch),
  FlightRadar24, MarineTraffic, NASA Worldview, Sentinel Hub, Wikipedia/Wikimedia Commons…
- **Exports**: formatted OSINT report, **GeoJSON** and raw data.
- **Images downloaded in WebP format** (quality 0.85) — satellite maps, Wayback/Yandex
  captures and annotated images, noticeably lighter than PNG.
- **Interface in English**.

## 🚀 Usage

### Locally

No dependency to install — the page only loads the **Leaflet** library from a CDN.

```bash
# Option 1: open the file directly
open index.html          # macOS

# Option 2 (recommended): serve locally to avoid some CORS blocks
python3 -m http.server 8000
# then open http://localhost:8000
```

> ⚠️ The page calls public APIs (Nominatim, Overpass, Open-Meteo) and loads remote tiles.
> Prefer serving it over **HTTPS** in production to avoid *mixed-content* / CORS blocks.

### Deployment

It is a **static** site: deployable as-is on GitHub Pages, Netlify, Vercel, Cloudflare
Pages or any static hosting. Just serve `index.html`.

## 🔌 Data sources & services used

| Service | Role |
|---|---|
| **Nominatim** (OpenStreetMap) | Geocoding / reverse geocoding |
| **Overpass API** | Nearby OSM objects (roads, stations, airports, POIs) |
| **Open-Meteo** | Weather & climate |
| **Esri World Imagery** | Satellite imagery |
| **OpenTopoMap** | Topographic / relief basemap |
| **Yandex** | Complementary satellite tiles |
| **CARTO Positron** | Light basemap for the landuse tools |
| **Leaflet / MapLibre GL** (CDN) | Interactive map engines |

## 📜 Attributions & data licenses

- OpenStreetMap / Nominatim / Overpass — **ODbL 1.0**, © OpenStreetMap contributors.
- OpenTopoMap — **CC-BY-SA**.
- Esri World Imagery — © Esri, Maxar, Earthstar Geographics (attribution required).
- Open-Meteo — **CC BY 4.0**.
- CARTO Positron — © CARTO, © OpenStreetMap contributors.

Please respect the **usage policies** (call frequency) of Nominatim and Overpass. GEOSINT
only aggregates **public** information and shows its source for each block: a datum with no
source is never presented as a fact.

## ⚖️ Scope of use

A tool intended for **legitimate** uses: open-source research (OSINT), journalistic
verification, cartographic analysis, education. The user is responsible for complying with
the applicable laws and the terms of use of the third-party services queried.

## 📁 Structure

```
index.html          Full application (HTML + CSS + JS, a single standalone file)
outil-landuse.html  Landuse layer tool (GeoJSON generation + cleaner, Leaflet + Positron)
carte-landuse.html  Positron / Maputnik map (MapLibre GL) consuming the layer
README.md           This document
LICENSE             MIT license
```

## 👤 Author

**safisaf5**

## 📄 License

Distributed under the **MIT** license — © 2026 safisaf5. See [LICENSE](LICENSE).
