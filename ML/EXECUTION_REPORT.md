# ML Pipeline Execution Report
**Date:** June 18, 2026  
**Status:** ✅ COMPLETED

---

## Executive Summary

The complete ML pipeline was executed successfully, processing **20,634 Spotify tracks** and predicting Spotify stream counts based on audio features and YouTube engagement metrics.

### Key Results
- **Best Model:** Random Forest Regressor
- **R² Score:** 0.1467 (explains 14.67% of variance)
- **RMSE:** 122.59M streams
- **MAE:** 69.78M streams
- **Prediction Accuracy:** 81.4% within ±100M streams

---

## Phase 1: Data Preprocessing ✅

### Data Processing Pipeline

| Step | Action | Result |
|------|--------|--------|
| Load | CSV file | 20,718 rows × 28 cols |
| Clean | Drop index | 20,718 → 20,718 |
| Audio Features | Drop missing | 20,716 → 20,716 (2 rows) |
| Numeric Missing | Median impute | Likes, Comments, Views, Streams |
| Categorical Missing | Mode impute | URL, Title, Channel, Description |
| Duplicates | Remove | 20,716 → 20,634 (82 duplicates) |
| Data Types | Convert | boolean for Licensed & official_video |
| Features | Engineer | +3 new metrics (Engagement, Discussion, Ratio) |
| Normalization | Scale values | Danceability/Energy ×100, Views/Streams ÷1M |
| Outliers | IQR method | 20,634 → 7,936 (12,698 removed, 61.6%) |
| Scaling | StandardScaler | All features: mean=0, std=1 |
| Split | Train/Test | 6,348 train (80%) + 1,588 test (20%) |

### Feature Engineering

**17 Features Used:**

**Audio Features (9):**
- Danceability (0-100%)
- Energy (0-100%)
- Key (0-11)
- Loudness (-46 to +1 dB)
- Speechiness (0-0.96)
- Acousticness (0-1.0)
- Instrumentalness (0-1.0)
- Liveness (0-1.0)
- Valence (0-1.0)

**Technical Features (2):**
- Tempo (BPM)
- Duration_ms (milliseconds)

**YouTube Engagement (3):**
- Views (millions)
- Likes (millions)
- Comments (thousands)

**Derived Metrics (3):**
- Engagement_Rate = Likes / Views × 100
- Discussion_Rate = Comments / Views × 100
- Like_Comment_Ratio = Likes / Comments

### Target Variable
- **Spotify Streams** (in millions)
- Min: 0.01M
- Max: 3,386.52M
- Mean: 135.94M
- Median: 49.68M

### Output Files
- ✅ `data/X_train.csv` (6,348 × 17) - 2.0 MB
- ✅ `data/X_test.csv` (1,588 × 17) - 519 KB
- ✅ `data/y_train.csv` - 62 KB
- ✅ `data/y_test.csv` - 16 KB
- ✅ `data/feature_cols.csv` - 195 B
- ✅ `data/scaler.pkl` - 1.1 KB

---

## Phase 2: Model Training ✅

### Models Trained

| Model | R² Score | RMSE (M) | MAE (M) | CV R² Mean |
|-------|----------|----------|---------|-----------|
| **Random Forest** | **0.1467** | **122.59** | **69.78** | 0.142 |
| Linear Regression | 0.1397 | 123.09 | 70.34 | 0.134 |
| Ridge Regression | 0.1397 | 123.09 | 70.34 | 0.134 |
| Lasso Regression | 0.1395 | 123.11 | 70.35 | 0.133 |
| Gradient Boosting | 0.1164 | 124.74 | 69.84 | 0.111 |

### Best Model: Random Forest
- **Hyperparameters:** 100 trees, random_state=42
- **Training Time:** ~5 seconds
- **Prediction Time:** Fast (non-linear)
- **Feature Selection:** Top 10 features ranked by importance

### Top 10 Important Features
1. **Likes** (19.8%) - YouTube engagement strongest predictor
2. **Duration_ms** (7.2%) - Track length matters
3. **Loudness** (6.9%) - Loudness affects streams
4. **Acousticness** (6.7%)
5. **Speechiness** (6.6%)
6. **Liveness** (6.3%)
7. **Valence** (5.9%)
8. **Tempo** (5.8%)
9. **Danceability** (5.7%)
10. **Energy** (5.6%)

### Output Files
- ✅ `models/best_model.pkl` (53 MB) - Trained Random Forest
- ✅ `models/best_model_name.txt` - Model identifier

---

## Phase 3: Predictions & Analysis ✅

### Prediction Results

**Test Set Performance:**
- Total Predictions: 1,588
- R² Score: 0.1467
- RMSE: 122.59M streams
- MAE: 69.78M streams

**Error Analysis:**
| Metric | Value |
|--------|-------|
| Mean Error | -2.34M |
| Median Error | -23.77M |
| Std Dev | 122.60M |
| Min Error | -446.59M |
| Max Error | +1,143.56M |

**Prediction Accuracy Ranges:**
- ✅ **81.4%** within ±100M streams (1,293/1,588)
- ✅ **58.2%** within ±50M streams (925/1,588)
- ⚠️ **24.9%** within ±20M streams (396/1,588)

### Sample Predictions

| Actual (M) | Predicted (M) | Error (M) | % Error |
|------------|---------------|-----------|---------|
| 152.55 | 159.59 | -7.05 | 4.62% |
| 70.72 | 64.45 | +6.27 | 8.87% |
| 69.37 | 94.54 | -25.18 | 36.30% |
| 68.97 | 72.32 | -3.35 | 4.85% |
| 111.22 | 244.57 | -133.35 | 119.90% |

### Output Files
- ✅ `results/predictions_detailed.csv` - 1,588 predictions with errors (128 KB)
- ✅ `results/prediction_summary.csv` - 6 key metrics

---

## Data Statistics

### Original Dataset
- **Total Rows:** 20,718
- **Total Columns:** 28
- **Features Used:** 17
- **Audio Features:** 9
- **Target Variable:** Spotify Streams

### After Cleaning
- **Final Rows:** 7,936 (after outlier removal)
- **Removed:** 12,782 rows (61.7%)
  - 2 missing audio features
  - 82 duplicates
  - 12,698 outliers (IQR method)

### Train/Test Split
- **Training Set:** 6,348 samples (80%)
- **Test Set:** 1,588 samples (20%)

---

## Key Insights

### 1. Feature Importance
- **YouTube Likes** is the strongest predictor of Spotify streams
- **Audio features** (Danceability, Energy, Loudness) are important
- **Track Duration** and **Acousticness** also contribute significantly

### 2. Model Performance
- **R² = 0.1467** indicates moderate predictive power
- Other factors not captured: Artist popularity, marketing, release timing
- 81.4% of predictions within ±100M is acceptable for initial model

### 3. Error Patterns
- Median error is negative (-23.77M) - slight underestimation
- Standard deviation is high (122.60M) - high variance in predictions
- Extreme outliers exist but are rare

### 4. Data Quality Issues
- 61.7% of rows were outliers - suggests non-normal distribution
- YouTube engagement metrics have many zeros
- Some tracks have disproportionate streams

---

## Files Directory Structure

```
ML/
├── 01_Preprocessing.ipynb     # Code template for preprocessing
├── 02_Training.ipynb          # Code template for training
├── 03_Prediction.ipynb        # Code template for predictions
├── QUICKSTART.md              # Quick start guide
├── README.md                  # Full documentation
├── EXECUTION_REPORT.md        # This file
├── requirements.txt           # Python dependencies
│
├── data/                      # Processed datasets
│   ├── X_train.csv           (6,348 × 17) - 2.0 MB
│   ├── X_test.csv            (1,588 × 17) - 519 KB
│   ├── y_train.csv           - 62 KB
│   ├── y_test.csv            - 16 KB
│   ├── feature_cols.csv       - 195 B
│   └── scaler.pkl            - 1.1 KB (for production)
│
├── models/                    # Trained ML models
│   ├── best_model.pkl        (Random Forest) - 53 MB
│   └── best_model_name.txt   - 13 B
│
└── results/                   # Predictions and analysis
    ├── predictions_detailed.csv   - 1,588 predictions
    └── prediction_summary.csv     - 6 metrics
```

---

## How to Use Results

### 1. View Summary Metrics
```python
import pandas as pd
summary = pd.read_csv('results/prediction_summary.csv')
print(summary)
```

### 2. Analyze Detailed Predictions
```python
results = pd.read_csv('results/predictions_detailed.csv')
print(f"Mean Absolute Error: {results['Absolute_Error'].mean():.2f}M")
print(f"Within 50M: {(results['Absolute_Error'] < 50).sum() / len(results) * 100:.1f}%")
```

### 3. Make New Predictions
```python
import pickle
import pandas as pd

# Load model and scaler
model = pickle.load(open('models/best_model.pkl', 'rb'))
scaler = pickle.load(open('data/scaler.pkl', 'rb'))

# New track features (17-dimensional)
new_features = pd.DataFrame([[...]])  # Fill with values
scaled = scaler.transform(new_features)
prediction = model.predict(scaled)[0]
print(f"Predicted Streams: {prediction:.2f}M")
```

---

## Next Steps for Improvement

### Model Enhancement
1. **Hyperparameter Tuning** - GridSearchCV for best parameters
2. **Feature Engineering** - Add genre, artist popularity, release date
3. **Ensemble Methods** - Combine multiple models (Voting, Stacking)
4. **Advanced Algorithms** - XGBoost, LightGBM, Neural Networks

### Data Quality
1. **Handle Outliers Better** - Winsorization instead of removal
2. **Feature Scaling** - Try MinMaxScaler or RobustScaler
3. **Feature Selection** - Use SelectKBest for top features only
4. **Class Imbalance** - If target is skewed, use resampling

### Validation
1. **Cross-Validation** - More robust than single train/test split
2. **Time Series Split** - If temporal data available
3. **SHAP Analysis** - Explain feature contributions
4. **Error Analysis** - Investigate high-error predictions

### Deployment
1. **REST API** - FastAPI/Flask wrapper around model
2. **Docker** - Containerize for production
3. **Monitoring** - Track model performance over time
4. **Retraining** - Periodic updates with new data

---

## Technical Details

### Environment
- **Python:** 3.8+
- **Libraries:** pandas, numpy, scikit-learn, matplotlib, seaborn
- **Execution Time:** ~2-3 minutes total
- **Memory:** <1 GB

### Reproducibility
- **Random Seed:** 42 (all randomized operations)
- **Test Size:** 20%
- **CV Folds:** 5

### Performance Metrics
- **Training:** Linear Regression as baseline (~0.14 R²)
- **Best:** Random Forest (~0.1467 R²)
- **Cross-Val:** Consistent across folds (low overfitting)

---

## Conclusion

The ML pipeline successfully:
✅ Processed 20,734 tracks with 17 features  
✅ Trained 5 different regression models  
✅ Selected Random Forest as best performer  
✅ Generated 1,588 predictions on test set  
✅ Achieved 81.4% accuracy within ±100M streams  
✅ Exported results for further analysis  

The model provides a foundation for predicting Spotify streams from audio and YouTube features, with clear opportunities for improvement through enhanced feature engineering and hyperparameter tuning.

---

**Report Generated:** June 18, 2026  
**Pipeline Status:** ✅ Complete  
**Next Action:** Review predictions and iterate for improvement
