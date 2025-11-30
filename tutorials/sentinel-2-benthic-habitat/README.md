# Sentinel-2 Level 2A Benthic Habitat Mapping

This tutorial demonstrates processing Sentinel-2 Level 2A Multi-Spectral Instrument (MSI) imagery for benthic habitat mapping in shallow coastal waters using Google Colab.

## Overview

The workflow includes:

1. **Sun Glint Removal (Deglinting)** - Hedley et al. (2005) method to remove specular reflection from water surface
2. **Depth Invariant Index (DII) Calculation** - Multiple band ratios to reduce depth effects on substrate classification
3. **K-Means Unsupervised Classification** - Clustering algorithm to identify distinct benthic habitat types

## Features

- ✅ Google Drive integration for data loading
- ✅ Hedley deglinting algorithm implementation
- ✅ Multiple DII indices (B2/B3, B3/B4, NDWI)
- ✅ K-Means clustering with customizable parameters
- ✅ Interactive visualization with Folium
- ✅ Area statistics and analysis
- ✅ Export results to Google Drive

## Requirements

- Google Earth Engine account
- Google Drive account
- Sentinel-2 Level 2A imagery
- Shapefile defining Area of Interest (AOI)

## Data Preparation

Upload the following to your Google Drive:

1. **Sentinel-2 imagery** (optional if using Earth Engine catalog)
2. **AOI shapefile** (.shp and associated files)

Create a folder structure like:
```
Google Drive/
└── sentinel_benthic_data/
    ├── aoi.shp
    ├── aoi.shx
    ├── aoi.dbf
    └── aoi.prj
```

## Usage

1. Open the notebook in Google Colab
2. Run the authentication cells for Earth Engine
3. Mount your Google Drive
4. Update the data paths to match your folder structure
5. Adjust parameters as needed:
   - Date range for imagery
   - Number of K-Means clusters
   - Deglinting parameters
6. Run all cells sequentially

## Key Parameters

### Deglinting
- `nir_band`: NIR band for glint detection (default: 'B8')
- `min_nir_percentile`: Percentile for minimum NIR value (default: 1)

### Classification
- `n_clusters`: Number of habitat classes (default: 5)
- `classification_bands`: Bands used for clustering

### Export
- `scale`: Export resolution in meters (default: 10m)
- `crs`: Coordinate reference system (default: 'EPSG:4326')

## Output

The notebook generates:

1. **Interactive maps** showing:
   - Original vs deglinted imagery
   - Depth invariant indices
   - Classification results

2. **Exported files** (to Google Drive):
   - Classified habitat map
   - Deglinted RGB composite
   - DII index bands

3. **Statistics**:
   - Area by habitat class
   - Distribution pie chart

## Interpretation Guide

Classification results typically identify:

- **Cluster 0**: Deep water / Dark substrate
- **Cluster 1**: Seagrass / Dense vegetation
- **Cluster 2**: Sand / Bright substrate
- **Cluster 3**: Mixed substrate
- **Cluster 4**: Coral / Reef structure

**Note**: Cluster assignments are not predetermined. Validate results with ground truth data.

## References

- Hedley, J. D., Harborne, A. R., & Mumby, P. J. (2005). Simple and robust removal of sun glint for mapping shallow‐water benthos. *International Journal of Remote Sensing*, 26(10), 2107-2112.

- Lyzenga, D. R. (1978). Passive remote sensing techniques for mapping water depth and bottom features. *Applied Optics*, 17(3), 379-383.

- Roelfsema, C., et al. (2018). Coral reef habitat mapping: A combination of object-based image analysis and ecological modelling. *Remote Sensing of Environment*, 208, 27-41.

## Tips for Best Results

1. **Image Selection**:
   - Choose images with low cloud cover (<10%)
   - Select images at low tide for better benthic visibility
   - Prefer calm sea conditions (reduced waves and glint)

2. **Deglinting**:
   - Verify deglinting effectiveness by comparing before/after visualizations
   - Adjust NIR threshold if deep water areas are not properly corrected

3. **Classification**:
   - Start with 5 clusters and adjust based on habitat complexity
   - Use ground truth data to validate and refine cluster assignments
   - Consider supervised classification if training data is available

4. **Validation**:
   - Compare results with high-resolution imagery (e.g., Planet, WorldView)
   - Conduct field surveys for accuracy assessment
   - Calculate confusion matrix if ground truth data exists

## Troubleshooting

**Issue**: Authentication errors
- **Solution**: Re-run `ee.Authenticate()` and follow prompts

**Issue**: Memory errors during export
- **Solution**: Reduce AOI size or increase `maxPixels` parameter

**Issue**: Poor deglinting results
- **Solution**: Adjust `min_nir_percentile` or select a different sample region

**Issue**: Too many/few clusters
- **Solution**: Adjust `n_clusters` parameter based on expected habitat diversity

## License

Copyright 2025 The Earth Engine Community Authors

Licensed under the Apache License, Version 2.0
