# Robust Knowledge-Based Graph Autoencoder for Fault Detection in the Tennessee Eastman Process

This repository contains the code for a knowledge-based Graph Autoencoder (GAE) framework for unsupervised fault detection in the Tennessee Eastman Process (TEP), with Kalman-filter-based reconstruction-residual filtering.

## Overview

Industrial process variables are physically coupled, but conventional data-driven fault detectors may treat them as independent features. This project represents process variables as nodes in a graph and uses process-flow knowledge to define relationships between them.

The workflow combines:

- **Knowledge-based graph topology:** adjacency relationships are constructed from process sub-units and known material/energy connections.
- **Graph Autoencoder (GAE):** learns normal operating patterns from fault-free data and reconstructs process measurements.
- **Residual filtering:** compares measured and reconstructed values, then applies Kalman Filter (KF) or Robust Kalman Filter (RKF) smoothing to reduce the effect of noise and transient spikes.
- **Node-consensus fault decision:** requires threshold violations across multiple nodes to reduce alarms caused by isolated sensor disturbances.

The method is evaluated on the Tennessee Eastman Process benchmark under multiple fault conditions.

## Repository Contents

```text
robust-gae-tep-fault-detection/
├── Data/                         # Dataset folder (data files are not distributed)
├── robust_gae_rkf_tep_fault_detection.ipynb
├── requirements.txt
├── README.md
└── LICENSE
```

The notebook filename above is a suggested name. If your notebook has a different filename, update the command and repository tree accordingly.

## Dataset Availability

The notebook expects the following files in a local `Data/` directory:

- `TEP_FaultFree_Training.csv`
- `TEP_FaultFree_Validation.csv`
- `TEP_Faulty_plus_FaultFree_Testing.csv`
- `Adjacency_Matrix.csv`

These filenames are listed to explain the expected inputs; the files themselves are not distributed here. Access to the datasets, if required, must be requested from the relevant data owners and is subject to their permission and applicable policies.

## Requirements

- Python 3.10 or later is a reasonable starting point; use the Python version tested with your environment.
- Dependencies are listed in [`requirements.txt`](requirements.txt).
- PyTorch installation may differ depending on whether you use a CPU or a CUDA-enabled GPU. Install the appropriate PyTorch build for your system before running the notebook if needed.

Install the listed packages:

```bash
pip install -r requirements.txt
```

For a reproducible research environment, record the Python version, operating system, PyTorch version, and whether CPU or GPU execution was used.

## Running the Notebook

1. Clone or download this repository.
2. Install the dependencies.
3. Obtain authorized access to the required data files and place them in the `Data/` directory, preserving the filenames listed above.
4. Open the notebook in Jupyter or Visual Studio Code.
5. Run the cells in order, starting with imports and data loading.

The notebook uses relative paths to load data from the `Data/` folder. Run it from the repository root, or adjust the `ROOT`/`DATA_DIR` configuration cell to match your environment.

## Method Summary

1. Load and preprocess fault-free training and validation measurements, along with test measurements and the graph adjacency matrix.
2. Construct graph-based representations of process variables using the knowledge-based adjacency structure.
3. Train the GAE using fault-free operating data to learn nominal process behaviour.
4. Calculate node-wise reconstruction residuals.
5. Apply KF or RKF filtering to smooth residuals and reduce the influence of noise and transient outliers.
6. Calibrate node-wise fault thresholds and apply node-consensus logic to determine fault alarms.
7. Evaluate fault detection performance across the selected TEP test conditions.

Implementation details and parameter settings should be taken from the notebook and accompanying research paper.

## Research Results

The accompanying paper reports the following aggregate results for the evaluated configurations on its extended TEP test set:

| Configuration | Fault Detection Rate (FDR) | False Alarm Rate (FAR) |
|---|---:|---:|
| Baseline GAE | 0.473 | 0.010 |
| GAE + KF | 0.844 | 0.102 |
| GAE + RKF | 0.935 | 0.098 |

These figures are reported results from the paper, not a guarantee that a fresh run will reproduce them. Reproduction depends on the exact data, preprocessing, parameter settings, random seeds, software versions, and evaluation protocol.

## Citation

If you use this code in academic work, please cite the associated paper. Replace the placeholder below with the final bibliographic details once they are confirmed.

```text
Vishnu Roshan and Jayaram Valluru,
“Development of Robust Knowledge-based Graph Autoencoders for Fault Detection:
Application to Tennessee Eastman Process.”
```