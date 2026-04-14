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
| Processed | `s3://nasa-radiant-data/helioai-datasets/us-fdlx-geo/us-fdl-x-geo-preprocessed-data/complete-data/*p` | | |
| Raw | `<DATASET_NAME>/raw_data/` | | |
| Results | `<DATASET_NAME>/results/` | | |
| Miscellaneous | `<DATASET_NAME>/miscellaneous/` | | |



# 2 Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

The Multiscale Geoeffectiveness challenge was designed to improve forecasting of ground geomagnetic perturbations and the associated risk of geomagnetically induced currents (GICs) by combining two different forecasting scales into a single operational concept. The project extends the original DAGGER model, which already provided global ML forecasts of ground geomagnetic perturbations, but sought to address two limitations: underprediction/oversmoothing in localized responses, and the need for more lead time than L1-driven forecasts alone can provide. The resulting project architecture combines an upgraded short-horizon ground-perturbation model (DAGGER++) with a longer-horizon solar-wind forecaster (SHEATH) that is driven by solar observations.

Given the need to mitigate GIC risk through forecasts that are both localized and timely, with uncertainty estimates attached, the dataset and pipeline were assembled to support real geoeffectiveness decisions in infrastructure domains such as power grids and rail systems, through an integrated forecast chain from the Sun to the ground ("Sun-to-mud”).

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1VeiMVwL9DtE1VLwSPQUvgDs3rP5D-P5Y/view>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2023-FDL-X-Geo/tree/main>)).


## 2.1 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

The processed dataset for Multiscale Geoeffectiveness is a two-part ML dataset built around adaptive forecast refinement. In the first branch, solar observations and/or derived solar features are converted into forecast solar-wind and IMF time series via SHEATH, extending lead time to several days. In the second branch, those solar-wind drivers, together with geomagnetic indices and station-aware information, are used by DAGGER++ to produce global forecasts of ground geomagnetic perturbations.  DAGGER++ also incorporates additional geospace state/response information, uncertainty estimation, and localized improvements relative to the original DAGGER model.
     

## 2.2 Raw Data

<!-- Appropriate description -->

The raw data for the project includes L1 solar-wind drivers and the ground magnetic response they induce. Solar wind parameters were accessed from OMNIWeb to use both as inputs to DAGGER and as targets for SHEATH. OMNIWeb data are propagated measurements from spacecraft near the Earth-Sun L1 point, such as ACE and WIND, and therefore provide the near-Earth upstream forcing conditions that link solar activity to geomagnetic response. 

# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<LINK_TO_GITHUB_REPO>).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |
