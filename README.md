# Massachusetts Heat Pump Adoption: Geospatial Analysis

A municipality-level geospatial analysis of residential heat pump adoption across all 351 Massachusetts municipalities, examining how heating fuel composition, electricity consumption patterns, climate variables, and solar PV deployment relate to heat pump adoption rates.

This research is part of the [Community-Engaged Action and Teaching through Heat (CATCH)](https://www.bu.edu/igs/) initiative at Boston University's Institute for Global Sustainability.

## Problem Statement

Massachusetts has set aggressive electrification targets, but heat pump adoption varies widely across municipalities. Understanding what drives adoption — and identifying communities that are overperforming or underperforming relative to expectations — can help target outreach, incentive programs, and grid planning more effectively.

## Research Questions

1. Which municipality-level factors (fuel mix, electricity intensity, climate, solar adoption) are most associated with heat pump adoption?
2. Can we build a predictive model to identify municipalities adopting heat pumps faster or slower than expected?
3. What are the grid impacts (peak demand, annual electricity, gas displacement) of different heat pump adoption scenarios?

## Data Sources

| Dataset | Source | Access |
|---|---|---|
| Heating fuel, housing, demographics | U.S. Census Bureau ACS (2013, 2023, 2024) | Public |
| Temperature, HDD, CDD | ERA5-Land via Copernicus Climate Data Store | Public |
| Municipality boundaries | Census TIGER/Line (county subdivisions) | Public |
| Solar PV installations | MassCEC Production Tracking System | Public |
| Residential electricity consumption | Mass Save / DNV Municipal Dashboard | Public-facing |
| Building energy simulations | NREL ResStock + EnergyPlus | Public framework |
| Heat pump installation data | Program administrator records | Restricted* |

\*Detailed heat pump installation/rebate data was obtained through the research partnership and is not redistributed in this repository. Aggregated municipality-level results are available upon request with PI approval.

## Technical Approach

### Municipal-Level Analysis
- Spatial joins of ACS, climate, electricity, and solar data to 351 Massachusetts municipality geometries using **GeoPandas** with CRS transformations (EPSG:4326 ↔ EPSG:26986)
- Calculation of adoption metrics: installs per 1,000 households, full-displacement rates
- HDD/CDD-normalized electricity intensity metrics per municipality
- Heating fuel composition shares (natural gas, delivered fuels, electricity)

### Statistical Modeling
- OLS regression modeling adoption as a function of fuel mix, electricity intensity, and climate
- Model progression: fuel-only (R² ≈ 0.40) → + heating electricity (R² ≈ 0.59) → + cooling electricity (R² ≈ 0.65)
- Residual analysis identifying overperforming and underperforming municipalities
- Correlation analysis finding strong associations (r ≈ 0.61, r ≈ 0.73) across 103 municipalities

### Building Energy Simulation
- 1,330 Suffolk County ResStock representative building models
- Baseline (up00) and heat pump upgrade (up04) EnergyPlus simulations
- Aggregation of hourly electricity, heating, cooling, gas, oil, and peak demand results
- Partial adoption scenarios (10%, 20%, 50%, 100%) modeling grid impacts:
  - **+18.4%** annual electricity increase
  - **−33.8%** natural gas reduction
  - **95.1%** rise in 15-minute peak demand

### NSI–ResStock Matching
- Probability-based matching of National Structure Inventory (NSI) building records to ResStock prototypes using an internal collaborator-provided matching workflow
- Application, validation, and town-level aggregation of probability-weighted building metrics

## Repository Structure

```
├── notebooks/
│   ├── 01_build_municipal_dataset.ipynb           # Data cleaning, merging, adoption rates
│   ├── 02_climate_and_electricity_features.ipynb   # ERA5-Land HDD/CDD, electricity metrics
│   ├── 03_municipal_eda.ipynb                      # Exploratory analysis, sensitivity checks
│   ├── 04_regression_and_residuals.ipynb           # OLS models, interactions, residuals
│   ├── 05_solar_pv_context.ipynb                   # Solar adoption and heat pump correlation
│   ├── 06_resstock_validation.ipynb                # Building simulation validation
│   └── 07_resstock_scenarios.ipynb                 # Grid impact scenario analysis
├── src/                                # Analysis utility scripts
├── figures/
│   ├── ma_heat_pump_adoption_map.png
│   └── ...
├── data/
│   ├── README.md                       # Data dictionary and access notes
│   ├── raw/                            # Not tracked (see .gitignore)
│   ├── processed/                      # Public-safe derived datasets only
│   └── sample/                         # Small example data for reproduction
├── requirements.txt
├── .gitignore
└── README.md
```

## Key Findings

- Heating fuel composition (delivered fuel share) and electricity consumption intensity are the strongest predictors of municipal heat pump adoption
- Municipalities with higher delivered fuel dependence (oil/propane) adopt heat pumps at significantly higher rates
- Full electrification of Suffolk County's residential heating would increase annual electricity demand by 18.4% while reducing gas consumption by 33.8%
- 15-minute peak electricity demand would rise by 95.1%, with critical implications for distribution grid planning

## Research Workflow

```
Municipality-Level Data Integration
        ↓
Statistical Modeling & Residual Analysis
        ↓
Town Interviews & Qualitative Validation
        ↓
Parcel-Level Housing Data (NSI)
        ↓
ResStock Building Energy Modeling
        ↓
Oil-to-Electric Transition Scenario Estimates
```

## Tools & Technologies

- **Geospatial:** GeoPandas, Shapely, PyProj, PyDeck
- **Data Processing:** Pandas, NumPy, SciPy
- **Visualization:** Matplotlib, Seaborn, Plotly
- **Statistical Modeling:** Statsmodels, Scikit-Learn
- **Climate Data:** ERA5-Land (NetCDF via CDS API)
- **Building Simulation:** NREL ResStock, EnergyPlus
- **Compute:** Boston University Shared Computing Cluster (HPC/SLURM)

## Reproduction Notes

Some data sources require registration or access agreements:
- **ERA5-Land:** Register at [Copernicus CDS](https://cds.climate.copernicus.eu/) (free)
- **ACS data:** Available via [Census Bureau](https://data.census.gov/) or `census` Python package
- **MassCEC PTS:** Available at [MassCEC](https://www.masscec.com/)
- **Heat pump installation data:** Contact the research team for access

## Citation

This work is part of the CATCH (Community-Engaged Action and Teaching through Heat) initiative at Boston University's Institute for Global Sustainability. If you use this code or methodology, please cite this repository.

## License

MIT License
