# Landsat–ERA5-Land Surface Heat Modelling Around Zayed International Airport

Code and data for the study **"Integrating Landsat and ERA5-Land for Machine Learning-Based Surface Heat Modelling Around Zayed International Airport, Abu Dhabi"** (Femina Anzil and Zainab Rasheed, Emirates Aviation University).

The project builds a spatially aggregated model of hot-season (June–September) land surface temperature (LST) around Zayed International Airport (ICAO: OMAA) for 2021–2025. Landsat 8–9 surface temperature and spectral indices are averaged within 0.1° ERA5-Land grid cells and matched with ERA5-Land meteorology. Linear regression, Random Forest, Gradient Boosting and XGBoost are then compared under three validation schemes.

## Key results

| Evaluation | Result (Gradient Boosting) |
| --- | --- |
| Date-grouped cross-validation | mean R² = 0.600 (linear regression 0.504) |
| 2025 temporal hold-out | R² = 0.694, RMSE = 2.27 °C |
| Spatial-group cross-validation | mean R² = 0.263, weakest fold R² = −1.479 |
| Hottest 10% of 2025 observations | mean underprediction 2.85 °C |
| Most influential predictor | NDBI |

The model is intended for retrospective cell-mean LST estimation within the sampled domain. It is not validated for pixel-level prediction, new locations or operational extreme-heat forecasting.

## Repository contents

```
landsat-era5-airport-lst/
├── notebooks/
│   └── landsat_era5_lst_model.ipynb   # full workflow: scene selection → integration → modelling → validation
├── data/
│   ├── integrated_dataset.csv          # 560 cell-date observations (40 dates × 14 cells)
│   └── landsat_scenes.csv              # 40 Landsat scene IDs and acquisition dates
├── figures/                            # figures as they appear in the paper and supplement
├── requirements.txt
├── LICENSE
└── README.md
```

### `data/integrated_dataset.csv`

One row per 0.1° grid cell per acquisition date.

| Column | Description | Unit |
| --- | --- | --- |
| `date` | Landsat acquisition date | YYYY-MM-DD |
| `cell_lat`, `cell_lon` | ERA5-Land grid-cell centre | ° |
| `LST` | Mean Landsat land surface temperature (response) | °C |
| `NDVI`, `NDBI`, `BSI` | Mean spectral indices | – |
| `air_temp` | 2-m air temperature at overpass | °C |
| `rel_humidity` | Relative humidity (Magnus approximation) | % |
| `wind_speed` | 10-m wind speed | m s⁻¹ |
| `solar_rad` | Daily surface solar radiation downwards | MJ m⁻² day⁻¹ |
| `precip` | Total precipitation | mm |
| `LST_sd`, `n_pixels` | Within-cell LST standard deviation and pixel count | °C, – |

> Update the column names above to match your CSV exactly.

## Data sources

- **Landsat 8–9 OLI/TIRS Collection 2 Level-2** (path 160, row 043): U.S. Geological Survey, https://doi.org/10.5066/P9OGBGM6
- **ERA5-Land hourly reanalysis**: Copernicus Climate Change Service Climate Data Store, https://doi.org/10.24381/cds.e2161bac

Raw imagery and reanalysis files are not redistributed here. Download them from the sources above using the scene list in `data/landsat_scenes.csv`.

## How to run

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/landsat-era5-airport-lst.git
   cd landsat-era5-airport-lst
   ```
2. Install the environment (Python 3.13):
   ```bash
   pip install -r requirements.txt
   ```
3. Open and run `notebooks/landsat_era5_lst_model.ipynb` from top to bottom. To reproduce only the modelling and validation, start from the section that loads `data/integrated_dataset.csv`.

All stochastic models use `random_state = 42`. GroupKFold assigns groups to folds deterministically, so fold composition is reproducible.

## Software versions

| Package | Version |
| --- | --- |
| Python | 3.13 |
| scikit-learn | 1.6.1 |
| XGBoost | 3.4.1 |
| SHAP | 0.52.0 |
| pandas | 2.2.3 |
| NumPy | 2.1.3 |

## Citation

If you use this code or data, please cite:

> Anzil, F., & Rasheed, Z. (2026). Integrating Landsat and ERA5-Land for machine learning-based surface heat modelling around Zayed International Airport, Abu Dhabi. *[Journal name, volume, pages — to be updated on publication]*.

Archived release: Zenodo DOI to be added.

## Licence

Released under the MIT Licence. See `LICENSE`.

## Contact

Femina Anzil (corresponding author): feminaanzil@gmail.com
