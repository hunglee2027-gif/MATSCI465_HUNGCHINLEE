# 4D-STEM Analysis Pipeline: Si/SiGe Heterostructure Characterization

This project implements an automated analytical pipeline for 4D-STEM (Four-Dimensional Scanning Transmission Electron Microscopy) datasets. The primary focus is on characterizing the interface of a Si/SiGe heterostructure through virtual imaging and diffraction statistical analysis.

## Task 1: Distinguish Navigation vs. Signal Axes
In this work, we generate the Diffraction Pattern (DP) and identify the **Navigation Axes** as (77, 17) and the **Signal Axes** as (480, 448).


## Task 2: Diffraction Analysis Function
We first sum all pixels in a diffraction pattern to monitor sample thickness and beam stability. Afterward, we calculate the ($\bar{x}, \bar{y}$) coordinates of the electron distribution to determine the **Center of Mass (CoM)**. 



Furthermore, we plot a line profile starting from the CoM to examine surrounding peaks. Our analysis shows the nearest peak is located at approximately **2.3 Å**.


## Task 3: Build the Analysis Pipeline
In this task, we define masks for **Bright Field (BF)** and **Annular Dark Field (ADF)** detectors:
* **Bright Field (BF)**: Positioned at the center of the diffraction pattern (0-20 pixel radius) to capture the transmitted beam. It highlights diffraction contrast and crystal defects, showing the structural layout of the sample.
* **Annular Dark Field (ADF)**: Positioned in an annular region (50-150 pixels) to collect high-angle incoherent scattered electrons. The resulting virtual ADF image indicates the chemical layout of the sample.

Apart from virtual images, the pipeline also generates three diagnostic maps to ensure data quality and reveal hidden physical properties:

* **Total Scattered Intensity**: This map sums all pixels in each diffraction pattern. It is used to monitor **sample thickness variations** and the **stability of the electron beam** during the scan.
* **CoM X & Y Positions**: These maps record the horizontal and vertical shifts of the diffraction pattern's center of mass.
    * **Scan Drift**: Gradual color changes help identify thermal or mechanical drift of the sample.
    * **Internal Fields**: Rapid shifts at the Si/SiGe interface indicate **internal electric fields or local strain** that deflect the electron beam, which is the fundamental principle of **Differential Phase Contrast (DPC)** imaging.


## Task 4: Apply to Si/SiGe Dataset
To identify the boundary of the Si/SiGe heterostructure, we analyze the line profile of the **Virtual ADF (VADF)** image. VADF provides superior **Atomic Number Contrast (Z-Contrast)**. 

Since Germanium ($Z = 32$) scatters electrons more strongly than Silicon ($Z = 14$), the SiGe layer appears significantly brighter than the Si substrate. By plotting a line profile across the interface, we extract data where the transition slope quantifies the **interface sharpness** and atomic diffusion.
