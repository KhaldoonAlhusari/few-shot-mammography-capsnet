# Few-Shot Mammography: CapsNet vs CNN Backbones

A view-specific, reproducible few-shot comparison of a Capsule Network (CapsNet) against three convolutional baselines — a compact CNN, VGG16, and ResNet50 — on the [INbreast](https://www.kaggle.com/datasets/ramanathansp20/inbreast-dataset) mammography dataset.

Experiments are conducted separately for CC and MLO views across 10, 20, and 31 labeled samples per class, repeated over five stratified folds.

---

## Overview

This repository accompanies the paper:

> **A View-Specific Few-Shot Comparison of Capsule Networks and Standard CNN Backbones on INbreast Mammography**  
> Khaldoon Alhusari  
> *6th International Conference on Image Processing and Capsule Networks (ICIPCN), IEEE, 2026*  
> DOI: [10.1109/ICIPCN67432.2026.11438702](https://doi.org/10.1109/ICIPCN67432.2026.11438702)

---

## Notebooks

| Notebook | Description |
|---|---|
| `FEW_SHOT_EXPERIMENT_CC.ipynb` | Full experiment pipeline for the CC view |
| `FEW_SHOT_EXPERIMENT_MLO.ipynb` | Full experiment pipeline for the MLO view |

Each notebook is self-contained and covers data loading, splits, augmentation, all four model definitions, training loops, and per-fold evaluation. To switch between shot regimes (10, 20, 31), change a single value in the **Configuration** cell at the top.

---

## Experimental Setup

- **Dataset:** INbreast — binary classification (Bi-Rads 1–2 = normal, Bi-Rads ≥ 3 = abnormal)
- **Views:** CC and MLO, processed independently
- **Few-shot regimes:** 10, 20, and 31 samples per class
- **Folds:** 5 stratified folds; test set held out (20%) before fold construction
- **Seed:** 0 (fixed across all random operations)

### Models

| Model | Framework | Notes |
|---|---|---|
| Compact CNN | TensorFlow/Keras | Three conv blocks, trained from scratch |
| VGG16 | TensorFlow/Keras | Frozen ImageNet backbone + dense head |
| ResNet50 | TensorFlow/Keras | Frozen ImageNet backbone + GAP head |
| CapsNet | PyTorch | Routing-by-agreement, margin loss |

---

## Key Results (Held-Out Test Accuracy, Mean ± Std)

| Model | CC 10-shot | CC 20-shot | CC 31-shot | MLO 10-shot | MLO 20-shot | MLO 31-shot |
|---|---|---|---|---|---|---|
| Compact CNN | 0.702 ± 0.010 | 0.654 ± 0.076 | 0.707 ± 0.000 | 0.571 ± 0.162 | 0.662 ± 0.063 | 0.648 ± 0.098 |
| VGG16 | 0.595 ± 0.158 | 0.600 ± 0.131 | 0.688 ± 0.028 | 0.543 ± 0.172 | 0.514 ± 0.169 | 0.605 ± 0.081 |
| ResNet50 | 0.459 ± 0.203 | 0.459 ± 0.203 | 0.541 ± 0.203 | 0.538 ± 0.187 | 0.386 ± 0.152 | 0.462 ± 0.187 |
| CapsNet | 0.585 ± 0.084 | 0.600 ± 0.070 | 0.644 ± 0.117 | 0.519 ± 0.046 | 0.619 ± 0.062 | 0.590 ± 0.041 |

*Some loss values were not reliably logged; accuracy comparisons remain valid.*

---

## Requirements

```
tensorflow
keras
torch
torchvision
numpy
pandas
scikit-learn
Pillow
tqdm
```

Install with:

```
pip install tensorflow torch torchvision numpy pandas scikit-learn Pillow tqdm
```

---

## Data Setup

1. Download INbreast from [Kaggle](https://www.kaggle.com/datasets/ramanathansp20/inbreast-dataset)
2. Preprocess images using the standardization pipeline from the companion repo [unsupervised-breast-density-estimation](https://github.com/KhaldoonAlhusari/unsupervised_breast_density_estimation) — this produces reoriented, artefact-free JPEGs organized into `CC/` and `MLO/` subfolders
3. Update `ROOT` and `LABELS_CSV` in the **Configuration** cell of each notebook

---

## Usage

Open either notebook and set the **Configuration** cell:

```python
ROOT        = "path/to/preprocessed/images"   # must contain CC/ and MLO/
LABELS_CSV  = "path/to/INbreast.csv"
VIEW        = "CC"      # or "MLO"
N_PER_CLASS = 10        # 10, 20, or 31
```

Then run all cells. Results are saved as `{VIEW}_{MODEL}_{N_PER_CLASS}.csv` in the working directory.

---

## Citation

If you use this code, please cite:

```bibtex
@inproceedings{alhusari2026fewshot,
  title={A View-Specific Few-Shot Comparison of Capsule Networks and Standard CNN Backbones on INbreast Mammography},
  author={Alhusari, Khaldoon},
  booktitle={2026 6th International Conference on Image Processing and Capsule Networks (ICIPCN)},
  pages={839--844},
  year={2026},
  publisher={IEEE},
  doi={10.1109/ICIPCN67432.2026.11438702}
}
```

---

## License

MIT License. See [LICENSE](LICENSE) for details.
