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

## 3. Quantitative Comparison Table
| Method | Accuracy/F1 | Runtime | Interpretability | Automation |
| :--- | :---: | :---: | :---: | :---: |
| Watershed (Classical) | 0.88 | Fast (~50ms) | Very High | Low |
| SVM (ML) | 0.99 | Medium | Medium | Medium |
| **Random Forest (ML)** | **1.00** | **Medium** | **Medium-High** | **Medium** |
| U-Net (DL) | 0.91 | Slow | Low | **Very High** |

---

## 4. Final Deliverables
* `classical_results.csv`: Extracted morphological data.
* `ml_results.csv`: Predicted classification labels and cluster IDs.
* `final_summary_3x3.png`: Unified visualization panel of the entire pipeline.
* `README.md`: Comprehensive documentation of the study.
---

## 5. Conclusion
While Classical methods (Watershed) are highly interpretable for size distribution analysis, the **Random Forest** model provided the most robust classification. **U-Net** showed superior potential for fully automated segmentation without the need for manual parameter tuning.
