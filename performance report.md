# Kernel Energy Consumption Predictor - Model and Performance Report

**Author:** Afraeim Kader  
**Date:** October 23, 2025  

---

### Selected Model: **Gradient Boosting Regressor**

I chose the Gradient Boosting Regressor for kernel energy consumption prediction for following reasons:

1. **Non-linear Relationship Modeling**
   - Kernel energy consumption has complex, non-linear relationships with configuration parameters (like, batch size, channels, kernel size)
   - Gradient Boosting do well at capturing these intricate patterns through sequential ensemble learning

2. **Robustness to Data Characteristics**
   - It can handles mixed data types
   - Resistant to outliers and missing values

3. **Interpretability**
   - Provides feature importance scores, helping understand which kernel parameters most influence energy consumption
   - Essential for hardware-software co-design decisions

4. **Computational Efficiency**
   - Fast training and inference times suitable for iterative model development

---

## 2. Training Overhead

### Training Time: **0.32 seconds** (~320 milliseconds)

**Classification: LOW OVERHEAD**
- **Taining** on 1,492 samples with **15 features**
- Best for online learning and model updates
- Minimal computational resources required - single CPU core
- No GPU acceleration needed


#### The model maintains excellent training efficiency with negligible overhead, making it ideal for:
- Continuous model updates as new kernel data becomes available
- Real-time retraining in adaptive systems
- Resource-constrained environments (edge devices, embedded systems)

---

## 3. Dataset Statistics

### Training and Evaluation Samples

| Dataset Split | Number of Samples | Percentage |
|--------------|-------------------|------------|
| **Training Set** | 1,492 | 80% |
| **Test Set** | 373 | 20% |
| **Total** | **1,865** | 100% |

### Feature Information
- **Number of Features:** 15
- **Feature Types:** 
  - **Categorical (encoded):** kernel_type (16 types)
  - **Numerical:** 14 configuration parameters
- **Configuration Parameters Extracted:**
  - `CIN`, `COUT` - Input/output channel counts
  - `HW` - Spatial dimensions (height/width)
  - `input_height`, `input_width`, `input_channels` - Explicit input tensor shape
  - `KERNEL_SIZE` - Convolution kernel size
  - `STRIDES` - Stride for convolution/pooling
  - `POOL_STRIDES` - Pooling-specific stride
  - `CIN1`, `CIN2`, `CIN3`, `CIN4` - Multi-input channel configurations
  - `config_id` - Unique configuration identifier
- **Data Source:** 16 different kernel types across CPU operations

---

## 4. Model Accuracy and Performance

### Test Set Performance Metrics (373 samples)

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **R² Score** | **0.121** | only 12.1% of variance |
| **RMSE** | **154.32** | Energy units |
| **MAE** | **29.12** | Average prediction off by ±29 units |
| **MSE** | 23,815.79 |  |

### Training Set Performance (1,492 samples)

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **R² Score** | **0.198** | Model learns patterns |
| **RMSE** | **51.03** | Lower than test set |
| **MAE** | **12.94** | Average training error ±12.9 units |
| **MSE** | 2,604.27 | Mean Squared Error |

### Overfitting Analysis
- **Overfitting Gap (R²):** **0.077** (0.198 - 0.121)
- **Status:** **Good Generalization** (gap < 0.10)
- Training and test performance are consistent, indicating stable predictions


## Conclusion

The **Gradient Boosting Regressor** with 15 extracted features shows R² 0.12 but seems insufficient for production deployment. 

---
#### Performance Report.JSON
```
{
  "model_type": "gradient_boosting",
  "training_time_seconds": 0.32093143463134766,
  "training_overhead": "Low",
  "training_samples": 1492,
  "test_samples": 373,
  "total_samples": 1865,
  "num_features": 15,
  "train_metrics": {
    "mae": 12.938556882238307,
    "rmse": 51.032004703370205,
    "r2": 0.19822624834860503,
    "mape": 25470.947405024854,
    "mse": 2604.265504044799
  },
  "test_metrics": {
    "mae": 29.116783776848088,
    "rmse": 154.3236393561936,
    "r2": 0.12081818390606913,
    "mape": 17878.879054180572,
    "mse": 23815.785664140512
  },
  "overfitting_gap": 0.0774080644425359
}
```
