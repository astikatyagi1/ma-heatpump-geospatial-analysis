# Data Directory

## Structure

```
data/
├── raw/           # Original source files (not tracked — see .gitignore)
├── processed/     # Public-safe derived datasets
├── sample/        # Small example files for reproducing notebook workflows
└── README.md      # This file
```

## Data Sources & Access

### Public (included or downloadable)

| Dataset | Description | How to obtain |
|---|---|---|
| ACS Housing/Fuel | Occupied units, heating fuel type by municipality | [data.census.gov](https://data.census.gov/) — Tables B25040, B25001 |
| ERA5-Land | 2m temperature, hourly, for HDD/CDD calculation | [Copernicus CDS](https://cds.climate.copernicus.eu/) — free registration |
| TIGER/Line | Massachusetts county subdivision boundaries | [Census TIGER](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html) |
| MassCEC PTS | Solar PV installations by municipality | [MassCEC](https://www.masscec.com/production-tracking-system-field-definitions) |
| Mass Save/DNV | Municipal residential electricity (monthly, aggregated) | [Mass Save dashboard](https://www.masssave.com/) |

### Restricted (not included)

| Dataset | Description | Access |
|---|---|---|
| Mass Save heat pump installations | Town-level installation counts, rebate details | Contact research team — obtained through research partnership |
| Utility-provided consumption data | Non-public granular consumption records | Restricted by data agreement |

## Notes

- Raw data files are gitignored to avoid uploading restricted or large datasets
- The `sample/` directory contains small synthetic or publicly derived subsets for testing notebook workflows
- All notebooks can run with public data; restricted data steps are clearly marked with comments
