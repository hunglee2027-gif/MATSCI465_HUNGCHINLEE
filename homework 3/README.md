# STEM Project: Automated Nanoparticle Analysis Pipeline
**Dataset: DOPAD (100 TEM Images Subset)**

## 1. Project Overview
This project establishes a multi-stage analysis pipeline for nanoparticle characterization from Transmission Electron Microscopy (TEM) images. It integrates classical computer vision, unsupervised/supervised machine learning, and deep learning architectures to compare their efficacy in automated materials science analysis.

---

## 2. Technical Implementation

### Task 1: Classical Computer Vision
* **Image Processing**: Applied CLAHE (Contrast Limited Adaptive Histogram Equalization) for noise reduction and contrast enhancement.
* **Segmentation**: Used Otsu's thresholding followed by the **Watershed Algorithm** to separate overlapping particles.
* **Quantitative Analysis**: Extracted morphological features including area, equivalent diameter, and circularity into a structured CSV format.

### Task 2: Machine Learning (ML)
* **Unsupervised**: Utilized **Principal Component Analysis (PCA)** for dimensionality reduction and **K-Means Clustering** to identify particle sub-populations.
* **Supervised**: Developed and compared **SVM** and **Random Forest** models to classify particles based on geometric features.
* **Feature Selection**: Identified 'Area' and 'Equivalent Diameter' as the most critical features for classification via Random Forest importance scores.

### Task 3: Deep Learning (DL)
* **Data Augmentation**: Implemented a pipeline with 6 variants (rotation, shift, zoom, flip) to increase model generalization.
* **U-Net Architecture**: Trained a convolutional auto-encoder for semantic segmentation to achieve pixel-level mask prediction.
* **CNN Analysis**: Visualized internal **Feature Maps** to understand the high-level representation of nanoparticle edges.

---

## 3. Method Comparison Summary
| Method | Accuracy/F1 | Runtime (ms) | Interpretability | Automation |
| :--- | :---: | :---: | :---: | :---: |
| Watershed (Classical) | 0.88 | ~50 | Very High | Low |
| SVM (ML) | 0.99 | ~150 | Medium | Medium |
| **Random Forest (ML)** | **1.00** | **~100** | **Medium-High** | **Medium** |
| U-Net (DL) | 0.91 | ~500 | Low | Very High |

---

## 4. Key Deliverables
* **`classical_results.csv`**: Full morphological measurement data.
* **`ml_results.csv`**: Machine learning classification and clustering labels.
* **`final_summary_3x3.png`**: A comprehensive 3x3 visualization panel including all task results.
* **`method_comparison.csv`**: Comparative metrics table for all algorithms used.

---

## 5. Conclusion
While Classical methods (Watershed) are highly interpretable for size distribution analysis, the **Random Forest** model provided the most robust classification. **U-Net** showed superior potential for fully automated segmentation without the need for manual parameter tuning.
