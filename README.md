# PLELog
This is the basic implementation of our submission in ICSE 2021: **Semi-supervised Log-based Anomaly Detection via Probabilistic Label Estimation**

---

## Description

`PLELog` is a novel approach for log-based anomaly detection via probabilistic label estimation. It is designed to effectivly detect anomalies in unlabeled log and meanwhile avoid the manual labeling effort for training data generation.
We use Semantic information within log events as fixed-length vectors and apply `HDBSCAN` to automatically clustering log sequences. After that, we also propose a Probabilistic Label Estimation approach to help reduce the noise introduced by error labeling and put "labeled" instances into `attention-based GRU network` for training. We conducted an empirical study to evaluate the effectiveness of `PLELog` on two open-source log data (i.e., HDFS and BGL). The results demonstrate the effectiveness of `PLELong`. In prticular, `PLELog` has been applied to two real-world systems from our university and a large corporation, further demonstrating its practicability.

## Project Structure

---

```
├─approaches  #RNN approaches here, including training, validating and testing.
├─config      #Config file for RNN Networks.
├─data        #Code for data processing.
├─dataset
│  ├─BGL
│  └─HDFS
├─model       #RNN models.
├─module      #Anomaly detection modules, including classifier, Attention etc.
├─outmodel    #Model parameters for trained models, detailed save path is set in config files.
├─logs        #Logs of PLELog.
├─output_res  #Output result of Attention-Based GRU classification model.
├─hdbscan_options.py #HDBSCAN and Probablistic label estimation approach.
├─pipeline.py  #Main entrance code.
├─requirements.txt  #Environment requiremenrs of PLELog, please refer to the Requirement Section for detailed information.
└─utils

```

## Datasets

---

We used `2` widely used, open-source Log Datasets, HDFS and BGL. In the future, we are planning on testing `PLELog` on more open-source log data.

| Software System | Description                        | Time Span  | # Messages | Data Size | Link                                                      |
| --------------- | ---------------------------------- | ---------- | ---------- | --------- | --------------------------------------------------------- |
| HDFS            | Hadoop distributed file system log | 38.7 hours | 11,175,629 | 1.47 GB   | Currently Omitted.                                                  |
| BGL             | Blue Gene/L supercomputer log      | 214.7 Days | 4,747,963  | 708.76MB  | [Usenix-CFDR Data](https://www.usenix.org/cfdr-data#hpc4) |

## Reproducibility:

---

### Environment

**Note:** We attach great importance on reproducibility of `PLELog`. To run and reproduce our results, please try install the suggested version of the key packages.

**Key Packages:**

```
pytorch v1.5.1
python v3.8.3
hdbscan v0.8.26
```

The mainly required python packages including pytorch, overrides, hdbscan, numpy, scikit-learn and scipy.
`Anaconda` is recommanded to mangage those packages and their versions.
hdbscan and overrides are not available while using anaconda, try using pip.

### Preparation

You need to follow these steps to **completely** run `PLELog`.
- **Step 1:** In order to run `PLELog` on different log data, create a directory under `dataset` folder **using unique and memorable name**(e.g. HDFS and BGL). `PLELog` will try to find the related files and create logs and results according to this name.
- **Step 2:** Move target log file (plain text, each raw contains one log message) into the folder of setp 1.
- **Step 3:** Run `utils/Drain.py` (make sure it has proper parameters) to finish log parsing and extract log templates. You can find the details about Drain parser from [IBM](https://github.com/IBM/Drain3).
- **Step 4:** Download [Stanford NLP word embeddings](https://nlp.stanford.edu/projects/glove/), rename as `nlp-word.vec` and put it under `dataset` folder.

**Note:** Since log can be very different, here in this repository, we only provide the processing approach of HDFS and BGL w.r.t our experimental setting.
If you want to apply `PLELog` on new log data, please refer to the `prepare_data` method in `pipeline.py` to add new pre-process methods.

## Anomaly detection

---

- **Complete:** You can run `PLELog` from the groud up by running `pipeline.py` after the preparation. The results will be shown in `logs` folder named after detailed settings. And the classification results are saved in `output_res` folder for further analysis.
- **Quick Start:** Since `HDBSCAN` may need hours to finish, we provide a trained model (on `BGL` dataset) and a test input as a quick start for `PLELog`, just run `test.py` under the correct environment.
  Logs will be written in `log/test.log`, you can find the results at the end of file.
Feel free to play with `PLELog` through the command parameters below: (The results of different settings should be separated, don't worry! :P)

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
  --thredshold THRESHOLD
                        Threshold for final classification, any instance with "anomalous score" higher than this threshold will be regarded as anomaly.
```

## Contact

---

We are happy to see `PLELog` actually can be applied in real world and willingly to contribute to the community. Feel free to contact us if you have any question!
Currently omitted.
