<img width="1790" height="601" alt="下载" src="https://github.com/user-attachments/assets/06e07d98-086c-4fbe-924c-ace5fc8dfc77" />
<img width="1790" height="601" alt="下载 (1)" src="https://github.com/user-attachments/assets/a6d27690-cb33-413c-a585-f74373b8f5ab" />



# Richmond Land-Cover Classification (2020–2024)
### Mapping Urban Change with Sentinel-2, K-Means and Random Forest

This project maps land cover in **Richmond upon Thames, London** using **Sentinel-2 satellite imagery** and compares two machine-learning classification approaches.

The objectives of the project are to:

- classify major land-cover types
- analyse land-cover change from 2020 to 2024
- compare **K-Means** and **Random Forest**
- evaluate which method provides more reliable classification results

Land-cover classes include:

- Water
- Vegetation
- Urban
- Bare soil / bright surface

This project compares an **unsupervised K-Means clustering workflow** with a **supervised Random Forest classification model** for annual land-cover mapping over **Richmond upon Thames, London** using Sentinel-2 satellite imagery.  
The study evaluates classification accuracy, temporal land-cover change, and methodological differences between supervised and unsupervised approaches.

---

<details open>
<summary><strong>Table of Contents</strong></summary>

- [Project Motivation and Background](#project-motivation-and-background)
- [Data Source and Pre-processing](#data-source-and-pre-processing)
- [Method Overview](#method-overview)
  - [3.1 Unsupervised K-Means](#31-unsupervised-k-means)
  - [3.2 Supervised Random Forest](#32-supervised-random-forest)
- [Notebooks and Quick Start](#notebooks-and-quick-start)
- [Results](#results)
- [Comparison of Methods](#comparison-of-methods)
- [Key Findings](#key-findings)
- [Environmental Cost](#environmental-cost)
- [References & Acknowledgements](#references--acknowledgements)

</details>

---

## 1. Project Motivation and Background

Urban land-cover classification is essential for understanding how cities change over time. Monitoring vegetation, built-up land, water, and exposed surfaces can support urban planning, environmental monitoring, and climate adaptation.

Satellite imagery provides a powerful way to observe land-cover change, but urban environments are spectrally complex because buildings, roads, vegetation and water often occur close together or within mixed pixels.

This project compares two different machine-learning approaches for mapping urban land cover:

- **K-Means clustering (unsupervised)** – groups pixels based on spectral similarity without training labels.
- **Random Forest classification (supervised)** – learns land-cover classes from labelled training data.

Using **Sentinel-2 imagery** for **2020, 2022 and 2024**, this project investigates both land-cover patterns and change over time in **Richmond upon Thames, London**.

> **Research question:**  
> *How do supervised and unsupervised classification methods differ in mapping land cover and detecting land-use change in Richmond upon Thames between 2020 and 2024?*

<p align="center">
  <img src="https://github.com/user-attachments/assets/6a0090a3-2516-4075-84f1-6222583a53cc" width="500">
</p>

<p align="center">
  <em>Location of Richmond upon Thames within Greater London.</em><br>
  <small>Source: <i>Richmond upon Thames location map (detailed borough map)</i>, Wikimedia Commons (CC BY-SA).</small>
</p>

---

## 2 Data Source and Pre-processing

This project uses **Sentinel-2 Level-2A surface reflectance imagery** for annual land-cover classification.

| Step | Detail |
|------|--------|
| **Data source** | Sentinel-2 Level-2A |
| **Years analysed** | 2020, 2022, 2024 |
| **Spatial resolution** | 10 m |
| **Study area** | Richmond upon Thames, London |
| **Spectral bands** | B2 (Blue), B3 (Green), B4 (Red), B8 (Near Infrared) |
| **Derived index** | NDVI |
| **Classes** | Water, Vegetation, Urban, Bare soil / bright surface |

Pre-processing included:

- Image clipping to study area
- Band extraction
- NDVI calculation
- Sampling pixels for model training
- Preparation of annual raster composites

All preprocessing steps are contained in:

```text
01_preprocessing.ipynb
```

---

## 3 Method Overview

Both classification methods use the same Sentinel-2 inputs, but differ in how classes are assigned.

---

## 3.1 Unsupervised K-Means

K-Means is an unsupervised clustering method that groups pixels into land-cover classes based on spectral similarity, without labelled training data.

### Workflow
- Use annual Sentinel-2 composites (2020, 2022, 2024)
- Randomly sample pixel data and standardise features
- Run K-Means clustering (**k = 4**)
- Interpret clusters manually based on spectral and spatial patterns
- Generate annual land-cover maps for change analysis
<img width="2028" height="768" alt="屏幕截图 2026-05-23 193855" src="https://github.com/user-attachments/assets/05ebe198-e300-498d-a766-72161aa8081b" />
<p align="center"><em>Figure 2. K-Means workflow</em></p>

### Model settings
- Pixel samples standardised using **StandardScaler**
- **k = 4** selected using elbow and silhouette analysis

### Land-cover classes
- Water
- Vegetation
- Urban
- Bare soil / bright surface

> K-Means is useful for exploratory classification, but cluster interpretation requires manual judgement.

---

## 3.2 Supervised Random Forest

Random Forest is a supervised classifier that uses labelled pixel samples to classify land-cover into four classes.

### Workflow
- Use **9,384 labelled pixel samples** across 4 classes
- Split data into **80% training / 20% testing**
- Train a **300-tree Random Forest** classifier
- Predict land-cover classes and evaluate performance
- Apply the trained model to annual Sentinel-2 imagery (2020, 2022, 2024)
<img width="2326" height="638" alt="屏幕截图 2026-05-23 194532" src="https://github.com/user-attachments/assets/c318ceee-9fab-4df8-8ed3-f9e0863560cc" />
<p align="center"><em>Figure 2. Random-Forest workflow</em></p>

### Evaluation metrics
- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
- Cohen’s Kappa

### Feature importance
- **NDVI:** 0.284
- **B8 (NIR):** 0.282
- **B2 (Blue):** 0.161
- **B3 (Green):** 0.144
- **B4 (Red):** 0.129

### Performance

| Metric | Score |
|--------|-------|
| Accuracy | **0.87** |
| Cohen’s κ | **0.81** |

> Random Forest produced more reliable classification results, especially for water and vegetation classes.
---

## 4 Notebooks and Quick Start

| Notebook | Purpose |
|----------|---------|
| **01_preprocessing.ipynb** | Data preparation and feature generation |
| **02_unsupervised_kmeans.ipynb** | K-Means clustering and cluster interpretation |
| **03_supervised_randomforest.ipynb** | Random Forest training, evaluation and change detection |

### Quick Start

Open notebooks in **Google Colab** or Jupyter and run sequentially.

---

## 5 Results

---

### 5.1 K-means classifications (2020，2022，2024) Classification Maps
<p align="center">
  <img width="500" alt="kmeans_classification_perfect_stable" src="https://github.com/user-attachments/assets/20dee379-096d-4428-8e00-34557cdc311a" />
</p>

<p align="center"><em>Figure 4. Animated K-Means land-cover classification maps for 2020, 2022 and 2024.</em></p>




### 5.2 Random-Forest classifications (2020，2022，2024) Classification Maps
<p align="center">
  <img width="500" alt="rf_classification_perfect_stable" src="https://github.com/user-attachments/assets/aea6c7e3-0046-4d3e-a9be-13e9db9edf67" />
</p>

<p align="center"><em>Figure 5. Animated Random Forest land-cover classification maps for 2020, 2022 and 2024.</em></p>


---

### 5.3 Feature Importance

<p align="center">
  <img width="500" alt="feature_importance" src="https://github.com/user-attachments/assets/a59f466b-b19f-4bec-b4bd-551580654d8c" />
</p>

<p align="center"><em>Figure 6. Random Forest feature importance for spectral bands and NDVI.</em></p>

Random Forest feature importance results show:

| Feature | Importance |
|---------|-----------:|
| NDVI | 0.284 |
| B8 | 0.282 |
| B2 | 0.161 |
| B3 | 0.144 |
| B4 | 0.129 |

**Interpretation:**

- **NDVI** was the most important predictor
- **B8 (Near Infrared)** was almost equally important
- Visible bands contributed less

This suggests vegetation-related spectral information plays the strongest role in classification.

---

### 5.4 Land-Cover Area Comparison (2020 vs 2024)

| Class | 2020 (%) | 2024 (%) | Change (%) |
|-------|---------:|---------:|-----------:|
| Water | 4.73 | 4.86 | +0.12 |
| Vegetation | 61.34 | 64.39 | +3.05 |
| Urban | 33.73 | 30.62 | -3.11 |
| Bare soil / bright surface | 0.20 | 0.13 | -0.06 |

**Interpretation:**

- Vegetation increased
- Urban area decreased slightly
- Water remained stable
- Bare soil occupied a very small proportion

---

### 5.5 Change Detection (2020 → 2024)

Visual change maps highlight:

- **Urban gain** (red)
- **Vegetation loss** (yellow)

Random Forest change summary:

| Metric | Value |
|--------|------:|
| Urban gain pixels | 3957 |
| Urban gain (%) | 0.19 |
| Vegetation loss pixels | 26519 |
| Vegetation loss (%) | 1.29 |
<p align="center">
  <img alt="change_detection_comparison" src="https://github.com/user-attachments/assets/26fb9802-28c0-4d8c-8639-a197aa8f9722" />
</p>

<p align="center"><em>Figure 9. Land-cover change detection (2020-2024), highlighting urban gain and vegetation loss in Richmond upon Thames based on Random Forest classification results.</em></p>



K-Means change summary:

| Metric | Value |
|--------|------:|
| Urban gain pixels | 3055 |
| Urban gain (%) | 0.15 |
| Vegetation loss pixels | 24784 |
| Vegetation loss (%) | 1.20 |

<p align="center">
  <img alt="area_comparison" src="https://github.com/user-attachments/assets/c26b28ea-d700-4e21-b649-0cf96590f7a6" />
</p>

<p align="center"><em>Figure 8. Land-cover change detection (2020-2024), highlighting urban gain and vegetation loss in Richmond upon Thames based on K-Means classification results.</em></p>

> Random Forest detected slightly more subtle change than K-Means.
---

## 6 Comparison of Methods

| Aspect | K-Means | Random Forest |
|--------|--------|--------------|
| Type | Unsupervised | Supervised |
| Training labels | Not required | Required |
| Interpretation | Manual | Automatic |
| Accuracy metric | No direct metric | 0.87 |
| Cohen’s κ | N/A | 0.81 |
| Change sensitivity | Conservative | More sensitive |
| Reliability | Moderate | Higher |

### Interpretation

**K-Means**

Advantages:

- No training labels needed
- Good for exploratory analysis
- Can reveal spectral groupings

Limitations:

- Clusters require manual interpretation
- Less stable between years

**Random Forest**

Advantages:

- Higher reliability
- Strong classification performance
- Better quantitative evaluation

Limitations:

- Requires labelled data
- Depends on training sample quality

---

## 7 Key Findings

### Main conclusions

- **Random Forest outperformed K-Means** for land-cover classification reliability.
- **Water was mapped consistently** by both methods.
- **Vegetation was the dominant land-cover class** in Richmond upon Thames.
- **Urban area showed slight decline between 2020 and 2024**.
- **Random Forest detected more subtle land-cover change** than K-Means.
- **NDVI and Near Infrared (B8) were the most important classification features**.

### Final answer to project question

This project shows that:

> **Random Forest is the more reliable method for urban land-cover classification and change detection in Richmond upon Thames, while K-Means provides useful exploratory clustering but requires more subjective interpretation.**

---


## 8 References & Acknowledgements

This repository was developed as a final project for:

**UCL – Artificial Intelligence for Earth Observation**

### References

- Sentinel-2 Level-2A imagery (Copernicus)
- ESA WorldCover
- Scikit-learn documentation
- UCL AI4EO course materials

### Acknowledgements

Special thanks to:

- UCL teaching team
- AI for Earth Observation course materials
- Open-source geospatial and machine-learning libraries

---
