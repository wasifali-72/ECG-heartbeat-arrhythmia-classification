# ECG Heartbeat Arrhythmia Classification

A multi-class classifier that predicts the type of a heartbeat from a single-beat ECG segment plus RR-interval features. The notebook covers data inspection, missing-value handling, a stratified train/validation split, and a baseline Random Forest model.

## Results (baseline, 20% stratified validation set)

| Metric | Value |
|---|---|
| Accuracy | 0.99 |
| Macro F1 | 0.90 |
| Weighted F1 | 0.99 |

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| 0 | 0.99 | 1.00 | 0.99 | 9,000 |
| 1 | 0.99 | 0.89 | 0.94 | 720 |
| 2 | 0.99 | 0.99 | 0.99 | 4,511 |
| 3 | 0.96 | 0.54 | 0.69 | 119 |

Class 3 is the hardest: precision is high but recall is only 0.54, so many rare-class beats are missed. This is the main area for improvement.

## Dataset

Files expected in `nppe2_dataset/`:

- `train.csv` – 71,748 rows × 255 columns
- `test.csv` – 68,914 rows × 254 columns (no label)
- `sample_submission.csv`

**Columns**

- `id` – beat identifier (dropped before training, kept in test for submission)
- `sig_0` … `sig_249` – 250 samples of the ECG waveform for one beat
- `pre_rr`, `post_rr`, `rr_ratio` – RR-interval features (before/after the beat and their ratio)
- `label` – target class (train only)

**Class distribution (train)**

| Label | Count |
|---|---|
| 0 | 45,000 |
| 1 | 3,599 |
| 2 | 22,552 |
| 3 | 597 |

The data is heavily imbalanced; class 3 is under 1% of samples.

## Pipeline

1. **Load data** from Google Drive (Colab).
2. **Inspect**: dtypes, missing values (31 in total), duplicates (none), class balance.
3. **Impute** missing values in `pre_rr`, `post_rr`, `rr_ratio` (7, 17 and 7 values) using the **training-set median**. The same medians are applied to the test set to avoid leakage.
4. **Split** features/target (253 features) and create an 80/20 **stratified** train/validation split (`random_state=42`) so the rare class is represented in validation.
5. **Train** a baseline `RandomForestClassifier(n_estimators=100, n_jobs=-1, random_state=42)`.
6. **Evaluate** with a classification report and a confusion matrix heatmap.

## Requirements

- Python 3
- pandas, numpy
- scikit-learn
- matplotlib, seaborn
- Google Colab (the notebook mounts Google Drive via `google.colab`)

## How to run

1. Upload the `nppe2_dataset` folder to `MyDrive` in Google Drive.
2. Open `ECG_heartbeat_arrhythmia_classification.ipynb` in Google Colab.
3. Run all cells and authorize the Drive mount when prompted.

To run locally, remove the `google.colab` mount cell and change the CSV paths to your local dataset folder.

## Possible next steps

- Handle class imbalance (`class_weight='balanced'`, SMOTE, or resampling) to raise class 3 recall.
- Try gradient boosting (XGBoost / LightGBM) or a 1D CNN on the raw waveform.
- Tune hyperparameters with cross-validation and optimize for macro F1.
- Generate predictions on `test.csv` and write a submission file in the `sample_submission.csv` format (the notebook prepares `X_test` and `test_ids` but does not yet produce predictions).# ECG Heartbeat Arrhythmia Classification

A multi-class classifier that predicts the type of a heartbeat from a single-beat ECG segment plus RR-interval features. The notebook covers data inspection, missing-value handling, a stratified train/validation split, and a baseline Random Forest model.

## Results (baseline, 20% stratified validation set)

| Metric | Value |
|---|---|
| Accuracy | 0.99 |
| Macro F1 | 0.90 |
| Weighted F1 | 0.99 |

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| 0 | 0.99 | 1.00 | 0.99 | 9,000 |
| 1 | 0.99 | 0.89 | 0.94 | 720 |
| 2 | 0.99 | 0.99 | 0.99 | 4,511 |
| 3 | 0.96 | 0.54 | 0.69 | 119 |

Class 3 is the hardest: precision is high but recall is only 0.54, so many rare-class beats are missed. This is the main area for improvement.

## Dataset

Files expected in `nppe2_dataset/`:

- `train.csv` – 71,748 rows × 255 columns
- `test.csv` – 68,914 rows × 254 columns (no label)
- `sample_submission.csv`

**Columns**

- `id` – beat identifier (dropped before training, kept in test for submission)
- `sig_0` … `sig_249` – 250 samples of the ECG waveform for one beat
- `pre_rr`, `post_rr`, `rr_ratio` – RR-interval features (before/after the beat and their ratio)
- `label` – target class (train only)

**Class distribution (train)**

| Label | Count |
|---|---|
| 0 | 45,000 |
| 1 | 3,599 |
| 2 | 22,552 |
| 3 | 597 |

The data is heavily imbalanced; class 3 is under 1% of samples.

## Pipeline

1. **Load data** from Google Drive (Colab).
2. **Inspect**: dtypes, missing values (31 in total), duplicates (none), class balance.
3. **Impute** missing values in `pre_rr`, `post_rr`, `rr_ratio` (7, 17 and 7 values) using the **training-set median**. The same medians are applied to the test set to avoid leakage.
4. **Split** features/target (253 features) and create an 80/20 **stratified** train/validation split (`random_state=42`) so the rare class is represented in validation.
5. **Train** a baseline `RandomForestClassifier(n_estimators=100, n_jobs=-1, random_state=42)`.
6. **Evaluate** with a classification report and a confusion matrix heatmap.

## Requirements

- Python 3
- pandas, numpy
- scikit-learn
- matplotlib, seaborn
- Google Colab (the notebook mounts Google Drive via `google.colab`)

## How to run

1. Upload the `nppe2_dataset` folder to `MyDrive` in Google Drive.
2. Open `ECG_heartbeat_arrhythmia_classification.ipynb` in Google Colab.
3. Run all cells and authorize the Drive mount when prompted.

To run locally, remove the `google.colab` mount cell and change the CSV paths to your local dataset folder.

## Possible next steps

- Handle class imbalance (`class_weight='balanced'`, SMOTE, or resampling) to raise class 3 recall.
- Try gradient boosting (XGBoost / LightGBM) or a 1D CNN on the raw waveform.
- Tune hyperparameters with cross-validation and optimize for macro F1.
- Generate predictions on `test.csv` and write a submission file in the `sample_submission.csv` format (the notebook prepares `X_test` and `test_ids` but does not yet produce predictions).v
