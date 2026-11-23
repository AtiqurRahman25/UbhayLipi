# UbhayLipi: Bilingual Alphanumeric Dataset (Sign Language)

A Bengali–English hand-gesture image dataset and supporting documentation for research in visual recognition.

This repository documents UbhayLipi, its data-curation workflow, and considerations for using the dataset in reproducible machine-learning experiments.

**Dataset record:** [Mendeley Data — Version 1](https://doi.org/10.17632/4yw7y4ftmy.1)  
**Dataset DOI:** `10.17632/4yw7y4ftmy.1`

---

## Overview

UbhayLipi brings Bengali and English alphanumeric hand-gesture imagery into a common dataset for computer-vision research.

The curation work included SHA-256-based duplicate detection, cross-class duplicate analysis, and class consolidation. These steps were undertaken to organize the image collection and investigate conflicting or repeated samples.

The dataset concerns isolated alphanumeric gestures. It should not be interpreted as a complete representation of a sign language, continuous signing, or sign-language grammar.

## Dataset at a Glance

| Item | Description |
|---|---|
| Name | UbhayLipi: Bilingual Alphanumeric Dataset |
| Modality | Hand-gesture images |
| Language/script coverage | Bengali and English |
| Documented class count | 83 |
| Images examined during the reported curation workflow | 17,566 |
| Main research task | Image classification |
| Dataset version linked here | Version 1 |
| Hosting platform | Mendeley Data |
| DOI | `10.17632/4yw7y4ftmy.1` |

**Count clarification:** The 17,566 figure describes the collection processed during curation. It should not be treated as the final released image count unless confirmed against the published dataset files.

For authoritative class names, file counts, and release details, consult the dataset record and its accompanying documentation.

## Dataset Access

The published dataset is available through Mendeley Data:

**[Access UbhayLipi — Version 1](https://doi.org/10.17632/4yw7y4ftmy.1)**

When reporting experiments:

- Identify the dataset version.
- Document any changes to the downloaded files.
- Report the number of images and classes actually used.
- Preserve the relationship between class names and numerical labels.
- Describe exclusions, preprocessing, and dataset splits.

## Data Curation

### 1. Exact-Duplicate Detection

SHA-256 hashes were used to identify byte-identical image files.

Files with matching hashes can be flagged for duplicate review. This procedure does not identify all visually equivalent images: resized, recompressed, cropped, or otherwise modified copies may have different hashes.

### 2. Cross-Class Duplicate Analysis

Duplicate checks were extended across class folders to identify identical files appearing under different labels.

A cross-class match is a review flag, not an automatic instruction to delete a sample or change its label. Resolution should consider the dataset’s class definitions and source information.

### 3. Class Consolidation

The curation workflow included class merging.

Any reproduction or extension of the dataset should document:

- Original class labels.
- Final class labels.
- The mapping between them.
- The reason for each merge.
- The resulting class counts.

This mapping is important for interpreting predictions and comparing experiments.

## Suggested Research Directions

Potential uses include:

- Supervised classification of isolated alphanumeric gestures.
- Comparison of visual feature representations.
- Analysis of class imbalance and visually similar categories.
- Investigation of duplicate handling and dataset quality.
- Evaluation of robustness to image transformations.

These are potential applications, not claims of validated performance or deployment readiness.

## Recommended Experimental Practice

### Prevent Data Leakage

Check for duplicate and near-duplicate samples before constructing training, validation, and test sets.

If multiple images originate from the same signer, recording session, or source collection, consider group-based splitting where suitable metadata are available. If those identifiers are unavailable, state this limitation.

Do not assume that a random image-level split measures generalization to unseen signers.

### Document Preprocessing

Report the steps actually used, including, where applicable:

- Image resizing.
- Colour-space conversion.
- Cropping or background processing.
- Normalization.
- Data augmentation.
- Treatment of unreadable or excluded files.

Apply training-time augmentation only to the training partition.

### Report More Than Overall Accuracy

Where appropriate, include:

- Overall accuracy.
- Macro-averaged precision, recall, and F1-score.
- Per-class sample counts and metrics.
- A confusion matrix.
- Analysis of representative errors.

Record the split procedure, random seed, software environment, and model configuration.

## Results and Reproducibility

This README does not claim a benchmark score.

Any results added to this repository should link to the code and configuration needed to reproduce them and identify the exact dataset version and evaluation split.

A useful results table should include:

| Model | Dataset Version | Split Protocol | Accuracy | Macro F1 | Configuration |
|---|---|---|---|---|---|
| Add only after evaluation | — | — | — | — | — |

Remove the placeholder table if no experiments are included.

## Limitations and Responsible Use

- Isolated gesture classification is narrower than complete sign-language recognition or translation.
- The documented number of classes does not establish complete coverage of either language.
- Results may depend on signer diversity, lighting, backgrounds, acquisition conditions, and class balance.
- Exact-file deduplication does not eliminate all near-duplicates or all sources of evaluation leakage.
- Performance on this dataset alone does not establish suitability for real-world accessibility applications.
- Use must comply with the published dataset licence and any applicable source-data conditions.

Claims about demographic coverage, participant consent, and anonymization should be supported by the dataset documentation rather than inferred from the images.

## Contributors

The contributor list reported for Version 1 is:

- A. Alam
- M. A. R. Sifat
- L. Ahamod
- J. Ali
- S. A. Al Mamun
- T. Islam

Please consult the published dataset record for the authoritative contributor names and order.

### Repository Maintainer

[Md. Atiqur Rahman Sifat](https://github.com/AtiqurRahman25)

Dataset authorship, repository maintenance, and implementation contributions are distinct roles. Contributions to individual scripts or analyses should be credited explicitly.

## Citation

If you use UbhayLipi, cite the published dataset version.

> Alam, A., Sifat, M. A. R., Ahamod, L., Ali, J., Al Mamun, S. A., & Islam, T. (2026). *UbhayLipi: Bilingual Alphanumeric Dataset* (Version 1) [Dataset]. Mendeley Data. https://doi.org/10.17632/4yw7y4ftmy.1

```bibtex
@misc{ubhaylipi2026,
  author    = {Alam, A. and Sifat, M. A. R. and Ahamod, L.
               and Ali, J. and Al Mamun, S. A. and Islam, T.},
  title     = {UbhayLipi: Bilingual Alphanumeric Dataset},
  year      = {2026},
  publisher = {Mendeley Data},
  version   = {1},
  doi       = {10.17632/4yw7y4ftmy.1},
  url       = {[https://doi.org/10.17632/4yw7y4ftmy.1](https://doi.org/10.17632/4yw7y4ftmy.1)}
}
```

Check this citation against the repository’s official citation export before use.

## Licensing

### Dataset

Consult the licence displayed on the Mendeley Data record. This README does not grant additional rights to the dataset or its source images.

### Repository Code and Documentation

Refer to the repository’s `LICENSE` file, if provided. Do not assume that a code licence also applies to the dataset.

## Questions and Corrections

Please open a GitHub issue to report documentation errors, reproducibility problems, or suspected data-quality issues.

When reporting an image-related issue, include the dataset version, relevant class label, and filename. Avoid uploading personal or restricted material.
