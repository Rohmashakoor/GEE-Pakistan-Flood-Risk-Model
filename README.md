# Pakistan 50-Year Flood Risk & Asset Exposure Engine

An interactive Google Earth Engine (GEE) application that models 50-year flood inundation hazard across Pakistan, Jammu & Kashmir, and Gilgit-Baltistan (Skardu), and overlays exposed population, cropland, and built infrastructure to support disaster risk assessment and planning.

**Live app:** https://project-e03cf9e5-91a7-4488-9fd.projects.earthengine.app/view/pakistan-50-year-flood-risk--asset-exposure-engine

---

## Overview

This tool combines a hydrologically corrected terrain model with a Sentinel-1 SAR record of Pakistan's 2022 "mega-flood" to classify land into moderate and high 50-year flood hazard zones. It then quantifies who and what sits inside those zones — population, agricultural land, and built-up area — with a region selector for national and provincial summaries.

It is designed as both a standalone risk-screening dashboard and a portfolio piece demonstrating a full GEE workflow: DEM hydro-conditioning, SAR change detection, multi-source hazard fusion, and exposure analytics with a custom UI.

## Key Features

- **Region-level filtering** — national view or focus on Sindh, Punjab, Khyber Pakhtunkhwa, Balochistan, Gilgit-Baltistan (Skardu), Azad Jammu & Kashmir, or Jammu & Kashmir
- **Live exposure statistics** — total flood area (km²), exposed population, inundated cropland (km²), and built infrastructure at risk (km²), recomputed per region
- **Multi-layer map** — DEM, HAND elevation model, hazard classification, and each exposure layer, independently toggleable
- **Custom dark-themed sidebar UI** with legends for every layer

## Methodology

### 1. Study Area
Region boundaries are drawn from `FAO/GAUL/2015/level1`, combining Pakistan's national boundary with Kashmir-, Gilgit-, Baltistan-, and Skardu-named admin-1 units to cover disputed/frontier areas often excluded from national datasets.

### 2. Hydro-Conditioned DEM & HAND
Height Above Nearest Drainage (HAND) is derived from the Copernicus GLO-30 DEM rather than used as a standalone elevation cutoff, since raw elevation alone misclassifies flood risk in areas near but not directly on a drainage line:

- Permanent water pixels are identified from JRC Global Surface Water (`occurrence > 40%`) and used as the reference stream network.
- For each pixel, the nearest stream elevation is found via a focal minimum search (5 km radius); where no stream is found within range, a local minimum elevation (3 km radius) is used as a fallback.
- HAND = DEM elevation − nearest/local stream elevation, clamped to 0–1000 m.

### 3. Sentinel-1 SAR 2022 Flood Benchmark
Actual observed flooding from Pakistan's 2022 floods is used to validate and reinforce the hazard model:
- Pre-flood VV backscatter baseline: April–May 2022 (median composite)
- Peak-flood VV backscatter: August–September 2022 (minimum composite, since standing water sharply lowers VV return)
- A backscatter drop greater than 3 dB, combined with a DEM elevation below 1200 m, flags SAR-observed flood extent.

### 4. 50-Year Flood Hazard Zonation
A pixel is classified as within the 50-year floodplain if it has low HAND (≤5 m) **and** at least one of the following holds:
- It falls within JRC's observed maximum water extent, **or**
- It was flagged by the Sentinel-1 2022 SAR flood detection, **or**
- It has low historical water recurrence (≤30%, i.e. rarely inundated — consistent with a rare, long-return-period event rather than a permanent water body), **or**
- HAND ≤ 2.5 m (very low relative elevation)

Within that floodplain, pixels are further split into:
- **High hazard** — HAND ≤ 2 m or SAR-flagged
- **Moderate hazard** — everything else within the 50-year floodplain

### 5. Critical Asset Exposure Overlay
The hazard mask is intersected with three exposure datasets:
- **Population** — WorldPop 100 m gridded population (2020)
- **Cropland** — ESA WorldCover v200 (2021), cropland class
- **Built infrastructure** — JRC Global Human Settlement Layer, built-surface (2020)

Exposure statistics (area, population count) are computed with `reduceRegion` at the national or selected-province level.

## Data Sources

| Dataset | GEE Asset ID | Resolution | Purpose |
|---|---|---|---|
| Copernicus DEM GLO-30 | `COPERNICUS/DEM/GLO30` | 30 m | Elevation / HAND derivation |
| JRC Global Surface Water | `JRC/GSW1_4/GlobalSurfaceWater` | 30 m | Permanent water, occurrence, recurrence, max extent |
| Sentinel-1 SAR GRD | `COPERNICUS/S1_GRD` | 10 m | 2022 flood benchmark (VV backscatter change) |
| WorldPop | `WorldPop/GP/100m/pop` | 100 m | Exposed population |
| ESA WorldCover v200 | `ESA/WorldCover/v200/2021` | 10 m | Cropland exposure |
| JRC GHSL Built-Surface | `JRC/GHSL/P2023A/GHS_BUILT_S/2020` | 10 m | Built infrastructure exposure |
| FAO GAUL 2015 (level 1) | `FAO/GAUL/2015/level1` | vector | Admin boundaries / region filtering |

## Tech Stack

- Google Earth Engine JavaScript API (`ui.Map`, `ui.Panel`, `ui.Select`)
- Server-side raster/vector processing entirely within GEE (no external backend)
- Deployed via Earth Engine Apps

## How to Use

1. Open the [live app](https://project-e03cf9e5-91a7-4488-9fd.projects.earthengine.app/view/pakistan-50-year-flood-risk--asset-exposure-engine).
2. Use the **Select Region Focus** dropdown to zoom to a province/territory and recompute statistics for it.
3. Toggle layers (DEM, HAND, hazard classes, exposure layers, debug masks) from the Earth Engine layer panel.
4. Read exposure metrics — flood area, exposed population, inundated cropland, and infrastructure at risk — from the sidebar panel.

## Repository Structure

```
.
├── README.md
├── LICENSE
└── src/
    └── pakistan-flood-risk-engine.js   # Full GEE script (Code Editor-ready)
```

To run/edit the script yourself: copy `src/pakistan-flood-risk-engine.js` into the [GEE Code Editor](https://code.earthengine.google.com/) and click Run.

## Limitations & Assumptions

- **Not a substitute for hydraulic/hydrodynamic modeling.** This is a hazard-screening tool using terrain and remote sensing proxies (HAND, historical water occurrence, one flood event), not a calibrated hydraulic model (e.g. HEC-RAS) with a true 50-year return-period discharge.
- **2022 SAR benchmark is a single extreme event**, used as a proxy for a ~50-year-type flood rather than a statistically derived return period.
- Region name matching (province → GAUL admin units) uses simple string matching and may not perfectly align with all administrative boundary revisions, particularly in contested Kashmir/Gilgit-Baltistan areas.
- WorldPop (2020), ESA WorldCover (2021), and GHSL (2020) exposure layers are snapshot datasets and do not reflect current-year population or land cover changes.
- Coarser reduction scales (100–200 m) are used for national-level statistics for performance; local-scale results may differ slightly from full-resolution computation.

## Author

Developed by **Rohma** — GIS & Remote Sensing Analyst, Lahore, Pakistan.
Flood susceptibility mapping, SAR-based hazard detection, and Google Earth Engine application development.

## License

Released under the [MIT License](LICENSE). Underlying satellite and geospatial datasets remain subject to their original providers' licenses (Copernicus, JRC, WorldPop, ESA, FAO).
