# Urban Water Quality Prediction

Predicting a Water Quality Index (WQI) for Indian surface water stations using historical pollution and temperature data, and training a Random Forest model to estimate WQI from raw water quality measurements.

## Dataset

Source: [Indian Water Quality Data](https://www.kaggle.com/datasets/anbarivan/indian-water-quality-data) (Kaggle, by anbarivan) — historical pollution and temperature readings from water monitoring stations across India.

Raw features include:
- Temperature
- Dissolved Oxygen (D.O.)
- pH
- Biochemical Oxygen Demand (B.O.D.)
- Conductivity
- Nitrate + Nitrite
- Total Coliform

## Methodology

1. **Data cleaning** — converted mixed-type columns to numeric with `pd.to_numeric(errors='coerce')`, then filled missing values with each column's mean.
2. **Water Quality Index (WQI) engineering** — each raw parameter is converted into a 0–100 sub-index score based on standard water quality guidelines (higher is better/safer), then combined into a single weighted WQI score:

   | Parameter | Weight |
   |---|---|
   | pH | 0.165 |
   | Dissolved Oxygen | 0.281 |
   | BOD | 0.234 |
   | Conductivity | 0.009 |
   | Nitrate | 0.028 |
   | Total Coliform | 0.281 |

3. **Modeling** — features scaled with `StandardScaler`, split 80/20 train/test, and a `RandomForestRegressor` trained to predict WQI directly from the raw measurements.

## Results

| Metric | Score |
|---|---|
| Testing R² | 0.9699 |
| MAE | 0.9362 |
| MSE | 5.5308 |
| RMSE | 2.3518 |

## Tech Stack

Python, pandas, NumPy, scikit-learn, seaborn, matplotlib

## How to Run

1. Clone this repo and install dependencies: `pip install pandas numpy scikit-learn seaborn matplotlib jupyter`
2. Download the dataset from the [Kaggle link above](https://www.kaggle.com/datasets/anbarivan/indian-water-quality-data) and place the CSV in the project folder.
3. Update the `file_path` variable in the first data-loading cell to point to your CSV.
4. Run the notebook top to bottom (Kernel → Restart & Run All).

## Conclusion

The Random Forest model explains about **97% of the variance** in Water Quality Index scores on unseen test data (R² = 0.97), with an average prediction error of under 1 point (MAE = 0.94) on a 0–100 WQI scale. This indicates that the six input measurements — pH, dissolved oxygen, BOD, conductivity, nitrate, and total coliform — carry strong predictive signal for overall water quality, and that a Random Forest is well suited to capturing the non-linear thresholds used in the WQI sub-index calculations.
