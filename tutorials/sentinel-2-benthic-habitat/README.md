# Sentinel-2 Level 2A Benthic Habitat Mapping

This tutorial demonstrates processing Sentinel-2 Level 2A Multi-Spectral Instrument (MSI) imagery for benthic habitat mapping in shallow coastal waters using Google Colab.

## Overview

This tutorial consists of **two parts**:

### Part 1: Unsupervised Classification (`index.ipynb`)

1. **Sun Glint Removal (Deglinting)** - Hedley et al. (2005) method to remove specular reflection from water surface
2. **Depth Invariant Index (DII) Calculation** - Multiple band ratios to reduce depth effects on substrate classification
3. **K-Means Unsupervised Classification** - Clustering algorithm to identify distinct benthic habitat types

### Part 2: Supervised Classification (`supervised-classification.ipynb`)

**NEW!** Use your field survey data for improved classification:

1. **Load Ground Truth Shapefile** - Import field survey points/polygons
2. **Extract Training Samples** - Sample imagery at ground truth locations
3. **Train Multiple Classifiers** - Random Forest, SVM, and CART
4. **Accuracy Assessment** - Confusion matrix and per-class accuracy
5. **Habitat Area Analysis** - Calculate area statistics per class

**When to use Part 2:** If you have field survey data (shapefile with habitat types), start here for more accurate results!

## Features

### Part 1 (Unsupervised):
- ✅ Google Drive integration for data loading
- ✅ Hedley deglinting algorithm implementation
- ✅ Multiple DII indices (B2/B3, B3/B4, NDWI)
- ✅ K-Means clustering with customizable parameters
- ✅ Interactive visualization with Folium
- ✅ Area statistics and analysis
- ✅ Export results to Google Drive

### Part 2 (Supervised):
- ✅ Ground truth shapefile import (points or polygons)
- ✅ Automatic training sample extraction
- ✅ Multiple classifiers: Random Forest, SVM, CART
- ✅ Accuracy assessment with confusion matrix
- ✅ Per-class accuracy metrics (Producer's/User's accuracy)
- ✅ Classifier comparison and automatic best selection
- ✅ Visual validation with ground truth overlay
- ✅ Detailed classification reports

## Requirements

- Google Earth Engine account
- Google Drive account
- Sentinel-2 Level 2A imagery
- Shapefile defining Area of Interest (AOI)

## Data Preparation

### For Part 1 (Unsupervised):

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

### For Part 2 (Supervised) - Additional Requirements:

Add **ground truth shapefile** from field survey:

```
Google Drive/
└── sentinel_benthic_data/
    ├── aoi.shp
    ├── ground_truth.shp     ← Field survey data
    ├── ground_truth.shx
    ├── ground_truth.dbf
    └── ground_truth.prj
```

**Ground Truth Shapefile Requirements:**
- Can be **points** (GPS locations) or **polygons** (habitat areas)
- Must have a column containing **habitat class names** (e.g., 'coral', 'seagrass', 'sand')
- Column can be named: 'class', 'habitat', 'type', or any name (you'll specify in the notebook)
- Recommended: **At least 50 samples per habitat class** for good accuracy
- Example attributes table:

| ID | habitat  | depth | date       |
|----|----------|-------|------------|
| 1  | coral    | 5.2   | 2024-03-15 |
| 2  | seagrass | 3.8   | 2024-03-15 |
| 3  | sand     | 2.1   | 2024-03-15 |

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

### Part 1 Output:

1. **Interactive maps** showing:
   - Original vs deglinted imagery
   - Depth invariant indices
   - K-Means classification results

2. **Exported files** (to Google Drive):
   - Classified habitat map
   - Deglinted RGB composite
   - DII index bands

3. **Statistics**:
   - Area by habitat class
   - Distribution pie chart

### Part 2 Output:

1. **Accuracy metrics**:
   - Overall accuracy (typically 70-95% with good ground truth)
   - Kappa coefficient
   - Confusion matrix
   - Producer's and User's accuracy per class

2. **Comparison charts**:
   - Classifier performance comparison
   - Per-class accuracy visualization

3. **Exported files** (to Google Drive):
   - Best classifier result (GeoTIFF)
   - Classification report (TXT)
   - Area statistics (CSV)

4. **Interactive maps**:
   - Side-by-side classifier comparison
   - Ground truth overlay for validation
   - Habitat distribution maps

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

### Part 2 (Supervised Classification) Troubleshooting

**Issue**: `HttpError 400: Property 'class' of feature '0_0': Invalid type. Expected type: Float. Actual type: String`
- **Cause**: Ground truth shapefile has text class names (e.g., 'Seagrass', 'Coral') but Earth Engine expects numeric values
- **Solution**: **DON'T manually change your shapefile!** The notebook now automatically converts text to numbers:
  - Cell 17 creates a mapping: `{'Coral': 0, 'Seagrass': 1, 'Sand': 2, ...}`
  - Text labels are converted to numbers internally
  - Results are converted back to text for display
  - **Just re-run the notebook with the updated version**

**Issue**: "Column 'habitat' not found"
- **Cause**: Your shapefile uses a different column name for habitat types
- **Solution**: Check your shapefile's attribute table and update `class_column` variable:
  ```python
  class_column = 'Kelas_Baru'  # or 'type', 'class', etc.
  ```

**Issue**: "Insufficient training samples"
- **Cause**: Not enough ground truth points for reliable classification
- **Solution**:
  - Add more field survey points (aim for 50+ per class)
  - Use polygon sampling instead of points
  - Reduce number of classes if some are rare

**Issue**: Low accuracy (<60%)
- **Possible causes**:
  - Ground truth data quality issues
  - Survey date too far from satellite image date
  - Habitat changed between survey and image date
  - Poor image quality (clouds, glint, turbidity)
- **Solutions**:
  - Validate ground truth in field
  - Select image closer to survey date
  - Add more training samples
  - Adjust classifier parameters
  - Check for labeling errors in shapefile

**Issue**: Confusion matrix shows high confusion between certain classes
- **Cause**: Classes are spectrally similar or overlap spatially
- **Solutions**:
  - Combine similar classes (e.g., 'dense seagrass' + 'sparse seagrass' → 'seagrass')
  - Add more discriminating bands/indices
  - Use texture features
  - Check if ground truth points are in transition zones

## License

Copyright 2025 The Earth Engine Community Authors

Licensed under the Apache License, Version 2.0
