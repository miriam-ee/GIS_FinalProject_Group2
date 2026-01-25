# GIS Analysis: Walking Accessibility to Childcare and Schools
### A Spatial Comparison of Family-Friendliness in Graz and Utrecht

## Table of Contents
- [Project Description](#project-description)
- [Research Questions](#research-questions)
- [Study Areas](#study-areas)
- [Installation & Setup](#installation--setup)
- [Data Sources](#data-sources)
- [Running the Analysis](#running-the-analysis)
- [Workflow Overview](#workflow-overview)
- [Key Outputs](#key-outputs)
- [Reproducibility Notes](#reproducibility-notes)
- [Limitations](#limitations)
- [License](#license)
- [Contributors](#contributors)

## Project Description

This project evaluates walking accessibility to childcare facilities and schools in Graz (Austria) and Utrecht (Netherlands) using network-based spatial analysis. The analysis uses slope-aware pedestrian networks, H3 hexagonal grids, and population data to identify areas with good or poor family-oriented accessibility within a 15-minute walking threshold.

The methodology combines:
- **OSM pedestrian networks** with slope-adjusted walking speeds (Tobler's hiking function)
- **H3 hexagonal grids** (resolution 8) for spatial aggregation
- **GHSL population data** for population-weighted accessibility metrics
- **Multi-source Dijkstra algorithm** for accessibility calculation

## Research Questions

1. How accessible are childcare facilities and schools within a 15-minute walking threshold across Graz and Utrecht?
2. How does Graz's family-friendly accessibility compare to Utrecht's, and which spatial patterns indicate potential areas for improvement?

## Study Areas

- **Graz, Austria** (Population: ~333,000; Area: 127 km²)
- **Utrecht, The Netherlands** (Population: ~396,000; Area: 99 km²)

## Installation & Setup

### Prerequisites
- Python 3.11 or higher (developed and tested with 3.11 and 3.12)
- Git
- Jupyter Notebook or VS Code with Jupyter extension

### Step 1: Clone the Repository
```bash
git clone https://github.com/miriam-ee/GIS_FinalProject_Group2.git
cd GIS_FinalProject_Group2
```

### Step 2: Install Dependencies
The notebook installs all required packages automatically in the first code cell:
```python
!pip install geopandas keplergl osmnx networkx shapely pandas numpy matplotlib rasterio rasterstats h3
```

**Required libraries:**
- `geopandas` - Geospatial data handling
- `osmnx` - OpenStreetMap network download
- `networkx` - Network analysis algorithms
- `shapely` - Geometric operations
- `h3` - Hexagonal grid generation
- `rasterio` - Raster data processing
- `rasterstats` - Zonal statistics
- `pandas`, `numpy` - Data manipulation
- `matplotlib` - Visualization
- `keplergl` - Interactive mapping

## Data Sources

All data is downloaded automatically or pre-processed in the repository:

| Data Type | Source | Format | Handling |
|-----------|--------|--------|----------|
| **City boundaries** | OpenStreetMap (via OSMnx) | Vector | Downloaded automatically |
| **Childcare/Schools (POIs)** | OpenStreetMap (via OSMnx) | Vector | Downloaded automatically |
| **Walking network** | OpenStreetMap (via OSMnx) | Graph | Downloaded automatically |
| **Population data** | Global Human Settlement Layer (GHSL) | Raster → Parquet | Pre-aggregated in repository |
| **Digital Elevation Model (DEM)** | Copernicus GLO-30 (OpenTopography) | GeoTIFF | Included in repository |

### Population Data Handling

The population raster files (.tif) are **too large for GitHub** (~100+ MB). To ensure reproducibility without requiring manual downloads:

1. **Pre-aggregated GeoParquet files** are included in `Data/Processed/`:
   - `ghsl_pop_graz.parquet`
   - `ghsl_pop_utrecht.parquet`

2. The code automatically checks for these files first and uses them if available.

3. **If you need to regenerate from scratch** (not required for normal use):
   - Download GHSL population data from [European Union GHSL Portal](https://human-settlement.emergency.copernicus.eu/download.php?ds=pop)
   - Place files in `Data/Raw/` as:
     - `GHS_POP_GRAZ_RAW.tif`
     - `GHS_POP_UTRECHT_RAW.tif`
   - The notebook will process and save new `.parquet` files

### DEM Files

Pre-processed DEM files are included in `Data/Processed/`:
- `dem_graz_4326.tif`
- `dem_utrecht_4326.tif`

Source: Copernicus GLO-30 Digital Elevation Model via [OpenTopography](https://portal.opentopography.org/raster?opentopoID=OTSDEM.032021.4326.3)

## Running the Analysis

### Standard Execution

   Open in VS Code with Jupyter extension

**Run all cells sequentially** 
   - No manual input required
   - All data downloads automatically
   - Processing time: ~3-15 minutes depending on system

### Cell-by-Cell Execution

For detailed exploration, run cells individually:
- Follow the notebook structure from top to bottom
- Each section builds on previous results
- Kepler maps require manual interaction (fill color based on field value)

### Customization for Other Cities

To analyze different cities:

1. **Modify city definitions** (Part A.1 and B.1):
```python
   place_name = "Your City, Country"
   target_crs = XXXX  # appropriate EPSG code
```

2. **Download population and DEM data** for your study area

3. **Update file paths** in population/DEM loading sections

## Workflow Overview

### A: Graz, Austria
1. **A.1 Data Preparation**
   - Define and import study area boundary
   - Download OSM childcare facilities and schools
   - Download walkable street network (custom filter for bridges, footways)
   - Create H3 hexagon grid (resolution 8)
   - Load population data (GHSL)
   - Load DEM and compute slope-adjusted walking speeds (Tobler's function)

2. **A.2 Accessibility Calculation**
   - Snap hexagon centroids to nearest network nodes
   - Snap POIs to nearest network nodes
   - Multi-source Dijkstra algorithm for shortest walking times
   - Attach travel times to hexagons

3. **A.3 Accessibility Indicators**
   - Binary indicators: childcare ≤15 min, school ≤15 min, both ≤15 min
   - Population-weighted accessibility statistics
   - Facility density metrics (per 10,000 inhabitants)

### B: Utrecht, The Netherlands
- Same workflow as Graz (B.1, B.2, B.3)
- Different CRS (EPSG:28992 - Dutch national grid)

### C: Visualization & Comparison
1. **C.1 Graz Visualizations**
   - Family-complete access maps
   - Threshold-based accessibility (categorical)
   - Continuous walking time maps
   - Walking time difference maps (school vs childcare)
   - Population-weighted accessibility
   - Accessibility vs population density scatterplots

2. **C.2 Utrecht Visualizations**
   - Same visualization suite as Graz

3. **C.3 Cross-City Comparison**
   - Side-by-side accessibility maps
   - Summary statistics table
   - Walking time distribution histograms
   - Accessibility inequality analysis (coefficient of variation)

## Key Outputs

### Maps
- **Binary accessibility maps**: Areas with/without 15-min access
- **Categorical maps**: Walking time thresholds (≤5, 5-10, 10-15, >15 min)
- **Continuous maps**: Actual walking times (capped at 30 min)
- **Difference maps**: School vs childcare accessibility
- **Interactive Kepler maps**: Exploratory visualization

### Statistics
- Population-weighted access rates (%)
- Facility density (per 10,000 inhabitants)
- Hexagon coverage statistics
- Correlation analysis (population density ↔ accessibility)
- Coefficient of variation (inequality metric)


## Reproducibility Notes

### Guaranteed Reproducibility
✅ **No manual downloads required** - All OSM data fetched automatically  
✅ **Population data pre-aggregated** - `.parquet` files included in repository  
✅ **DEM files included** - Pre-processed for both cities  
✅ **No path modifications needed** - Relative paths work from repository root  
✅ **Automated environment setup** - Dependencies installed in first cell  

### Expected Variations
- **Minor OSM data differences**: OpenStreetMap is continuously updated, so POI counts may vary slightly
- **Processing time**: 3-15 minutes depending on system (depending on network and system performance)
- **Kepler map rendering**: Interactive maps may require adjustments 

### File Structure
```
GIS_FinalProject_Group2/
├── Data/
│   ├── Processed/
│   │   ├── ghsl_pop_graz.parquet          # Pre-aggregated population
│   │   ├── ghsl_pop_utrecht.parquet
│   │   ├── dem_graz_4326.tif              # DEM files
│   │   └── dem_utrecht_4326.tif
│   └── Raw/                               # (Empty - in .gitignore)
├── GIS Analysis techniques 2: Final Project.ipynb
└── README.md
```

## Limitations

1. **OSM Data Completeness**: Accessibility results depend on OSM data quality, which varies by region
2. **Walking Network**: Custom filter includes footways/bridges but may miss informal paths
3. **Population Data**: GHSL data is modeled and may not reflect exact 2025 population distribution
4. **Slope Modeling**: Tobler's function is a generalization; individual walking speeds vary significantly
5. **H3 Resolution**: Resolution 8 (~460m edge length) may aggregate heterogeneous neighborhoods
6. **15-Minute Threshold**: Fixed threshold doesn't account for individual mobility constraints
7. **Static Analysis**: Does not consider temporal variations (opening hours, seasonal accessibility)

## License

This project is licensed under the MIT License.

## Contributors

**Group 2:**
- Conni S.
- Paul B.
- Elias P.
- Miriam E.

**Course:** GIS Analysis Techniques 2  
**Date:** January 2026

---

## Quick Start
```bash
# 1. Clone
git clone https://github.com/miriam-ee/GIS_FinalProject_Group2.git

# 2. Open notebook
jupyter notebook "GIS Analysis techniques 2: Final Project.ipynb"

# 3. Run all cells (Cell → Run All)
```

**That's it!** All data downloads and processing happen automatically.



# GIS_FinalProject_Group 2 

#### Walking Accessibility to Childcare and Schools: ###
#### A Spatial Comparison of Family-Friendliness in Graz and Utrecht ###

## Table of content for the README.md
- Description of the Project
- Dependencies
- Installing
- Executing program 
- Research Question
- Study Area
- Data Sources
- Workflow Overview/ Overview of the .ipynb
- Workflow Description
- Reproducibility
- Limitations
- License
- Contribution 

# Description of the Project

# Dependencies
- Developed in Python 3.11
- installations are at the top of the .ipynb file 

# Installing
- pull from https://github.com/miriam-ee/GIS_FinalProject_Group2.git
- for a different study area, adapt data integration section

# Executing program
- if needed, adapt data integration; otherwise no user input needed
- run cell by cell or whole notebook for all results 

# Research Question
- How accessible are childcare facilities and schools within a 10–15 minute walking threshold across Graz and Utrecht?
- How does Graz’s family-friendly accessibility compare to Utrecht’s, and which spatial patterns indicate potential areas for improvement?

# Study Area
- Graz, Austria
- Utrecht, The Netherlands

# Data Sources

# Workflow Overview/ Python Script Overview
- Settings
- A: Graz, Austria
  - A.1: Data Preparation
  - A.2: Accessibility Calculation
  - A.3: Accessibility Indicator - Family Friendliness
- B: Utrecht, The Netherlands
  - B.1: Data Preparation
  - B.2: Accessibility Calculatio
  - B.3:Accessibility Indicator - Family Friendliness
- C: Statistics, Visualizations & Mapping
  - C.1: Graz
  - C.2: Utrecht
  - C.3: Comparison Maps 

# Workflow Description
- A: Graz, Austria
  - A.1: Data Preparation
    - Define Study Area
    - Import OSM Data for childcare und education facilities
    - Get the walking network
    - Create the h3 hexagon grid
    - Population Data 
      - import the data from the Global Human Settlement Layer
      - The Challenge here was that the .tif has a large size and cannot be included in the GitHUb Repository. If saved and imported locally, then other users could not run the entire script top-to-bottom without downloading the data and adopting the paths. Therefore the aim was to include the data so that anyone can run the script without any adaptations. The workflow for this was the following:
      - If a processed GeoParquet can be found in the Repository, then this will be used. This will be the case for anyone running the script since the Processed GeoParquet already exists in the GitHub Repository. But of course, this processed GeoParquet needed to be computed at the beginning. So if the processed file could nor be found in the Data/Processed/ Order, then it would use the TIFF from Data/Raw/. Since this was only necessary for the first time, the Data/Raw/ was then included in the .gitignore file. If the GeoParquet is not in the Repository yet, then the TIFF needs to be saved locally in Data/Raw/ in order compute the GeoParquet. Anyhow, since I ran the script in the beginning, the GeoParquet will be saved in the Respository anyhow.   

# Reproducibility


# Limitations


# License
- MIT License 
  
# Contribution 

