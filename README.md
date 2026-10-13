# Prostate Tumor Segmentation Using Attention U-Net and HRNet-W32

**Deep learning-based prostate tumor segmentation from MRI | PyTorch | Medical Image Analysis | 2.5D Context | Cross-Validation**

## Overview

This project explores automated segmentation of prostate tumor regions in magnetic resonance imaging (MRI) using a **U-Net-style encoder–decoder architecture**, an **HRNet-W32 feature extractor**, and **additive attention gates**. The approach incorporates **2.5D slice context** to combine information from neighboring MRI slices while predicting a segmentation mask for a target slice.

The objective is to localize suspected tumor regions at the pixel level, compare predictions with annotated ground-truth masks, and assess model generalization across multiple validation folds.

> **Research/educational use only:** Segmentation outputs are experimental and are not intended for clinical diagnosis or treatment decisions.

## Key Features

- **MRI tumor segmentation:** Predicts binary masks for prostate tumor regions.
- **U-Net-style decoder:** Reconstructs spatially detailed segmentation predictions from learned features.
- **HRNet-W32 backbone:** Maintains and combines multi-resolution representations to preserve spatial information.
- **Additive attention gates:** Help the decoder focus on relevant features passed through skip connections.
- **2.5D slice context:** Uses neighboring slice information to provide through-plane context without the full computational cost of a 3D model.
- **5-fold evaluation:** Tracks training and validation loss across separate folds.
- **Qualitative comparison:** Displays MRI scans, annotated masks, predicted masks, overlays, and pixel-wise differences.

## Model Architecture

```text
MRI input (2.5D slice context)
              |
              v
      HRNet-W32 encoder
   (multi-resolution features)
              |
              v
       U-Net-style decoder <--- Additive attention-gated skip features
              |
              v
     Segmentation prediction
              |
              v
    Predicted binary tumor mask
```

### Why these components?

| Component | Purpose |
| --- | --- |
| **HRNet-W32** | Learns rich multi-scale features while retaining higher-resolution representations. |
| **U-Net decoder** | Combines contextual features with spatial detail for dense segmentation. |
| **Additive attention gates** | Modulate skip features to emphasize regions useful for tumor localization. |
| **2.5D input** | Supplies adjacent-slice anatomical context while retaining a slice-wise prediction pipeline. |
| **Cross-validation** | Helps assess sensitivity to the training/validation split. |

## Workflow

1. **Load MRI scans and corresponding tumor annotations.**
2. **Prepare input slices and masks**, including the preprocessing and 2.5D context configuration defined in the notebook.
3. **Build the model** using HRNet-W32, U-Net-style decoding, and additive attention gates.
4. **Train and validate across five folds**, monitoring loss for each fold.
5. **Generate tumor masks** on held-out examples.
6. **Visualize results** against ground-truth annotations to inspect agreement and errors.

## Results

### 1. Per-Fold Training and Validation Loss

![Per-fold training and validation loss curves]
<img width="466" height="396" alt="image" src="https://github.com/user-attachments/assets/c235d3c3-306a-4323-8d95-3d7d7103b9e8" />


The plot shows a consistent reduction in **training loss** across the five folds, reaching approximately **0.30–0.32** near the end of training. **Validation losses** generally decrease early, then fluctuate or plateau around **0.39–0.46**, depending on the fold.

**Interpretation:** The model learns meaningful training-set patterns, but the persistent train–validation gap suggests some degree of overfitting or limited generalization. Fold-to-fold variation also indicates sensitivity to the data split. These loss curves alone do not establish segmentation accuracy.

### 2. Segmentation Visualization

![T2W MRI, ground truth, prediction, overlays and difference map]<img width="464" height="266" alt="image" src="https://github.com/user-attachments/assets/83d8641d-2efc-4747-9e9f-fa55c280c5f9" />


The visualization includes:

| Panel | Description |
| --- | --- |
| **T2W** | Original T2-weighted prostate MRI slice. |
| **Ground Truth** | Expert-provided tumor mask shown in white. |
| **Prediction** | Tumor region predicted by the model. |
| **T2W + GT** | Ground-truth tumor region overlaid on the MRI in green. |
| **T2W + Prediction** | Predicted tumor region overlaid on the MRI in red. |
| **Difference** | Pixel-wise comparison of the prediction and reference annotation. |

The illustrated sample shows substantial visual overlap between the predicted and annotated tumor regions. Quantitative metrics on the complete held-out data are needed before drawing conclusions about overall performance.

### Recommended Evaluation Metrics

For a fuller evaluation, report **Dice Similarity Coefficient (DSC)**, **Intersection over Union (IoU)**, **precision**, **recall**, and optionally **Hausdorff distance (HD95)** on held-out cases. These values are **not reported here** because they cannot be established from the two figures alone.

## Technology Stack

- **Language:** Python
- **Deep learning:** PyTorch
- **Architecture:** U-Net, HRNet-W32, additive attention gates
- **Domain:** Computer vision, medical image segmentation
- **Experimentation:** Jupyter Notebook
- **Visualization:** MRI overlays and loss curves

See the notebook for the exact dependency imports, dataset handling, loss function, hyperparameters, and training configuration.

## Repository Structure

```text
Prostate-tumor-segmentation/
├── unet_hrnet_tumor_seg_MULTIMODAL (2).ipynb
├── README.md
└── assets/
    ├── training_validation_loss.png
    └── segmentation_results.png
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Kapilmahale/Prostate-tumor-segmentation.git
cd Prostate-tumor-segmentation
```

### 2. Set up a Python environment

```bash
python -m venv .venv
```

Activate the environment:

**Windows (PowerShell):**
```powershell
.venv\Scripts\Activate.ps1
```

**Linux/macOS:**
```bash
source .venv/bin/activate
```

### 3. Open the notebook

Install Jupyter and open the notebook:

```bash
pip install jupyter
jupyter notebook
```

Open **`unet_hrnet_tumor_seg_MULTIMODAL (2).ipynb`**, install the libraries imported by the notebook, and configure the dataset paths for your environment. If using Google Colab, upload/open the notebook there and configure its runtime and storage paths accordingly.

> **Note:** MRI datasets and annotations may have access or usage restrictions. Obtain them from their authorized source and do not commit sensitive patient data or large training datasets to the repository.

## Limitations and Future Work

- Reduce the gap between training and validation loss through regularization, augmentation, and hyperparameter tuning.
- Evaluate with patient-level separation to prevent leakage between training and validation sets.
- Report Dice, IoU, recall, precision, and HD95 for every fold, including mean and standard deviation.
- Compare different 2.5D context sizes and alternative segmentation backbones.
- Assess performance across MRI acquisition protocols and external datasets.
- Investigate small-lesion detection and false-positive/false-negative patterns.

## Author

**Kapil Mahale**  
GitHub: [@Kapilmahale](https://github.com/Kapilmahale)

## Disclaimer

This project is intended for academic research and learning. It has not been validated for clinical use.
