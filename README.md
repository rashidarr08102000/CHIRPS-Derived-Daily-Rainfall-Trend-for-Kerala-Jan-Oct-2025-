# CHIRPS-Derived-Daily-Rainfall-Trend-for-Kerala-Jan-Oct-2025-
Analyzes average daily rainfall over Kerala (Jan–Oct 2025) using CHIRPS Daily data in Google Earth Engine. The script filters data by date and region, computes mean rainfall for each day, and visualizes temporal rainfall variation with a time-series chart.

## 🛰️ Dataset Information

- **Dataset ID:** `UCSB-CHG/CHIRPS/DAILY`  
- **Description:** Climate Hazards Group InfraRed Precipitation with Station Data (CHIRPS) — provides daily precipitation estimates from satellite and gauge data.  
- **Resolution:** ~5.6 km (0.05°)  
- **Temporal Coverage:** 1981–present  
- **Unit:** mm/day

---

## 🗺️ Study Area
- **Region:** Kerala, India  
- **Time Period:** January 1 – October 31, 2025  
- **AOI Input:** User-defined geometry in GEE (`var aoi = geometry;`)

---

## 💻 Script Overview

```javascript
// Define Area of Interest (AOI)
var aoi = geometry;

// Set time range
var startDate = '2025-01-01';
var endDate = '2025-10-31';

// Define rainfall dataset and parameters
var imageCollection = 'UCSB-CHG/CHIRPS/DAILY';
var bandName = 'precipitation';
var resolution = 5566; // in meters

// Load and filter the CHIRPS rainfall data
var rainfall = ee.ImageCollection(imageCollection)
  .filter(ee.Filter.date(startDate, endDate))
  .select(bandName);

// Generate a time-series chart of average daily rainfall
var chart = ui.Chart.image.series({
  imageCollection: rainfall,
  region: aoi,
  reducer: ee.Reducer.mean(),
  scale: resolution
}).setOptions({
  title: 'Average Daily Rainfall over Kerala (Jan–Oct 2025)',
  hAxis: {title: 'Date'},
  vAxis: {title: 'Rainfall (mm/day)'}
});

print(chart);

// Display the study area
Map.addLayer(aoi);
Map.centerObject(aoi);
