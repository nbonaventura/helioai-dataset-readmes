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
| Processed | `s3://nasa-radiant-data/helioai-datasets/hl-arcade/2025-hl-arcade-development-landing/aft/lisa/AFT_Baseline/{YYYY}/{NN}/AFTmap*.h5`; SDOMLv2 Zarr-format files | | |
| Raw | `SDO archive FITS images` | | |
| Results | `s3://nasa-radiant-data/helioai-datasets/hl-arcade/2025-hl-arcade-development-features/data/sunpde_output/prod/val_test_*/preds/*png` | | |
<!--| Miscellaneous | `<DATASET_NAME>/miscellaneous/` | | |-->


# 2 Dataset Description

<!-- Add a brief description of the dataset and the challenge it addresses -->

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<https://drive.google.com/file/d/1fTI2N0cOcLgbzVkk7QRWpNYnPsgFvwb4/view>)).
- Work on this project is still ongoing; when completed, the full source code used to process the data and create the models will be linked here. 
<!-- (see the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2025-HL-Active-Regions/>)).-->

Using high-resolution observations from instruments such as SDO/HMI and SDO/AIA, the ARCADE system combines physics-based Surface Flux Transport modeling with deep learning to characterize the magnetic evolution of solar active regions that can give rise to flares and coronal mass ejections. The hybrid approach extracts key parameters describing magnetic field structure and integrates uncertainty quantification to produce interpretable forecasts. Results demonstrate accurate short-term predictions of active region emergence up to six hours in advance, providing an important first step for downstream space weather models.

The ARCADE forecasting model was designed to work with any of five different data modes obtained with the Helioseismic and Magnetic Imager (HMI, Scherrer et. al 2012) and Atmospheric Imaging Assembly (AIA, Lemen et al. 2012) instruments onboard the Solar Dynamics Observatory (SDO), namely magnetograms, dopplergrams, extreme-ultraviolet images at 171 Å and 304 Å, and continuum-intensity maps. The SDO/HMI exposes continual, full-disk images of the Sun, while the SDO/AIA images the solar atmosphere at 10 different wavelengths, every 10 seconds. 

## 2.1 Raw Data

<!-- Appropriate description -->

The raw data (i.e., before being processed specifically for input to a machine learning model), includes cleaned and calibrated, science-ready SDO HMI and AIA FITS image data products downloadable from the primary SDO data archive hosted by the [Joint Science Operations Center (JSOC)](http://jsoc.stanford.edu/) at Stanford University; as well as the [Virtual Solar Observatory (VSO)](https://sdac.virtualsolar.org/cgi/search) search interface that accesses multiple solar data archives. 

## 2.2 Processed Data

<!-- Briefly Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined 
     - If approproate, this section will point to the train/test/validation sets -->

### 2.2.1 Observed Data

To process and prepare SDO archive data for input to the ARCADE machine-learning models, the following steps were taken:

 - Data cleaning and correction: Removal of bad frames, corrupted files, artifacts, images with spacecraft anomalies (eclipses, safe modes) and saturated pixels (e.g., during large flares in AIA); and subtraction of spacecraft/observer motion from Dopplergram velocities using available metadata or velocity correction maps.

 - Image co-registration: Aligning images so that the same solar features fall on the same pixels over time and across instruments/wavelengths.

 - Adjusting for projection effects: Correction for the fact that features away from the solar disk center are foreshortened and that HMI line-of-sight measurements are not equal to radial values except at the disk center.

 - Normalization: Scale intensities/values to a consistent range suitable for ML training.

 - Removal of geometric effects: Removal of large-scale patterns caused by the Sun’s spherical shape, limb darkening, and other geometric distortions unrelated to physical evolution.

 - Re-structuring: SDO data in the form of individual FITS files, corresponding to each of the five independent data types, were combined into a single Zarr formatted file, resulting in a data cube with a t_obs axis containing the timestamps of each data set; a channel axis containing the 5 data modes; and x and y axes of length 4096 each to match the pixel dimensions of the data images.
     
### 2.2.1 Simulated Validation Data

The ARCADE forecasting system also incorporates physics-based simulated data from the Advective Flux Transport (AFT) model as a complementary dataset to the observational SDO inputs. AFT produces time-evolving, full-Sun maps of the radial magnetic field using a Surface Flux Transport (SFT) framework that models key physical processes including differential rotation, meridional flow, turbulent diffusion, and flux emergence.

Unlike observational magnetograms, which are limited to line-of-sight measurements, AFT provides a physically consistent estimate of the global radial magnetic field across the entire solar surface, making it a critical resource for evaluating large-scale magnetic evolution.

The simulated AFT baseline data contain:
- Full-disk, time-evolving magnetogram maps generated via SFT physics
- Radial magnetic field estimates over the entire solar surface
  
# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<https://github.com/FrontierDevelopmentLab/2025-HL-Active-Regions/>).
2. Requirements for *using* the data products


| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | 20 GB |
| **Storage** | |







