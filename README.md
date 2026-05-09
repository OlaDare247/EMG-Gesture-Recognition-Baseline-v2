# EMG-Gesture-Recognition-Baseline-v2
A pilot EMG gesture-recognition benchmark built on a frozen Ninapro DB1 subject-10 subset, using sliding-window segmentation, handcrafted time-domain features, and a LinearSVC baseline for myoelectric control.

## Overview
This project presents a pilot benchmark for **EMG-based gesture recognition** using a frozen single-subject subset derived from the **Ninapro DB1** dataset. The goal is to build a clean and reproducible baseline for **myoelectric control** using classical EMG signal processing and machine learning.

Version 2 focuses on a **stable subject-10 pilot sample**, with preprocessing, sliding-window segmentation, handcrafted time-domain feature extraction, and a **LinearSVC** classifier.

## Motivation
This project was developed to extend my work in biomedical data analysis from imaging into **biosignal pattern recognition**, especially in areas related to:

- EMG signal analysis
- myoelectric gesture recognition
- human-machine interaction
- assistive and rehabilitation technologies
- robust pattern recognition for noisy biological signals

## Dataset
- **Source:** Open-source CSV subset derived from **Ninapro DB1**
- **Pilot subject:** **Subject 10**
- **Saved raw sample shape:** **(16713, 38)**
- **Saved windowed sample shape:** **(1597, 20, 10)**

### Labels
The frozen pilot sample contains:
- **0** = rest
- **1** = gesture 1
- **2** = gesture 2

For classifier training in this version, the **rest class is removed**, and the final model is trained on **gesture classes 1 and 2 only**.

## Pipeline
The Version 2 workflow is:

1. Load frozen pilot sample from Google Drive
2. Use 10 EMG channels (`emg_0` to `emg_9`)
3. Apply sliding-window segmentation
   - **Window size:** 200 ms
   - **Step size:** 100 ms
4. Remove rest windows (`label = 0`)
5. Extract handcrafted time-domain EMG features:
   - RMS
   - Mean Absolute Value (MAV)
   - Waveform Length (WL)
   - Zero Crossings (ZC)
   - Slope Sign Changes (SSC)
6. Train/test split with stratification
7. Train **LinearSVC**
8. Evaluate using:
   - Accuracy
   - Macro F1-score
   - Classification report
   - Confusion matrix

## Model
- **Classifier:** LinearSVC
- **Preprocessing:** StandardScaler
- **Channels used:** 10
- **Feature vector size:** 50

## Results
### Final Version 2 Results
- **Model:** LinearSVC
- **Dataset:** Frozen Ninapro DB1 pilot (**subject 10**)
- **Window size:** 200 ms
- **Step size:** 100 ms
- **Window tensor shape:** `(1597, 20, 10)`
- **Non-rest classes used for training:** `[1, 2]`
- **Test samples:** `198`
- **Accuracy:** **0.9646**
- **Macro F1-score:** **0.9646**

### Classification performance
| Class | Precision | Recall | F1-score | Support |
|------|-----------:|-------:|---------:|--------:|
| 1 | 0.9792 | 0.9495 | 0.9641 | 99 |
| 2 | 0.9510 | 0.9798 | 0.9652 | 99 |

### Confusion matrix summary
- Class 1 correctly classified: **94**
- Class 1 misclassified as class 2: **5**
- Class 2 correctly classified: **97**
- Class 2 misclassified as class 1: **2**

These results show that the baseline model can separate the two non-rest gesture classes very effectively in this frozen pilot setting.

## File Structure
```text
EMG_Project/
├── raw/
│   └── ninapro_db1_subject10_labels0_9_sample.csv
├── processed/
│   ├── X_subject10.npy
│   ├── y_subject10.npy
│   ├── groups_subject10.npy
│   ├── Xw_subject10.npy
│   ├── yw_subject10.npy
│   └── gw_subject10.npy
├── results/
│   ├── sample_info.txt
│   └── v2_baseline_results.txt
└── README.md
