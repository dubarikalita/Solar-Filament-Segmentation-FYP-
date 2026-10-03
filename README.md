# 🌞 Deep Learning-Based Detection and Instance Segmentation of Solar Filaments in H-Alpha Images

## 🎯 Problem Statement

Solar filaments are elongated, thread-like structures present in the Sun's atmosphere. They can be observed in H-alpha solar images, where they generally appear as relatively dark structures against the brighter solar background.

The problem addressed in this project is the **automatic detection and segmentation of solar filaments from H-alpha solar images**.

Given an H-alpha image of the Sun, the system should identify the pixels that belong to solar filaments and separate them from the background.

In simple terms:

> **Input:** H-alpha solar image  
> **Output:** Segmentation mask showing the locations of solar filaments

This is a **pixel-level image segmentation problem**, because the objective is not only to determine whether a filament is present in an image, but to identify the exact pixels corresponding to each filament.

The project also aims to progress beyond basic segmentation toward **instance-level separation and structural analysis of individual solar filaments**.

---

## 🎯 Project Objective

The main objective of this project is to develop an automated and reliable pipeline for detecting and segmenting solar filaments in H-alpha images using computer vision and deep-learning techniques.

The project objectives are:

1. **Understand the solar filament segmentation problem** and study the characteristics of H-alpha solar images.

2. **Analyse the available dataset and annotations** to understand the number, size, shape, and structural properties of solar filament instances.

3. **Generate accurate ground-truth segmentation masks** from the provided filament annotations.

4. **Develop a classical image-processing baseline** using techniques such as intensity thresholding, solar-disk masking, and morphological processing.

5. **Evaluate the baseline segmentation performance** using quantitative metrics such as Dice Score and Intersection over Union (IoU).

6. **Develop a supervised deep-learning-based segmentation model**, beginning with a U-Net architecture, to improve filament segmentation.

7. **Compare the deep-learning approach with the classical baseline** using the same ground-truth data and evaluation metrics.

8. **Progress toward instance-level segmentation**, where individual filament structures can be separated from one another.

9. **Analyse the structural properties of detected filaments**, such as their length, orientation, shape, and spine characteristics.

10. **Develop a complete pipeline** that can eventually support further analysis of solar filament structures and their evolution.

# ✅ Work Completed So Far

The first phase of the project focused on understanding the dataset, analysing the filament annotations, generating ground-truth masks, and establishing a classical segmentation baseline.

The following work has been completed so far:

## 1. Dataset Structure Analysis

We first studied the structure of the provided annotation dataset and understood how the image information and filament annotations are organized.

The dataset analysis showed:

- **707 unique image files**
- **1154 annotated image entries**
- **8199 annotated filament instances**
- Image resolution: **2048 × 2048 pixels**
- Images used in the current analysis: **8-bit grayscale JPEG**

Some image filenames occur in multiple annotation entries with different `image_id` values. Therefore, `image_id` was used as the annotation-level identifier, while `file_name` represents the underlying image file.

---

## 2. Filament Instance Analysis

We analysed how many filament instances are present in each annotated image entry.

The observed distribution was:

| Property | Value |
|---|---:|
| Minimum filaments | 1 |
| Maximum filaments | 26 |
| Mean filaments | ≈ 7.10 |
| Median filaments | 7 |

This showed that the number of filament instances varies considerably between image entries.

Therefore, the segmentation system needs to handle images containing both a small number of filaments and images containing multiple filament structures.

---

## 3. Filament Area Analysis

The area of each annotated filament was analysed to understand the variation in filament size.

The observed values were:

| Property | Value |
|---|---:|
| Minimum area | 9 pixels |
| Median area | ≈ 1228 pixels |
| Mean area | ≈ 2119.83 pixels |
| Maximum area | 37739 pixels |

This analysis showed that the dataset contains filament structures with significantly different sizes.

---

## 4. Filament Category Analysis

The annotation categories were also analysed.

| Category | Number of Annotations |
|---|---:|
| Left | 2535 |
| Right | 2590 |
| Unidentifiable | 3074 |
| Ambiguous | 0 |

This helped us understand the distribution of the different annotation categories present in the dataset.

---

## 5. Bounding-Box Analysis

The bounding-box dimensions of the filament instances were analysed.

### Bounding-Box Width

- Minimum: ≈ 3.82 pixels
- Median: ≈ 74.52 pixels
- Mean: ≈ 103.22 pixels
- Maximum: ≈ 1067.31 pixels

### Bounding-Box Height

- Minimum: ≈ 3.47 pixels
- Median: ≈ 69.61 pixels
- Mean: ≈ 91.79 pixels
- Maximum: ≈ 709.81 pixels

The large variation in bounding-box dimensions indicates that the annotated filament structures have substantially different spatial sizes.

---

## 6. Spine Length Analysis

The provided filament spine information was also analysed.

The observed spine lengths were:

| Property | Value |
|---|---:|
| Minimum | ≈ 12.07 pixels |
| Median | ≈ 111.27 pixels |
| Mean | ≈ 147.43 pixels |
| Maximum | ≈ 1104.95 pixels |

All **8199 annotated filament instances** had valid spine information.

The spine information may become useful in later stages of the project for structural and instance-level filament analysis.

---

## 7. Ground-Truth Mask Generation

The segmentation information provided in the annotation file was used to generate pixel-level ground-truth masks.

The basic process was:

```text
Filament Annotation
        ↓
Segmentation Polygon
        ↓
Polygon Rasterization
        ↓
Ground-Truth Mask
```
The generated masks have the same spatial dimensions as the original images:

```text
2048 × 2048
```

The generated binary masks contain:
```text
0   → Background
255 → Filament
```
This provided the reference masks required for evaluating segmentation methods and for future supervised deep-learning training.

## 8. Ground-Truth Visualization

The generated ground-truth masks were visually inspected by overlaying the annotated filament regions on the original H-alpha images.

This step was used as a sanity check to verify that:

- The images were loaded correctly.
- The annotation coordinates were interpreted correctly.
- The filament regions were aligned with the original images.
- The generated ground-truth masks represented the annotated filament structures.

## 9. Classical Segmentation Baseline

Before moving to deep learning, we developed a simple classical image-processing baseline.

The baseline uses:

- Intensity thresholding
- Morphological processing
- Solar-disk masking

The basic pipeline is:

```text
H-alpha Image
      ↓
Intensity Thresholding
      ↓
Morphological Processing
      ↓
Solar-Disk Masking
      ↓
Final Segmentation
```
A preliminary intensity threshold of **90** was used in the current experiment.

A preliminary solar-disk radius of **850 pixels** was also used.

> **Note:** These values are experimental settings for the current baseline and are not considered universally optimal parameters.

## 10. Baseline Evaluation

The classical baseline was evaluated by comparing its predicted segmentation mask with the ground-truth mask.

Two segmentation metrics were used:

- **Dice Score**
- **Intersection over Union (IoU)**

The evaluation was first tested on individual examples and was then extended to **50 annotated image entries** for a more meaningful preliminary evaluation.

## 11. Preliminary Results on 50 Image Entries

The classical baseline was evaluated on:

> **50 annotated image entries**

The obtained results were:

| Metric | Result |
|---|---:|
| Mean Dice | **0.034** |
| Median Dice | **0.0089** |
| Mean IoU | **0.0178** |
| Median IoU | **0.0045** |

These results provide a quantitative reference point for the current classical segmentation approach.

## 12. Baseline Interpretation

The preliminary results show that the tested fixed-threshold approach produces relatively low overlap with the annotated filament regions under the current experimental setup.

This indicates that relying only on fixed image intensity thresholding has limitations for this segmentation problem.

Therefore, the classical baseline provides a useful starting point and reference for the next stage of the project.

## 📌 Current Status

### Completed

- [x] Problem understanding
- [x] Dataset structure analysis
- [x] Filament instance-count analysis
- [x] Filament area analysis
- [x] Category distribution analysis
- [x] Bounding-box analysis
- [x] Spine-length analysis
- [x] Ground-truth mask generation
- [x] Ground-truth visualization
- [x] Classical thresholding baseline
- [x] Solar-disk masking
- [x] Morphological post-processing
- [x] Dice and IoU evaluation
- [x] Preliminary evaluation on 50 annotated image entries

### Next Planned Step

- [ ] Prepare the dataset for supervised deep learning
- [ ] Implement a U-Net-based segmentation model
- [ ] Train the model using the ground-truth masks
- [ ] Evaluate U-Net using Dice, IoU, Precision, and Recall
- [ ] Compare U-Net performance with the classical baseline
