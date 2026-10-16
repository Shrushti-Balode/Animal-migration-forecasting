## Animal Migration Forecasting with Climate Context

This project builds a deep learning model to **forecast migratory animal trajectories** (starting with white storks from Movebank) using GPS-like time series data, optionally conditioned on environmental features.

### 1. Project structure

- `white_stork_gps.csv/`  
  Raw Movebank export (currently contains acceleration data; you will typically also export GPS locations as a separate file).
- `requirements.txt`  
  Python dependencies.
- `data_prep.py`  
  Utilities to load, clean, and window Movebank-style GPS data into model-ready sequences.
- `model.py`  
  PyTorch sequence model (LSTM) for trajectory forecasting.
- `train.py`  
  End-to-end training script.

### 2. Expected GPS data format

For **trajectory forecasting**, the loader in `data_prep.py` assumes a **Movebank GPS export** with at least:

- `timestamp` – datetime string
- `individual-local-identifier` – animal ID
- `location-long` – longitude
- `location-lat` – latitude

Your current file  
`white_stork_gps.csv/LifeTrack White Stork SW Germany_2013-2019-acceleration-1of3.csv`  
is an **acceleration sensor file** (no latitude/longitude), so it cannot by itself be used for migration path prediction.

To run the full trajectory pipeline you should:

1. Export the **GPS (location)** data for the same study from Movebank as CSV.
2. Save it under, for example:

   - `white_stork_gps.csv/white_stork_locations.csv`

3. Ensure it has the columns listed above (Movebank exports usually do).

The code will then use that GPS file as the main data source.

### 3. Setup

```bash
python -m venv .venv
.venv\Scripts\activate  # on Windows
pip install -r requirements.txt
```

### 4. Basic usage

After you have a GPS CSV (e.g. `white_stork_gps.csv/white_stork_locations.csv`), run:

```bash
python train.py --data-path white_stork_gps.csv/white_stork_locations.csv
```

This will:

1. Load and clean the data.
2. Build sliding-window sequences per individual.
3. Train an LSTM model to predict the **next K future positions** from the last **T past positions**.
4. Save the trained model weights to `checkpoints/trajectory_lstm.pt`.

### 5. Next steps

- Add climate / vegetation features by joining external datasets (e.g. MODIS NDVI, temperature) on time and approximate location, then extending the feature vector in `data_prep.py`.
- Experiment with more advanced architectures (`TransformerEncoder`, seq2seq, graph-based models).
- Evaluate migration forecasts seasonally and under different climate scenarios.

