# Solar Flux Forecasting using LSTM Networks
This project focuses on the analysis and preprocessing of 10.7 cm Solar Flux (F10.7) data, a widely used measurement of solar activity in space weather studies, solar physics, and terrestrial atmospheric phenomena.

The main objective is to prepare the dataset for time-series forecasting tasks using temporal analysis techniques, feature engineering, and solar periodicity detection.

---

# Data Preparation

To better understand the patterns within the data, several lag features were implemented.

## Lag Features

Lag features are temporal characteristics generated from shifted versions of the original signal. These features allow the model to learn temporal dependencies using past solar flux values.

Implemented examples:

- Daily lag
- Weekly lag
- Monthly lag
- Yearly lag
- Solar rotation lag (~27 days)

The implementation of these lag features made it possible to identify which past values were most similar to current observations. In this case, the daily lag and solar rotation lag showed the highest similarity. This information helps determine the historical context provided to the model for future predictions.

---

## Solar Rotation Cycles

The Sun exhibits an approximate 27-day periodicity caused by its rotation. This pattern frequently appears in solar flux measurements due to the reappearance of active solar regions.

---

# Modeling

# Model Implementation

## Model 1: *Predicting the Daily 10.7-cm Solar Radio Flux Using the Long Short-Term Memory Method (2022)*

### Description

The model used was a Long Short-Term Memory (LSTM) neural network designed to capture temporal dependencies in sequential data. The architecture employed 50 neurons and a learning rate of 0.001.

According to the original paper, the most accurate forecast horizon was one day. Therefore, the model was implemented using daily forecasting recommendations.

Initially, the model was trained for 100 epochs, resulting in underfitting. The number of epochs was then increased to 300. This decision was justified because the dataset contained three measurements per day, and data preparation increased the amount of information processed compared to using daily averages.

---

### Implemented Architecture

| Parameter | Value |
|------------|--------|
| Layers | 1 |
| Neurons | 50 |
| Activation Function | ReLU |
| Optimizer | Adam |
| Learning Rate | 0.001 |
| Epochs | 300 |
| Batch Size | 32 |

### Advantages

- The original model incorporates the solar cycle.
- Learns complex temporal patterns.
- Can model solar cycles at different time scales.

---

## Model 2: *Deep Learning LSTM-based Approaches for 10.7 cm Solar Radio Flux Forecasting up to 45 Days*

### Description

This model implements three layers with 24 neurons each, consisting of an input layer, hidden layer, and output layer.

These layers enable the model to identify patterns in the data depending on the forecasting horizon. This capability comes from the hidden (forget) layer, which retains only the most relevant information for future predictions.

This model was selected due to its ability to forecast up to 45 days ahead, including the 27-day solar rotation cycle.

During implementation, forecasting horizons of 1, 3, and 27 days were tested. Similar to the findings reported in the paper, forecasts based on daily historical data produced the most accurate results.

---

### Configuration Used

| Parameter | Value |
|------------|--------|
| Layers | 3 |
| Neurons | 24 |
| Activation Function | tanh |
| Optimizer | Adam |
| Learning Rate | 0.001 |
| Epochs | 225 |
| Batch Size | 48 |

---

### Input Variables

- The variable used in both models was the adjusted solar flux value, **fluxadjflux**, since it is the standard variable commonly used in the field.
- Unlike the scientific papers referenced, this project evaluated solar flux at the hourly level rather than using daily averages.

---

# Evaluation

At the beginning of the evaluation phase, R² and loss functions were implemented based on previous practice exercises involving CNN and RNN time-series models.

After reviewing the scientific literature and the metrics used in those studies, additional evaluation metrics were incorporated.

To evaluate model performance, three widely used forecasting metrics were employed:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- Correlation Coefficient (R) / Coefficient of Determination (R²)

---

# Evaluation Metrics

## Mean Absolute Error (MAE)

### Formula

```math
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i-\hat{y}_i|
```

### Justification

MAE measures the average magnitude of absolute prediction errors between observed and predicted values. It provides a direct interpretation of the error in the same units as the F10.7 solar flux.

According to Zhang et al. (2022), MAE quantifies the deviation between observed and predicted values, providing a clear measure of predictive performance. Lower MAE values indicate higher prediction accuracy.

Additionally, because MAE does not square errors, it is less sensitive to outliers and better reflects overall model behavior across the entire time series.

---

## Root Mean Squared Error (RMSE)

### Formula

```math
RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
```

### Justification

RMSE measures the average deviation between observed and predicted values while penalizing large errors more heavily due to the squared term.

RMSE is widely used for evaluating forecasting performance because it emphasizes larger prediction errors and provides an intuitive measure of model accuracy.

Lower RMSE values indicate better model performance.

---

## Coefficient of Determination (R²)

### Formula

```math
R^2 = 1 - \frac{SS_{res}}{SS_{tot}}
```

### Justification

The coefficient of determination (R²) quantifies the proportion of variance in the observed data explained by the model.

While MAE and RMSE evaluate prediction errors, R² assesses how well the model reproduces the overall behavior of the time series.

Values closer to 1 indicate stronger explanatory power and better agreement between actual and predicted values.

---

## General Interpretation

A model is considered superior when it:

- Produces lower MAE values.
- Produces lower RMSE values.
- Achieves higher R² values.

These metrics allow simultaneous evaluation of both prediction accuracy and the model's ability to reproduce the temporal dynamics of the F10.7 solar flux.

---

# Experimental Results

## Model Performance

| Metric | Model 1 | Model 2 |
|----------|----------|----------|
| MAE | 9.93 | 12.86 |
| RMSE | 17.39 | 21.79 |
| R² | 0.8278 | 0.73 |

### Prediction Graph Model 1

(<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/f025909f-22f8-4369-b357-6593f5ecf8af" />


### Prediction Graph Model 2

(<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/d77a4c95-c5a7-4754-9d87-c75e49ae64ba" />
)
---

## Comparative Analysis

### Accuracy

Model 1 achieved better results across all evaluated error metrics.

| Metric | Model 1 | Model 2 |
|----------|----------|----------|
| MAE | 9.93 | 12.86 |
| RMSE | 17.39 | 21.79 |
| MAPE | 5.13% | 6.33% |

This indicates that Model 1's predictions were, on average, closer to the actual F10.7 values.

### Ability to Capture Temporal Patterns

Model 1 achieved an R² value of 0.8278, outperforming Model 2's 0.7301.

This indicates that Model 1 explained approximately 82.8% of the variance in the time series and captured solar activity patterns more effectively.

### Computational Cost

Model 1 required longer training times due to its more complex architecture and sequence processing requirements.

However, this additional computational cost resulted in consistent improvements across all evaluation metrics.

Model 2 trained faster and required fewer computational resources but produced less accurate predictions.

### Robustness

Both models demonstrated good generalization capability on the test set.

However, Model 1 achieved lower errors and higher explanatory power simultaneously, indicating better adaptation to the temporal characteristics of the analyzed series.

---

# Model Selection

## Selected Model

**Model 1**

### Justification

After comparing both models using MAE, RMSE, MAPE, and R², Model 1 was selected as the final model.

The model achieved:

- MAE = 9.93
- RMSE = 17.39
- MAPE = 5.13%
- R² = 0.8278

These results demonstrate that Model 1 not only generates more accurate predictions but also captures the temporal patterns present in the F10.7 solar flux more effectively.

<img width="855" height="547" alt="image" src="https://github.com/user-attachments/assets/dffb7277-1cb2-41ed-aaeb-b2e39f4f4e56" />
---

# Final Model Improvements

After defining the architecture, several optimization attempts were performed.

## Attempt 1: tanh Activation Function

The learning curve of Model 1 (R² ≈ 0.85) suggested testing the **tanh** activation function, motivated by its common use in LSTM architectures.

### Results

Performance deteriorated to approximately:

- R² ≈ 0.80

The best training run achieved R² ≈ 0.85, which did not improve predictive accuracy.
<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/65b150d4-0c7e-47a5-8040-6ff420b85f40" />
---

## Attempt 2: Increasing Epochs

The number of training epochs was increased to 600.

### Results

This change led to overfitting without improving performance.
<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/4cf49f44-7cf4-454c-a91f-c4e16a75af00" />
---

## Attempt 3: Early Stopping

Early Stopping was implemented with a maximum of 100 epochs.

### Results

This version produced the best results, converging between 120 and 130 epochs.

| Metric | Model 1 | Model 2 | Final Model |
|----------|----------|----------|----------|
| MAE | 9.93 | 12.86 | 6.09 |
| RMSE | 17.39 | 21.79 | 11.30 |
| R² | 0.83 | 0.73 | 0.93 |

<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/522bea30-4fa4-4082-a719-5596c642eff9" />

---

# Final Model Prediction

| Timestamp | Actual Fluxadjflux | Predicted Fluxadjflux | Absolute Error |
|------------|------------|------------|------------|
| 17:00 | 135.4000 | 139.7660 | 4.3660 |
| 20:00 | 135.6000 | 135.4461 | 0.1539 |
| 23:00 | 136.5000 | 136.0847 | 0.4153 |

<img width="1490" height="590" alt="image" src="https://github.com/user-attachments/assets/b7b1be00-f69b-48d0-8def-45dcd91d08c5" />
---

# General Conclusions

The project successfully developed a model capable of predicting the F10.7 solar flux with a coefficient of determination of **R² = 0.93**, achieving competitive performance compared to the state-of-the-art studies analyzed.

Although the model did not surpass the accuracy reported in the referenced scientific papers, it is important to note that those studies use daily average solar flux values, whereas the model developed in this project operates on the three daily measurements available. This represents a more detailed forecasting problem with greater variability.

Furthermore, the combination of effective data preparation, relevant temporal features, and training optimization through Early Stopping resulted in a robust model with strong generalization capabilities.

The results demonstrate the potential of LSTM networks for modeling solar activity and supporting applications related to:

- Space weather monitoring
- Satellite communications
- Systems sensitive to solar activity variations

---

# Future Improvements

A potential improvement for future work is incorporating the exact measurement times of the solar flux observations.

During the analysis, it was observed that the three daily measurements are not always collected at the same hours throughout the year. These temporal variations are currently not explicitly represented in the model.

Including measurement time as an input feature could provide a more accurate representation of the conditions under which the data are collected and improve predictive performance.

Additionally, having advance access to scheduled measurement times could enable more accurate same-day forecasts, increasing the practical value of the system for solar activity and space weather monitoring applications.

---

# References

## Model 1

Zhang, W., Zhao, X., Feng, X., Liu, C., Xiang, N., Li, Z., & Lu, W. (2022). *Predicting the Daily 10.7-cm Solar Radio Flux Using the Long Short-Term Memory Method*. Universe, 8(1), 30.

## Model 2

Jerse, G., & Marcucci, A. (2024). *Deep Learning LSTM-based Approaches for 10.7 cm Solar Radio Flux Forecasting up to 45 Days*. Astronomy and Computing.

## Dataset

Government of Canada. (2019). *About the Solar Flux Data*.

## Final Google Colab Notebook

https://drive.google.com/file/d/1qXCicjbVbAcF3Aq8u8VcoLCbhjs1FCfJ/view?usp=sharing

(Shared in case GitHub does not properly render Jupyter Notebook widgets.)
