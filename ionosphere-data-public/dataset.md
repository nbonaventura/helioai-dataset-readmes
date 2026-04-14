# 1 Access Instructions

Data is stored on Amazon Web Services (AWS). Access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/
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
| Processed | `s3://nasa-radiant-data/helioai-datasets/ionosphere-data-public/` | | |
| Raw | `<DATASET_NAME>/raw_data/` | | |
| Results | `<DATASET_NAME>/results/` | | |
| Miscellaneous | `<DATASET_NAME>/miscellaneous/` | | |


# 2 Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1ccJgu6uuz_8vGgOAzNdFmL7TmIXQVfBl/view>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2025-HL-Ionosphere>)).

The lonosphere-Thermosphere Twin project introduces a large-scale, ML-ready dataset integrating heterogeneous observations across the Sun-Earth system, designed to enable global forecasting of ionospheric Total Electron Content (TEC). The dataset aligns solar, geomagnetic, and ionospheric measurements into a time-synchronized, spatially consistent format, overcoming a key bottleneck in space weather ML: the lack of standardized, unified data products.

The dataset spans 2010-2024, combining dense global TEC maps, sparse GNSS-derived measurements, solar irradiance proxies, solar wind parameters, geomagnetic indices, and auxiliary spatial features such as orbital geometry and quasi-dipole coordinates.

This unified structure enables models to learn the nonlinear coupling between solar forcing, magnetospheric response, and ionospheric variability, forming the foundation for a digital twin of the ionospherethermosphere system.


## 2.1 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

The processed dataset is a fully aligned, ML-ready data cube indexed by time, where all input features and targets are:
- temporally synchronized (common cadence ~15 minutes),
- spatially aligned on a global latitude-longitude grid,
- cleaned, normalized, and gap-filled,
- structured for efficient querying and model ingestion.

Key preprocessing steps include:
- Standardizing missing values (NaNs across all sources)
- Temporal resampling and forward-filling with gap thresholds
- Removal of highly sparse or unreliable features
- Feature normalization and transformation (e.g., log-scaling for TEC)
- Alignment of heterogeneous cadences into a unified timeline

A key innovation is the introduction of a storm-aware data split, where geomagnetic events are identified using a physics-based classification (Kp-based "Mestici scale") and entire storm intervals are excluded from training, ensuring robust out-of-sample evaluation.


## 2.2 Raw Data

<!-- Appropriate description -->

The raw data consist of multi-source observational datasets spanning solar, heliospheric, and ionospheric domains:
- Ionospheric targets:
- JPL Global lonospheric Maps (dense TEC, 15-min cadence, netCDF)
- Madrigal GNSS TEC measurements (sparse, 5-min cadence)
- Solar and heliospheric drivers:
- OMNI solar wind and IMF data (1-min cadence)
- Solar flux proxies (F10.7, S10.7, M10.7, Y10.7)
- SDO-FM EUV embeddings ( 15 -second cadence)
- Geomagnetic indices:
- Kp, Ap (global)
- AE, AU, AL (auroral)
- SYM-H, ASY-D (mid/low latitude)
- Auxiliary features:
- Orbital geometry (Sun/Moon position, zenith angles)
- Quasi-dipole magnetic coordinates

These datasets are originally distributed across multiple archives (e.g., OMNIWeb, JPL, Madrigal) and require substantial preprocessing before use.


# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<LINK_TO_GITHUB_REPO>).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | Multi-core CPU required for preprocessing and inference workflows|
| **RAM** | High memory required due to global grids and multi-source data |
| **GPU** | Required for training (deep learning models); recommended for large-scale inference|
| **Storage** | Very high (multi-terabyte datasets, model checkpoints, experiment logs)|
