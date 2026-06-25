# ML Pipeline - Spotify vs YouTube EDA

## Overview
This ML pipeline predicts Spotify stream counts based on audio features and YouTube engagement metrics.

## Directory Structure
```
ML/
├── 01_Preprocessing.ipynb    # Data preprocessing and feature engineering
├── 02_Training.ipynb         # Model training and evaluation
├── 03_Prediction.ipynb       # Predictions and analysis
├── data/                     # Processed datasets
│   ├── X_train.csv
│   ├── X_test.csv
│   ├── y_train.csv
│   ├── y_test.csv
│   ├── feature_cols.csv
│   └── scaler.pkl
├── models/                   # Trained models
│   ├── best_model.pkl
│   └── best_model_name.txt
├── results/                  # Prediction results and visualizations
│   ├── model_comparison.png
│   ├── feature_importance.png
│   ├── predictions_analysis.png
│   ├── residuals_analysis.png
│   ├── predictions_detailed.csv
│   └── prediction_summary.csv
└── README.md                # This file
```

## Quick Start

### Step 1: Preprocessing
```python
# Run 01_Preprocessing.ipynb
# This will:
# - Load raw data
# - Handle missing values
# - Create feature engineering
# - Scale features
# - Split data (80/20 train/test)
# - Save processed data
```

### Step 2: Training
```python
# Run 02_Training.ipynb
# This will:
# - Load preprocessed data
# - Train 5 different models
# - Evaluate performance
# - Identify best model
# - Generate feature importance
# - Save best model
```

### Step 3: Prediction
```python
# Run 03_Prediction.ipynb
# This will:
# - Load trained model
# - Make predictions on test set
# - Analyze prediction accuracy
# - Create visualizations
# - Export results
```

## Features Used (17 total)

### Audio Features (9)
- **Danceability**: How suitable a track is for dancing (0-100)
- **Energy**: Intensity and activity (0-100)
- **Key**: Pitch class (0-11)
- **Loudness**: Overall loudness (dB)
- **Speechiness**: Presence of spoken words (0-1)
- **Acousticness**: Acoustic sound (0-1)
- **Instrumentalness**: Lack of vocals (0-1)
- **Liveness**: Audience presence (0-1)
- **Valence**: Musical positiveness (0-1)

### Technical Features (2)
- **Tempo**: Beats per minute
- **Duration_ms**: Track length (normalized)

### YouTube Engagement (3)
- **Views**: YouTube video views (millions)
- **Likes**: YouTube likes (millions)
- **Comments**: YouTube comments (thousands)

### Derived Metrics (3)
- **Engagement_Rate**: Likes per view
- **Discussion_Rate**: Comments per view
- **Like_Comment_Ratio**: Likes per comment

## Models Trained

1. **Linear Regression** - Baseline model
2. **Ridge Regression** - L2 regularization
3. **Lasso Regression** - L1 regularization
4. **Random Forest** - 100 trees, ensemble method
5. **Gradient Boosting** - Boosted decision trees

## Target Variable
- **Stream**: Spotify stream count (in millions)
- Type: Regression
- Train/Test Split: 80/20

## Key Metrics

- **R² Score**: Coefficient of determination
- **RMSE**: Root Mean Squared Error
- **MAE**: Mean Absolute Error
- **MAPE**: Mean Absolute Percentage Error
- **Cross-validation**: 5-fold CV

## Data Processing Pipeline

```
Raw Data (CSV)
    ↓
Load & Validate
    ↓
Handle Missing Values
    ├─ Audio features: Drop rows
    ├─ Numeric: Median imputation
    └─ Categorical: Mode imputation
    ↓
Remove Duplicates
    ↓
Type Conversion
    ↓
Feature Engineering
    ├─ Engagement Rate = Likes / Views
    ├─ Discussion Rate = Comments / Views
    └─ Like-Comment Ratio = Likes / Comments
    ↓
Normalize Values
    ├─ Danceability & Energy: ×100
    ├─ Views & Streams: ÷1M
    ├─ Likes: ÷1M
    └─ Comments: ÷1K
    ↓
Remove Outliers (IQR method)
    ↓
StandardScaler (Fit on train, transform test)
    ↓
Train/Test Split (80/20)
    ↓
Ready for ML Models
```

## Feature Importance
Based on the best performing model, features are ranked by their contribution to predictions.

## Performance Analysis

### Model Comparison
- Compare R², RMSE, and MAE across all models
- Best model selected based on R² score
- Cross-validation prevents overfitting

### Prediction Analysis
- Actual vs Predicted scatter plots
- Error distribution analysis
- Residual plots for model diagnostics
- Confidence intervals (95%)

## How to Use Results

### View Predictions
```python
# Read detailed predictions
results = pd.read_csv('results/predictions_detailed.csv')
print(results.head())
```

### Load Scaler for New Data
```python
import pickle
with open('data/scaler.pkl', 'rb') as f:
    scaler = pickle.load(f)
```

### Load Best Model
```python
with open('models/best_model.pkl', 'rb') as f:
    model = pickle.load(f)
```

## Making Predictions on New Data

```python
# Create feature dataframe with required columns
new_data = pd.DataFrame({
    'Danceability': [75],
    'Energy': [80],
    # ... all 17 features
})

# Scale features
new_data_scaled = scaler.transform(new_data)

# Make prediction
prediction = model.predict(new_data_scaled)
print(f"Predicted Streams: {prediction[0]:.2f}M")
```

## Troubleshooting

### Missing Data Directory
```python
import os
os.makedirs('data', exist_ok=True)
os.makedirs('models', exist_ok=True)
os.makedirs('results', exist_ok=True)
```

### Common Issues
- **FileNotFoundError**: Ensure you're in the ML directory
- **ScalerNotFitted**: Run preprocessing first
- **ModelNotFound**: Run training first

## Next Steps

1. ✅ Preprocessing: Data cleaning and feature engineering
2. ✅ Training: Model selection and evaluation
3. ✅ Prediction: Generate predictions and analyze results
4. 📊 Hyperparameter Tuning: Grid/Random search
5. 📈 Ensemble Methods: Combine multiple models
6. 🔍 SHAP Analysis: Feature importance interpretation
7. 📦 Model Deployment: REST API / Production

## References

- [Spotify Web API](https://developer.spotify.com/documentation/web-api)
- [YouTube API](https://developers.google.com/youtube/v3)
- [Scikit-learn Documentation](https://scikit-learn.org)

## Author
Data Science Project - Spotify vs YouTube Analysis

## Last Updated
2026-06-18
