# DC Urban Heat Change Analysis (2015–2025)

This repository analyzes changes in land surface temperature (LST) in Washington, DC using **Landsat 8 thermal imagery**, **land cover data**, and **census income data**. The project explores how urban heat island (UHI) effects have evolved over a 10-year period and investigates potential correlations with socioeconomic indicators.

We integrate zonal statistics, robust regression, outlier detection, and OSINT analysis of climate-related news to contextualize urban heat change and its spatial patterns.

**Live data + interactive maps** are too large to host here. You can [access them on Google Drive](https://drive.google.com/drive/folders/1YKCJtaHW_XrL2SuLIOhImUCbyeQlTM-B?usp=drive_link).

---

## Summary of Findings

- Surface temperatures increased in most of DC from June 2015 to June 2025.
- Raw ΔT (temperature change) reached as high as **+63.5°F** in some zones — later flagged as outliers and removed.
- After z-score filtering (±3), most ΔT values ranged from **-5°F to +5°F**.
- **Robust regression** using median household income shows no statistically significant association with heat gain (p > 0.99).
- **AI-enhanced OSINT analysis** of climate headlines suggests intensifying summer heatwaves in DC but no consistent spatial explanation for heat spikes.

---

## Methodology

### Input Data

| Type | Source |
|------|--------|
| **Thermal Bands** | Landsat 8 ST_B10 (2015 & 2025) |
| **Land Cover** | `Annual_NLCD_LndCov_2024_CU_C1V1.tif.aux` |
| **Vector Boundary** | DC extent used to clip rasters |
| **Census Income** | Median household income via Census API (GEOID merge) |
| **News OSINT** | Geotagged articles filtered for heat-related terms |

### Analysis Workflow

1. **Temperature Conversion**
   - Landsat DN → LST Kelvin → °C → °F

2. **Land Cover Classification**
   - NLCD classes mapped to Urban, Vegetation, Water, Other

3. **Zonal Statistics**
   - Per-polygon mean temps (2015, 2025)
   - ΔT (Temp_2025 – Temp_2015) calculated

4. **Outlier Filtering**
   - Z-score filter applied to ΔT
   - Data capped to 1st–99th percentile

5. **Regression Modeling**
   - `statsmodels.RLM` with HuberT norm
   - Dependent: ΔT, Independent: Median income
   - Non-significant relationship detected

6. **OSINT Integration**
   - GPT-4 model used to cross-analyze geotagged headlines
   - Summary adds narrative layer to spatial change data

---

## Interactive Maps

![](temporal-heat-map-dc.png)

Maps include:

- Mean Surface Temperature (°F) for 2015
- Mean Surface Temperature (°F) for 2025
- ΔT Change Zones (°F) with `coolwarm` gradient
- Filtered version without extreme outliers

Explore and modify maps via `leafmap` in the provided notebook.

---

## Files

- `dc_heat_analysis.ipynb` — Main analysis notebook (zonal stats, regression, maps)
- `uhi_temp_change_2015_2025_fahrenheit.shp` — Final ΔT shapefile (post-filter)
- `uhi_landcover_polygons.shp` — Classified NLCD zones
- `nlcd_2024_dc_clipped.tif` — Clipped land cover raster
- `lst_2015_fahrenheit.tif`, `lst_2025_fahrenheit.tif` — Surface temperature rasters

Full dataset download:
[📂 Google Drive Folder](https://drive.google.com/drive/folders/1YKCJtaHW_XrL2SuLIOhImUCbyeQlTM-B?usp=drive_link)

---

## Tech Stack

- Python 3
- `rasterio`, `geopandas`, `numpy`, `leafmap`, `statsmodels`, `matplotlib`
- OpenAI API (for AI-enhanced OSINT summaries)
- Census API (median income extraction)

---

## Citation

If using this analysis or derived products in research, please cite:

> Burnett, C. (2025). *DC Urban Heat Change Analysis (2015–2025)*. Independent spatial regression and AI-assisted media analysis.

---

## Contact

Have questions or want to collaborate?

**Email:** [charlaburnett89@gmail.com](mailto:charlaburnett89@gmail.com)  
**GitHub:** [@charlaburnett](https://github.com/charlaburnett)

---

© 2025 Charla Burnett. Released under the MIT License. Data usage governed by relevant satellite and census terms.
