# Image Analysis of Benign and Malignant Skin Lesions (DS4002 - Group 14)

The goal of this project is to evaluate whether a deep learning image classification model can accurately distinguish between benign and malignant skin lesions using dermoscopic images. The project utilizes an EfficientNet-B0 backbone with a custom classification head. The study compares binary classification and 11-class classification across image-only and multimodal (image + metadata) architectures.

## Contents of the Repository

This repository is organized to ensure full reproducibility of the analysis. It includes the original metadata, instructions for image acquisition, the source code for four distinct modeling experiments, and the resulting performance metrics and visualizations.

* **DATA/**: Contains clinical metadata, ground truth labels, and instructions for acquiring the image dataset.
* **SCRIPTS/**: Contains Jupyter Notebooks for data preprocessing, training, and evaluation.
* **OUTPUT/**: Contains saved model checkpoints, confusion matrices, and training logs.
* **LICENSE.md**: The MIT License governing the use of this repository.

## Section 1: Software and Platform Section

### Software
* **Python 3.9+**: The primary programming language.
* **Jupyter Notebook**: Used for interactive development and execution.
* **Google Colab**: Used for hardware acceleration (NVIDIA T4 GPU).

### Add-on Packages
The following Python libraries must be installed:
* `torch` (v2.0+)
* `torchvision`
* `timm` (PyTorch Image Models)
* `pandas`
* `numpy`
* `matplotlib`
* `seaborn`
* `scikit-learn`
* `Pillow`

### Platform
* **Development Platform**: macOS / Windows
* **Execution Platform**: Google Colab (Linux-based environment)

## Section 2: A Map of your documentation
```text
Ds4002-Group14-Project3/
│
├── LICENSE.md
├── README.md
│
├── DATA/
│   ├── How_to_Acquire_Images.md
│   ├── MILK10k_Training_GroundTruth.csv
│   ├── MILK10k_Training_Metadata.csv
│   └── Data_Appendix.pdf
│
├── SCRIPTS/
│   ├── 11-Class_Image-Only.ipynb
│   ├── 11-Class_Multimodal.ipynb
│   ├── Binary_Image-Only.ipynb
│   └── Binary_Multimodal.ipynb
│
└── OUTPUT/
    ├── 11-Class_Image-Only/
    │   ├── 11_class_results.png
    │   ├── 11class_epoch_log.png
    │   ├── curves_11class.png
    │   └── test_cm_11class.png
    ├── 11-Class_Multimodal/
    │   ├── 11class_mm_epoch_log.png
    │   ├── 11class_mm_results.png
    │   ├── curves_11class_mm.png
    │   └── test_cm_11class_mm.png
    ├── Binary_Image-Only/
    │   ├── Binary_Image_Best_Epoch.png
    │   ├── Binary_Image_Classification_Report.png
    │   ├── Binary_Image_Confusion_Matrix.png
    │   ├── Binary_Image_Epoch_Log.png
    │   └── Binary_Image_Plots.png
    └── Binary_Multimodal/
        ├── Binary_mm_Best_Epoch.png
        ├── Binary_mm_Classification_Report.png
        ├── Binary_mm_Confusion_Matrix.png
        ├── Binary_mm_Epoch_Log.png
        └── Binary_mm_Plots.png
```

## Section 3: Instructions for reproducing your results

### 1. Repository Setup

Clone the repository to your local machine or upload the folder structure to your Google Drive for Colab use.
```bash
git clone [https://github.com/your-username/Ds4002-Group14-Project3.git](https://github.com/your-username/Ds4002-Group14-Project3.git)
cd Ds4002-Group14-Project3
```

### 2. Data Acquisition

Due to file size constraints on GitHub, the raw image files are not hosted in this repository. Follow these steps:

1.  Navigate to `DATA/How_to_Acquire_Images.txt`.
2.  Follow the provided link to the ISIC/MILK10k archive.
3.  Download the image zip file and extract the contents into `DATA/MILK10k_Training_Input/`.

### 3. Environment Preparation

Install the required packages using pip:
```bash
pip install torch torchvision timm pandas numpy matplotlib seaborn scikit-learn pillow
```

### 4. Running the Analysis

The scripts are modular and can be run independently depending on the desired experiment. Each notebook contains code to:

1.  Load and merge the CSV metadata from the `DATA` folder.
2.  Perform a stratified split by `lesion_id` to ensure no images of the same lesion appear in multiple sets.
3.  Initialize the EfficientNet-B0 model with pre-trained weights.
4.  Execute the training loop, featuring a 3-epoch backbone freeze to stabilize the classification head and prevent overfitting.
5.  Generate and save performance plots to the corresponding subfolder in `OUTPUT`.

Open any notebook in the `SCRIPTS` folder and select "Run All" to reproduce the metrics and figures.

### 5. Verification of Results

The reproduction is successful if:

1.  The script generates a `.pth` model checkpoint in the `OUTPUT` directory.
2.  The Validation AUC stabilizes or improves across 20 epochs.
3.  The confusion matrix and training curves are saved as `.png` files in the experiment-specific folder.

## Implementation Notes
* **Class Imbalance**: Weighted sampling is implemented in the data loaders to address the high frequency of Nevus (NV) cases relative to malignant classes.
* **Model Selection**: Validation AUC is the primary metric used for checkpointing the best model, as it is more robust to imbalanced datasets than standard accuracy.
* **Architecture**: EfficientNet-B0 serves as a pretrained backbone for feature extraction; only the final fully connected layers are trained initially.
* **Hardware**: GPU acceleration (NVIDIA T4 or higher) is recommended. Training on a CPU will result in significantly longer execution times.
* **Reproducibility**: Manual random seeds are set throughout the scripts; however, minor variations in floating-point calculations on different GPU architectures may result in slight differences in final metrics.

## References

[1] International Skin Imaging Collaboration, "ISIC 2019: Training data," ISIC Archive, 2019. [Online]. Available: https://archive.isic-archive.com/
[2] M. Tan and Q. Le, "EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks," in Proceedings of the 36th International Conference on Machine Learning, vol. 97, 2019, pp. 6105-6114.
[3] Project TIER, "The TIER Protocol 4.0," 2023. [Online]. Available: https://www.projecttier.org/tier-protocol/protocol-4-0/
