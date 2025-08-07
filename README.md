# VectorScope: Similarity Search with Embeddings

App link: https://ee-iameztoy.projects.earthengine.app/view/vectorscope

## Overview

**VectorScope** is a Google Earth Engine application that leverages Google’s Satellite Embedding V1 to map “pixels that look like your samples.” You can either draw sample points within the UI or load them from an existing Earth Engine asset. The app computes a cosine-similarity heat-map of 64-dimensional embeddings for any year between 2017 and 2025, allows threshold tuning, toggles the heat-map on/off, and exports a binary mask to your choice of projections (WGS 84, UTM, EPSG 3587).

## Features

- **AOI drawing**: define your area of interest with a single polygon or rectangle.  
- **Sample points**: draw your own points or load a point FeatureCollection asset.  
- **Year & threshold**: select year (2017–2025) and similarity threshold (0.80–0.99).  
- **Heat-map & mask**: view continuous similarity, then generate a binary magenta mask above threshold.  
- **Export**: save the mask as an Earth Engine asset in WGS 84, UTM (auto-zone), or EPSG 3587.

## Usage

1. Open `code/main.js` in the Earth Engine Code Editor.  
2. Click **Run** to load the UI on the map.  
3. Draw your AOI (polygon or rectangle).  
4. Choose sample points by either:  
   - Drawing points in a new geometry layer, **or**  
   - Checking “Use sample points from asset” and entering your FeatureCollection ID.  
5. Select a year (default = 2020) and adjust the similarity threshold.  
6. Click **Run Analysis** to display the heat-map and mask.  
7. (Optional) Enter an asset ID, choose a projection, and click **Export Mask → Asset**.
