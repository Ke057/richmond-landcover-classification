![banner](figures/rf_classification_maps.png)

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

1. [Project Motivation and Background](#1--project-motivation-and-background)  
2. [Data Source and Pre-processing](#2--data-source-and-pre-processing)  
3. [Method Overview](#3--method-overview)  
   * [3.1 Unsupervised K-Means](#31--unsupervised-k-means)  
   * [3.2 Supervised Random Forest](#32--supervised-random-forest)  
4. [Notebooks and Quick Start](#4--notebooks-and-quick-start)  
5. [Results](#5--results)  
6. [Comparison of Methods](#6--comparison-of-methods)  
7. [Key Findings](#7--key-findings)
8. [Environmental Cost](#8--Environmental-cost)  
9. [References & Acknowledgements](#8--references--acknowledgements)

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

### 3.1 Unsupervised K-Means

K-Means clustering groups pixels according to spectral similarity without using labelled training data.

Workflow:

- Sample Sentinel-2 pixels
- Standardise spectral features
- Run K-Means clustering (`k = 4`)
- Interpret clusters using spectral signatures
- Assign land-cover meaning manually

Land-cover labels were interpreted as:

- Water
- Vegetation
- Urban
- Bare soil / bright surface

K-Means is useful for exploratory classification, but cluster interpretation requires manual judgement.

---

### 3.2 Supervised Random Forest

Random Forest uses labelled training data to classify land-cover types.

Workflow:

- Use labelled training samples
- Train Random Forest classifier
- Predict land-cover classes
- Evaluate performance on test set
- Apply model to annual imagery

Evaluation metrics:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
- Cohen’s Kappa

### Random Forest Performance

| Metric | Score |
|--------|------:|
| **Accuracy** | **0.87** |
| **Cohen’s κ** | **0.81** |

Random Forest produced strong classification performance, especially for water and vegetation classes.

---

## 4 Notebooks and Quick Start

| Notebook | Purpose |
|----------|---------|
| **01_preprocessing.ipynb** | Data preparation and feature generation |
| **02_unsupervised_kmeans.ipynb** | K-Means clustering and cluster interpretation |
| **03_supervised_randomforest.ipynb** | Random Forest training, evaluation and change detection |

### Quick Start

Clone the repository:

```bash
git clone https://github.com/Ke057/richmond-landcover-classification.git
```

Open notebooks in **Google Colab** or Jupyter and run sequentially.

---

## 5 Results

---

### 5.1 Random Forest Classification Maps

```text
figures/rf_classification_maps.png
```

Random Forest classification maps were generated for:

- 2020
- 2022
- 2024

Land-cover classes:

- Water
- Vegetation
- Urban
- Bare soil / bright surface

These maps show the spatial distribution of land cover across Richmond upon Thames.

---

### 5.2 Feature Importance

```text
figures/feature_importance.png
```

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

### 5.3 Land-Cover Area Comparison (2020 vs 2024)

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

### 5.4 Change Detection (2020 → 2024)

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

K-Means change summary:

| Metric | Value |
|--------|------:|
| Urban gain pixels | 3055 |
| Urban gain (%) | 0.15 |
| Vegetation loss pixels | 24784 |
| Vegetation loss (%) | 1.20 |

Random Forest detected slightly more subtle change than K-Means.

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

## Repository Structure

```text
richmond-landcover-classification/
│── 01_preprocessing.ipynb
│── 02_unsupervised_kmeans.ipynb
│── 03_supervised_randomforest.ipynb
│── figures/
│── README.md
```

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
