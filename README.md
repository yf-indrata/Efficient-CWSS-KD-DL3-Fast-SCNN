# Efficient Crop–Weed Semantic Segmentation via Knowledge Distillation from DeepLabV3+ to Fast-SCNN

Official code repository accompanying the paper:

> **Efficient crop–weed semantic segmentation via knowledge distillation from DeepLabV3+ to Fast-SCNN**  
> Indra Agustian, Igi Ardiyanto, Sunu Wibirama  
> *Green Technologies and Sustainability*, Vol. 4, Issue 2, 100357, April 2026  
> **Paper (ScienceDirect):** [https://www.sciencedirect.com/science/article/pii/S2949736126000230](https://www.sciencedirect.com/science/article/pii/S2949736126000230)  
> **DOI:** [https://doi.org/10.1016/j.grets.2026.100357](https://doi.org/10.1016/j.grets.2026.100357)

---

## Repository Structure

| File | Description |
| :--- | :--- |
| `DeepLabV3+_02.ipynb` | Teacher model training, evaluation, and inference using DeepLabV3+ (`efficientnet-b4` backbone) on CWFID. |
| `Fast-SCNN.ipynb` | Baseline student model (`Fast-SCNN`) trained from scratch using combined Dice + Focal loss. |
| `KD03.ipynb` | Knowledge Distillation training from frozen DeepLabV3+ teacher to Fast-SCNN student with dynamic alpha scheduling and alpha-aware early stopping. |
| `Evaluation.ipynb` | Evaluation notebook for comparing saved model checkpoints. |
| `Save patch_dataset.ipynb` | Utility notebook for exporting patched dataset splits to disk. |

## Dataset & Preprocessing Pipeline

Experiments use the public **Crop/Weed Field Image Dataset (CWFID)** (`v1.0`, downloaded directly in each notebook from `https://github.com/cwfid/dataset/archive/v1.0.tar.gz`) with 3 semantic classes:
- `0`: Background — RGB `(0, 0, 0)`
- `1`: Crop — RGB `(0, 255, 0)`
- `2`: Weed — RGB `(255, 0, 0)`

The three main training notebooks (`DeepLabV3+_02.ipynb`, `Fast-SCNN.ipynb`, `KD03.ipynb`) follow a unified dataset processing pipeline:
1. **Load Dataset** — Download/extract CWFID, load sorted image–mask pairs, and convert RGB masks to class index labels.
2. **Split Dataset** — Image-level split (`70%` Train / `20%` Validation / `10%` Test, `seed=42`) prior to patching to prevent cross-split patch leakage.
3. **Patching** — Sliding-window patching (`patch_size=512`, `stride=256`) generated independently per split via `CWFIDPatchDataset`.
4. **Augmentation & DataLoader** — Training augmentations (`HorizontalFlip`, `RandomBrightnessContrast`, `RandomCrop`, `Normalize`) applied exclusively to the training split; deterministic normalization (`Normalize` + `ToTensorV2`) applied to validation and test splits.

---

## Citation

If you use this code or refer to this work in your research, please cite the following paper:

**Paper URL:** [https://www.sciencedirect.com/science/article/pii/S2949736126000230](https://www.sciencedirect.com/science/article/pii/S2949736126000230)

### BibTeX

```bibtex
@article{Agustian2026EfficientCWSS,
  title     = {Efficient crop--weed semantic segmentation via knowledge distillation from {DeepLabV3+} to {Fast-SCNN}},
  author    = {Agustian, Indra and Ardiyanto, Igi and Wibirama, Sunu},
  journal   = {Green Technologies and Sustainability},
  volume    = {4},
  number    = {2},
  pages     = {100357},
  year      = {2026},
  month     = apr,
  publisher = {Elsevier BV},
  issn      = {2949-7361},
  doi       = {10.1016/j.grets.2026.100357},
  url       = {https://www.sciencedirect.com/science/article/pii/S2949736126000230}
}
```

### Plain Text (IEEE Style)

> I. Agustian, I. Ardiyanto, and S. Wibirama, "Efficient crop–weed semantic segmentation via knowledge distillation from DeepLabV3+ to Fast-SCNN," *Green Technologies and Sustainability*, vol. 4, no. 2, p. 100357, Apr. 2026, doi: [10.1016/j.grets.2026.100357](https://doi.org/10.1016/j.grets.2026.100357). Available: [https://www.sciencedirect.com/science/article/pii/S2949736126000230](https://www.sciencedirect.com/science/article/pii/S2949736126000230)

### Plain Text (APA Style)

> Agustian, I., Ardiyanto, I., & Wibirama, S. (2026). Efficient crop–weed semantic segmentation via knowledge distillation from DeepLabV3+ to Fast-SCNN. *Green Technologies and Sustainability*, *4*(2), 100357. https://doi.org/10.1016/j.grets.2026.100357
