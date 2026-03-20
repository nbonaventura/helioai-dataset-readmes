**1 Access Instructions**
------------------------

Data is stored on Amazon Web Services (AWS). Data access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/hl-therm/ 
```

Downloading files is done by e.g. 
```
aws s3 cp --no-sign-request <AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data).

| Data Product               | AWS Path    | Size | Download time (@100 Mbps) |
|-----------------------|-------------|-----------|----------|
| Raw      | `s3://nasa-radiant-data/helioai-datasets/hl-therm/raw_data/` | 45 GB     |   1 hour   |
| Processed             |  `s3://nasa-radiant-data/helioai-datasets/hl-therm/processed_data/`   | 21 GB    | 0.5 hours      |
| Models | `s3://nasa-radiant-data/helioai-datasets/hl-therm/models/` | 4.6 MB | ~seconds | 



**2 Dataset Description**
-----------------------

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](https://helioai.org/dev/artifact/04b6c417-c722-484c-a668-9426bbbb0cd7/details)).
- The full source code used to process the data and create the models (see the [GitHub Repository](https://github.com/FrontierDevelopmentLab/2024-hl-thermo-cl)).



**This dataset corresponds to Thermospheric Density Continual Learning challenge. It is what was used to create the 2024 iteration of KARMAN. There are three main components: raw data, processed data, and machine learning models.**



**2.1 Raw Data**

The raw data contains thermospheric density measurements and solar weather information drawn from publicly available sources (mostly satellite data). The sources are: 
- In-situ density observations from 8 low-Earth-orbit satellites (CHAMP, GOCE, GRACE A/B/C, SWARM A/B/C), provided by TU Delft. 
- GOES EUV Irradiance: Extreme ultraviolet solar irradiance measured by geostationary GOES satellites at 7 wavelengths (25.6–140.5 nm) at 1-minute resolution.
- OMNIWeb Solar Wind & Geomagnetic Data: Interplanetary magnetic field components (Bx, By, Bz), solar wind velocity and plasma properties, and geomagnetic activity indices (AE, AL, AU, SYM/H, etc.) at 1-minute resolution from 2000 onward. 
- SOHO Solar Irradiance: Solar EUV irradiance at 30 nm and 25 nm from the SOHO spacecraft's SEM instrument at 15-second resolution
Space Weather Indices & Proxies: Daily-cadence solar and geomagnetic indices including F10.7, S10.7, M10.7, Y10.7 (solar radio flux and UV/EUV proxies), the Ap geomagnetic index, and dDst/dT (rate of change of the disturbance storm-time index).



**2.2 Processed Data**

The raw data undergoes processing to create a structured dataset suitable for training machine learning models. This involves cleaning, filtering, applying quality standards and transforming the raw measurements into a format that can be used for model training. 

Raw data is processed to enable the training of the ML models. There are various stages to this but the end goal is to have a collection of training examples that pair together a measurements of the density from the satellites with the solar and geomagnetic conditions at that moment in time. Each of the raw data inputs is first separately processed to standardise, filter and transform it, before being combined together so it can be used to create the models. 

**2.3 Models**

Two ML models are included:
- A simple “nowcasting” model, which is not intended for any use other than for instructional purposes. 
- A Temporal Fusion Transformer (TFT) model designed for the  forecasting the thermospheric density. 

An additional data sample is included which can be used as example data to test inference.

Instructions on how to use the TFT model are given in this [colab notbook](https://colab.research.google.com/github/FrontierDevelopmentLab/2024-HL-Thermo-CL/blob/main/public/inference_tft_colab.ipynb).

**3 System Requirements**
-----------------------
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
