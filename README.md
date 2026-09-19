# Kainji Lake National Park Wildfire Susceptibility

A reproducible geospatial and machine-learning workflow for analysing wildfire occurrence and susceptibility in **Kainji Lake National Park (KLNP), Nigeria** using Google Earth Engine and Google Colab.

The repository supports the study **“Savannah Wildfire Risk and Induced Degradation of Kainji Lake National Park in Nigeria: Insight for Response Prioritization and Sustainable Management.”** It contains the Google Earth Engine scripts used for data preparation and extraction, the resulting CSV datasets, the study-area shapefiles, and the Google Colab notebook used for subsequent analysis and modelling.

---

## 1. Study Workflow

The workflow follows a sequential structure:

**Environmental and human drivers → Data preprocessing and spatial harmonization → Fire-occurrence extraction → Wildfire susceptibility modelling → Model interpretation and visualization**

The analysis uses a common **0.05° × 0.05° analytical grid** across KLNP. Environmental variables are prepared and harmonized to this spatial unit before being combined with monthly fire-occurrence observations.

The main predictor variables include:

* Land surface temperature (LST)
* Rainfall
* Vapour pressure deficit (VPD)
* Soil moisture
* Wind speed
* Enhanced Vegetation Index (EVI)
* Land cover
* Slope
* Global Human Modification (gHM)

Fire occurrence is represented as a binary response:

* `1` = fire detected
* `0` = no fire detected

The workflow covers the period **2002–2020**, with 2001 used as an antecedent year for the preparation of lagged predictor variables.

---

## 2. Repository Structure

```text
Kainji-Lake-NP-Wildfire-Susceptibility/
│
├── data/
│   ├── CSV/
│   │   └── Processed and extracted datasets
│   │
│   ├── GEE Script/
│   │   └── Google Earth Engine scripts for data preparation
│   │       and extraction
│   │
│   └── KainjiLake_Shapefiles/
│       └── Study-area boundary and supporting shapefile data
│
├── KLNP_Wildfire_Susceptibility_Analysis.ipynb
│
├── README.md
├── LICENSE
└── .gitignore
```

### Folder descriptions

**`data/CSV/`**
Contains the tabular datasets generated from the geospatial extraction workflow. These files provide the inputs used by the Colab notebook.

**`data/GEE Script/`**
Contains the Google Earth Engine scripts used to prepare, harmonize and extract the environmental, human-modification and fire-related variables.

**`data/KainjiLake_Shapefiles/`**
Contains the spatial boundary and other shapefile components associated with the KLNP study area.

**`KLNP_Wildfire_Susceptibility_Analysis.ipynb`**
The main Google Colab notebook. It loads the processed datasets, performs analysis and modelling, and generates the required outputs.

**`LICENSE`**
Specifies the terms under which the repository contents may be used.

**`.gitignore`**
Lists files and temporary outputs that should not be committed to the repository.

---

## 3. Data and Remote-Sensing Sources

The workflow integrates multiple satellite and gridded environmental datasets available through Google Earth Engine.

| Variable           | Dataset                         | Native Resolution | Role                       |
| ------------------ | ------------------------------- | ----------------: | -------------------------- |
| LST                | MODIS MOD11A2                   |              1 km | Thermal condition          |
| EVI                | MODIS MOD13Q1                   |             250 m | Vegetation condition       |
| Land cover         | MODIS MCD12C1                   |            ~0.05° | Land-cover characteristics |
| Rainfall           | CHIRPS Daily                    |            ~0.05° | Precipitation              |
| VPD                | TerraClimate                    |           ~4.6 km | Atmospheric dryness        |
| Soil moisture      | TerraClimate                    |           ~4.6 km | Moisture availability      |
| Wind speed         | TerraClimate                    |           ~4.6 km | Wind condition             |
| Slope              | SRTM-derived                    |              30 m | Terrain condition          |
| Human modification | Global Human Modification (gHM) |             300 m | Human influence            |
| Burned area        | MODIS MCD64A1                   |             500 m | Fire occurrence            |

The datasets are harmonized to the common **0.05° × 0.05° grid** used as the main analytical spatial unit.

Further information on the datasets can be obtained from the Google Earth Engine Data Catalog:

* [MODIS MOD11A2 – Land Surface Temperature](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD11A2)
* [MODIS MOD13Q1 – Vegetation Indices](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD13Q1)
* [MODIS MCD12C1 – Land Cover](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD12C1)
* [CHIRPS Daily – Precipitation](https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY)
* [TerraClimate](https://developers.google.com/earth-engine/datasets/catalog/IDAHO_EPSCOR_TERRACLIMATE)
* [SRTM](https://developers.google.com/earth-engine/datasets/catalog/USGS_SRTMGL1_003)
* [Global Human Modification](https://developers.google.com/earth-engine/datasets/catalog/TNC_HM_v3_300m_c)
* [MODIS MCD64A1 – Burned Area](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD64A1)

---

## 4. Spatial and Temporal Design

### Spatial framework

All datasets are harmonized to a common:

```text
0.05° × 0.05° analytical grid
```

This provides a consistent spatial framework for combining datasets with different native spatial resolutions.

Grid cells are retained when at least **50% of the cell area overlaps the Kainji Lake National Park boundary**.

### Temporal framework

The modelling period is:

```text
2002–2020
```

The year **2001** is used as an antecedent year to provide the previous-month environmental conditions needed for the first modelling year.

Environmental and vegetation predictors are temporally aligned with the fire observations so that conditions preceding a fire event are used to model fire occurrence.

---

## 5. Google Earth Engine Workflow

The scripts in:

```text
data/GEE Script/
```

are responsible for the geospatial data-processing stage.

The GEE workflow generally involves:

1. Loading the KLNP boundary.
2. Creating the common 0.05° analytical grid.
3. Selecting grid cells with sufficient overlap with the park.
4. Preparing the environmental and human-modification datasets.
5. Applying appropriate quality control and scale-factor conversions.
6. Harmonizing datasets with different native spatial resolutions.
7. Extracting monthly environmental variables.
8. Extracting monthly burned-area information from MCD64A1.
9. Creating the binary fire-occurrence variable.
10. Exporting the resulting data as CSV files.

The generated CSV files are stored in:

```text
data/CSV/
```

### Important note for Google Earth Engine users

Some GEE scripts may require an authenticated Google Earth Engine account and access to the relevant Earth Engine project/assets.

Before running the scripts, check:

* Earth Engine authentication
* Earth Engine project configuration
* Study-area asset paths
* Export destinations
* Dataset availability

The shapefiles provided in the repository can also be used to independently inspect or reconstruct the study-area boundary where required.

---

## 6. Running the Google Colab Notebook

The main analysis notebook is:

```text
KLNP_Wildfire_Susceptibility_Analysis.ipynb
```

The notebook is located in the **root directory** of the repository.

### Option 1: Open the notebook directly in GitHub

Open the repository on GitHub and select:

```text
KLNP_Wildfire_Susceptibility_Analysis.ipynb
```

Then open the notebook in Google Colab using the **Open in Colab** option, where available.

### Option 2: Clone the repository into Google Colab

Open [Google Colab](https://colab.research.google.com/) and create a new notebook.

Run:

```python
!git clone https://github.com/ArrenOnom/Kainji-Lake-NP-Wildfire-Susceptibility.git
```

Then move into the repository:

```python
%cd /content/Kainji-Lake-NP-Wildfire-Susceptibility
```

Check the directory structure:

```python
!find . -maxdepth 3 -type f | sort
```

The notebook can then be opened or executed from its repository location.

You may also open the notebook programmatically in Colab using:

```python
from google.colab import drive
```

or simply navigate through the **Files** panel to:

```text
/content/Kainji-Lake-NP-Wildfire-Susceptibility/
```

and locate:

```text
KLNP_Wildfire_Susceptibility_Analysis.ipynb
```

---

## 7. Recommended Notebook Execution Order

For reproducibility, run the notebook from the first cell to the last cell without skipping preprocessing steps unless the required processed CSV files already exist.

The notebook is organized around the following analytical stages:

```text
1. Environment and package setup
        ↓
2. Repository and data-path configuration
        ↓
3. Load CSV datasets
        ↓
4. Data inspection and quality checks
        ↓
5. Data preparation
        ↓
6. Exploratory analysis
        ↓
7. Wildfire susceptibility modelling
        ↓
8. Model evaluation
        ↓
9. Variable interpretation
        ↓
10. Maps, figures and tables
```

Where the required CSV files are already available in `data/CSV/`, the Google Earth Engine extraction stage does not need to be repeated before running the notebook.

---

## 8. Expected Local Paths in Colab

After cloning the repository, the main paths used by the notebook should follow this structure:

```python
REPO_DIR = "/content/Kainji-Lake-NP-Wildfire-Susceptibility"

DATA_DIR = f"{REPO_DIR}/data"

CSV_DIR = f"{DATA_DIR}/CSV"

GEE_DIR = f"{DATA_DIR}/GEE Script"

SHAPEFILE_DIR = f"{DATA_DIR}/KainjiLake_Shapefiles"
```

This makes the notebook independent of a specific user's local computer structure and allows the repository to be reproduced directly in Colab.

---

## 9. Reproducibility

The repository is intended to support transparent and reproducible research.

To reproduce the analysis:

```text
1. Clone or open the repository.
2. Open KLNP_Wildfire_Susceptibility_Analysis.ipynb.
3. Confirm that the CSV files are available in data/CSV/.
4. Verify the expected file paths.
5. Install any notebook dependencies when prompted.
6. Run the notebook sequentially.
7. Review the generated tables, figures and model outputs.
```

The Google Earth Engine scripts provide the geospatial data-extraction stage, while the Colab notebook provides the statistical and machine-learning analysis stage.

---

## 10. Data Processing Principles

Several processing decisions are applied consistently throughout the workflow.

### Spatial harmonization

Datasets with different native resolutions are brought to the common 0.05° analytical grid rather than forcing all variables to a finer resolution. This reduces artificial spatial precision and provides a common unit for statistical modelling.

### Quality control

Satellite products are subjected to the quality-control information supplied with the respective datasets where applicable.

### Fire-occurrence definition

Fire occurrence is treated as a binary response:

```text
1 = burned
0 = valid observation without detected fire
```

Observations with insufficient valid fire-data coverage are excluded from the modelling dataset rather than being classified as non-fire observations.

### Temporal ordering

Predictor variables are temporally aligned so that environmental conditions preceding the fire observation are used as predictors. This helps reduce temporal leakage in the modelling framework.

---

## 11. Machine-Learning Analysis

The Colab notebook contains the machine-learning stage used to investigate relationships between wildfire occurrence and the selected environmental and human drivers.

The modelling framework uses **XGBoost binary classification** to estimate wildfire occurrence from the predictor variables.

Model interpretation can be performed using techniques such as:

* Feature importance
* SHAP analysis
* Partial dependence analysis
* Model performance metrics
* Spatial visualization of predicted susceptibility

The exact model settings and analysis parameters are documented in the notebook so that the computational workflow can be inspected and reproduced.

---

## 12. Repository Use for Reviewers

Reviewers and other researchers can inspect the workflow at three levels:

### Geospatial processing

```text
data/GEE Script/
```

Contains the Earth Engine scripts used to prepare and extract the spatial-temporal datasets.

### Input data

```text
data/CSV/
```

Contains the processed tabular datasets used for modelling.

### Statistical and machine-learning analysis

```text
KLNP_Wildfire_Susceptibility_Analysis.ipynb
```

Contains the main analytical workflow, including data preparation, modelling, evaluation and visualization.

This separation allows the data-extraction and modelling stages to be independently examined.

---

## 13. Citation

If you use this repository, workflow, or derived datasets in academic work, please cite the associated research article and acknowledge the original datasets used in the analysis.

Dataset-specific citations should follow the attribution requirements of the individual data providers.

---

## 14. Repository Information

**Repository:**
[Kainji-Lake-NP-Wildfire-Susceptibility](https://github.com/ArrenOnom/Kainji-Lake-NP-Wildfire-Susceptibility)

**Main notebook:**
`KLNP_Wildfire_Susceptibility_Analysis.ipynb`

**Study area:**
Kainji Lake National Park, Nigeria

**Analysis period:**
2002–2020

**Spatial analytical unit:**
0.05° × 0.05° grid

**Primary platforms:**
Google Earth Engine and Google Colab

---

## 15. License

This repository is distributed under the terms specified in the [`LICENSE`](./LICENSE) file.

Please also observe the licensing, citation and attribution requirements of the original satellite, climate, land-cover and human-modification datasets used in this workflow.

---

## 16. Acknowledgement

This workflow makes use of publicly available geospatial datasets provided through Google Earth Engine and their respective data producers. The original data providers should be appropriately acknowledged and cited when the datasets or derived products are reused.
