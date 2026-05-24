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

1. [Project Motivation and Background](#project-motivation-and-background)
2. [Data Source and Pre-processing](#data-source-and-pre-processing)
3. [Method Overview](#method-overview)
  - [3.1 Unsupervised K-Means](#31-unsupervised-k-means)
  - [3.2 Supervised Random Forest](#32-supervised-random-forest)
4. [Notebooks and Quick Start](#notebooks-and-quick-start)
5. [Results](#results)
6. [Comparison of Methods](#comparison-of-methods)
7. [Key Findings](#key-findings)
8. [Environmental Cost](#environmental-cost)
9. [References & Acknowledgements](#references--acknowledgements)

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

## 2. Data Source and Pre-processing

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

## 3. Method Overview

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



### Feature Importance

- **B8 (NIR): 0.287**
- **NDVI: 0.283**
- **B2 (Blue): 0.161**
- **B3 (Green): 0.144**
- **B4 (Red): 0.125**

### Performance

| Metric | Score |
|--------|-------|
| Accuracy | **0.87** |
| Cohen’s κ | **0.81** |

> Random Forest produced more reliable classification results, especially for water and vegetation classes.
---

## 4. Notebooks and Quick Start

| Notebook | Purpose |
|----------|---------|
| **01_preprocessing.ipynb** | Data preparation and feature generation |
| **02_unsupervised_kmeans.ipynb** | K-Means clustering and cluster interpretation |
| **03_supervised_randomforest.ipynb** | Random Forest training, evaluation and change detection |

### Quick Start

Open notebooks in **Google Colab** or Jupyter and run sequentially.

---

## 5. Results

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
| B8 | 0.287 |
| NDVI | 0.283 |
| B2 | 0.161 |
| B3 | 0.144 |
| B4 | 0.125 |


**Interpretation:**

B8 (Near Infrared) and NDVI were the two most important predictors, indicating that vegetation-related spectral information played the strongest role in land-cover classification. Visible bands contributed less, with B4 having the lowest importance.

---

### 5.4 Land-Cover Area Comparison (2020, 2022 and 2024)


| Class | 2020 (%) | 2022 (%) | 2024 (%) | Change 2020–2022 (%) | Change 2022–2024 (%) | Change 2020–2024 (%) |
|-------|---------:|---------:|---------:|---------------------:|---------------------:|---------------------:|
| Water | 4.71 | 3.90 | 4.86 | -0.81 | +0.96 | +0.16 |
| Vegetation | 61.29 | 63.09 | 64.28 | +1.79 | +1.20 | +2.99 |
| Urban | 33.81 | 32.86 | 30.72 | -0.95 | -2.13 | -3.08 |
| Bare soil / bright surface | 0.19 | 0.16 | 0.13 | -0.04 | -0.03 | -0.07 |

Vegetation increased from **61.29%** in 2020 to **64.28%** in 2024, while urban area decreased from **33.81%** to **30.72%**. Water remained relatively stable overall, and bare soil / bright surface occupied only a very small proportion of the study area.

---

### 5.5 Change Detection (2020 → 2024)

Visual change maps highlight:

- **Urban gain** (red)
- **Vegetation loss** (yellow)

Random Forest change summary:

| Period | Urban gain pixels | Urban gain (%) | Vegetation loss pixels | Vegetation loss (%) |
|--------|------------------:|---------------:|-----------------------:|--------------------:|
| 2020 → 2022 | 32,717 | 3.54 | 1,597 | 0.17 |
| 2022 → 2024 | 20,912 | 2.26 | 7,051 | 0.76 |
| 2020 → 2024 | 24,864 | 2.69 | 5,341 | 0.58 |

<p align="center">
  <img alt="change_detection_comparison" src="https://github.com/user-attachments/assets/26fb9802-28c0-4d8c-8639-a197aa8f9722" />
</p>

<p align="center"><em>Figure 9. Land-cover change detection (2020-2024), highlighting urban gain and vegetation loss in Richmond upon Thames based on Random Forest classification results.</em></p>



K-Means change summary:

| Period | Urban gain pixels | Urban gain (%) | Vegetation loss pixels | Vegetation loss (%) |
|--------|------------------:|---------------:|-----------------------:|--------------------:|
| 2020 → 2022 | 2,587 | 0.28 | 74,956 | 8.11 |
| 2022 → 2024 | 3,692 | 0.40 | 9,708 | 1.05 |
| 2020 → 2024 | 4,101 | 0.44 | 23,738 | 2.57 |

<p align="center">
  <img alt="area_comparison" src="https://github.com/user-attachments/assets/c26b28ea-d700-4e21-b649-0cf96590f7a6" />
</p>

<p align="center"><em>Figure 8. Land-cover change detection (2020-2024), highlighting urban gain and vegetation loss in Richmond upon Thames based on K-Means classification results.</em></p>

> The results show that Random Forest detected more urban gain than K-Means, while K-Means detected substantially higher vegetation loss in the 2020 → 2022 period. This difference reflects the greater sensitivity of Random Forest to built-up change and the stronger dependence of K-Means on cluster interpretation.
---


## 6. Comparison of Methods

Random Forest Classification Accuracy

The Random Forest model was trained and validated using labelled samples derived from **ESA WorldCover 2021**, then applied to Sentinel-2 imagery from **2020, 2022 and 2024** for annual land-cover classification and change detection.

The model was evaluated using a held-out **20% test set** from the 2021 labelled dataset.

- Accuracy: **0.87**
- Cohen’s κ: **0.81**

<p align="center">
  <img width="500" alt="environmental_cost_summary" src="https://github.com/user-attachments/assets/49e91533-55b9-4917-83d3-1822d01f1411" />
</p>

<p align="center"><em>Figure 4. Confusion matrix showing Random Forest classification performance on the held-out 2021 test set.</em></p>

> The confusion matrix shows strong classification performance across all four land-cover classes, with most predictions concentrated along the diagonal, indicating correct classification.


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

## 7. Key Findings

### Main conclusions

- **Random Forest outperformed K-Means** in land-cover classification reliability and produced more accurate class predictions.
- **Water was mapped consistently** by both methods across all years.
- **Vegetation was the dominant land-cover class** in Richmond upon Thames throughout the study period.
- **Urban land cover showed a noticeable decline** between 2020 and 2024 (−3.08%), while vegetation increased (+2.99%).
- **Random Forest was more sensitive to urban gain detection**, whereas K-Means identified greater vegetation loss in some periods.
- **B8 (Near Infrared) and NDVI were the most important classification features**, indicating that vegetation-related spectral information played the strongest role in classification.



This project shows that:

**Random Forest is the more reliable method for urban land-cover classification and change detection in Richmond upon Thames, while K-Means provides useful exploratory clustering but requires more manual interpretation and is more sensitive to cluster definition.**


## 8. Environmental Cost

This project estimates the computational environmental cost of running the three notebooks using runtime, assumed CPU power, and UK grid carbon intensity.

| Stage | Runtime (s) | Runtime (hrs) | Energy (kWh) | CO₂e (g) | Cost (£) |
|---|---:|---:|---:|---:|---:|
| Preprocessing | 318.61 | 0.0885 | 0.001770 | 0.412 | 0.0005 |
| K-Means classification | 13.19 | 0.0037 | 0.000073 | 0.017 | 0.0000 |
| Random Forest classification | 86.21 | 0.0239 | 0.000479 | 0.112 | 0.0001 |
| **Total** | **418.01** | **0.1161** | **0.002322** | **0.541** | **0.0006** |

**Assumptions:** CPU power = 20 W, carbon intensity = 0.233 kg CO₂e/kWh, electricity cost = £0.30/kWh.

The total estimated computational footprint of the project was approximately **0.541 g CO₂e**, indicating that the workflow was lightweight and had a very small environmental cost. The preprocessing notebook contributed the largest share because it had the longest runtime, while K-Means was the least computationally expensive stage.



## 9. References & Acknowledgements

This repository was developed as a final project for:

**UCL – Artificial Intelligence for Earth Observation**

### References

1. Breiman, L. (2001). *Random Forests*. Machine Learning, 45(1), 5–32. https://doi.org/10.1023/A:1010933404324

2. Zhang, T., Su, J., Xu, Z., Luo, Y., & Li, J. (2021). *Sentinel-2 satellite imagery for urban land cover classification by optimized Random Forest classifier*. Applied Sciences, 11(2), 543. https://doi.org/10.3390/app11020543

3. Drusch, M., Del Bello, U., Carlier, S., et al. (2012). *Sentinel-2: ESA’s optical high-resolution mission for GMES operational services*. Remote Sensing of Environment, 120, 25–36. https://doi.org/10.1016/j.rse.2011.11.026

4. Rouse, J. W., Haas, R. H., Schell, J. A., & Deering, D. W. (1974). *Monitoring vegetation systems in the Great Plains with ERTS*. NASA Special Publication, 351, 309–317. (NDVI original paper)  https://ntrs.nasa.gov/api/citations/19740022614/downloads/19740022614.pdf

5. Abdi, A. M. (2020). *Land cover and land use classification performance of machine learning algorithms in a boreal landscape using Sentinel-2 data*. GIScience & Remote Sensing, 57(1), 1–20. https://doi.org/10.1080/15481603.2019.1650447

6. Pelc-Mieczkowska, R. (2021). *The application of Sentinel-2 data for automatic forest cover changes assessment*. Civil and Environmental Engineering Reports, 31(4), 148–166. https://doi.org/10.2478/ceer-2021-0054

7. Foody, G. M. (2002). *Status of land cover classification accuracy assessment*. Remote Sensing of Environment, 80(1), 185–201.https://doi.org/10.1016/S0034-4257(01)00295-4

8. Tempa, K., et al. (2024). *Utilizing Sentinel-2 satellite imagery for land use/land cover and NDVI change analysis*. Applied Sciences, 14(4), 1578.  https://www.mdpi.com/2076-3417/14/4/1578

### Acknowledgements

Special thanks to:

- UCL teaching team
- AI for Earth Observation course materials
- Open-source geospatial and machine-learning libraries

---
