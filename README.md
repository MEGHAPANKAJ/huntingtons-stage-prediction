# Comparative Analysis of Machine Learning Models for Predicting Huntington's Disease Stage

A comparison of eight machine learning models for classifying Huntington's disease stage (Early, Middle, Late, Pre-symptomatic) from clinical, genetic and demographic data, evaluated under three feature selection settings.

**Author:** Megha P (IMS22165), School of Data Science, IISER Thiruvananthapuram
**Supervisor:** Dr. Pranshu Mandal

## Overview

The project asks which model, and which feature selection strategy, best predicts disease stage. Every model is run in three scenarios:

1. Before feature selection (all preprocessed features)
2. After variance threshold and correlation-based feature selection
3. After Chi-Square and ANOVA F-test feature selection

The full write-up, with classification reports and confusion matrices, is in the report PDF in this repository.

## Dataset

[Huntington's Disease Dataset on Kaggle](https://www.kaggle.com/datasets/rajmohnani12/huntingtondisease-dataset/data)

It contains age, sex, family history, HTT CAG repeat length, motor symptoms, cognitive decline, chorea score, brain volume loss, functional capacity, gene mutation type, HTT gene expression level, protein aggregation level and genomic annotations (gene or factor, chromosome location, function, effect, category). The target is **Disease Stage** with four roughly balanced classes (about 2,400 test samples each).

The CSV is not included in this repository. Download it from Kaggle and place it in the project folder as `Huntington_Disease_Dataset.csv`.

## Method

**Preprocessing**
- Dropped Patient ID, Random Protein Sequence and Random Gene Sequence.
- Cleaned Cognitive Decline so that "None" entries stored as empty strings, zeros or "nan" are not treated as missing values, then ordinal encoded (None=0, Mild=1, Moderate=2, Severe=3).
- Ordinal encoded Motor Symptoms (0 to 3).
- Label encoded the other categorical features and the target (Early=0, Late=1, Middle=2, Pre-symptomatic=3).
- Standardised continuous features to zero mean and unit variance.

**Feature selection**
- *Correlation based:* variance threshold of 0.05 (no features removed), then a correlation threshold of 0.85, which removed Effect and Category.
- *Chi-Square and ANOVA:* removed Sex, Effect, Motor Symptoms, Gene or Factor and Chromosome Location (Chi-Square), and Chorea Score and HTT Gene Expression Level (ANOVA F-test).

**Models:** Logistic Regression, Naive Bayes, SVM (RBF kernel), K-Nearest Neighbors (k=5), K-Means (clusters aligned to labels by majority vote), Decision Tree, Random Forest (100 trees) and Gradient Boosting. A 80/20 train-test split with `random_state=42` is used. Metrics are accuracy, precision, recall, F1 score and confusion matrices.

## Results

Test accuracy for each model and scenario:

| Model | Before FS | After Correlation | After Chi2 + ANOVA |
|-------|-----------|-------------------|--------------------|
| Naive Bayes | 0.2542 | 0.2555 | 0.2547 |
| Decision Tree | 0.2504 | 0.2492 | 0.2559 |
| Random Forest | 0.2576 | 0.2460 | 0.2519 |
| Gradient Boosting | 0.2481 | 0.2482 | 0.2462 |
| SVM | 0.2563 | 0.2532 | 0.2457 |
| Logistic Regression | 0.2576 | 0.2579 | **0.2600** |
| K-Means | 0.2510 | 0.2536 | 0.2506 |
| KNN | 0.2552 | 0.2548 | 0.2553 |

Logistic Regression with Chi-Square and ANOVA feature selection performed best, at about 26% accuracy. With four balanced classes, chance level is 25%, so all models perform only slightly above chance. The features in this dataset appear to carry little discriminative signal for disease stage, and the differences between models are small.

## Repository structure

```
.
├── Comparative_Analysis_of_Machine_Learning_Models.ipynb   # Full code and outputs
├── Comparative_Analysis_of_Machine_Learning_Models.pdf     # Project report
├── requirements.txt
└── README.md
```

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Download the dataset from Kaggle and save it as `Huntington_Disease_Dataset.csv`.
4. Update the CSV path in the notebook. It currently points to `/content/drive/MyDrive/Huntington/Huntington_Disease_Dataset.csv` (Google Colab with Drive), so change it to `Huntington_Disease_Dataset.csv` if the file is in the same folder.
5. Open and run the notebook:
   ```bash
   jupyter notebook Comparative_Analysis_of_Machine_Learning_Models.ipynb
   ```

## Future work

- Feature engineering informed by domain knowledge, such as higher level clinical features.
- Target encoding, frequency encoding or neural embeddings for categorical variables.
- Hyperparameter tuning and cross-validation.
- Deep learning models designed for tabular data.
- Validation on independent datasets.

## References

1. Huntington's Disease Dataset, Kaggle: https://www.kaggle.com/datasets/rajmohnani12/huntingtondisease-dataset/data
