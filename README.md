# UAV-Based 3D Terrain Modelling

Generation of a **Digital Height Model (DHM)** from overlapping UAV (drone) imagery using stereo photogrammetry and the classical **parallax equation**. The pipeline takes four georegistered aerial GeoTIFFs, computes dense stereo disparity between consecutive image pairs, converts disparity to terrain height, and mosaics the results into a single georeferenced DHM.

## Overview

The project reconstructs relative terrain height from a strip of nadir UAV images that overlap along the flight line. For each consecutive image pair it estimates per-pixel disparity (the apparent shift of ground points between the two views) and converts that disparity into height using the photogrammetric parallax relationship. Individual height tiles are then merged into one continuous, georeferenced surface.

## Flight & Camera Parameters

| Parameter | Value |
| --- | --- |
| Flying height above ground (H) | 33 m |
| Camera focal length (f) | 5.74 mm |
| Ground Sampling Distance (GSD) | 2.5 cm/pixel |
| Sensor pixel size | 4.35 µm |
| Input images | S1–S4 georegistered GeoTIFFs |

## Pipeline

1. **Load imagery & geotransforms** — read the four GeoTIFFs and their affine transforms / CRS with `rasterio`.
2. **Compute air base (B)** — derive the baseline distance between consecutive exposure stations from the image-centre world coordinates.
3. **Crop stereo overlap** — extract the geographically overlapping region for each image pair.
4. **Dense disparity (StereoSGBM)** — estimate disparity with OpenCV Semi-Global Block Matching (Hirschmüller, 2008), which handles low-texture aerial scenes better than block matching.
5. **Disparity → height** — apply the parallax equation to convert disparity into terrain height in metres.
6. **Mosaic & export** — merge the per-pair height tiles into a global DHM and write it out as a georeferenced GeoTIFF.

## Repository Contents

| File | Description |
| --- | --- |
| `Assignment1_DHM_Optimized.ipynb` | Main notebook implementing the full DHM pipeline (Google Colab). |
| `S1_B_georegistered_tiff.tif` | Input georegistered UAV image 1. |
| `S2_B_georegistered_tiff.tif` | Input georegistered UAV image 2. |
| `S3_B_georegistered_tiff.tif` | Input georegistered UAV image 3. |
| `S4_B_georegistered_tiff.tif` | Input georegistered UAV image 4. |
