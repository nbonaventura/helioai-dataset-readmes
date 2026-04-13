
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
| Processed | `s3://nasa-radiant-data/helioai-datasets/us-fdlx-ard/sdomlv2a/*zarr/` | | |
| Raw | `s3://` | | |
| Results | `s3://nasa-radiant-data/helioai-datasets/us-fdlx-ard/sdomlv2a/*zarr/` | | |
| Miscellaneous | `s3://` | | |


# 2 Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

This projects uses SDO data from the AIA and HMI instruments, processed into SDOMLv2 format, with 512x512 image resolution and 2-minute cadence, uploaded in a cloud-friendly format. This data is calibrated to Level 1.5 for AIA data (sourced from synoptic images), with HMI data sourced from line-of-sight magnetograms. Reprojection issues from standard HMI processing (NaN values around solar limb) have been corrected and removed from the v2 version of the SDOML dataset. This dataset covers the entirety of solar cycle 24 and is updating on an ongoing basis to cover the current extent of solar cycle 25, making it an ideal training set for machine learning applications that require large spans of time and high cadences.

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1WXEELZ1SLRS7wFgTGafXlC8RVgwCCqd9/view?usp=sharing>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2023-FDL-X-ARD>)).


## 2.1 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

The processed data products created by ARD-EUV are the project's main contribution. Rather than leaving users to pull and preprocess disparate SDO streams themselves, the project builds a cloud-based large-scale data ingestion, processing, and ML platform that produces synchronized, analysis-ready products suitable for both direct scientific analysis and downstream ML training. The team launched SDOMLv2, with analysis-ready data for AIA, HMI, and EVE spanning 2010-2023, and built a cloud-scale SDO computational platform. Therefore, the processed data here are the infrastructure layer that turns the raw SDO archive into a reproducible ML dataset.

Processing steps:

- Ingest raw SDO AIA, HMI, and EVE streams over the 2010-2023 interval.

- Apply AIA auto-calibration / enhancement methods to improve image consistency.

- Build synchronized, cloud-accessible analysis-ready products for AIA, HMI, and EVE.

- Use the processed image products together with EVE irradiance targets to train Virtual 
  EVE Reloaded, an updated EUV spectral irradiance estimator.
       

## 2.2 Raw Data

<!-- Appropriate description -->

The raw data for ARD-EUV come primarily from the three major SDO instrument streams that are most relevant for solar irradiance and image-based ML:

- AIA (Atmospheric Imaging Assembly): multi-wavelength solar images, especially in UV and EUV channels.
- HMI (Helioseismic and Magnetic Imager): photospheric magnetic and continuum measurements.
- EVE (Extreme Ultraviolet Variability Experiment): spectral irradiance measurements that provide the target or reference spectrum for Virtual EVE-style learning.

Physically, these three instrument streams are complementary. AIA provides the coronal and chromospheric image context that can be used to infer irradiance; HMI contributes photospheric and magnetic structure; and EVE provides the spectrally resolved irradiance ground truth required to train or validate virtual-instrument models. SDOML v2 includes analysis-ready data for SDO AIA, HMI, and EVE from 2010 until 2023.

Data format and access:
- AIA/HMI: solar image products from the SDO archive included in SDOMLv2
- EVE: irradiance data stream from the SDO/EVE instrument, included in SDOML v2/ARD-EUV 


# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<LINK_TO_GITHUB_REPO>).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | NVIDIA V100 GPUs were used to train Virtual EVE, with multiple V100s in parallel |
| **Storage** | The raw SDO dataset is ~20 petabytes, reduced to ~30 terabytes by downscaling images from 4096×4096 to 512×512 |
