# 1 Access Instructions

Data is stored on Amazon Web Services (AWS). Access is through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/hl-geo/
```

Downloading files is done by e.g.
```
aws s3 cp --no-sign-request <AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data.

| Data Product | AWS Path | Size | Download time (@100 Mbps) |
|-------------|----------|------|---------------------------|
| Processed | `s3://nasa-radiant-data/helioai-datasets/hl-geo/processed_data/` | 123 GB | 3 hours |
| Raw | `s3://nasa-radiant-data/helioai-datasets/hl-geo/raw_data/` | 91 GB | 2 hours |



# 2 Dataset Description

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](https://helioai.org/dev/artifact/89d1911b-7803-44e7-b792-076edb2dc5ed/details)).
- The full source code used to process the data and create the models (see the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2024-HL-GeoCL/)).

## Project summary

This project forecasts geomagnetic perturbations at ground stations worldwide using two complementary ML models:

- SHEATH: An MLP that translates solar disk imagery (SDO) into solar wind parameter predictions at L1, providing multi-day advance warning of incoming
conditions
- DAGGER-CL: A GRU that takes real-time solar wind measurements (ACE/DSCOVR) and predicts magnetic field perturbations (dBe, dBn) at ~535 ground stations with
~30-minute lead time

Together they form a two-stage forecasting pipeline: SHEATH offers early situational awareness from the Sun itself, while DAGGER-CL provides high-fidelity,
station-level nowcasts once the solar wind is measured in situ. The dataset includes the raw inputs and processed training data.



# 2.1 Processed Data

The raw data undergoes processing to create a structured dataset suitable for training machine learning models. This involves cleaning, filtering, applying quality standards and transforming the raw measurements into a format that can be used for model training. The processing steps include:

  - Cleaning: Replace sentinel fill-values with NaN; rename columns to a common schema across ACE/DSCOVR/OMNI
  - Resampling: 1-hour for SHEATH, 1-minute for DAGGER-CL
  - Merging: Temporally align solar wind data with geomagnetic indices using backward-looking asof join
  - Derived features: Compute clock angle, trig combinations, dynamic pressure, electric field proxy, dipole tilt (~26 total solar wind features)
  - SDO processing (SHEATH only): Backtrack L1 time to Sun-observation time; segment AIA 193Å into coronal holes / active regions via GMM; extract mean
  intensities per region across all 12 channels (~26 SDO features)
  - Windowing (DAGGER-CL only): Slice 90-minute lookback windows as input; target is SuperMAG dB components at a future timestamp
  - Scaling: Yeo-Johnson power transform + z-score (DAGGER-CL) or z-score only (SHEATH); scalers fitted on training set and saved for inference

## 2.2 Raw Data

- ACE & DSCOVR: In-situ solar wind measurements from spacecraft at the L1 Lagrange point (~1.5M km sunward of Earth). They measure the speed, density, and
temperature of the solar wind plasma, plus the interplanetary magnetic field (IMF) vector. DSCOVR replaced ACE as the primary real-time monitor in 2016; the
project uses both for historical coverage.
- OMNI: A NASA-curated compilation of solar wind data from multiple L1 spacecraft (including ACE and DSCOVR), time-shifted to the Earth's bow shock nose and
cross-calibrated. Serves as a clean, gap-filled historical baseline.
- NOAA SWPC RTSW: A real-time JSON feed that blends whichever L1 spacecraft (ACE or DSCOVR) is currently operational. Used for the near-real-time (NRT)
inference pipeline.
- SuperMAG: Ground-truth magnetometer data from ~175+ stations worldwide. Provides geomagnetic perturbation measurements (N/E/Z components) and derived indices
(SME, SML, SMU). This is what the model is ultimately trying to predict.
- SDO (AIA + HMI): Remote-sensing solar disk observations from NASA's Solar Dynamics Observatory. AIA provides EUV images across 10 wavelength channels
(capturing coronal structure); HMI provides vector magnetograms (photospheric magnetic field maps). Consumed via the SDOML Zarr archive and used as input to the
SHEATH model.
- GFZ Geomagnetic Indices: Derived geophysical indices from the GFZ Potsdam observatory network: Kp/ap (global geomagnetic activity), Hp30/ap30 (30-min
high-latitude activity), F10.7 solar radio flux, and sunspot number. Used as additional features characterising solar and geomagnetic conditions.



# 3 System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2024-HL-GeoCL/).
2. Requirements for *using* the data products: to use the raw and processed data, any modern computer with enough storage is sufficient.
