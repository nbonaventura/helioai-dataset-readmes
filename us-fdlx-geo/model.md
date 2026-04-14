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

The Multiscale Geoeffectiveness challenge produced a coupled, multi-stage modeling system designed to forecast geomagnetic disturbances across different temporal and physical scales. The architecture consists of two primary models working in sequence:

1. SHEATH (Solar-wind Heliospheric Evolution And Time-series forecaster)
2. DAGGER++ (Deep learning model for global geomagnetic perturbation forecasting)

These models were designed to operate together as a multiscale "Sun-to-ground" forecasting system, where each model handles a different part of the physical chain from solar activity to Earth impact.

There are three levels of description available for the combined model:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1VeiMVwL9DtE1VLwSPQUvgDs3rP5D-P5Y/view>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2023-FDL-X-Geo>)).

<!-- Describe the ML models included. For each model, include:
     - Model architecture
     - Purpose (nowcasting, forecasting, classification, etc.)
     - Any caveats on intended use -->

<!-- If applicable, link to inference notebooks or usage examples -->

## 2.1 SHEATH model (long-lead-time component)

SHEATH is a solar-wind forecasting model that uses remote solar observations and/or derived solar features to predict solar-wind and interplanetary magnetic field (IMF) conditions at L1 with lead times of multiple days.

 - Input: solar observations/features (including SDOML-enabled inputs in later integration stages)
 - Output: time series of solar-wind parameters (e.g., velocity, IMF components) at L1
 - Role: extend forecast horizon beyond what is possible using only in-situ L1 measurements

Conceptually, SHEATH learns the mapping from solar surface/coronal state to heliospheric conditions before those conditions are directly measured.

<!-- Instructions on how to use the model(s) are given in this [notebook](<LINK_TO_NOTEBOOK>).-->

## 2.2 DAGGER++ model (short-lead-time component)

DAGGER++ is an enhanced version of the DAGGER model, originally developed by FDL to forecast global geomagnetic field perturbations.

Input:
   - Measured or predicted solar-wind parameters (from OMNI or SHEATH),
   - Geomagnetic indices,
   - Station-aware or spatial context features
     
Output:
  - Global maps of dB and dB/dt (geomagnetic perturbations),
  - Localized estimates relevant to GIC risk

DAGGER++ introduces several improvements over the original DAGGER:
  - Localized forecasting improvements (reduced spatial smoothing)
  - Uncertainty estimation for operational decision-making
  - Imbalance-aware regression correction to better capture extreme events


## 2.3 Coupled multiscale architecture

The key modeling innovation of the challenge is the integration of SHEATH and DAGGER++ into a single pipeline:

 - SHEATH provides long-lead-time solar-wind forecasts (days ahead)
 - DAGGER++ provides high-fidelity, short-lead-time geomagnetic forecasts (minutes to hours)
 - Together, they implement adaptive forecast refinement, where:
            - early forecasts are coarse but early,
            - later forecasts become more accurate as better inputs become available

## 2.3 Model Output

1. Solar-wind forecast time series (SHEATH outputs)
 - Predicted solar-wind and IMF parameters at L1
 - Multi-day lead time
 - Used as:
 	 - standalone forecast products, and
 	 - inputs to DAGGER++

2. Global geomagnetic perturbation maps (DAGGER++ outputs)
 - Spatially resolved predictions of:
     - dB (magnetic field perturbation)
     - dB/dt (rate of change, critical for GIC risk)
 - Delivered as:
     - global grids,
     - localized estimates for specific regions

3. Localized geoeffectiveness indicators
Derived products relevant to:
- power grid risk,
- pipeline currents,
- communication disruption

These outputs are specifically designed to support operational decision-making, not just scientific evaluation.

4. Time-resolved inference outputs
 - Timestamped predictions along the Sun-to-Earth chain
 - In some integration contexts (e.g., ARD platform):
     - stored in structured databases (e.g., BigQuery)
     - generated in "atomic" timestamp increments

# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the model. These can be found in the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2023-FDL-X-Geo>).
2. Requirements for *using* the model.


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |
