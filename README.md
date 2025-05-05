# Network KPI Anomaly Detection

![Anomaly Detection](https://img.shields.io/badge/ML-Anomaly%20Detection-blue)
![Python](https://img.shields.io/badge/Python-3.8%2B-brightgreen)

## Project Overview

This project implements an end-to-end machine learning pipeline to identify anomalous behavior in telecom network cells based on throughput (THP_DL) and PRB utilization (PRB_UTILIZACION_DL) metrics. The system uses multiple anomaly detection techniques combined with an ensemble approach for higher accuracy.

### Features

- **Data cleaning and preprocessing** with comprehensive quality checks
- **Feature engineering** for time-series network data
- **Ensemble anomaly detection** using multiple algorithms:
  - Statistical methods (Z-score, IQR)
  - Machine learning approaches (Isolation Forest, LOF, DBSCAN)
  - Time series deviation analysis
- **Anomaly classification** with root cause analysis
- **Alert generation system** with prioritization by severity
- **Visualization** of KPIs, trends, and detected anomalies
- **API service** for integration with operational systems

## Dataset

The system works with telecom network KPI data containing the following key fields:

- `DATE_SK`: Date of measurement
- `CELL_SK`: Unique cell identifier
- `BANDA`: Frequency band (AWS, 1900)
- `THP_DL`: Throughput downlink (Mbps)
- `PRB_UTILIZACION_DL`: Physical Resource Block utilization (0-1)

## Pipeline Structure

The anomaly detection pipeline follows these key stages:

1. **Data Loading and Initial Exploration**
   - Detect data types, missing values, duplicates
   - Statistical overview and integrity checks

2. **Data Cleaning**
   - Remove duplicates and invalid values
   - Cap PRB utilization to valid range [0,1]
   - Winsorize extreme values (±3 IQR per band)
   - Time-aware imputation of missing values

3. **Exploratory Data Analysis**
   - Statistical comparison between frequency bands
   - Correlation analysis between KPIs
   - Cell performance profiling
   - Temporal and cyclic pattern analysis

4. **Feature Engineering**
   - Lag features (1, 3, 7 days)
   - Rolling window statistics (mean, standard deviation)
   - Deviation from moving averages
   - Resource efficiency metrics
   - Normalized Z-scores at different aggregation levels
   - Temporal context features (day of week, weekend)

5. **Anomaly Detection**
   - Statistical thresholding (Z-score, IQR)
   - Isolation Forest for multivariate detection
   - Local Outlier Factor for density-based detection
   - DBSCAN for cluster-based detection
   - Time series deviation analysis
   - Ensemble voting across methods

6. **Root Cause Analysis**
   - Classification of anomaly types
   - Feature contribution analysis
   - Severity assessment
   - Operational recommendations

7. **Alert Generation System**
   - Alert prioritization by severity
   - Integration with notification systems
   - REST API for operational access
   - Kafka streaming for real-time processing

## Key Findings

The analysis revealed several patterns in network anomalies:

- Anomaly distribution varies by frequency band (2.21% in 1900 band vs 0.75% in AWS)
- Most problematic cells show anomaly rates of 10-14%
- Throughput anomalies (76.5%) are more common than PRB utilization anomalies (49.6%)
- Four main anomaly types were identified based on THP/PRB patterns:
  1. High THP, Low PRB: Efficient performance (benchmark cells)
  2. High THP, High PRB: High demand with good performance
  3. Low THP, Low PRB: Service disruption
  4. Low THP, High PRB: Inefficiency/congestion (highest priority)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/username/telecom-anomaly-detection.git
cd telecom-anomaly-detection
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Running the Full Pipeline

The main script runs the complete anomaly detection pipeline:

```bash
python 01_ML_Anomaly_Detection.py
```

### Using the Alert System API

Start the alert system with the API server:

```bash
python alert_system.py --mode api
```

Access endpoints:
- `GET /api/health` - System health check
- `GET /api/alerts` - List all active alerts
- `GET /api/alert/<alert_id>` - Get specific alert details
- `POST /api/alert/<alert_id>/resolve` - Mark alert as resolved

## Project Structure

```
Test-Tigo/
│
├── data/
│   ├── raw/                            # Original input data files (e.g. Excel)
│   │   └── Network_KPIs_202501_202503.xlsx
│   └── processed/                      # Cleaned and feature-engineered datasets
│       ├── telecom_anomalies.feather
│       ├── telecom_features.feather
│       ├── telecom_features_full.feather
│       └── telecom_kpi_clean.feather
│
├── docs/                               # Documentation and reports
│   ├── 20250424 Prueba Técnica Especialista ML.docx
│   └── 20250424 Prueba Técnica Especialista ML.pdf
│
├── models/                             # Saved models and model artifacts
│
├── notebooks/                          # Jupyter notebooks
│   ├── alert_system.log
│   └── 01_ML_Anomaly_Detection.ipynb
│
├── reports/                            # Generated reports and visual outputs
│
├── src/                                # Source code and main modules
│
├── .gitignore                          # Git ignore rules
├── README.md                           # Project overview and instructions
└── requirements.txt                    # Python package dependencies
```

## Future Improvements

- Implement deep learning models (LSTM autoencoders) for sequence anomaly detection
- Add geographic clustering for spatial anomaly patterns
- Incorporate weather and event data for contextual anomaly detection
- Develop automated remediation recommendations
- Implement A/B testing for anomaly detection thresholds