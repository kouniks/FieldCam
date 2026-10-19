# 🛰️ FieldCam v1 — Geo-Camera for Field Validation & Ground-Truthing

**FieldCam v1** is a lightweight, zero-backend web application designed for environmental fieldwork, Land Use / Land Cover (LULC) ground-truthing, and spatial validation surveys. It captures photographs using your smartphone's camera and burns a tamper-evident **Satellite/Street mini-map**, **reverse-geocoded place name**, **GPS telemetry**, and **sample metadata** directly into the exported JPEG alongside **CSV** and **GeoJSON** logs.

🔗 **Live App:** `https://kouniks.github.io/FieldCam/`

---

## ✨ Key Features

* **Dual-Mode Field Workflow:**
  * **Project Mode:** Designed for structured surveys. Stamps customizable **Project Name**, **Auto-Incrementing Plot/Sample ID** (e.g., `PLOT-001` $\rightarrow$ `PLOT-002`), **Validation Class / LULC Category**, and **Field Observer Notes**.
  * **Default Mode:** One-tap **Geo-Stamp Preset** for rapid photo documentation without manual metadata entry.
* **Embedded Map Watermark:** Dynamically stitches a 3×3 grid of **Esri World Imagery (Satellite)** or **OpenStreetMap (Street)** tiles centered on your exact GPS coordinates, complete with a target crosshair, accuracy halo, North arrow, and zoom level indicator (`Z15`–`Z18`).
* **Automatic Place Name Detection:** Performs live reverse geocoding (via OpenStreetMap Nominatim with BigDataCloud fallback) to stamp the local neighbourhood, suburb, city, and state onto every capture.
* **High-Accuracy Telemetry:** Displays and records both **Decimal Degrees** and **Degrees-Minutes-Seconds (DMS)** coordinates, **Altitude (m ASL)**, real-time **Horizontal GPS Accuracy ($\pm$ meters)**, **Day of the Week**, **Local Time**, and **UTC Timestamp**.
* **Portrait & Landscape Auto-Fit Engine:** Dynamically scales the watermark card and typography strictly relative to the horizontal frame width so telemetry text never clips on mobile portrait captures.
* **GIS-Ready Batch Export:** Exports all session captures in a single click as **CSV** or **GeoJSON** (`FeatureCollection`) for immediate use in **QGIS**, **Google Earth Engine (GEE)**, or **Python** (`GeoPandas`).
* **100% Client-Side & Private:** Runs entirely in the mobile browser. No photos or GPS coordinates are uploaded to external servers, and no paid API keys are required.

---

## 🎛️ Modes of Operation

| Feature | Project Mode | Default Mode |
| :--- | :--- | :--- |
| **Primary Use Case** | Structured ground-truthing & plot surveys | Quick field snapshots & general geo-tagging |
| **Header Stamp** | `PLOT-001 • BUILT-UP / URBAN [FieldCam v1]` | `FieldCam v1 • GEO-STAMP PRESET` |
| **Custom Metadata** | Project Name, Plot ID, LULC Class, Notes | Hidden (Uses automatic preset) |
| **Plot ID Auto-Increment** | Yes (`PLOT-001` $\rightarrow$ `PLOT-002`) | No (`FieldCam_<Day>_<Timestamp>.jpg`) |
| **Map & GPS Telemetry** | Full (Map, Place Name, Dec/DMS, Alt, Acc, Day/Time) | Full (Map, Place Name, Dec/DMS, Alt, Acc, Day/Time) |

---

## 📱 Mobile Field Guide

1. **Launch over HTTPS:** Open the GitHub Pages URL in **Chrome** (Android) or **Safari** (iOS) and grant **Camera** and **Location** permissions.
2. **Add to Home Screen (Recommended):** Tap the browser menu (**⋮** or **Share**) and select **Add to Home Screen** to launch FieldCam v1 as a full-screen app.
3. **Wait for GPS Lock:** Step outdoors and check the top-right status pill:
   * 🟢 **Green (`≤ 12m`):** High-precision lock — ready for validation capture.
   * 🟡 **Amber (`13m – 30m`):** Moderate lock — wait a few seconds for satellite refinement.
   * 🔴 **Red (`> 30m`):** Coarse lock — move away from indoor roofs or dense canopy.
4. **Select Mode & Map Layer:**
   * Use the top toggle to switch between **Project Mode** and **Default Mode**.
   * Tap the **`SAT` / `OSM`** button on the mini-map preview to switch between Satellite imagery and Street tiles.
   * In **Project Mode**, tap **📝** on the bottom bar to configure your Project Name, starting Plot ID, LULC Class, Map Zoom, and Notes.
5. **Capture & Export:**
   * Tap the green shutter button to capture a geo-stamped frame.
   * Tap **🗂️** on the bottom bar to open the **Captured Validation Log**, save individual JPEGs, or download the session **CSV** / **GeoJSON** files.

---

## 🗺️ GIS & Data Analysis Integration

* **QGIS:** Drag and drop the exported `.geojson` file directly into your QGIS canvas, or load the `.csv` file via *Layer $\rightarrow$ Add Layer $\rightarrow$ Add Delimited Text Layer* (`X field = longitude`, `Y field = latitude`, `CRS = EPSG:4326`).
* **Google Earth Engine (GEE):** Convert the exported `.csv` or `.geojson` into a Shapefile/FeatureCollection asset to validate classified LULC rasters or train Random Forest / CART classifiers.
* **Python (`GeoPandas`):**
  ```python
  import geopandas as gpd

  gdf = gpd.read_file("FieldCam_v1_2026-10-19.geojson")
  print(gdf[["plot_id", "validation_class", "place_name", "gps_accuracy_m", "geometry"]].head())
