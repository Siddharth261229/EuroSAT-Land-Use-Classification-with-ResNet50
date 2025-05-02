# EuroSAT-Land-Use-Classification-with-ResNet50
This project focuses on classifying land use and land cover (LULC) using the [EuroSAT](https://github.com/phelber/eurosat) dataset, leveraging **Transfer Learning with ResNet50** and a **Custom Convolutional Neural Network (CNN)**.

EuroSAT is a publicly available satellite image dataset for land-use/land-cover (LULC) classification. It contains 27,000 Sentinel-2 image patches across 10 land-cover classes
arxiv.org
. Classes include AnnualCrop, Forest, HerbaceousVegetation, Highway, Industrial, Pasture, PermanentCrop, Residential, River and SeaLake
arxiv.org
github.com
. Each patch is a 64×64 pixel cutout (with 10 m ground resolution)
arxiv.org
github.com
. EuroSAT images come in two versions: an RGB subset (3-band composite) and a 13-band multispectral version
tensorflow.org
. Most implementations focus on the RGB data (JPEG) for ease of use
tensorflow.org
. The dataset is georeferenced and drawn from 34 European countries, covering varied terrain and seasons

## 📁 Dataset: EuroSAT (RGB)

- **Source**: Sentinel-2 satellite imagery
- **Images**: 27,000 RGB image patches
- **Size**: 64×64 pixels
- **Classes**: 10
    - AnnualCrop
    - Forest
    - HerbaceousVegetation
    - Highway
    - Industrial
    - Pasture
    - PermanentCrop
    - Residential
    - River
    - SeaLake

---

## 🧠 Model Architecture

- **Backbone**: ResNet50 (frozen initially)
- **Head**:
  - Global Average Pooling
  - Dropout (0.5)
  - Dense layer with softmax (10 outputs)

## 📊 Model Performance Interpretation

### 1. Accuracy and Loss over Epochs

#### 📈 Accuracy Plot

- **Blue Line (Train Accuracy)**:  
  Shows a consistent upward trend, indicating effective learning on the training set.  
  ✅ Reaches above 90%.

- **Orange Line (Test Accuracy)**:  
  Fluctuates sharply between epochs.  
  ❌ Ranges from 20% to 90% – indicating **instability** and **poor generalization**.

**🔍 Interpretation**:
- Likely **overfitting**: model memorizes training data but fails on validation set.
- Potential **data imbalance** or **validation leakage**.
- Could be influenced by **small batch stats** (e.g., BatchNorm instability).

#### 📉 Loss Plot

- **Blue Line (Train Loss)**:  
  Smooth and decreasing – model minimizing error on training set.

- **Orange Line (Test Loss)**:  
  Highly erratic with extreme spikes (some >100).

**🔍 Interpretation**:
- Severe **numerical instability** or **exploding gradients**.
- Mismatch in batch size or class presence during validation.
- Model sensitive to underrepresented or similar-looking classes.

---

### 2. 🔢 Confusion Matrix Analysis

This confusion matrix illustrates model predictions versus true labels for the 10 land cover classes.

| **Class**             | **Key Observations**                                                                 |
|------------------------|--------------------------------------------------------------------------------------|
| **AnnualCrop**         | Confused with `HerbaceousVegetation` (171) and `Pasture` (126).                     |
| **Forest**             | Almost perfect predictions (1061 correct).                                          |
| **HerbaceousVegetation** | Minor confusion with `AnnualCrop` and `Pasture`.                                |
| **Highway**            | Strongly confused with `Industrial` (117) – structural similarity.                  |
| **Industrial**         | Well predicted (890 correct), some mix-up with `Residential`.                       |
| **Pasture**            | Confusion with `HerbaceousVegetation` (44) and `PermanentCrop` (75).                |
| **PermanentCrop**      | Confused with `Pasture` (121) and `HerbaceousVegetation` (327).                     |
| **Residential**        | Excellent performance (1048 correct).                                               |
| **River**              | Confused with `HerbaceousVegetation` (44) and `Highway` (90).                       |
| **SeaLake**            | Very high accuracy (1032 correct).                                                  |

#### ✅ Summary

- **Strong Classes**: `Forest`, `Residential`, `SeaLake`
- **Confused Classes**: `AnnualCrop`, `Pasture`, `PermanentCrop`, `HerbaceousVegetation`
- Likely causes:
  - **Spectral similarity** in RGB bands
  - **Class imbalance**
  - **Natural semantic overlap** between vegetation types
