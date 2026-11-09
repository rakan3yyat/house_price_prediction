# House Price Prediction

**Type:** Regression

## Goal
Predict the **sale price of a house** (USD) from its size, location, quality and neighborhood.

**Target:** `sale_price` (USD)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/house_sales.csv` - about 150,000 rows (+0.5% duplicate rows) and 50 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `Property ID` | `property_id` | ID / personal detail -> drop |
| `Owner Name` | `owner_name` | ID / personal detail -> drop |
| `Agent Phone` | `agent_phone` | ID / personal detail -> drop |
| `Street Address` | `street_address` | ID / personal detail -> drop |
| `Zip Code` | `zip_code` | ID / personal detail -> drop |
| `Listing Date` | `listing_date` | date saved as text (`%m/%d/%Y`) |
| `City` | `city` | mixed upper / lower case, extra spaces |
| `Latitude` | `latitude` | - |
| `Longitude` | `longitude` | - |
| `Region Code` | `region_code` | category saved as a number |
| `Property Type` | `property_type` | short codes: `SF`, `TH`, `CO`, `MF` |
| `YearBuilt` | `year_built` | 1% missing; wrong values: `1,066`, `2,099` |
| `Living Area (sq ft)` | `living_area_sqft` | number saved as text (`12 sqft`); 1% missing; wrong values: `0`, `99,999` |
| `Lot Size (sqft)` | `lot_size_sqft` | number saved as text (`1,250.00`); 5% missing; extreme outliers |
| `Beds` | `bedrooms` | wrong values: `33`, `40` |
| `Baths` | `bathrooms` | 2% missing |
| `Floors` | `floors` | - |
| `Garage Spaces` | `garage_spaces` | 3% missing |
| `Has Basement` | `has_basement` | short codes: `Y`, `N` |
| `Basement Area Sqft` | `basement_area_sqft` | - |
| `Has Pool` | `has_pool` | short codes: `Y`, `N` |
| `Has Fireplace` | `has_fireplace` | short codes: `T`, `F` |
| `Has Central Air` | `has_central_air` | short codes: `Y`, `N` |
| `Heating Type` | `heating_type` | 2% missing; mixed upper / lower case, extra spaces |
| `Roof Type` | `roof_type` | - |
| `Exterior Material` | `exterior_material` | mixed upper / lower case, extra spaces |
| `Condition Rating` | `condition_rating` | 2% missing |
| `Kitchen Quality` | `kitchen_quality` | short codes: `PO`, `FA`, `GD`, `EX` |
| `Neighborhood Quality` | `neighborhood_quality` | - |
| `Distance To City Center Km` | `distance_to_city_center_km` | number saved as text (`12 km`) |
| `Distance To School Km` | `distance_to_school_km` | 2% missing |
| `Distance To Highway Km` | `distance_to_highway_km` | - |
| `School Rating` | `school_rating` | 4% missing; wrong values: `-1`, `99` |
| `Crime Rate / 1000` | `crime_rate_per_1000` | 3% missing; extreme outliers |
| `Median Neighborhood Income` | `median_neighborhood_income` | number saved as text (`$1,250.00`) |
| `View Quality` | `view_quality` | - |
| `Waterfront` | `waterfront` | short codes: `T`, `F` |
| `Solar Panels` | `solar_panels` | - |
| `Energy Efficiency Rating` | `energy_efficiency_rating` | - |
| `Days On Market` | `days_on_market` | - |
| `Num Photos` | `num_photos` | - |
| `Listing Agent Experience Years` | `listing_agent_experience_years` | - |
| `Hoa Fee Monthly` | `hoa_fee_monthly` | number saved as text (`$1,250.00`) |
| `Last Renovation Date` | `last_renovation_date` | date saved as text (`%Y-%m-%d`) |
| `Property Tax Annual` | `property_tax_annual` | number saved as text (`$1,250.00`) |
| `Country` | `country` | only one value |
| `Listing Status` | `listing_status` | only one value |
| `Is Foreclosure` | `is_foreclosure` | almost only one value |
| `Photo Count` | `photo_count` | exact copy of `num_photos` |
| `Sale Price ($)` | `sale_price` | number saved as text (`$1,250.00`); **TARGET** |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `house_price_prediction.ipynb` | The full project notebook |
| `data/house_sales.csv` | The dataset |
| `data/house_sales_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target histogram and box plot
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `house_price_prediction.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
