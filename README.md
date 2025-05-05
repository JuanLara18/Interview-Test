# Network Anomaly Detection

This repository contains the solution to a technical challenge for an ML Specialist position. The challenge focuses on analyzing telecom network data and building an anomaly detection system to identify potential service degradation events.

## Challenge Overview

The challenge involves analyzing mobile telecommunications network data with metrics from different cell towers operating in various frequency bands (AWS 1700/2100 MHz and 1900 MHz). The main objective is to detect anomalies that could represent risks of speed reduction or high load increases.

### Key Metrics

- **THP_DL**: Downlink Throughput - Effective download speed perceived by users
- **PRB_UTILIZACION_DL**: Downlink PRB Utilization - Level of resource usage in the downlink

## Repository Structure

```
├── data/
│   ├── processed/          # Cleaned and feature-engineered datasets
│   └── raw/                # Original data (Network_KPIs_202501_202503.xlsx)
├── docs/                   # Challenge description and requirements
├── models/                 # Serialized models
├── notebooks/              
│   └── 01_ML_Anomaly_Detection.ipynb   # Main notebook with all analyses
├── reports/                # Generated visualizations
└── README.md
```

## Implementation

The solution consists of three main components:

1. **Exploratory Data Analysis (EDA)**
   - Analysis of outliers, missing data, and general patterns
   - Comparison of performance patterns between different frequency bands
   - Visualizations of the relationship between throughput and resource utilization

2. **Anomaly Detection Model**
   - Implementation of various anomaly detection techniques
   - Ensemble approach combining statistical and machine learning methods
   - Identification of cells with abnormal behavior

3. **Alert Automation Strategy**
   - Design of an alert system based on the anomaly detection model
   - Technical approach for integrating the model into a monitoring system
   - Tools and technologies that would be used for implementation

## Key Results

The analysis revealed significant differences in performance between the AWS and 1900 MHz bands. The anomaly detection system successfully identified unusual patterns in network performance that could indicate service issues, with a particular focus on throughput degradation and resource utilization spikes.

## Running the Notebook

The main analysis is contained in the Jupyter notebook `01_ML_Anomaly_Detection.ipynb`, which includes all code, visualizations, and detailed explanations of the approach.