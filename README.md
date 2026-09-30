# Multimodal Stress Detection from Wearable Signals

This project investigates binary **REST vs. STRESS classification** from multimodal physiological signals collected with the Empatica E4 wearable device. The goal is to build and evaluate a machine-learning pipeline that combines information from cardiovascular, electrodermal, temperature, and movement signals while avoiding participant-level data leakage.

## Dataset

The project uses the **Wearable Device Dataset from Induced Stress and Structured Exercise Sessions (v1.0.1)** available on PhysioNet. Only the stress-induction sessions are used.

The analysis includes signals from:

- Blood Volume Pulse (BVP/PPG)
- Electrodermal Activity (EDA)
- Skin Temperature (TEMP)
- 3-axis Accelerometer (ACC)

After preprocessing and quality control, the final dataset contains **4,540 windows from 35 participants**, with 3,902 REST and 638 STRESS windows.

## Methodology

The raw signals are segmented into **30-second windows with 50% overlap** and converted into physiological and statistical features, including PPG/HRV, EDA/SCR, temperature, and accelerometer features. Baseline-normalized features are also calculated to capture participant-specific changes.

Feature selection combines multiple relevance measures with correlation filtering and modality balancing, resulting in a final **30-feature multimodal representation**.

Model performance is evaluated using **nested participant-wise cross-validation**, where feature selection and decision-threshold tuning are performed using training participants only. Logistic Regression, Random Forest, SVM, ExtraTrees, and XGBoost are evaluated. A separate random window-level experiment is included to demonstrate the effect of participant and overlapping-window leakage.

## Results

Under the primary participant-wise evaluation, the models achieved balanced accuracies of approximately **0.67–0.71**. Among the final retained models:

| Model | Balanced Accuracy | STRESS Recall | ROC-AUC |
| --- | ---: | ---: | ---: |
| Logistic Regression | 0.699 | 0.687 | 0.781 |
| Random Forest | 0.707 | 0.664 | 0.786 |
| XGBoost | 0.698 | 0.701 | 0.791 |

Random window-level splitting produced substantially higher performance, highlighting the importance of participant-wise evaluation when working with overlapping physiological time-series windows.

## Notebooks

- **`01_preprocessing_and_feature_extraction.ipynb`** — signal preprocessing, protocol segmentation, window generation, multimodal feature extraction, and quality control.
- **`02_eda_feature_selection_evaluation.ipynb`** — exploratory analysis, feature selection, participant-wise nested cross-validation, model comparison, and leakage analysis.
- **`03_final_model_training.ipynb`** — training of the final Logistic Regression, Random Forest, and XGBoost models using the selected 30-feature set.
