# 🛡️ Web Intrusion Detection System (FastAPI + ML Pipelines)

A **production-grade Web Intrusion Detection System** built using **FastAPI** and **Scikit-Learn Pipelines**, designed to detect malicious network traffic with consistent **training–inference pipelines** and industry-standard ML engineering practices.

This project avoids common ML deployment issues such as **feature mismatch, preprocessing drift, and data leakage** by strictly following pipeline-based training and inference.

---

## 🚀 Key Highlights

- ✅ End-to-end **Sklearn Pipelines**
- ✅ **FastAPI backend** with Swagger UI
- ✅ Multiple ML models (DT, RF, XGBoost)
- ✅ Custom preprocessing transformer
- ✅ Model selection via Swagger dropdown
- ✅ No feature mismatch (37 vs 40 issue resolved correctly)
- ✅ Production-ready architecture

---

## 🧠 Problem Statement

Detect whether a given network connection is **normal or an intrusion** based on network traffic features such as protocol type, service, byte transfer behavior, error rates, and host-level statistics.

---

## 🏗️ System Architecture

Client / Swagger UI
↓
FastAPI (Raw JSON Input)
↓
Sklearn Pipeline
├─ DropAndClip (Custom Transformer)
├─ ColumnTransformer (Encoding + Scaling)
└─ ML Model (DT / RF / XGB)
↓
Prediction + Confidence Score

---

## 📂 Project Structure
ml_models/
│
├── main.py # FastAPI application
├── preprocess.py # Custom DropAndClip transformer
│
├── models/
│ ├── dt_pipeline.joblib
│ ├── rf_pipeline.joblib
│ └── xgb_pipeline.joblib
│
├── training/
│ └── train_models.ipynb # Model training notebooks
│
├── requirements.txt
└── README.md

---


---

## 🧪 Feature Engineering Strategy

### 🔹 Input Contract
- API accepts **raw network features only**
- No manual preprocessing at API level

### 🔹 Custom Transformer: `DropAndClip`
Handles:
- Dropping **leakage columns**
- Clipping extreme values using quantiles
- Log-transforming skewed numerical features

```python
leakage_cols = ['num_compromised', 'num_root', 'num_shells', 'num_access_files']
clip_cols = ['src_bytes', 'dst_bytes', 'duration']
```

### 🔹 Encoding & Scaling

- Categorical → OneHotEncoder

- Numerical → StandardScaler

- Entire preprocessing handled inside pipeline

### 🧠 Machine Learning Models

- LR (logistic regression) 
- SVM
- NB( naives Bayes) 
- KNN
- Decision Tree Classifier
- Random Forest Classifier
- XGBoost Classifier

All models are trained using the same preprocessing pipeline to ensure feature consistency.


``` python
Pipeline([
    ("dropclip", DropAndClip(...)),
    ("preprocessor", ColumnTransformer(...)),
    ("model", RandomForestClassifier())
])
```

## 🌐 FastAPI Endpoints
### 🔹 Health Check
- GET /

### 🔹 Predict Intrusion
- POST /predict?model=rf

## Supported models:

- lr -> Linear Regression

- dt → Decision Tree

- rf → Random Forest 

- xgb → XGBoost (default)

Swagger UI provides a dropdown for model selection.

## Sample Request 

``` json
{
  "duration": 120,
  "protocol_type": 0,
  "service": 34,
  "flag": 1,
  "src_bytes": 181,
  "dst_bytes": 5450,
  "land": 0,
  "logged_in": 1,
  "root_shell": 0,
  "is_guest_login": 0,
  "count": 5,
  "srv_count": 3,
  "serror_rate": 0.0,
  "srv_serror_rate": 0.0,
  "rerror_rate": 0.0,
  "srv_rerror_rate": 0.0,
  "same_srv_rate": 0.8,
  "diff_srv_rate": 0.2,
  "srv_diff_host_rate": 0.1,
  "dst_host_count": 50,
  "dst_host_srv_count": 25,
  "dst_host_same_srv_rate": 0.6,
  "dst_host_diff_srv_rate": 0.4,
  "dst_host_same_src_port_rate": 0.3,
  "dst_host_srv_diff_host_rate": 0.2,
  "dst_host_serror_rate": 0.0,
  "dst_host_srv_serror_rate": 0.0,
  "dst_host_rerror_rate": 0.0,
  "dst_host_srv_rerror_rate": 0.0
}
```

## 📤 Sample Response
``` json
{
  "model_used": "xgb",
  "prediction": 1,
  "confidence": 0.9824
}
```


`
