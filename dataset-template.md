**1 Access Instructions**
------------------------

Data is stored on Amazon Web Services (AWS). Data access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/
```

Downloading files is done by e.g.
```
aws s3 cp --no-sign-request <AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data.

<!-- Add/remove rows as necessary for your project -->

| Data Product | AWS Path | Size | Download time (@100 Mbps) |
|-------------|----------|------|---------------------------|
| Raw | `s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/raw_data/` | | |
| Processed | `s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/processed_data/` | | |
| Models | `s3://nasa-radiant-data/helioai-datasets/<DATASET_NAME>/models/` | | |
| Results | ... | ... | ... |
| Miscellaneous | ... | ... | ... |



**2 Dataset Description**
-----------------------

There are three levels of description available for this dataset:
- A high-level summary (this document) for users to quickly become familiar with the dataset.
- A detailed description (see the [Technical Memorandum](<LINK_TO_TECHNICAL_MEMORANDUM>)).
- The full source code used to process the data and create the models (see the [GitHub Repository](<LINK_TO_GITHUB_REPO>)).



**<BRIEF_DESCRIPTION_OF_DATASET_AND_CHALLENGE>**



**2.1 Raw Data**

<!-- Describe the raw data sources used in this dataset. Include:
     - What measurements/observations are included
     - Where the data comes from (instruments, satellites, surveys, etc.)
     - Temporal resolution and coverage
     - Any relevant wavelengths, bands, or parameters -->



**2.2 Processed Data**

<!-- Describe the processing pipeline applied to the raw data. Include:
     - What transformations are applied (cleaning, filtering, standardization, etc.)
     - What the final training examples look like (input/output pairs)
     - How the different raw sources are combined -->

**2.3 Models**

<!-- Describe the ML models included. For each model, include:
     - Model architecture
     - Purpose (nowcasting, forecasting, classification, etc.)
     - Any caveats on intended use -->

<!-- If applicable, link to inference notebooks or usage examples -->
Instructions on how to use the model(s) are given in this [notebook](<LINK_TO_NOTEBOOK>).

**3 System Requirements**
-----------------------
There are two sets of system requirements:
1. Requirements to *create* the data products. These can be found in the [GitHub Repository](<LINK_TO_GITHUB_REPO>).
2. Requirements for *using* the data products
   1. To use the data, any modern computer with enough storage is sufficient.
   2. To use the model(s), see the table below

| Component | Minimum |
|-----------|---------|
| **CPU** | |
| **RAM** | |
| **GPU** | |
| **Storage** | |
