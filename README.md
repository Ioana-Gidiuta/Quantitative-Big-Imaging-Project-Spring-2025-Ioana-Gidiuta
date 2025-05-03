# Quantitative Big Imaging

**Project Proposal**  
**Ioana Gidiuta**  
**Interpretable Disease Profiling from Retinal Fundus Images Using Vascular Features and Patient Metadata**

**Drive Link for Pre-processed Datasets: [https://drive.google.com/drive/folders/1QHtnvaXpZlL2R88R4GOkHn1QGCaDfS7X?usp=drive_link](https://drive.google.com/drive/folders/1QHtnvaXpZlL2R88R4GOkHn1QGCaDfS7X?usp=drive_link)**
---

## 1. Introduction

Early detection of ocular and systemic diseases through retinal imaging is increasingly powered by AI. However, many models rely on black-box deep learning architectures that lack clinical interpretability. This project proposes a hybrid approach combining interpretable structural features—such as vascular morphology, optic disc and fovea characteristics, lens opacity signals, and patient age—for multi-label classification of diseases from fundus photographs.

---

## 2. Objectives

- Develop a working vessel segmentation supervised learning model (UNet) for high fidelity vessel recognition  
- Apply the model to a larger clinical dataset and compute relevant and clinically-interpretable features from fundus vascularization images  
- Compute population statistics and statistical relevance of different features for disease classification  

---

## 3. Future Steps

- Develop a reliable network for fovea and optic disk segmentation and extract position, dimension and symmetry metrics for both  
- Analyze texture patterns (entropy, blurring) in central fundus  
- Develop a clinically interpretable classification pipeline using all structural features extracted from fundus images  
- Perform multi-label disease classification covering diabetic retinopathy (DR), glaucoma, cataract, AMD, myopia and hypertension  
- Compare the performance of classical ML models (Random Forest, XGBoost, K-Means) against deep learning baselines (e.g., U-Net-based CNN)  

---

## 4. Datasets

### Primary Dataset: OIA-ODIR  
[https://github.com/nkicsl/OIA-ODIR](https://github.com/nkicsl/OIA-ODIR)  

- Real-world dataset with 5,000 patients, each with left and right eye fundus images  
- Labeled with one or more of eight disease classes: Normal (N), Diabetes (D), Glaucoma (G), Cataract (C), AMD (A), Hypertension (H), Myopia (M), and Other (O)  
- Includes patient age  
- Multi-label, varying image resolutions and quality due to different capture devices  

### Auxiliary Dataset: DRIVE  
[https://www.kaggle.com/datasets/andrewmvd/drive-digital-retinal-images-for-vessel-extraction](https://www.kaggle.com/datasets/andrewmvd/drive-digital-retinal-images-for-vessel-extraction)  

- Used for supervised vessel segmentation training  
- High-quality annotated fundus images with ground truth vessel maps  
- Purpose: Build a vessel segmentation module that generalizes to ODIR images  

---

## 5. Methodology

### 5.1 Preprocessing

- Apply gradient correction to address lighting inconsistencies  
- Standardize image resolution across datasets  
- Normalize illumination and enhance contrast (e.g., CLAHE or histogram equalization)  
- Center crop or pad to focus on the retinal area  
- For OIA-ODIR - extract the mask for each fundus image manually, apply the mask and further correct for edge effects  

### 5.2 Vessel Segmentation & Morphological Feature Extraction

- **Segmentation**: Train a U-Net on the DRIVE dataset and transfer to ODIR images  
- **Features Extracted**:  
  - Vessel density (global and regional)  
  - Tortuosity and curvature (implemented but not applied because of limited computational resources)  
  - Fractal dimension  
  - Branching angles  
  - Vessel width distribution  

### 5.3 Merging with Patient Annotation Data

- Construct a dataframe combining:  
  - Vascular features from segmentation  
  - Patient age, ID, and gender  

- Compute Mean and Standard Deviation (SD) for:  
  - Vessel density  
  - Quadrant vessel densities  
  - Fractal Dimension  
  - Mean width and standard deviation of width for vessels  
  - Patient Age  

- **Compute Statistical Significance (Pairwise Comparisons)**:  
  - Performed pairwise statistical tests between disease classes  
  - Used Welch's t-test (unequal variance) for each feature (e.g., vessel_density)  
  - Computed p-values for all disease pairs  
  - Applied FDR correction (Benjamini-Hochberg) to control false discoveries  
  - Annotated significance levels as:  
    - * = p < 0.05  
    - ** = p < 0.01  
    - *** = p < 0.001  

- **Visualized results in**:  
  - Bar plots (mean ± SD)  
  - Heatmaps of corrected p-values with star annotations  

### 5.4 Naive ML Learning Model for Disease Classification - Work in Progress

- Trained classical interpretable models:  
  - Random Forest  
  - XGBoost  
  - K-Means (for unsupervised exploration of disease clusters)  

- Partial results revealed the need for more developed networks, more relevant features, such as fovea, optic disk and density information, as proposed in section 3
