---
title: Pet Skin Disease Recognition
#date: '2024-06-29'
tags:
  - deep-learning
  - paper-notes
# cover:
#   image: "network.png"
#   hidden: false
hideSummary: true
hideMeta: True
---

## Project Overview
[Project Repository in Github](https://github.com/gladuz/pet-disease-recognition)

Participaed in a project with Daewoong Foundation to develop a deep learning system for recognizing skin diseases in pets, aiming to assist veterinarians in diagnosis.

## Key Contributions
- Implemented dataset parser for complex data structures
- Used U-Net based image segmentation model using PyTorch
- Collaborated on pipeline combining object detection and classification

## Technical Highlights
1. **Dataset Handling**: 
   - Parsed and cleaned ~55,000 images with 6 lesion types
   - Converted annotations to YOLO format

2. **U-Net Segmentation Model**:
   - Addressed challenges of small object segmentation (93% of lesions <5% of image area)

3. **Integrated Pipeline**:
   - Combined segmentation, object detection, and classification
   - Implemented post-processing for improved accuracy

## Challenges Overcome
- Inconsistent image quality and small object sizes
- Complex dataset structure with missing data
- Model overfitting in semantic segmentation

## Tools Used
Python, PyTorch, Jupyter Notebooks, Git/GitHub