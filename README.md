# UbhayLipi: Bilingual Alphanumeric Dataset (Sign Language)

Image-consolidation material and preliminary classification experiments for Bengali–English alphanumeric hand-gesture recognition.

**Dataset:** [UbhayLipi on Mendeley Data](https://doi.org/10.17632/4yw7y4ftmy.1)  
**Version:** 1  
**DOI:** `10.17632/4yw7y4ftmy.1`

## Overview

This repository contains image-consolidation material organized into Bangla, English, and digit groups, together with two notebooks for classification experiments.

The notebooks explore:

1. A comparison of a convolutional neural network (CNN), support vector machine (SVM), and k-nearest neighbours (k-NN).
2. Training a separate CNN with augmentation, saving the model, and performing single-image prediction.

**Important:** Both recorded experiments use **1,202 images across six classes**. Their results are not evaluations of the full 83-class UbhayLipi dataset.

The task is classification of isolated gesture images—not continuous sign-language recognition or translation.

## Repository Structure

```text
UbhayLipi/
├── Consolidated Images/
├── Model Training & Testing/
└── README.md
```

| Directory | Contents |
|---|---|
| `Consolidated Images/` | Image-consolidation material separated into Bangla, English, and digit groups |
| `Model Training & Testing/` | Two notebooks containing the experiments described below |

## Dataset and Curation

The reported dataset-curation work included:

- SHA-256-based exact-file duplicate detection.
- Cross-class duplicate analysis.
- Class consolidation.

The curation workflow examined 17,566 images and involved an 83-class organization.

The 17,566 figure refers to images examined during curation; it should not be interpreted as the final released image count without checking the published dataset.

For authoritative release information, contributor details, and licensing, consult the [Mendeley Data record](https://doi.org/10.17632/4yw7y4ftmy.1).

## Environment and Dependencies

The supplied notebooks use Google Colab and access images stored in Google Drive.

Main dependencies:

```text
numpy
pandas
matplotlib
opencv-python
scikit-learn
tensorflow
```

The Drive-mounting cells require the Google Colab environment.

Exact package versions were not included in the recorded outputs. Results may vary between environments and runs.

## Input Data Layout

Both notebooks expect individual class folders directly inside `dataset_dir` or `DATASET_PATH`:

```text
SIGN_LANGUAGE_DATA/
├── class_1/
│   ├── image_001.jpg
│   └── ...
├── class_2/
│   ├── image_001.jpg
│   └── ...
└── ...
```

The folder names above are illustrative. Actual class labels are derived from the input directory names.

Point the notebooks to the directory containing the individual gesture classes, not a higher-level directory containing only Bangla, English, and digit groups.

The recorded experiments detect six classes. The complete class list should be inspected from the notebook:

```python
# Experiment 1
print(encoder.classes_.tolist())

# Experiment 2
print(train_data.class_indices)
```

## Experiment 1: CNN, SVM, and k-NN Comparison

### Data Preparation

Images are:

1. Loaded using OpenCV.
2. Resized to 64 × 64 pixels.
3. Scaled by dividing pixel values by 255.
4. Assigned numerical labels using `LabelEncoder`.

The code retains OpenCV's BGR channel order; no explicit conversion to RGB is shown.

### Configuration

| Setting | Value |
|---|---|
| Images loaded | 1,202 |
| Classes | 6 |
| Image shape | 64 × 64 × 3 |
| Initial training partition | 961 images |
| Test partition | 241 images |
| Test fraction | 20% |
| Split random state | 42 |
| Stratified split | Not specified |
| CNN validation | 10% of the initial training partition |

The CNN reserves part of the initial training partition for validation. The SVM and k-NN use the full initial training partition.

### CNN Architecture

```text
Input: 64 × 64 × 3
→ Conv2D: 32 filters, 3 × 3, ReLU
→ MaxPooling2D: 2 × 2
→ Conv2D: 64 filters, 3 × 3, ReLU
→ MaxPooling2D: 2 × 2
→ Flatten
→ Dense: 128 units, ReLU
→ Dropout: 0.5
→ Dense: 6 units, Softmax
```

| Training setting | Value |
|---|---|
| Optimizer | Adam |
| Loss | Sparse categorical cross-entropy |
| Epochs | 5 |
| Batch size | 32 |
| Model parameters | 1,625,926 |

### Classical Models

The SVM and k-NN classifiers use flattened, normalized image pixels rather than learned CNN features.

```python
svm_model = SVC(kernel="rbf")
knn_model = KNeighborsClassifier(n_neighbors=5)
```

### Recorded Results

The following values are taken from the supplied notebook output for a single run on the 241-image test partition.

| Model | Test Accuracy | Macro Precision | Macro Recall |
|---|---:|---:|---:|
| CNN | 99.17% | 0.9921 | 0.9928 |
| SVM | 100.00% | 1.0000 | 1.0000 |
| k-NN | 99.59% | 0.9957 | 0.9964 |

The notebook also generates a confusion matrix for each model.

These results apply only to the documented six-class split. The SVM's 100% accuracy does not establish perfect recognition on new signers, new capture conditions, or the full dataset.

## Experiment 2: CNN Training and Single-Image Prediction

This notebook builds and trains a separate CNN, saves it, plots training curves, and demonstrates prediction for one image.

It does not load or independently test the CNN from Experiment 1.

### Configuration

| Setting | Value |
|---|---|
| Images | 1,202 |
| Classes | 6 |
| Input shape | 128 × 128 × 3 |
| Training images | 962 |
| Validation images | 240 |
| Batch size | 32 |
| Epochs | 5 |
| Optimizer | Adam |
| Loss | Categorical cross-entropy |
| Model parameters | 6,517,574 |

### Preprocessing and Augmentation

The notebook uses `ImageDataGenerator` with:

```python
ImageDataGenerator(
    rescale=1./255,
    rotation_range=15,
    zoom_range=0.2,
    shear_range=0.2,
    horizontal_flip=True,
    validation_split=0.2
)
```

In the recorded implementation, the training and validation subsets use the same augmentation-enabled generator. Consequently, validation images are also subject to random transformations.

`shuffle=False` preserves validation ordering; it does not disable augmentation.

Whether horizontal flipping preserves the correct gesture label should be checked for each class.

### CNN Architecture

```text
Input: 128 × 128 × 3
→ Conv2D: 32 filters, 3 × 3, ReLU
→ MaxPooling2D: 2 × 2
→ Conv2D: 64 filters, 3 × 3, ReLU
→ MaxPooling2D: 2 × 2
→ Conv2D: 128 filters, 3 × 3, ReLU
→ MaxPooling2D: 2 × 2
→ Flatten
→ Dense: 256 units, ReLU
→ Dropout: 0.5
→ Dense: 6 units, Softmax
```

### Recorded Validation Result

At the end of epoch 5, the supplied output reports:

| Metric | Value |
|---|---:|
| Validation accuracy | 82.50% |
| Validation loss | Approximately 0.4925 |

These are validation results under the recorded augmentation configuration, not independent test-set results.

### Model Saving

The supplied notebook saves the model in HDF5 format:

```python
model.save(
    "/content/drive/MyDrive/SIGN_LANGUAGE_ Model/sign_language_model.h5"
)
```

This path is specific to the original Google Drive setup. Update it for your environment and ensure the destination directory exists.

### Single-Image Prediction

The notebook:

1. Loads an image at 128 × 128 pixels.
2. Scales its pixel values by 1/255.
3. Runs the trained model.
4. Selects the class with the highest output score.
5. Displays the image and predicted label.

The supplied example produces:

```text
Predicted Sign: B_(b)
```

The example image is located inside the same source directory used to create the training and validation subsets. Its partition membership is not established in the shown code.

This is an inference demonstration, not evidence of performance on an independent external image.

The prediction function uses the model and class mapping held in the active session; it does not reload the saved model.

## Relationship Between the Experiments

The two notebooks use different:

- CNN architectures.
- Image resolutions.
- Data-partitioning procedures.
- Validation configurations.
- Augmentation settings.
- Image-loading pipelines.

Their CNN scores should not be interpreted as a controlled comparison.

| Experiment | Reported metric | Result |
|---|---|---:|
| Experiment 1 | CNN test accuracy on 241 images | 99.17% |
| Experiment 2 | Final-epoch validation accuracy on an augmented validation subset | 82.50% |

## How to Run

1. Obtain the appropriate dataset version and review its usage conditions.
2. Open a notebook from `Model Training & Testing/` in Google Colab.
3. Mount Google Drive.
4. Update the dataset path to your own class-folder directory.
5. Verify the detected class names and image counts.
6. Run preprocessing, training, and evaluation cells in order.
7. Update output paths before saving models.
8. Record the software versions, configuration, and results for your run.

The hard-coded paths in the notebooks are examples from the original environment and must be adapted.

## Limitations

The supplied experiments are preliminary.

- Only six classes are evaluated.
- The relationship between the selected classes and the full dataset requires explicit documentation.
- Duplicate and near-duplicate checks are not shown in the experiment code.
- Signer-disjoint and session-disjoint evaluation are not demonstrated.
- The first experiment does not explicitly stratify its train/test split.
- The second experiment applies random augmentation to validation images.
- Python, NumPy, and TensorFlow randomness are not fully controlled.
- Results are reported from individual runs rather than repeated experiments.
- Exact dependency versions are not recorded.
- The single-image prediction example does not establish external generalization.

These limitations do not demonstrate that data leakage occurred. They identify checks and evaluation details that are not established by the supplied code.

## Planned Improvements

- Publish the six-class label list and per-class image counts.
- Document how the experimental subset was selected.
- Save explicit training, validation, and test manifests.
- Check exact and near-duplicate overlap between partitions.
- Use signer- or session-aware splits where metadata permit.
- Separate training augmentation from validation preprocessing.
- Verify that augmentations preserve class meaning.
- Report macro F1-score and per-class results.
- Repeat experiments across multiple seeds.
- Save class mappings alongside trained models.
- Add a standalone saved-model inference workflow.
- Extend evaluation to the full dataset.

These are planned improvements, not claims of completed functionality.

## Citation

If you use UbhayLipi, cite the published dataset version:

**UbhayLipi: Bilingual Alphanumeric Dataset.** Mendeley Data, Version 1.  
DOI: [10.17632/4yw7y4ftmy.1](https://doi.org/10.17632/4yw7y4ftmy.1)

Use the Mendeley Data record's citation export for the authoritative contributor list and complete citation.

## Licensing

Consult the published dataset record for dataset licensing and source-data conditions.

Any repository code licence should be stated separately. A code licence does not automatically grant rights to redistribute dataset images.

## Maintainer

[Md. Atiqur Rahman Sifat](https://github.com/AtiqurRahman25)

For documentation corrections, data-quality concerns, or reproducibility questions, please open a GitHub issue.
