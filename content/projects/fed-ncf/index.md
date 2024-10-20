---
title: Federated Learning for Movie Recommendations
#date: '2024-06-29'
# tags:
#   - deep-learning
#   - paper-notes
# cover:
#   image: "network.png"
#   hidden: false
hideSummary: true
hideMeta: True
---

## Project Overview
[Project Repository](https://github.com/gladuz/fed-ncf)

Developed a privacy-preserving movie recommendation system using federated learning techniques on the MovieLens dataset, comparing performance with centralized approaches.

## Key Contributions
- Implemented Neural Collaborative Filtering (NCF) model using PyTorch
- Applied federated learning using PySyft to preserve user privacy
- Conducted comparative analysis between federated and centralized approaches

## Technical Highlights
1. **Data Processing**: 
   - Preprocessed MovieLens dataset, encoding user/movie IDs and normalizing ratings
   - Implemented data splitting for training and testing

2. **Model Architecture**:
   - Developed NCF model combining Matrix Factorization and Multi-Layer Perceptron
   - Implemented separate embedding layers for enhanced feature learning

3. **Federated Learning Implementation**:
   - Utilized PySyft for simulating distributed environment

## Key Findings
- Federated NCF (RMSE: 1.048) performed comparably to centralized NCF (RMSE: 0.9619)
- Larger batch sizes improved federated learning performance
- Complex models (NCF) outperformed simpler ones (Matrix Factorization) in federated settings

## Challenges Overcome
- Balanced communication frequency and computation in federated setup
- Managed memory constraints in PySyft simulations
- Debugged distributed learning environment