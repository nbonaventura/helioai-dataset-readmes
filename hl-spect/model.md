# 1 Access Instructions

Models are is stored on Amazon Web Services (AWS). Access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/
```

Downloading files is done by e.g.
```
aws s3 cp --no-sign-request s3://nasa-radiant-data/helioai-datasets/<AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the file or directory you want to download (see below) and `<LOCAL PATH>` with the path on your local machine where you want to save the data.

<!-- Add/remove rows as necessary for your project
The ideal case is that within each of these categories, data are uniformly structured.
For example, "processed" may correspond to train/test/validation data, in which we expect a tabular format (consistent column names, different rows) for each training example. 
Different models may have different train/test/validation sets, this can be explained -->


# 2 Model Description

<!-- Add a brief description of the model and the challenge it addresses -->

There are three levels of description available for this model:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1hsxBvVRLFBoHWV8CKWkSo8HYE1TNG6re/view>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2024-HL-SPI3S-SuNeRF>)).

The SPI3S challenge produced a coupled, physics-informed + deep-learning modeling system designed to estimate solar spectral irradiance from arbitrary heliospheric viewpoints, specifically targeting Mars as a use case. The architecture combines two primary models:

1. SuNeRF (Solar Neural Radiance Field) - 3D solar reconstruction model
2. MEGS-AI (spectral irradiance model) - spectral prediction model

These are integrated into an end-to-end pipeline capable of mapping multi-viewpoint solar observations → 3D solar structure → spectral irradiance at arbitrary locations.

## 2.1 SuNeRF (Solar Neural Radiance Field)

<!-- Describe the ML models included. For each model, include:
     - Model architecture
     - Purpose (nowcasting, forecasting, classification, etc.)
     - Any caveats on intended use -->

<!-- If applicable, link to inference notebooks or usage examples -->

SuNeRF is a Neural Radiance Field (NeRF)-based model adapted to solar physics, designed to reconstruct the 3D structure of the solar corona and atmosphere from multi-viewpoint observations.

Input:
    - Multi-viewpoint EUV solar images (e.g., SDO + STEREO)
    - Viewing geometry / spacecraft positions
Output:
    - Continuous 3D representation of the solar corona
    - Physical proxies such as:
        - electron density
        - temperature structure

Key characteristics:

- Physics-informed constraints embedded in the model
- Time-dependent (dynamic solar evolution)
- Enables virtual viewpoints anywhere in the heliosphere


## 2.2 MEGS-AI (spectral irradiance model)

MEGS-AI is a deep learning model trained to estimate full solar spectral irradiance from solar observations.

Input:
    - Solar image data (real or synthesized views from SuNeRF)
Output:
    - Spectral irradiance across EUV wavelength bands
    - Virtual replacement for:
         SDO/EVE MEGS-A instrument

Key role in SPI3S:
- Converts geometric / image-based representation to physically meaningful irradiance spectrum
- Extends Virtual EVE concept to off-Earth viewpoints


## 2.3 Coupled SuNeRF + MEGS-AI pipeline

The core innovation of SPI3S is the integration of these two models:

1. SuNeRF reconstructs the 3D Sun
2. Synthetic solar images are generated at a target location (e.g., Mars)
3. MEGS-AI converts those images into spectral irradiance


Multi-viewpoint solar images → SuNeRF → 3D Sun
                                   ↓
                         Virtual viewpoint (Mars)
                                   ↓
                             Synthetic images
                                   ↓
                             MEGS-AI
                                   ↓
                     Spectral irradiance at Mars

                     
## 2.4 Model Output

The SPI3S challenge produced scientific data products demonstrating 3D irradiance modeling capability beyond Earth orbit.

1. Spectral irradiance at Mars (primary output)
     Predicted solar spectral irradiance at:
  	  - Mars orbital position
     Covers:
  	  - EUV wavelength bands
     Time-dependent outputs:
 	   - evolving with solar activity

2. Synthetic solar images from arbitrary viewpoints
     Generated from SuNeRF:
         - virtual images of the Sun
         - from Mars perspective (or other locations)

3. 3D solar atmosphere reconstructions
     - Continuous volumetric representation of the corona
     - Derived physical quantities:
         - density
         - temperature proxies

4. Time-resolved irradiance sequences
 Irradiance predictions as a function of:
    - time
    - solar evolution


# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the model. These can be found in the [GitHub Repository](<LINK_TO_GITHUB_REPO>).
2. Requirements for *using* the model.


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |
