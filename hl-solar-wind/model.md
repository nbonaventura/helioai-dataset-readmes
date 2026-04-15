# 1. Model Description

<!-- Add a brief description of the model and the challenge it addresses -->

Understanding the origins of solar wind structures is essential for improving space weather prediction. The Frontier Development Lab Heliolab 2025 'Decoding Solar Wind' challenge developed CIPHER, a semi-automated machine learning framework that combines clustering, human-in-the-loop labeling, and foundation model embeddings to classify solar wind phenomena such as switchbacks and stream interaction regions using Parker Solar Probe data.

By linking these labeled solar wind structures with solar disk observations from SDO, the team trained models capable of predicting solar wind conditions throughout the inner heliosphere. This enables improved forecasting of solar wind impacts on Earth and spacecraft, and provides new insights into the physical processes driving solar wind variability.

The key models from the project include:

1. CIPHER Pipeline:
A semi-automated framework for time series analysis that combines compression, clustering, and human-in-the-loop labeling to generate labeled datasets.

2. SDO-based Prediction Model:
A deep learning model built on top of a foundation model (SDOFM) trained on solar disk imagery. This model predicts solar wind structure classes at spacecraft locations using only solar images.

3. Neural Field Model:
A model that extends predictions across the inner heliosphere, enabling spatially continuous predictions of solar wind structures beyond the original spacecraft trajectory.

These models demonstrate the ability to link solar surface activity with in-situ solar wind measurements, enabling the prediction of solar wind conditions throughout the inner heliosphere.

A detailed description of the models may be found in the project [Technical Memorandum](); and the full source code used to train and run the models in the project [GitHub Repository](https://github.com/FrontierDevelopmentLab/2025-HL-Solar-Wind/).


# 2. Access Instructions

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
