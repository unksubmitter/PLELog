# PLELog
This poroject is the implementataion of Log-Based Anomaly Detection via Probabilistic Label Estimation.

----------

## Description
PLELog is a novel approach for log-based anomaly detection via probabilistic label estimation. The goal is to effectivly detect anomalies in unlabeled log data and meanwhile avoid the manual labeling effort for training data generation. We use Semantic information within log events as fixed-length vectors and apply HDBSCAN to automatically clustering log sequences. After that, we also propose a Probabilistic Label Estimation approach to help reduce the noise introduced by error labeling and put "labeled" instances into attention-based GRU network for training. We conducted an empirical study to evaluate the effectiveness of PLELog on two open-source log data (i.e., HDFS and BGL). The results demonstrate the effectiveness of PLELong. In prticular, PLELog has been applied to two real-world  systems from our university and a large corporation, further demonstrating its practicability.

## Project Structure
***
```
├─approaches # RNN approaches here, including training, validating and testing.
├─config # Config file for RNN Networks
├─data # Code for data processing.
├─dataset
│  ├─BGL
│  └─HDFS
├─model # RNN models
├─module # Anomaly detection modules, including classifier, Attention etc.
├─outmodel # Model parameters for trained models, detailed save path is set in config files.
├─logs
├─output_res # Output based for classification model.
├─hdbscan_options.py # Probablistic label estimation part of code.
├─pipeline.py # Main entrance code.
├─requirements.txt # Environment requiremenrs of python.
└─utils
```
## Basic Usage
***
### Requirements:
The main required python packages including Pytorch, overrides, hdbscan, numpy, scikit-learn and scipy.
Anaconda is recommanded to mangage those packages and their versions. hdbscan and overrides are not available while using anaconda, try using pip.


### Preparation
- In order to run PLELog on different log data, create a directory under `dataset` folder named as the target log data and will be used further in the command parameters of `pipeline.py`.
- Move target log file (plain text, each raw contains one log message) into the log data folder.
- Run `utils/Drain.py` (with modified parameters) to finish log parsing and extract log templates.
- Download Stanford NLP word vectors from GitHub and rename as `nlp-word.vec` and put it under dataset folder.

### Anomaly detection

To run PLELog, simply run `python pipeline.py`. The results will be shown in `logs` folder named after different settings. And the detection results are saved in `output_res` folder for further analyze.

Feel free to change different settings through the command parameters below:
```
usage: pipeline.py [-h] [--config_file CONFIG_FILE] [--gpu GPU] [--hdbscan_option HDBSCAN_OPTION]
                   [--dataset DATASET] [--train_ratio TRAIN_RATIO] [--dev_ratio DEV_RATIO]
                   [--test_ratio TEST_RATIO] [--min_cluster_size MIN_CLUSTER_SIZE]
                   [--min_samples MIN_SAMPLES] [--reduce_dim REDUCE_DIM]

optional arguments:
  -h, --help            show this help message and exit
  --config_file CONFIG_FILE
                        Configuration file for Attention-Based GRU Network.
  --gpu GPU             GPU ID if using cuda, -1 if cpu.
  --hdbscan_option HDBSCAN_OPTION
                        Different strategies of HDBSCAN clustering. 0 for PLELog_noP, 1 for PLELog, -1
                        for upperbound.
  --dataset DATASET     Choose dataset, HDFS or BGL.
  --train_ratio TRAIN_RATIO
                        Ratio of train data.
  --dev_ratio DEV_RATIO
                        Ratio of dev data.
  --test_ratio TEST_RATIO
                        Ratio of test data.
  --min_cluster_size MIN_CLUSTER_SIZE
                        Minimum cluster size, a parameter of HDBSCAN
  --min_samples MIN_SAMPLES
                        Minimum samples, a parameter of HDBSCAN
  --reduce_dim REDUCE_DIM
                        Target dimension of FastICA
```

