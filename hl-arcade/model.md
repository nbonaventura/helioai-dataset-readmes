# 1 Access Instructions

Models are stored on Amazon Web Services (AWS). Access is given through the AWS Command Line Interface (CLI). Instructions on how to install and use are given in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Listing files is done by e.g.:
```
aws s3 ls --no-sign-request s3://nasa-radiant-data/helioai-datasets/hl-arcade/ 
```

Downloading files is done by e.g. 
```
aws s3 cp --no-sign-request s3://nasa-radiant-data/helioai-datasets/<AWS PATH> <LOCAL PATH> --recursive
```
You will need to replace `<AWS PATH>` with the path to the data sample you want to download (see table) and `<LOCAL PATH>` with the path on your local machine where you want to save the data).



## 2 Models Description



### Model 1 (# MB)
- AWS PATH: `hl-therm/models/karman_tft_forecast_mape_14.936_params_1074865.torch`
- Usage Instructions: Instructions on how to use the TFT model are given in : github/FrontierDevelopmentLab/../../ ipynb).
- Type: 
 - Architecture: # parameters 
- Accuracy: % MSE on validation set

### Model 2 (# MB)
- AWS PATH: `hl-arcade/models/...` 
- Usage Instructions: Instructions on how to use the model are given in this [colab notbook](https://colab.research.google.com/github/FrontierDevelopmentLab/2024-HL-Thermo-CL/blob/main/public/inference_nowcast_example.ipynb). Note this notebook is for instructional use only.
- Type: 
- Architecture: # parameters , ...
- Approach: 
- Accuracy: % MSE on validation set

### Example {} Inference Data (# MB)
- AWS PATH: `hl-arcade/data/sample_input.h5`
- Description: This file is an AFT simulated basline map (magnetogram) needed to run inference...


