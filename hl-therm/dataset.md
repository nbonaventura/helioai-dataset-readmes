# 1 Access Instructions

Data is stored on Amazon Web Services (AWS). Data access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/hl-therm/ 
```

Downloading files is done by e.g. 
```
aws s3 cp --no-sign-request s3://nasa-radiant-data/helioai-datasets/<AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data).

| Data Product               | AWS Path    | Size | Download time (@100 Mbps) |
|-----------------------|-------------|-----------|----------|
| Processed             |  `hl-therm/processed_data/`   | 21 GB    | 0.5 hours      |
| Raw      | `hl-therm/raw_data/` | 45 GB     |   1 hour   |



# 2 Dataset Description
This dataset corresponds to Thermospheric Density Continual Learning challenge. It is what was used to create the 2024 iteration of KARMAN. There are two main components: raw data and processed data.

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](https://helioai.org/dev/artifact/04b6c417-c722-484c-a668-9426bbbb0cd7/details)).
- The full source code used to process the data and create the models (see the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2024-hl-thermo-cl)).




## 2.1 Processed Data

The raw data (see below) is processed to create a structured dataset suitable for training machine learning models. This involves cleaning, filtering, applying quality standards and transforming the raw measurements into a format that can be used for model training.

### OMNIWEB data (3.1 GB)
- AWS PATH: `hl-therm/processed_data/physical-drivers-processed/OMNIWEB/{YYYY}/{SUBSET}_omni_{YYYY}_{MM}.parquet`
- Available Years: 2000-2025
- Available subsets: `indices`, `magnetic_field`, `solar_wind_velocity`

### SOHO data (0.5GB)
- AWS PATH: `hl-therm/processed_data/physical-drivers-processed/SOHO/{YYYY}/{YY}_{MM}_{DD}_v4.00.parquet`
- Available Years: 2000-2025

### NRLMSISE-00 data (4.6 GB)
- AWS PATH: `hl-therm/processed_data/physical-drivers-processed/nrlmsise00_time_series.csv`

### GOES data (0.7 GB)
- AWS PATH: `hl-therm/processed_data/satellite-data-processed/GOES/{YYYY}/goes_irradiance_{YYYY}_{WAVELENGTH}nm.parquet`
- Available wavelengths: 256, 284, 304, 1175, 1216, 1335, 1405
- Available Years: 2010-2024

### Tudelft data (11 GB)
- AWS PATH: `hl-therm/processed_data/satellite-data-processed/tudelft/version_0{VERSION}/{SATELLITE}_data/`
- Available satellites: 
    - `GOCE` (version 1), years 2009-2013
    - `Swarm`(version 1), years 2013-2025
    - `CHAMP`(version 2), years 2000-2010
    - `GRACE-FO` (version 2), years 2018-2025 
    - `GRACE`(version 2), years 2002-2017


### Space Weather Indices & Proxies data (0.2 MB)
- AWS PATH: `hl-therm/processed_data/sw-indices/combined_incides.parquet`

## 2.2 Raw Data 

The raw data contains thermospheric density measurements and solar weather information drawn from publicly available sources (mostly satellite data). The sources are: 
- In-situ density observations from 8 low-Earth-orbit satellites (CHAMP, GOCE, GRACE A/B/C, SWARM A/B/C), provided by TU Delft. 
- GOES EUV Irradiance: Extreme ultraviolet solar irradiance measured by geostationary GOES satellites at 7 wavelengths (25.6–140.5 nm) at 1-minute resolution.
- OMNIWeb Solar Wind & Geomagnetic Data: Interplanetary magnetic field components (Bx, By, Bz), solar wind velocity and plasma properties, and geomagnetic activity indices (AE, AL, AU, SYM/H, etc.) at 1-minute resolution from 2000 onward. 
- SOHO Solar Irradiance: Solar EUV irradiance at 30 nm and 25 nm from the SOHO spacecraft's SEM instrument at 15-second resolution
Space Weather Indices & Proxies: Daily-cadence solar and geomagnetic indices including F10.7, S10.7, M10.7, Y10.7 (solar radio flux and UV/EUV proxies), the Ap geomagnetic index, and dDst/dT (rate of change of the disturbance storm-time index).

# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Reprository](https://github.com/FrontierDevelopmentLab/2024-hl-thermo-cl).
2. Requirements for *using* the data products
   1. To use the data, any modern computer with sufficient storage is sufficient.
   2. To use the model(s), see the table below

| Component | Minimum |
|-----------|---------|
| **CPU** | Any modern CPU (even 2 cores is fine) |
| **RAM** | 8 GB |
| **GPU** | None — inference runs on CPU |
| **Storage** | ~10 MB for the model weights + storage for inference data|
