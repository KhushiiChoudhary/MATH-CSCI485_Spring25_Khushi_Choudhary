# Recursive Feature Elimination (RFE) with Linear Regression

## Introduction to Recursive Feature Elimination (RFE)

Recursive Feature Elimination (RFE) is a feature selection method that helps identify the most important features for a predictive model. It works by iteratively fitting a model and removing the least significant features based on model coefficients until a predefined number of features remain. This technique is useful for improving model interpretability and reducing overfitting.

## Overview
This project implements **Recursive Feature Elimination (RFE)** using **Linear Regression** to identify the most important features in predicting diabetes progression. The analysis is performed on the **Diabetes dataset** from `sklearn.datasets.load_diabetes()`.

## Requirements

Ensure you have the following dependencies installed:
- numpy
- pandas
- matplotlib
- scikit-learn

## Features
- Dataset: Diabetes dataset containing 442 samples and 10 features.
- Feature Selection Method: Recursive Feature Elimination (RFE).
- Model Used: Linear Regression.
- Evaluation Metric: R² Score.
- Visualization: A plot of R² score vs. the number of retained features.
- Key Findings: Identified s1 (Serum Cholesterol Level), s5 (Log Serum Triglycerides Level), and BMI as the most important features.

## Results
- Optimal Number of Features Identified: 2
- Most Important Features:
  - s1 (Serum Cholesterol Level): 931.49
  - s5 (Log Serum Triglycerides Level): 736.20
  - BMI (Body Mass Index): 542.43
- R² Score of Initial Model: 0.4526
- Final Feature Selection:** Highlighted significant metabolic factors influencing diabetes progression.


## Author
- Khushi Choudhary
- GitHub: https://github.com/KhushiiChoudhary

---
For more details, check the **report.pdf** or explore the **A1_RFE.ipynb** notebook.
