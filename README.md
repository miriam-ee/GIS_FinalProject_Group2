# GIS_FinalProject_Group 2 

#### Walking Accessibility to Childcare and Schools: ###
#### A Spatial Comparison of Family-Friendliness in Graz and Uppsala ###

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
- How accessible are childcare facilities and schools within a 10–15 minute walking threshold across Graz and Uppsala?
- How does Graz’s family-friendly accessibility compare to Uppsala’s, and which spatial patterns indicate potential areas for improvement?

# Study Area
- Graz, Austria
- Uppsala, Sweden

# Data Sources

# Workflow Overview/ Python Script Overview
- Settings
- A: Graz, Austria
  - A.1: Data Preparation
  - A.2: Accessibility Calculation
  - A.3: Accessibility Indicator - Family Friendliness
- B: Uppsala, Sweden
  - B.1: Data Preparation
  - B.2: Accessibility Calculatio
  - B.3:Accessibility Indicator - Family Friendliness
- C: Statistics, Visualizations & Mapping
  - C.1: Graz
  - C.2: Uppsala
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

