# Sloan Digital Sky Survey (SDSS) DR14 - Exploratory Data Analysis and Classification

This project involves an in-depth exploratory data analysis (EDA) of the Sloan Digital Sky Survey (SDSS) Data Release 14. The primary goal is to classify celestial objects into three categories: **Star**, **Galaxy**, and **Quasar (QSO)**. An XGBoost Classifier is implemented and optimized to achieve high prediction accuracy.

## Dataset

[Kaggle](https://www.kaggle.com/code/emirhanhasrc/eda-xgboost-classifier-gridsearch)
The dataset is from the Sloan Digital Sky Survey, a major multi-spectral imaging and spectroscopic redshift survey. This specific dataset is from Data Release 14 and contains 10,000 observations of celestial objects.

### Dataset Features

The dataset contains the following features:

| Feature | Description | Type |
| :--- | :--- | :--- |
| **objid** | Object Identifier | Numeric |
| **ra** | Right Ascension (J2000) | Numeric |
| **dec** | Declination (J2000) | Numeric |
| **u, g, r, i, z** | Photometric filter bands (ultraviolet, green, red, near-infrared, far-infrared) | Numeric |
| **run** | Run Number | Numeric |
| **rerun** | Rerun Number | Numeric |
| **camcol** | Camera column | Numeric |
| **field** | Field number | Numeric |
| **specobjid** | Spectroscopic Object Identifier | Numeric |
| **class** | Object Class (STAR, GALAXY, QSO) | Categorical |
| **redshift** | Redshift | Numeric |
| **plate** | Plate number | Numeric |
| **mjd** | Modified Julian Date | Numeric |
| **fiberid** | Fiber ID | Numeric |

## Exploratory Data Analysis (EDA)

A comprehensive EDA was performed to understand the data's characteristics and the relationships between features.

1.  **Data Cleaning**: Several identifier columns (`objid`, `specobjid`, `rerun`, `camcol`, `field`, `run`, `fiberid`) were dropped as they do not contribute to the classification model.
2.  **Class Distribution**: A countplot was used to visualize the distribution of the three classes. The dataset is imbalanced, with a majority of objects being Galaxies, followed by Stars, and then QSOs.
3.  **Coordinate Distribution**: Boxplots for Right Ascension (`ra`) and Declination (`dec`) showed that these coordinates are somewhat useful in distinguishing between the classes, though there is significant overlap.
4.  **Pairwise Relationships**: A pairplot was generated after encoding the `class` variable into numerical values. This helped visualize the relationships between all pairs of features, colored by class.
5.  **Correlation Matrix**: A heatmap of the correlation matrix revealed strong correlations between the photometric filter bands (`u`, `g`, `r`, `i`, `z`), and also between `plate` and `mjd`.
6.  **Redshift Distribution**: Histograms of the `redshift` values for each class were plotted. The distributions are distinct for each class, with Stars having a redshift close to zero, Galaxies having a wider range of positive redshifts, and QSOs having the highest and most spread-out redshift values. This indicates that redshift is a very important feature for classification.

## Modeling and Evaluation

An XGBoost Classifier was chosen for this task due to its efficiency and high performance on structured data.

### 1. Data Preprocessing

-   The `class` column was label-encoded into numerical values (GALAXY: 0, QSO: 1, STAR: 2).
-   The data was split into features (X) and the target variable (y).
-   The dataset was then divided into a training set and a testing set.

### 2. Baseline XGBoost Model

A standard `XGBClassifier` with `n_estimators=100` was trained. The model achieved a very high accuracy score of **98.76%** on the test set.

**Classification Report (Baseline):**

| | precision | recall | f1-score | support |
| :--- | :--- | :--- | :--- | :--- |
| **0 (GALAXY)** | 0.99 | 0.99 | 0.99 | 1687 |
| **1 (QSO)** | 0.95 | 0.93 | 0.94 | 271 |
| **2 (STAR)** | 0.99 | 1.00 | 1.00 | 1342 |
| **accuracy** | | | **0.99** | **3300** |
| **macro avg** | 0.98 | 0.97 | 0.98 | 3300 |
| **weighted avg** | 0.99 | 0.99 | 0.99 | 3300 |

### 3. Hyperparameter Tuning

`GridSearchCV` was used to find the optimal hyperparameters for the XGBoost model. The following parameter grid was used:
-   `n_estimators`:
-   `learning_rate`: [0.01, 0.1]
-   `max_depth`:
-   `colsample_bytree`: [0.3, 0.4, 0.5, 0.8, 1]

**Best Parameters Found:**
-   `colsample_bytree`: 0.3
-   `learning_rate`: 0.1
-   `max_depth`: 5
-   `n_estimators`: 200

### 4. Tuned Model Evaluation

The XGBoost model was retrained with the best parameters found by `GridSearchCV`. The tuned model achieved a slightly higher accuracy score of **98.85%**.

**Classification Report (Tuned):**

| | precision | recall | f1-score | support |
| :--- | :--- | :--- | :--- | :--- |
| **0 (GALAXY)** | 0.99 | 0.99 | 0.99 | 1687 |
| **1 (QSO)** | 0.96 | 0.92 | 0.94 | 271 |
| **2 (STAR)** | 0.99 | 1.00 | 1.00 | 1342 |
| **accuracy** | | | **0.99** | **3300** |
| **macro avg** | 0.98 | 0.97 | 0.98 | 3300 |
| **weighted avg** | 0.99 | 0.99 | 0.99 | 3300 |

## Conclusion

The exploratory data analysis successfully highlighted the key features for distinguishing between stars, galaxies, and quasars, with `redshift` being a particularly strong indicator. The XGBoost Classifier demonstrated excellent performance, achieving an accuracy of over **98.7%** even with the baseline model. Hyperparameter tuning using `GridSearchCV` further improved the model's accuracy to **98.85%**, showcasing the robustness of the model for this classification task. The high precision and recall values across all classes confirm that the model is highly effective in classifying celestial objects from the SDSS dataset.
