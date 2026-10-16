# 🦅 Animal Migration Forecasting with Climate Context

An LSTM-based deep learning project for forecasting migratory bird trajectories from GPS tracking data.

## Overview

This project uses the previous 20 GPS positions of a white stork to predict its next 5 positions, represented by latitude and longitude coordinates.

## Model Architecture

| Parameter | Value |
|---|---|
| Architecture | LSTM |
| Input sequence | 20 GPS positions |
| Forecast horizon | 5 future positions |
| Hidden size | 128 |
| Number of layers | 2 |
| Dropout | 0.2 |
| Loss function | Mean Squared Error (MSE) |
| Optimizer | Adam |

## Dataset

- **Source:** [Movebank](https://www.movebank.org/)
- **Study:** LifeTrack White Stork SW Germany (2013–2019)
- **Required data:** GPS location records containing timestamps, animal identifiers, latitude, and longitude.

The dataset is not included in this repository. Obtain the appropriate location data from Movebank and configure its local path before training. Acceleration sensor records alone are insufficient for geographic trajectory forecasting.

## Project Structure

```text
ML_CP/
├── data_prep.py
├── model.py
├── train.py
├── requirements.txt
├── README.md
├── .gitignore
└── checkpoints/        # Generated locally
```

## Installation

Requires Python and pip.

```bash
python -m venv .venv
```

Activate the environment on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
python -m pip install -r requirements.txt
```

## Usage

Prepare a GPS CSV with these columns:

- `timestamp`
- `individual-local-identifier`
- `location-long`
- `location-lat`

Run the training pipeline using the path to your local GPS file:

```bash
python train.py --data-path "PATH_TO_YOUR_GPS_FILE.csv"
```

Optional parameters include `--epochs`, `--batch-size`, `--max-animals`, and `--device`.

The training script saves the best validation checkpoint to `checkpoints/trajectory_lstm.pt` by default and reports final test loss.

## Reported Experiment

An earlier experiment reported a best validation MSE of approximately 0.006. This result has not been independently reproduced as part of this repository documentation.

MSE is calculated on raw latitude and longitude coordinates, so it should not be interpreted directly as a distance in kilometres.

## Future Improvements

- Incorporate temperature, vegetation, and other environmental features.
- Compare LSTM performance with Transformer and sequence-to-sequence models.
- Evaluate spring and autumn migration separately.
- Visualize actual and predicted trajectories on geographic maps.
- Evaluate forecast errors using meaningful geographic distance metrics.

## Technologies

Python · PyTorch · Pandas · NumPy · Scikit-learn · Matplotlib · LSTM

## Author

Shrushti Balode