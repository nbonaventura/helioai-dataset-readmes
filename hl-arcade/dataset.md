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
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data.

| Data Product               | AWS Path    | Size | Download time (@100 Mbps) |
|-----------------------|-------------|-----------|----------|
| Processed             |  `hl-arcade/processed_data/`   |  GB    |  hours      |
| Raw      | `hl-arcade/raw_data/` |  GB     |    hours   |



# 2 Dataset Description




## 2.1 Processed Data

The raw data (see below) is processed to create a structured dataset suitable for training machine learning models. This involves cleaning, filtering, applying quality standards and transforming the raw measurements into a format that can be used for model training.

### {} data (# GB)
- AWS PATH: `hl-arcade/processed_data/*.zarr`
- Format: Zarr
- Available Years: 
- Available data channels: magnetograms, ...


## 2.2 Raw Data 

The raw data contains {}.
The sources are: 


# 3. System Requirements

There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Reprository](https://github.com/FrontierDevelopmentLab/2025-HL-Active-Regions).
2. Requirements for *using* the data products
   1. To use the data, any modern computer with sufficient storage is sufficient.
   2. To use the model(s), see the table below

| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |

