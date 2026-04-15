# 1. Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

This dataset supports the Frontier Development Lab Heliolab 2025 'Decoding Solar Wind Structure' challenge, which aims to identify, classify, and predict solar wind phenomena using a combination of in-situ spacecraft measurements and solar disk observations.

The dataset integrates multi-source data from the Parker Solar Probe (PSP) mission, solar imagery from the Solar Dynamics Observatory (SDO), and derived machine learning features and labels. It includes raw time series measurements, processed machine-learning-ready datasets, and trained models.

A key component of this work is the CIPHER framework, a semi-automated pipeline that combines time series compression, clustering, and human-in-the-loop labeling to generate high-quality labeled datasets of solar wind structures such as switchbacks and stream interaction regions.

The labeled datasets described below are used to train machine learning models, including foundation-model-based approaches leveraging SDO imagery, to predict solar wind structures across the inner heliosphere. For a detailed description of the data, refer to the project [Technical Memorandum](https://drive.google.com/file/d/1uKu_Z8KO7sTQyWQnSDJ4aPbfVF9DozGx/view); and for the full source code used to process the data, see the project [GitHub Repository](https://github.com/FrontierDevelopmentLab/2025-HL-Solar-Wind).


## 1.1 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

The processed dataset consists of machine-learning-ready features and labeled data derived from the raw PSP and SDO datasets.

Processing is performed through the CIPHER pipeline, which includes the following steps:

1. Time series segmentation and compression using symbolic representations (e.g., iSAX).
2. Clustering of segments using unsupervised learning methods such as HDBSCAN.
3. Human-in-the-loop validation and labeling of clusters to produce high-quality annotations of solar wind structures.
4. Feature extraction and alignment with solar disk observations.
5. Generation of training, validation, and test splits for machine learning tasks.


The processed datasets include:
- Feature matrices derived from PSP time series data
- Labels for solar wind structures (e.g., switchbacks, stream interaction regions)
- Embeddings extracted from SDO imagery using foundation models

These datasets are stored in formats suitable for machine learning workflows (e.g., NumPy arrays, PyTorch tensors, or parquet files).

## 1.2 Raw Data

<!-- Appropriate description -->

The raw data consists primarily of in-situ solar wind measurements from the Parker Solar Probe (PSP), including plasma parameters and magnetic field observations at high temporal resolution.

Additional raw data sources include solar disk imagery from the Solar Dynamics Observatory (SDO), which provides multi-wavelength observations of the Sun used to extract features and embeddings via foundation models.

These datasets are originally obtained from publicly available NASA archives and mission data repositories. The PSP data is provided as time series measurements, while SDO data consists of high-resolution image sequences across multiple wavelengths.

# 2. Access Instructions

Data is stored on Amazon Web Services (AWS). Access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/hl-solar-wind/<DATASET_NAME>/
```

Downloading files is done by e.g.
```
aws s3 cp --no-sign-request s3://nasa-radiant-data/helioai-datasets/<AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data.

<!-- Add/remove rows as necessary for your project
The ideal case is that within each of these categories, data are uniformly structured.
For example, "processed" may correspond to train/test/validation data, in which we expect a tabular format (consistent column names, different rows) for each training example. 
Different models may have different train/test/validation sets, this can be explained -->

| Data Product | AWS Path | Size | Download time (@100 Mbps) |
|-------------|----------|------|---------------------------|
| Processed | `2025-hl-solar-wind-development-data/psp_interpolated_1min_labeled_zv2.zip` | | |
| Raw | `2025-hl-solar-wind-development-data/raw-psp/{sub-folder}/{year}/*cdf` | | |
| Results | `CIPHER: 2025-hl-solar-wind-development-features/cipher/valmir/*pdf & *png; PSP-KDM: 2025-hl-solar-wind-development-features/psp-understanding/runs/*pdf & *csv` | | |
<!--| Miscellaneous | `<DATASET_NAME>/miscellaneous/` | | |-->

# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2025-HL-Solar-Wind).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | Modern multi-core CPU |
| **RAM** | 16 GB recommended |
| **GPU** | Optional (required for model training, not for inference) |
| **Storage** | > 10 TB recommended for full dataset (raw + processed) |

