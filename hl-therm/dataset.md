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
| Models | `hl-therm/models/` | 5.6 MB | ~seconds | 
| Processed             |  `hl-therm/processed_data/`   | 21 GB    | 0.5 hours      |
| Raw      | `hl-therm/raw_data/` | 45 GB     |   1 hour   |



# 2 Dataset Description


There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](https://helioai.org/dev/artifact/04b6c417-c722-484c-a668-9426bbbb0cd7/details)).
- The full source code used to process the data and create the models (see the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2024-hl-thermo-cl)).



**This dataset corresponds to Thermospheric Density Continual Learning challenge. It is what was used to create the 2024 iteration of KARMAN. There are three main components: raw data, processed data, and machine learning models.**

## 2.1 Models

Two ML models are included in the models directory, a forecasting model and a nowcasting model, as well as some example data. The forecasting model is the main product of this dataset, and is intended to be used for forecasting the thermospheric density. The nowcasting model is a simple model that is not intended for any use other than for instructional purposes.

### TFT Forecasting Model (4.5 MB)
- AWS PATH: `hl-therm/models/karman_tft_forecast_mape_14.936_params_1074865.torch`
- Usage Instructions: Instructions on how to use the TFT model are given in this [colab notbook](https://colab.research.google.com/github/FrontierDevelopmentLab/2024-HL-Thermo-CL/blob/main/public/inference_forecast_example.ipynb).
- Type: Temporal Fusion Transformer — forecasts density using ~7 days of space-weather history
Architecture: 1,074,865 parameters (LSTMs + multi-head attention + variable selection networks)
- Accuracy: 14.936% MAPE on validation set

### Nowcasting Model (0.1 MB)
- AWS PATH: `hl-therm/models/karman_mlp_nowcast_mape_15.14_params_35585.torch` 
- Usage Instructions: Instructions on how to use the nowcasting model are given in this [colab notbook](https://colab.research.google.com/github/FrontierDevelopmentLab/2024-HL-Thermo-CL/blob/main/public/inference_nowcast_example.ipynb). Note this notebook is for instructional use only.
- Type: Nowcasting MLP — predicts density at the current time using instantaneous features only (no time-series history)
- Architecture: 35,585 parameters — a small feedforward network
- Approach: log_exp_residual — predicts a correction (residual) on top of an exponential atmosphere baseline, in log-space
- Accuracy: 15.14% MAPE on validation set

### Example Inference Data (1 MB)
- AWS PATH: `hl-therm/models/sample_inputs_tft.pt`
- Description: This file is a PyTorch dictionary containing a sample of everything needed to run TFT inference, shown in the table below. The first three rows (keys) are the direct model inputs. The rest is metadata for evaluation and denormalization.


| Key | Shape | Description |
|-----|-------|-------------|
| `static_feats_numeric` | `(N, 8)` | Satellite orbital parameters: altitude, latitude, longitude sin/cos, day-of-year sin/cos, time-of-day sin/cos |
| `historical_ts_numeric` | `(N, 100, 25)` | ~7-day history (100 steps × 100 min) of 25 space-weather features: OMNI indices (6), OMNI magnetic field (3), OMNI solar wind (4), SOHO EUV (2), NRLMSISE-00 densities (10) |
| `future_ts_numeric` | `(N, 1, 1)` | NRLMSISE-00 density at the forecast target time |
| `target` | `(N,)` | Ground truth density in normalized log-space (what the model is trained to predict) |
| `ground_truth` | `(N,)` | Actual thermospheric density in physical units (kg/m³) |
| `nrlmsise00` | `(N,)` | NRLMSISE-00 baseline density (for comparison) |
| `dates` | list of N strings | Timestamp for each sample |
| `normalization_dict` | dict | The scaling parameters (min/max, quantile transforms) used during preprocessing |

The first three keys are the direct **model inputs**. The rest is **metadata** for evaluation and denormalization. All values are already preprocessed (scaled/normalized) and ready to feed directly into the TFT.


## 2.2 Processed Data

The raw data undergoes processing to create a structured dataset suitable for training machine learning models. This involves cleaning, filtering, applying quality standards and transforming the raw measurements into a format that can be used for model training. 

Raw data is processed to enable the training of the ML models. There are various stages to this but the end goal is to have a collection of training examples that pair together a measurements of the density from the satellites with the solar and geomagnetic conditions at that moment in time. Each of the raw data inputs is first separately processed to standardise, filter and transform it, before being combined together so it can be used to create the models. 

## 2.3 Raw Data 

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
