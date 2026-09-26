# Classical column capital detection with YOLOv8n

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Silvana-99/classical-capital-detection-yolo/blob/main/notebooks/01_Training_Evaluation.ipynb)

**AECO problem.** Documenting classical architectural details from photographs is time consuming. This educational detector locates and names three column capital types: Doric, Ionic and Corinthian. It does not determine a building's overall architectural style. Success means a reproducible keyless notebook, high validation detection quality, and an honest test on unseen photos.

## Dataset and classes

Source: [Roboflow Universe Architecture Segmentation, version 1](https://universe.roboflow.com/architecture-dataset/architecture-segmentation/dataset/1). The frozen three-class derived export is [in GitHub Release v1.0](https://github.com/Silvana-99/classical-capital-detection-yolo/releases/tag/v1.0) ([direct ZIP](https://github.com/Silvana-99/classical-capital-detection-yolo/releases/download/v1.0/classical_capitals_v1_80-20_yolo11.zip)); SHA256: `e4de1715ff07be6658f256984923a555c5fbb829759f6de1ba14b9a092e268bf`. Train: **862 images**; validation: **216 images** (80/20, seed 42), with **232 annotated validation objects**. The derived set remaps source labels to the three classes, includes 180 negative examples in the candidate pool, and removes exact duplicate files before splitting. Per-class counts of images containing each label overlap: training Doric 232, Ionic 254, Corinthian 230; validation Doric 67, Ionic 70, Corinthian 45. See [label rules](docs/class_definitions.md). The original version's preprocessing and augmentation settings should be copied from the source export metadata if available; this derived subset did not add a new preprocessing transform. The training framework applied its default augmentations.

**Rights:** the source was identified as CC BY 4.0 on its Roboflow listing when exported; verify its current license and each included image's redistribution rights before wider use. The derived dataset credits the original uploader and links version 1. The five external photos have individual Commons attribution and licenses in [image credits](docs/image_credits.md). No client photographs are included knowingly. Code/notebooks are offered for educational and internal use; pretrained Ultralytics code and weights retain their own license terms.

## Reproduce in a fresh Colab session

1. Click the **Open in Colab** badge. Set Runtime > Disconnect and delete runtime, then Runtime > Run all. No Drive mount, Roboflow account, secret, or local install is required.
2. The notebook verifies the public dataset SHA256, installs `ultralytics==8.4.163`, downloads released [`best.pt`](https://github.com/Silvana-99/classical-capital-detection-yolo/releases/download/v1.0/best.pt), validates the 216-image split and saves plots in `/content/results/validation`.
3. The notebook writes five annotated validation examples, ten validation predictions, and five new-image predictions to `/content/results/evidence`. Set `DOWNLOAD_RESULTS=True` in its final cell to download a ZIP. If the external Commons service is unavailable, rerun that cell later; the public GitHub Release is the primary data source.
4. Set `FULL_TRAIN=True` only if you want to repeat the full 30-epoch training; the default Run all evaluates the published 30-epoch weights to save time.

The [baseline notebook](notebooks/00_Baseline_Inference.ipynb) runs a generic COCO detector on the same domain; generic COCO labels do not include these capital classes. It is an exploratory baseline, not a comparable mAP experiment.

### Reproducibility checklist

- [x] Frozen [version 1 derived ZIP](https://github.com/Silvana-99/classical-capital-detection-yolo/releases/download/v1.0/classical_capitals_v1_80-20_yolo11.zip) and SHA256 above; primary source [https://universe.roboflow.com/architecture-dataset/architecture-segmentation/dataset/1](https://universe.roboflow.com/architecture-dataset/architecture-segmentation/dataset/1).
- [x] Model: YOLOv8n; pinned Ultralytics 8.4.163; Python package installed in notebook.
- [x] Original training: **30 epochs**, **batch 16**, **640 px**, seed **42**; Tesla T4; ~0.143 h (~8.6 min) in the original recorded run.
- [x] Public best weights: [best.pt](https://github.com/Silvana-99/classical-capital-detection-yolo/releases/download/v1.0/best.pt).
- [x] Fresh-session proof: after completing Run all from GitHub, add local date/time, Colab GPU/CPU, and elapsed runtime here. The final cell has been shown running in Colab; the screenshot alone does not confirm every previous cell.
- [x] Add the original `results.png` training-curve export from the 30-epoch Drive run under `results/curves/` if available. Current curves are validation precision/recall plots.

## Results

| Split / trial | Precision | Recall | mAP50 | mAP50-95 |
| --- | ---: | ---: | ---: | ---: |
| Validation, 216 images / 232 objects | 0.960 | 0.939 | 0.975 | 0.694 |

Per class: Doric P 0.969, R 0.859, mAP50 0.944, mAP50-95 0.544; Ionic 0.954, 0.987, 0.987, 0.754; Corinthian 0.957, 0.971, 0.993, 0.785. On five external Commons images at confidence 0.25, two capitals were detected correctly and three were missed; this tiny selected set is illustrative and not a measured test-set accuracy. Lowering confidence to 0.10 did not recover them.

**Takeaways:** Doric has the weakest recall in validation. New weathered/detached Doric examples and one close-up Ionic capital were missed. The [failure analysis](docs/error_analysis.md) identifies specific false positives and false negatives and recommends three targeted data improvements. Exact-file duplicate removal does not rule out near-duplicate scenes, so validation may be optimistic.

## Evidence, governance and PDF pack

- [Validation curves](results/curves/), [3 annotation montages](results/evidence/annotation_examples/), [10 validation predictions](results/evidence/validation_predictions/), [5 new-image predictions](results/evidence/new_image_predictions/).
- [Class definitions](docs/class_definitions.md), [error analysis](docs/error_analysis.md), [governance checklist](docs/governance_checklist.md), [image credits](docs/image_credits.md).
- [6-slide PDF](pdf/classical_capitals_slides.pdf) and [2-page mini-report](pdf/classical_capitals_mini_report.pdf).

This model is for visual inventory research with human review, not conservation decisions, structural assessment, or automated approvals.
