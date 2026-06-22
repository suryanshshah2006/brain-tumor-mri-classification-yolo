# Brain Tumor MRI Classification Using YOLOv8 and Grad-CAM

This repository presents a reproducible deep-learning pipeline for multi-class brain tumor classification from MRI images using YOLOv8n-Classification.

The project compares standard image augmentation with a restricted MRI-aware augmentation strategy across three random seeds. The final model is selected using validation macro-F1, and its decisions are interpreted using Grad-CAM.

## Overview

The system classifies brain MRI images into four categories:

* Glioma
* Meningioma
* No Tumor
* Pituitary Tumor

The complete workflow includes:

* Deterministic train-validation-test preparation
* Transfer learning with YOLOv8n-Classification
* Default and MRI-aware augmentation experiments
* Multi-seed evaluation
* Validation-based model selection
* Confusion matrix analysis
* Per-class precision, recall, and F1-score
* ROC and Precision-Recall curves
* Matthews Correlation Coefficient
* Grad-CAM visual explanations

## Dataset

The project uses the Brain Tumor MRI Dataset by Masoud Nickparvar.

Dataset split used in this repository:

| Split      | Images |
| ---------- | -----: |
| Training   |  4,480 |
| Validation |  1,120 |
| Testing    |  1,600 |
| Total      |  7,200 |

The original training portion was divided into training and validation sets using an 80:20 split with a fixed seed of 123. The original test set was kept separate and was not used during training or model selection.

The dataset itself is not included in this repository.

## Model

The experiments use:

* Model: YOLOv8n-Classification
* Input resolution: 224 × 224
* Epochs: 50
* Batch size: 32
* Pretrained weights: ImageNet-pretrained YOLOv8n classification weights
* Optimizer: Automatically selected by Ultralytics
* Random seeds: 0, 42, and 123

YOLOv8n was selected because it provides a lightweight classification architecture with approximately 1.44 million parameters and low computational cost.

## Experimental Design

Six independent experiments were performed.

| Augmentation Strategy            | Seeds      |
| -------------------------------- | ---------- |
| Ultralytics default augmentation | 0, 42, 123 |
| MRI-aware augmentation           | 0, 42, 123 |

### MRI-Aware Augmentation

The MRI-aware strategy limits transformations that may introduce unrealistic medical-image variations.

It includes:

* Rotation: ±5°
* Translation: 5%
* Scale variation: 10%
* Horizontal flipping
* Mild brightness variation
* Random erasing: 10%
* No vertical flipping
* No hue alteration
* No saturation alteration
* No MixUp
* No CutMix

## Evaluation Metrics

The models are evaluated using:

* Accuracy
* Balanced accuracy
* Macro precision
* Macro recall
* Macro F1-score
* Matthews Correlation Coefficient
* Macro ROC-AUC
* Macro average precision

Results are reported separately for the validation and test sets.

Mean and standard deviation across three seeds are also calculated for each augmentation strategy.

## Model Selection

The final model is selected exclusively using validation macro-F1.

The test set is used only after model selection to estimate final generalization performance.

This prevents test-set performance from influencing the selection process.

## Results

The best validation model was obtained using MRI-aware augmentation with seed 42.

| Metric                  | Validation |      Test |
| ----------------------- | ---------: | --------: |
| Accuracy                |     0.9893 |    0.9488 |
| Balanced Accuracy       |  Add value | Add value |
| Macro Precision         |  Add value | Add value |
| Macro Recall            |  Add value | Add value |
| Macro F1-score          |     0.9893 |    0.9474 |
| MCC                     |     0.9857 |    0.9338 |
| Macro ROC-AUC           |     0.9994 |    0.9901 |
| Macro Average Precision |  Add value | Add value |

Although the seed-42 MRI-aware model achieved the highest validation macro-F1, individual test performance varied across seeds. Therefore, the aggregated mean and standard deviation across seeds should be considered alongside the selected-model results.

## Explainability With Grad-CAM

Grad-CAM is applied to the final selected model to visualize image regions that influenced its predictions.

The generated heatmaps indicate discriminative regions used by the classifier. These visualizations should not be interpreted as precise tumor segmentations.

Grad-CAM is used here as a qualitative explainability method rather than a clinical localization tool.

Example outputs are available in:

```text
results/gradcam_grid.png
```

## Repository Structure

```text
.
├── notebooks/
│   └── brain_tumor_mri_classification_yolov8_gradcam.ipynb
├── results/
│   ├── all_model_metrics.csv
│   ├── seed_aggregated_metrics.csv
│   ├── confusion_matrix.png
│   ├── roc_curves.png
│   ├── precision_recall_curves.png
│   └── gradcam_grid.png
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/brain-tumor-mri-classification-yolov8.git
cd brain-tumor-mri-classification-yolov8
```

Install the required packages:

```bash
pip install -r requirements.txt
```

## Requirements

Main dependencies:

```text
ultralytics
torch
torchvision
numpy
pandas
matplotlib
scikit-learn
grad-cam
Pillow
```

## Running the Notebook

1. Download the Brain Tumor MRI Dataset from Kaggle.
2. Add the dataset to your Kaggle notebook or update the dataset path.
3. Enable GPU acceleration.
4. Run the notebook cells sequentially.
5. Allow all six training experiments to complete.
6. Run the evaluation and Grad-CAM cells.
7. Export the generated results.

## Reproducibility

The project uses fixed seeds for:

* Python random
* NumPy
* PyTorch
* CUDA
* YOLO training

The dataset split is created once using seed 123 and remains identical across all experiments.

Minor numerical variation may still occur depending on:

* GPU architecture
* CUDA version
* PyTorch version
* Ultralytics version

## Limitations

* The project uses a single publicly available dataset.
* External clinical validation was not performed.
* Grad-CAM provides qualitative explanations and not tumor segmentation.
* MRI acquisition differences between hospitals may affect generalization.
* The model is intended for research and educational purposes only.

## Disclaimer

This project is not a medical diagnostic system.

It should not be used for clinical decision-making without extensive validation, regulatory review, and supervision by qualified medical professionals.

## Author

Suryansh Shah

B.Tech Computer Science and Engineering
Specialization in Artificial Intelligence and Machine Learning
Manipal University Jaipur

## License

This repository is released under the MIT License.

The dataset is governed by its original source license and is not redistributed through this repository.
