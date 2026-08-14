# Shape vs. Texture Bias in Breast Cancer Image Classification

CS 7643 Deep Learning project investigating how a convolutional neural network responds to original and stylized mammography images. The experiment trains a ResNet-56 classifier on the CBIS-DDSM breast cancer dataset and compares performance on standard images with performance on texture-altered images.

> This repository is an academic experiment, not a clinical diagnostic system. Its outputs must not be used for medical decisions.

## Project overview

CNNs can rely heavily on local texture instead of global shape. This project examines that behavior in a medical-imaging setting by classifying breast abnormalities as benign or malignant and evaluating the same model on two image domains:

- Original CBIS-DDSM mammograms
- Pre-generated stylized mammograms intended to alter texture while retaining structural information

Calcification and mass cases are prepared and evaluated separately. The notebook contains the full workflow: dataset download, CSV preprocessing, PyTorch dataset construction, model definition, training, and evaluation.

## Repository contents

```text
.
├── cnn_experiment_on_breast_cancer.ipynb  # End-to-end experiment
└── README.md
```

The dataset, stylized images, model checkpoints, and Kaggle credentials are intentionally not stored in this repository.

## Method

- **Dataset:** CBIS-DDSM JPEG dataset from Kaggle
- **Tasks:** binary pathology classification for calcifications and masses
- **Labels:** benign variants are mapped to class `0`; malignant cases are mapped to class `1`
- **Model:** CIFAR-style ResNet-56 implemented in PyTorch
- **Input size:** `128 × 128`
- **Split:** 80% training and 20% validation via `random_split`; the supplied CBIS-DDSM test CSVs form the test sets
- **Class imbalance:** focal loss with effective-number class weighting
- **Optimizer:** SGD with momentum and weight decay
- **Training:** 10 epochs, batch size 32, initial learning rate 0.01, with reductions after epochs 6 and 8
- **Evaluation:** loss, accuracy, and confusion matrix on original and stylized test images

The notebook automatically selects MPS, CUDA, or CPU, although its saved run was executed in Google Colab with a GPU.

## Recorded notebook results

These values are the outputs currently saved in the notebook. The original and stylized test sets differ in size because the stylized CSVs are filtered to images available in Google Drive, so the accuracies are not a controlled, like-for-like comparison.

| Abnormality | Test domain | Images | Loss | Accuracy |
| --- | --- | ---: | ---: | ---: |
| Calcification | Original | 4,999 | 0.3022 | 50.95% |
| Calcification | Stylized | 1,210 | 0.3446 | 37.19% |
| Mass | Original | 4,399 | 0.3219 | 54.13% |
| Mass | Stylized | 1,072 | 0.4124 | 70.34% |

Accuracy alone can obscure strong class imbalance. Refer to the confusion matrices in the notebook when interpreting these results.

## Requirements

The notebook was developed with Python 3.10 and uses:

- PyTorch and torchvision
- pandas and NumPy
- Pillow and OpenCV
- scikit-learn
- Matplotlib and seaborn
- TensorFlow/Keras utilities used during preprocessing
- Kaggle CLI

Google Colab is the simplest supported environment because several notebook cells use Colab-specific Drive paths and shell commands. A GPU is recommended, and downloading the source dataset requires approximately 5 GB before extraction.

## Running the experiment

1. Open `cnn_experiment_on_breast_cancer.ipynb` in Google Colab and enable a GPU runtime.
2. Download a Kaggle API token (`kaggle.json`) from your Kaggle account.
3. Create the following Drive layout, or update the hard-coded paths in the notebook:

   ```text
   /content/drive/MyDrive/cnn-bio/
   ├── kaggle.json
   ├── breast_cancer_stylized/
   └── results/
       └── checkpoints/
   ```

4. Run the notebook from top to bottom. It downloads and extracts the [CBIS-DDSM Breast Cancer Image Dataset](https://www.kaggle.com/datasets/awsaf49/cbis-ddsm-breast-cancer-image-dataset) into `/content/bc_data`, preprocesses its metadata, and builds the train and test CSVs.
5. Supply the stylized image set under `breast_cancer_stylized/` before running the stylized-data sections. The notebook does not generate those images.

The expected extracted layout is:

```text
/content/bc_data/
├── csv/
└── jpeg/
```

To run outside Colab, install the dependencies in your own environment and replace `/content/...` and Google Drive paths with local paths. The notebook currently has no environment lockfile, command-line entry point, or automated test suite.

## Reproducibility notes

- `random_split` is called without a fixed generator seed, so validation splits and results can vary between runs.
- The notebook saves the best checkpoint as `results/checkpoints/resnet-56.pth`; separate runs can overwrite the same file.
- Only the notebook and its embedded outputs are versioned. Raw data and generated artifacts must be obtained separately.
- The stylized subsets contain only files found during path filtering and are smaller than the original test sets.

## License and data attribution

No license is currently included for this repository's code. The linked CBIS-DDSM Kaggle dataset is distributed under its own stated license; review its terms and citation requirements before reuse.
