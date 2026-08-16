# Nairobi-Kiambu Spatial Flood Risk Framework

An end-to-end spatial data science framework leveraging gradient-boosted decision trees (LightGBM) and Earth Observation (EO) data to model high-resolution flood susceptibility across the rapidly urbanizing Nairobi-Kiambu corridor.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-username/nairobi-kiambu-flood-risk/blob/main/notebooks/01_flood_risk_model.ipynb)

---

## Project Overview

Unplanned urban and peri-urban expansion along the Nairobi-Kiambu border has significantly altered local hydrological regimes. Rapid loss of green cover and the expansion of impermeable surfaces diminish natural soil infiltration, causing moderate rain events to trigger localized flash flooding.

This project bridges the gap between point-based statistical evaluation and full-coverage spatial prediction by:
1. Multi-Source Data Ingestion: Extracting terrain (SRTM DEM), hydrologic (TWI), vegetation (NDVI), built-up (NDBI), and climate (CHIRPS precipitation) layers via Google Earth Engine.
2. Machine Learning Modeling: Training a LightGBM gradient boosting model to capture complex, non-linear interactions among hydro-geomorphological features and historical flood occurrences.
3. Wall-to-Wall Inference: Generating continuous predictions across all ~2.7 million 30-meter grid cells covering the Nairobi-Kiambu study area.
4. WebGIS Delivery: Exporting georeferenced GeoTIFF risk surfaces for interactive browser-based spatial analysis and asset underwriting.

---

## System & Model Architecture

* **1. Raw Spatial Inputs**
  * SRTM DEM (30m Elevation)
  * Sentinel-2 / Landsat Optical Imagery
  * CHIRPS Daily Rainfall Metrics

* **2. Geospatial Feature Extraction**
  * Hydro-Geomorphology: Slope, Aspect, Topographic Wetness Index (TWI)
  * Surface Covers: NDVI (Vegetation), NDBI (Built-up), NDWI (Water)
  * Climatic & Proximity: Seasonal Rainfall, Distance to River

* **3. LightGBM Gradient Boosting**
  * Input Data: Sampled spatial training matrix
  * Feature Interaction: Learns non-linear spatial dependencies
  * Performance: R2 = 0.9289 across validation points

* **4. Wall-to-Wall Raster Inference**
  * Spatial Coverage: ~2.7 Million Grid Cells (30m Resolution)
  * Continuous Export: Georeferenced GeoTIFF Risk Surface

* **5. WebGIS & Commercial Delivery**
  * Interactive Map: Browser-based visualization
  * Use Cases: Site-level asset pricing & exposure scoring

---

## Repository Structure

* **nairobi-kiambu-flood-risk/**
  * **data/**
    * **raw/** (Primary EO rasters and boundary shapefiles)
    * **processed/** (Sampled spatial grid points in CSV/Parquet format)
  * **notebooks/**
    * **01_flood_risk_model.ipynb** (Earth Engine feature ingestion, LightGBM training, and inference)
  * **outputs/**
    * **maps/** (Exported 30m GeoTIFF flood probability rasters)
    * **webgis/** (Standalone interactive HTML map files)
  * **src/**
    * **processing.py** (Feature engineering and spatial normalization utilities)
    * **evaluation.py** (Model validation metrics and performance plots)
  * **README.md** (Project documentation and Data Dictionary)
  * **requirements.txt** (Python package dependencies)

---

## Technical Stack

* Language: Python 3.10+
* Geospatial Processing: Google Earth Engine (ee), geemap, rasterio, geopandas, rioxarray
* Machine Learning & Analytics: lightgbm, scikit-learn, numpy, pandas
* Visualization & WebGIS: folium, matplotlib, seaborn

---

## Key Performance Results

* Predictive Accuracy: Achieved an R2 score of 0.9289 across 10,000 spatial validation points.
* Spatial Resolution: Continuous 30-meter grid cell prediction across the entire Nairobi-Kiambu bounding box (~2.7 million grid cells).
* Commercial Applications: Enables coordinate-level flood risk scoring for localized property pricing, portfolio exposure monitoring, and parametric insurance underwriting.

---

# Data Dictionary

### Spatial Metadata
* Study Area: Nairobi-Kiambu Peri-Urban Corridor, Kenya
* Coordinate Reference System: EPSG:4326 (WGS 84) / EPSG:32637 (UTM Zone 37N)
* Resolution: 30 meters x 30 meters per grid cell

| Variable Name | Data Type | Units / Range | Source Dataset | Description |
| :--- | :--- | :--- | :--- | :--- |
| cell_id | String / Int | Unique ID | Spatial Grid | Unique identifier for each 30m grid cell. |
| latitude | Float64 | Decimal Degrees | Spatial Grid | Y-coordinate centroid of the grid cell. |
| longitude | Float64 | Decimal Degrees | Spatial Grid | X-coordinate centroid of the grid cell. |
| elevation | Float32 | Meters (m) | SRTM DEM (30m) | Height above mean sea level. |
| slope | Float32 | Degrees | Derived from DEM | Incline angle of terrain; flatter areas denote higher surface runoff accumulation. |
| aspect | Float32 | Degrees | Derived from DEM | Compass direction facing of slope face (0 to 360 degrees). |
| twi | Float32 | Continuous Index | Derived from DEM | Topographic Wetness Index; measures terrain control on water accumulation. |
| dist_to_river | Float32 | Meters (m) | HydroSHEDS / OSM | Straight-line distance to nearest river or drainage channel. |
| chirps_precip | Float32 | Millimeters (mm) | CHIRPS Rainfall | Mean seasonal peak precipitation metric. |
| ndvi | Float32 | -1.0 to 1.0 | Sentinel-2 / Landsat | Normalized Difference Vegetation Index; proxy for green cover and natural retention. |
| ndbi | Float32 | -1.0 to 1.0 | Sentinel-2 / Landsat | Normalized Difference Built-up Index; proxy for surface impermeability and urban density. |
| ndwi | Float32 | -1.0 to 1.0 | Sentinel-2 / Landsat | Normalized Difference Water Index; identifies surface water bodies and saturated soils. |
| flood_occurrence | Int64 | 0 (No) or 1 (Yes) | Ground Truth | Target label indicating historical flood observation at sample locations. |
| predicted_risk_score | Float32 | 0.0 to 1.0 | LightGBM Output | Model-predicted probability score of flood susceptibility for each 30m cell. |
| risk_category | String | Low / Medium / High / Very High | Reclassified Output | Categorical risk band assigned based on predicted probability thresholds. |

---

## Setup & Execution

1. Clone Repository:
   git clone https://github.com/your-username/nairobi-kiambu-flood-risk.git
   cd nairobi-kiambu-flood-risk

2. Install Required Packages:
   pip install -r requirements.txt

3. Authenticate Google Earth Engine:
   import ee
   ee.Authenticate()
   ee.Initialize()

4. Run Pipeline:
   Open notebooks/01_flood_risk_model.ipynb directly in Google Colab using the top badge or run locally via Jupyter.
