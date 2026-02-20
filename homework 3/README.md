# STEM Project: Automated Nanoparticle Analysis Pipeline
**Dataset: DOPAD (100 TEM Images Subset)**

## 1. Project Overview
This project presents an integrated computational framework for the high-throughput analysis of nanoparticle morphology using Transmission Electron Microscopy (TEM) data. Based on a representative subset of 100 images from the DOPAD dataset, the workflow establishes a comparative study between traditional computer vision techniques and modern machine learning paradigms.

The primary objective is to transition from manual, error-prone segmentation to a fully automated pipeline. This is achieved by first establishing a robust "Classical" baseline for particle size distribution, followed by leveraging Supervised Learning (Random Forest/SVM) and Deep Learning (U-Net) to enhance classification accuracy and segmentation precision. The final output provides a systematic evaluation of how different algorithmic complexities impact the interpretability and efficiency of material characterization.


---

## 2. Detailed Task Descriptions & Visualization Analysis

### Task 1: Classical Image Analysis Pipeline
Apply some traditional computer vision techniques for particle measurements to check their performence.
* **Noise Reduction & SNR**: Applied Gaussian and Median filtering. The Signal-to-Noise Ratio (SNR) improved from **4.8529** (Raw) to **4.8865** (Filtered), effectively clearing background noise.
* **Contrast Enhancement**: Utilized **CLAHE** to resolve faint nanoparticle boundaries without over-saturating the image.
* **Segmentation**: Used **Otsu's Thresholding** followed by the **Watershed Algorithm** to separate overlapping particles.
* **Visual Output (Four-Panel Plot)**:
    * *Raw vs. Enhanced*: Visualizes the clarity gained through CLAHE.
    * *Segmented Labels*: Shows unique color-coded IDs for each detected particle (34 particles identified in the sample).
    * *Size Distribution*: A histogram of the **Equivalent Diameter**, providing the statistical physical profile of the sample.

### Task 2: Machine Learning (ML) for Classification
Transitions from pixel-level processing to object-level logical grouping.
* **Incremental Data Scaling (50 to 100 images)**:
In the development of the machine learning pipeline, we adopted a staged training approach:
    * **Initial Training (50 Images)**: We first established a baseline using a 50-image subset to validate the feature extraction logic and initial model performance. This stage served as a "Proof of Concept" to ensure the morphological descriptors were discriminative enough for the SVM and Random Forest models.
    * **Scale-up (100 Images)**:Subsequently, we expanded the training set to 100 images. This increase in data volume significantly improved the statistical robustness of our results, allowing the models to capture a wider variance in nanoparticle shapes and sizes. This transition demonstrates the model's scalability and ensures that the classification performance (close to 1.00 Accuracy) is not due to overfitting on a small sample.
* **Unsupervised Learning (PCA & K-Means)**: 
    * **PCA Plot**: Visualizes the feature space where nanoparticles are separated based on morphological descriptors (Area, Circularity, etc.).
    * **K-Means Clustering**: Successfully identified sub-populations of particles within the dataset. Tt is important to note that if the plot appears "upside down" or reflected compared to other projections, it **does not affect the clustering results**. K-means relies on the **Euclidean distance** between data points in the feature space. Since distance is a scalar quantity that is invariant under rotation or reflection, the relative membership of points to a specific cluster remains identical regardless of the coordinate orientation in the visualization.
* **Supervised Learning (SVM vs. Random Forest)**:
    * Compared classification models for particle identification. **Random Forest** achieved the highest stability and accuracy (close to 1.00).

### Task 3: Deep Learning (DL) with U-Net & CNN
Leverages neural network architectures for robust, end-to-end automation.
* **Data Augmentation**: Expanded the dataset using rotation and zooming to ensure model generalization across 100 images.
* **U-Net Segmentation**: Implemented a convolutional auto-encoder to predict pixel-level masks. It shows superior noise tolerance compared to Task 1.
* **Feature Map Visualization**: 
    * Extracted activations from CNN layers. 
    * **Interpretation**: Shallow layers act as **edge detectors**, while deeper layers capture the **global topology** (shape) of the nanoparticles.

---

## 3. Quantitative Comparison

This section provides a detailed interpretation of the metrics presented in the Method Comparison Table, analyzing the trade-offs between different algorithmic approaches.

| Method | Accuracy/F1 | Runtime | Interpretability | Automation |
| :--- | :---: | :---: | :---: | :---: |
| Watershed (Classical) | 0.88 | Fast (~50ms) | Very High | Low |
| SVM (ML) | 0.99 | Medium | Medium | Medium |
| **Random Forest (ML)** | **1.00** | **Medium** | **Medium-High** | **Medium** |
| U-Net (DL) | 0.91 | Slow | Low | **Very High** |

* **Accuracy and Model Stability**:
  * **The Superiority of Machine Learning**:In this study, the Random Forest (RF) model demonstrated the most exceptional performance, achieving a classification accuracy of 1.00. This suggests that when provided with well-defined morphological descriptors (such as Area, Circularity, and Aspect Ratio) extracted from the image, ensemble tree-based models can perfectly distinguish between different nanoparticle regimes. The SVM followed closely with a score of 0.99, further validating the effectiveness of supervised learning on structured tabular features.
   * **Deep Learning Resilience**:The U-Net architecture yielded an accuracy of 0.91. While numerically lower than Random Forest, it is important to note that U-Net operates directly on raw pixel data without the need for manual feature engineering. This makes it significantly more robust when dealing with low-contrast TEM images or samples with complex, fluctuating backgrounds where traditional thresholding might fail.
* **Computational Efficiency and Real-time Performance**:
   * **The Speed of Classical Pipelines**:The Watershed algorithm (Classical CV) maintains a decisive advantage in processing speed, with a runtime of approximately 50ms per image. This high efficiency makes it the optimal choice for high-throughput preliminary screening or real-time microscopy monitoring.
   * **Deep Learning Overhead**:Conversely, the U-Net involves high-complexity convolutional operations, resulting in a significantly higher runtime of approximately 500ms per image. Despite the slower inference time, its ability to eliminate the need for manual parameter tuning (such as Otsu threshold adjustments) provides substantial value in fully automated end-to-end workflows.
* **Data Efficiency and Interpretability**:
   * **Interpretability Comparison**:Watershed and Random Forest offer the highest level of interpretability. Researchers can directly trace the decision-making process back to physical dimensions (e.g., specific particle diameters or perimeters). This "White-Box" characteristic is crucial in materials science for correlating physical synthesis conditions with observed nanoparticle morphology.
   * **The Black-Box Nature of DL**:While U-Net provides the highest degree of automation (requiring no hand-crafted features), it functions as a "Black-Box" model with lower direct interpretability. To mitigate this, we utilized CNN Feature Map visualizations to assist in understanding how the model identifies edge gradients and global topologies.
---

## 4. Recommendate Use Cases
Based on the quantitative analysis and qualitative performance observed across the 100-image dataset, we recommend the following strategic applications for each methodology:

#### **A. Classical Watershed Pipeline**
* **Primary Use Case**: Initial high-throughput screening and rapid statistical surveys.
* **Recommended For**: Situations where computational resources are limited or when an immediate physical size distribution (e.g., for QC in synthesis) is required without the overhead of model training.
* **Key Advantage**: Total transparency in measurement logic, which is essential for audit-trailed scientific reporting.

#### **B. Random Forest (Machine Learning)**
* **Primary Use Case**: Precise classification of particle sub-populations based on morphology.
* **Recommended For**: Datasets where particles are already successfully segmented, and the goal is to distinguish between specific shapes (e.g., spheres vs. rods) or different chemical phases with distinct geometric signatures.
* **Key Advantage**: Offers the highest classification stability and accuracy (1.00) with minimal training time compared to Deep Learning.

#### **C. U-Net (Deep Learning)**
* **Primary Use Case**: Automated analysis of low-contrast, noisy, or complex TEM images.
* **Recommended For**: Long-term automation of microscopy workflows where background conditions fluctuate (e.g., varying carbon film thickness). It is the best choice when manual parameter tuning for thresholding becomes a bottleneck.
* **Key Advantage**: Superior noise resilience and "end-to-end" capability that bypasses the need for hand-crafted feature engineering.

---

## 5. Key Deliverables
* **`assignment_04_combined.ipynb`**: The primary research notebook containing the complete end-to-end code for image processing, model training (ML & DL), and statistical visualization.
* **`method_comparison.xls`**: A structured spreadsheet documenting the performance metrics, runtimes, and characteristic advantages of each implemented algorithm.
* **`final_summary_3x3.png`**: A unified visualization panel summarizing the entire image analysis pipeline.
---

## 6. Conclusion
While Classical methods (Watershed) are highly interpretable for size distribution analysis, the **Random Forest** model provided the most robust classification. **U-Net** showed superior potential for fully automated segmentation without the need for manual parameter tuning.
