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
| Processed | `<DATASET_NAME>/processed_data/` | | |
| Raw | `<DATASET_NAME>/raw_data/` | | |
| Results | `<DATASET_NAME>/results/` | | |
| Miscellaneous | `<DATASET_NAME>/miscellaneous/` | | |



# 2 Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1hsxBvVRLFBoHWV8CKWkSo8HYE1TNG6re/view>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/emassara/2024-hl-radiation-ml/>)).

The SPI3S on Mars challenge was designed to estimate solar spectral irradiance from arbitrary viewpoints in the solar system, including Mars, by combining 3D reconstruction of the solar corona with a learned mapping from solar images to irradiance. The underlying scientific motivation is that solar irradiance is strongly shaped by the Sun's three-dimensional, time-evolving atmosphere, but direct measurements are only available from a limited set of spacecraft and viewpoints. Existing approaches can estimate irradiance at Earth or reconstruct coronal structure from a small number of observations, but they do not by themselves provide a full end-to-end system for estimating the Sun's spectral output at other planetary vantage points. The SPI3S dataset and workflow were assembled to solve this problem.

The challenge combines multi-viewpoint EUV image sequences, spectral irradiance measurements, and synthetic coronal density/temperature data from MHD simulations into a single end-to-end pipeline. In the first stage, a SuNeRF-based model learns a 4D representation of the corona, effectively a time-evolving 3D model of electron density and plasma temperature - from simultaneous observations in multiple EUV channels and from multiple spacecraft viewpoints. In the second stage, an updated MEGS-AI model learns to infer either spectral bands or the full EUV spectral irradiance from solar images. Together, these two components make it possible to synthesize a view of the Sun from Mars and then estimate the irradiance that would be measured there, with validation against MAVEN/EUVM measurements from the Martian perspective.


## 2.1 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

The processed data for SPI3S include a set of ML-ready training products feeding different parts of the end-to-end pipeline. For the SuNeRF-DT stage, the processed inputs are multi-viewpoint, multi-wavelength EUV image sequences aligned in time and associated with observer geometry so that the model can learn a 4D representation of the corona. 

For the MEGS-AI stage, the processed data consist of solar image representations paired with spectral irradiance targets. The 2024 SPI3S pipeline extends prior MEGS-AI work so that the model estimates not merely a few irradiance bands but the full EUV spectrum. Once SuNeRF-DT has synthesized a novel view of the Sun from Mars or another vantage, that synthetic solar image becomes the processed input to MEGS-AI, which then outputs the irradiance estimate. The memo makes clear that the full pipeline therefore depends on two levels of processed data products: first, geometry-aware multi-viewpoint coronal image sequences for 4D reconstruction, and second, image-to-spectrum training pairs for irradiance inference.

[Data processing steps](https://github.com/emassara/2024-hl-radiation-ml/blob/main/scripts/datasets.py):

- Gather a time sequence of EUV images from multiple instruments and viewpoints, including SDO/ AIA and STEREO/EUVI.

- Use the image sequences together with observer positions and instrument response information to train SuNeRF-DT, a radiative-transfer-aware version of SuNeRF that reconstructs 4D coronal electron density and plasma temperature.

- Synthesize novel solar viewpoints from the trained SuNeRF representation, including the view of the Sun as seen from Mars.

- Train an updated MEGS-AI network to map EUV images to either spectral bands or the full EUV irradiance spectrum.

- Feed the synthetic Mars-view images into MEGS-AI to estimate irradiance at Mars and validate those estimates against MAVEN/EUVM observations.

The validation and test sets were cut out events (short periods of time) as defined in in this [script](https://github.com/emassara/2024-hl-radiation-ml/blob/main/scripts/events.py).

## 2.2 Raw Data

<!-- Appropriate description -->

The raw data for SPI3S includes Extreme UltraViolet satellite images from SDO/AIA and STEREO/EUVI to provide multi-viewpoint solar observations in different wavelength channels. These image data are essential because the SuNeRF component requires simultaneous or near-simultaneous views of the corona from different vantage points to reconstruct the 3D coronal scene. The challenge also uses irradiance measurements from SDO/EVE and MAVEN/EUVM, which serve as the primary targets or validation references for spectral irradiance estimation. In addition, the project uses synthetic density and temperature data derived from MHD simulations by Predictive Science Inc. (PSI), which are used to train and validate the SuNeRF-DT representation against known ground truth.


# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<https://github.com/emassara/2024-hl-radiation-ml/>).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |
