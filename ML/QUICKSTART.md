# ML Pipeline - Quick Start Guide

## 📋 Setup

### 1. Install Dependencies
```bash
cd ML/
pip install -r requirements.txt
```

### 2. Verify Directory Structure
```
ML/
├── 01_Preprocessing.ipynb  ← Start here
├── 02_Training.ipynb       ← Then here
├── 03_Prediction.ipynb     ← Finally here
├── data/                   (auto-created)
├── models/                 (auto-created)
├── results/                (auto-created)
├── README.md
└── requirements.txt
```

---

## 🚀 Execution Steps

### Phase 1: DATA PREPROCESSING (01_Preprocessing.ipynb)
**What it does:**
- ✅ Loads data from `../Data/Spotify_Youtube.csv`
- ✅ Handles 20,718 tracks × 27 features
- ✅ Removes 2 invalid rows, cleans 469 missing YouTube URLs
- ✅ Engineers 3 new engagement metrics
- ✅ Scales features using StandardScaler
- ✅ Splits into 80% train / 20% test

**Output files created:**
- `data/X_train.csv` - Training features (16,572 samples)
- `data/X_test.csv` - Test features (4,143 samples)
- `data/y_train.csv` - Training target (Streams)
- `data/y_test.csv` - Test target (Streams)
- `data/feature_cols.csv` - Feature names
- `data/scaler.pkl` - Fitted scaler for production

**Key Features (17 total):**
- Audio: Danceability, Energy, Key, Loudness, Speechiness, Acousticness, Instrumentalness, Liveness, Valence
- Technical: Tempo, Duration
- YouTube: Views, Likes, Comments
- Derived: Engagement_Rate, Discussion_Rate, Like_Comment_Ratio

---

### Phase 2: MODEL TRAINING (02_Training.ipynb)
**What it does:**
- ✅ Loads preprocessed data from `data/`
- ✅ Trains 5 different regression models:
  - Linear Regression (baseline)
  - Ridge Regression (L2 regularization)
  - Lasso Regression (L1 regularization)
  - Random Forest (100 trees)
  - Gradient Boosting (100 estimators)
- ✅ Evaluates each model with multiple metrics
- ✅ Performs 5-fold cross-validation
- ✅ Identifies best performing model
- ✅ Generates feature importance rankings

**Output files created:**
- `models/best_model.pkl` - Best trained model
- `models/best_model_name.txt` - Model name
- `results/model_comparison.png` - R², RMSE, MAE charts
- `results/feature_importance.png` - Top 15 features

**Expected Results:**
- R² Score: ~0.60-0.65 (captures 60-65% of variance)
- RMSE: ~200-250M streams
- Best model typically: Random Forest or Gradient Boosting

---

### Phase 3: PREDICTION (03_Prediction.ipynb)
**What it does:**
- ✅ Loads best trained model
- ✅ Makes predictions on test set
- ✅ Calculates performance metrics
- ✅ Creates diagnostic visualizations
- ✅ Analyzes residuals and errors
- ✅ Provides prediction templates

**Output files created:**
- `results/predictions_detailed.csv` - Full predictions + errors
- `results/prediction_summary.csv` - Summary metrics
- `results/predictions_analysis.png` - 4-panel analysis plot
- `results/residuals_analysis.png` - Residual diagnostics

**Metrics Included:**
- R² Score: Model fit quality
- RMSE: Root Mean Squared Error
- MAE: Mean Absolute Error
- MAPE: Mean Absolute Percentage Error
- Error distribution: Histogram + statistics

---

## 📊 Data Flow Diagram

```
Raw Data (CSV)
    ↓
01_Preprocessing
    ├─ Input: ../Data/Spotify_Youtube.csv
    ├─ Process: Clean, Engineer, Scale
    └─ Output: data/X_train, X_test, y_train, y_test
    ↓
02_Training
    ├─ Input: Preprocessed data + features
    ├─ Process: Train 5 models, Evaluate
    └─ Output: models/best_model.pkl + results/
    ↓
03_Prediction
    ├─ Input: Best model + test data
    ├─ Process: Predict, Analyze, Visualize
    └─ Output: results/predictions_detailed.csv + plots
```

---

## 💡 Tips & Tricks

### Running in Jupyter
```bash
jupyter notebook
# Open 01_Preprocessing.ipynb
# Run cells in order from top to bottom
# Cell outputs will appear below each cell
```

### Monitoring Progress
- Preprocessing: ~2-5 minutes (depends on RAM)
- Training: ~5-10 minutes (depends on CPU cores)
- Prediction: ~1-2 minutes (fast, one-pass)

### Making New Predictions
```python
# After running all 3 notebooks:
import pickle
import pandas as pd

model = pickle.load(open('models/best_model.pkl', 'rb'))
scaler = pickle.load(open('data/scaler.pkl', 'rb'))

# Your new track features (must be 17-dimensional)
new_features = pd.DataFrame([[...]])
scaled = scaler.transform(new_features)
prediction = model.predict(scaled)[0]
print(f"Predicted Streams: {prediction:.2f}M")
```

### Checking Results
```python
# View best model predictions
results = pd.read_csv('results/predictions_detailed.csv')
print(results.head())
print(f"Mean Error: {results['Error'].mean():.2f}M")
print(f"Accuracy (within 50M): {(results['Absolute_Error'] < 50).sum() / len(results) * 100:.1f}%")
```

---

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| `FileNotFoundError: Spotify_Youtube.csv` | Verify path: `../Data/Spotify_Youtube.csv` exists |
| `ModuleNotFoundError: sklearn` | Run: `pip install scikit-learn` |
| `MemoryError` | Close other apps, reduce batch size |
| `Missing data/ folder` | Preprocessing creates it automatically |
| `Models show bad R²` | Check for data leakage, verify scaling |

---

## 📈 Expected Performance

### Baseline (Linear Regression)
- R² ≈ 0.45-0.50
- RMSE ≈ 300-350M

### Tree-Based (Random Forest/Gradient Boosting)
- R² ≈ 0.60-0.65
- RMSE ≈ 200-250M

### Factors Affecting Performance
- ✅ Audio features strong predictors
- ✅ YouTube views highly correlated with Streams
- ⚠️ Missing values handled conservatively
- ⚠️ Some implicit factors not captured (artist popularity, release date, marketing)

---

## 📊 Viewing Results

### Model Comparison Chart
```bash
open results/model_comparison.png  # macOS
# or
display results/model_comparison.png  # Linux
```

### Detailed Predictions
```python
pd.read_csv('results/predictions_detailed.csv').to_excel('predictions.xlsx')
```

### Summary Stats
```python
summary = pd.read_csv('results/prediction_summary.csv')
print(summary)
```

---

## 🎯 Next Steps

After running the pipeline:

1. **Understand Results**
   - Review `prediction_summary.csv`
   - Examine feature importance
   - Check error distribution

2. **Improve Model**
   - Tune hyperparameters in 02_Training.ipynb
   - Add new features in 01_Preprocessing.ipynb
   - Try ensemble methods

3. **Production Deployment**
   - Export `best_model.pkl` + `scaler.pkl`
   - Create FastAPI/Flask wrapper
   - Deploy with Docker

4. **Further Analysis**
   - SHAP values for explainability
   - Partial dependence plots
   - Cross-dataset validation

---

## ❓ Questions?

Refer to:
- **README.md** - Full technical documentation
- **Notebook docstrings** - In-code explanations
- **Scikit-learn docs** - Model parameters

---

**Happy ML Engineering! 🚀**
