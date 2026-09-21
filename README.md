# Mark 15 — EC3: Land Use / Land Cover of Bangladesh and Dhaka District

Part of the [Mark 15](https://github.com/strangec137) open-source geospatial mapping portfolio.

Two maps built from ESA WorldCover 2021 (10 m):

1. **Bangladesh LULC** — whole country, single panel.
2. **Bangladesh → Dhaka Division → Dhaka district** — a three-panel zoom chain, with Dhaka
   district shown at full 10 m resolution.

![Bangladesh LULC](outputs/EC3_Map1_Bangladesh_LULC.png)
![Dhaka district LULC](outputs/EC3_Map2_Dhaka_LULC.png)

## ⚠️ Before You Run This Notebook

**Prepare your shapefiles or geopackages first.**

The ESA WorldCover raster is not included in this repo (see below) — download it from the
Release page before running the notebook.

## Required input files

| File | Where it goes | Notes |
|---|---|---|
| `LULC_ESA_WorldCover_Bangladesh-*.tif` (2 tiles) | `data/lulc/` | ESA WorldCover 2021 v200, 10 m, via Google Earth Engine. Download from this repo's [Releases](../../releases) page. |
| `Bangladesh_Country_Boundary.gpkg` | `data/boundaries/` | GADM 4.1, district level (`NAME_2`) |
| `dhaka_district.gpkg` (layer `dhaka_district`) | `data/boundaries/` | GADM 4.1, single Dhaka district polygon |
| `north_arrow.png` | `assets/` | Compass rose image |

## Reproduce

```bash
git clone https://github.com/strangec137/mark15-bangladesh-lulc-ec3.git
cd mark15-bangladesh-lulc-ec3
conda create -n mark15-ec3 python=3.11
conda activate mark15-ec3
pip install -r requirements.txt
# download the WorldCover tiles from Releases into data/lulc/
jupyter notebook EC3_Bangladesh_LULC.ipynb
# Kernel -> Restart & Run All
```

Needs internet for the basemap tiles (Esri World Street Map) and roughly 4 GB RAM.

## Notebook structure

| Cell | What it does |
|---|---|
| 1 | Setup — relative paths, checks all inputs are present |
| 2 | Class table, colours, helper functions |
| 3 | Load boundaries, drop the duplicate Panchagarh rows, build outlines |
| 4 | Clip the LULC raster to Dhaka district at full 10 m |
| 5 | One strip pass over both WorldCover tiles — exact class statistics and the blurred draw image |
| 6 | Draw and save Map 1 (Bangladesh) |
| 7 | Draw and save Map 2 (Dhaka zoom chain) |

## Known limitations

- Percentages are shares of the GADM land area; large river channels and coastal water outside
  GADM are left blank on the maps.
- Map 1's drawn image is a Gaussian-smoothed 80 m majority class, to avoid speckle at country
  scale. Every class statistic and Map 2 panel (c) use the full 10 m data.
- A thin strip at the south-west edge of Dhaka district has no data in the source raster and
  shows the basemap through.

## Style reference

Academic/journal style: Georgia font, degree-formatted graticule, inward ticks, real geodesic
scale bars (`pyproj.Geod`), north arrow, semi-transparent legends, 600 DPI PNG export.

## Licence

- **Code:** MIT (see `LICENSE`).
- **Maps in `outputs/`:** no open licence — please ask before reusing.

## Credits / attribution

- Land cover: [ESA WorldCover 2021 v200](https://esa-worldcover.org/) (10 m), exported via
  Google Earth Engine.
- Boundaries: [GADM 4.1](https://gadm.org/) — free for academic and non-commercial use;
  redistribution or commercial use needs prior permission from GADM. This is not legal advice.
- Basemap: Esri World Street Map, © Esri and its data partners.
