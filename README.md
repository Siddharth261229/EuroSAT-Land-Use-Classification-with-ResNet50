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

