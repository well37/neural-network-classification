
# Neural Networks for Classification

Academic project on neural network classification with regularization and model comparison.

- Shallow and deep neural networks
- Regularization techniques (early stopping, dropout)
- Comparison with Lasso and LassoNet models

Tools: Python, pandas, scikit-learn

## Experiment 1: Binary Classification (Heart Disease)

### Dataset
This experiment uses the **Heart Disease Health Indicators** dataset, a cleaned and consolidated version of the **BRFSS 2015** survey (health-related risk behaviors and conditions).  
Source: Kaggle — “Heart Disease Health Indicators Dataset”.

- **Samples:** 253,680  
- **Features:** 21 (binary/ordinal; e.g., HighBP, HighChol, CholCheck, BMI, Smoker, etc.)  
- **Target:** `HeartDiseaseorAttack` (binary)  
- **Goal:** Compare model **runtime (computational efficiency)** and **classification accuracy** on a large dataset.

### Models Compared (MLP variants)
Activation: **ReLU** in hidden layers, **Sigmoid** in output.

1. Shallow MLP + Early Stopping  
2. Shallow MLP (smaller width) + Early Stopping  
3. Deep MLP (2 layers) + Early Stopping  
4. Deep MLP (2 layers) + Dropout  
5. Deep MLP (3 layers) + Early Stopping  
6. Deep MLP (3 layers) + Dropout  

### Training Setup
- Train/test split: 80% / 20%  
- Optimizer: Adam  
- Loss: binary cross-entropy  
- Epochs: 15  
- Batch size: 256 (dropout models)  
- Metric: Accuracy

Dataset source: https://www.kaggle.com/datasets/alexteboul/heart
disease-health-indicators-dataset

## Results (20% hold-out)

| Model | Architecture (hidden units) | Regularization | Train acc | Test acc | Runtime |
|:--|:--|:--|--:|--:|--:|
| (1) Shallow | (none) | Early stopping | 0.908 | 0.907 | ~4 min |
| (2) Shallow (smaller) | (none; 10 units) | Early stopping | 0.908 | 0.906 | ~4 min |
| (3) Deep (1 hidden) | 10 | Early stopping | 0.907 | 0.906 | ~4 min |
| (4) Deep (1 hidden) | 10 | Dropout (0.50) | 0.906 | 0.905 | **~45 s** |
| (5) Deep (2 hidden) | 10–10 | Early stopping | 0.908 | 0.907 | ~4 min |
| (6) Deep (2 hidden) | 10–10 | Dropout (0.50, 0.25) | 0.906 | 0.905 | **~55 s** |

## Conclusion
- All six MLP variants achieved similar performance (**~0.905–0.907 test accuracy**).
- Early stopping runs showed small validation fluctuations, but the effect on final accuracy was minimal.
- **Dropout models were the most computationally efficient** (≈45–55 seconds vs ~4 minutes), so the best choices in this experiment are the **dropout variants**.



## Experiment 2: **Multiclass classification on Gene Expression (RNA-Seq)

### Dataset
**Gene Expression Cancer RNA-Seq (HiSeq PANCAN)** — five tumor types: **BRCA, KIRC, COAD, LUAD, PRAD**.  
Source: Samuele Fiorini, **UCI Machine Learning Repository (Dataset 401)**.

- **Samples:** 801  
- **Features:** 20,531 gene-expression variables  
- **Target:** 5-class tumor label  
- **Goal:** Multiclass classification in a high-dimensional setting (features ≫ samples), evaluating **LassoNet** for classification and feature selection.

### Models Compared
1. LassoNet + Cross-Validation  
2. LassoNet  
3. LassoNet + Dropout  
4. L1 baseline (Lasso / L1-regularized classifier)  
5. Deep feedforward neural network (MLP)

### Training / Preprocessing
- Feature normalization (required for LassoNet)  
- Label encoding + one-hot encoding (for neural network)  
- Train/test split: **75% / 25%**  
- Optimizer: **Adam**  
- Loss: **categorical cross-entropy**  
- Epochs: **20**, Batch size: **64**  
- Metric: **Accuracy**

Dataset link: https://archive.ics.uci.edu/dataset/401/gene+expression+cancer+rna+seq


## Results (25% hold-out)

| Model | Best accuracy | Runtime | Notes |
|:--|--:|--:|:--|
| LassoNet + CV | 0.360 | ~12 min | Low accuracy; slow |
| LassoNet | 0.363 | ~18 min | Low accuracy; slow |
| LassoNet + Dropout (0.5) | 0.363 | ~2 min | Faster, but accuracy unchanged |
| L1 baseline (Lasso) | 0.980 (α=1e-4) | ~11 s | Very sensitive to α |
| MLP (1024-256) | 0.995 | ~10 s | Best overall |

## Conclusion
- LassoNet variants underperformed on this dataset (~0.36 accuracy), and were slow (except dropout).
- The L1 baseline performed well only at a small regularization strength (α=1e-4).
- The best model in this project was the MLP (0.995 accuracy, ~10 s runtime).

