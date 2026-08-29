# Factors Associated with Poor Self-Rated Health

## Overview

This project uses exploratory data analysis and machine learning to examine which demographic, socioeconomic, lifestyle, and physical factors are most important for predicting poor self-rated health.

The analysis focuses on the following research question:

> **Which demographic, socioeconomic, lifestyle, and physical factors are most strongly associated with poor self-rated health?**

The project compares three machine learning models and uses grouped permutation importance to identify which factors make the greatest predictive contribution.

---

## Dataset

The analysis uses adult health survey data from the **2024 BRFSS Survey** containing demographic, socioeconomic, lifestyle, physical health, and self-rated health information.

The original dataset contained hundreds of variables. For this project, six predictors relevant to the research question were selected:

- **Age**
- **Education**
- **Income/Poverty**
- **BMI Category**
- **Smoking Status**
- **Physical Activity**

The target variable was based on respondents' self-rated health status.

---

## Target Variable

Self-rated health was originally recorded in five categories:

- Excellent
- Very good
- Good
- Fair
- Poor

For machine learning, these categories were converted into a binary outcome:

| Value | Health Status |
|---|---|
| 0 | Good or Better |
| 1 | Fair or Poor |

This resulted in approximately:

- **83.7% Good or Better**
- **16.3% Fair or Poor**

Because the Fair/Poor group was substantially smaller, accuracy alone was not sufficient for evaluating model performance.

---

## Methods

### Data Preparation

The data was prepared using the following steps:

1. Selected variables relevant to the research question
2. Cleaned and renamed variables
3. Removed observations with missing values in the selected variables
4. Created a binary `poor_health` target variable
5. Split the data into training and testing sets
6. Used stratified sampling to preserve the class distribution
7. One-hot encoded categorical variables
8. Standardized features for Logistic Regression

The final dataset contained **30,623 observations**.

---

## Machine Learning Models

Three classification models were evaluated:

- Logistic Regression
- Random Forest
- Gradient Boosting

Model performance was evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- Macro F1-score
- Confusion matrices

Because Fair/Poor health was the minority class, particular attention was given to the model's ability to identify respondents in that group.

---

## Results

### Model Comparison

| Model | Accuracy | Fair/Poor Recall | Fair/Poor F1 | Macro F1 |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.837 | 0.14 | 0.21 | 0.56 |
| Random Forest | 0.754 | **0.38** | **0.34** | **0.59** |
| Gradient Boosting | 0.837 | 0.14 | 0.22 | 0.57 |

Logistic Regression and Gradient Boosting achieved higher overall accuracy. However, both models identified only about 14% of respondents reporting Fair or Poor health.

The Random Forest model had lower overall accuracy but performed substantially better at identifying the Fair/Poor group. It achieved the highest:

- Fair/Poor recall
- Fair/Poor F1-score
- Macro F1-score

For this reason, Random Forest was selected for the feature importance analysis.

### Model Comparison Chart

![Model Comparison](model_comparison_f1.png)

---

## Factor Importance

Standard Random Forest feature importance was initially calculated, but grouped permutation importance was used for the final analysis.

This approach evaluates the importance of each original factor as a group. This is particularly useful because variables such as education, income, BMI, smoking, and physical activity were represented by multiple one-hot encoded columns.

The grouped permutation importance results were:

| Rank | Factor | Mean Decrease in F1 Score |
|---|---|---:|
| 1 | **Age** | **0.0538** |
| 2 | **Income/Poverty** | **0.0423** |
| 3 | **Physical Activity** | **0.0378** |
| 4 | BMI | 0.0129 |
| 5 | Education | 0.0079 |
| 6 | Smoking | 0.0049 |

### Grouped Permutation Importance

![Grouped Permutation Importance](grouped_permutation_importance.png)

Among the variables included in this analysis, **age had the greatest predictive importance**, followed by **income/poverty** and **physical activity**.

---

## Key Findings

- Overall accuracy was misleading because the dataset was imbalanced.
- Logistic Regression and Gradient Boosting achieved approximately 84% accuracy but performed poorly at identifying Fair/Poor health.
- Random Forest achieved the strongest performance for the minority Fair/Poor group.
- Age had the greatest predictive importance.
- Income/Poverty and Physical Activity were the next most important factors.
- BMI, Education, and Smoking made smaller predictive contributions in this model.

---

## Limitations

Several limitations should be considered when interpreting these results.

- Self-rated health is subjective and may not correspond directly to objective measures of health.
- This analysis identifies predictive relationships and does not establish causation.
- Only six predictors were included, and other potentially important factors were not examined.
- The models had limited performance in identifying all respondents reporting Fair or Poor health.
- Feature importance indicates a variable's contribution to the model and should not be interpreted as the size of a causal effect.

---

## Technologies Used

- Python
- pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

---

## Project Structure

```text
project-folder/
│
├── data/
│   └── adult24.csv
│
├── notebooks/
│   └── self_rated_health_analysis.ipynb
│
├── model_comparison_f1.png
├── grouped_permutation_importance.png
│
├── README.md
└── requirements.txt
