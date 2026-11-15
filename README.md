# SaveSmart AI

> **Project:** Senior Design Project (team) · BSc Computer Engineering, Khalifa University · 2nd place in energy efficiency, Solar Decathlon Middle East

Forecasting code for the SaveSmart app, uploaded by a teammate. It trains an LSTM model (Keras) on hourly power-use data and forecasts power use for the next week. The repo also has a script that reads live power from a smart plug.

Part of the SaveSmart senior design project. See also the [Xamarin app prototype](https://github.com/MariamAlz/SaveSmart). The team's final app, which runs a version of this pipeline, is in [SaveSmart-App](https://github.com/MariamAlz/SaveSmart-App).

## Features

- **LSTM forecaster** (`model/AI.py`): the `DeepModelTS` class turns a time series into lagged input windows. It trains a one-layer LSTM with a dense output (Adam optimiser, MSE loss), predicts on a hold-out split and forecasts *n* steps ahead recursively.
- **Pipeline** (`model/pipeline.py`): loads `model/data/DAYTON_hourly.csv`, averages duplicate timestamps and sorts by time. It then trains on 90% of the data and plots the forecast against the last 10%. Finally it retrains on all the data and plots a 168-hour (one-week) forecast.
- **Configuration** (`model/conf.yml`): lag of 24 hours, an LSTM layer of 64 units, 10 epochs and a 0.10 validation split.
- **Smart plug reading** (`scripts/HS110.py`): polls a TP-Link HS110 smart plug on the local network once a second with `python-kasa` and prints its real-time power. Set the plug's IP address in the script.
- **Regressor set-up** (`model/ml_tests.py`): loads the same data and defines a set of scikit-learn regressors (linear, lasso, ridge, elastic net, Huber, LARS, decision tree and others) for comparison.

## Project structure

```
model/
  AI.py              DeepModelTS: data windowing, LSTM training, prediction
  pipeline.py        Train, validate, forecast 168 hours ahead and plot
  conf.yml           Pipeline hyperparameters
  ml_tests.py        scikit-learn regressor set-up
  data/DAYTON_hourly.csv   Hourly load data (Datetime, DAYTON_MW), 2004 to 2018
scripts/
  HS110.py           Real-time power reading from a TP-Link HS110 plug
```

## Getting started

```sh
pip install tensorflow "pandas<2" numpy matplotlib pyyaml scikit-learn python-kasa
python model/pipeline.py
```

Notes:

- It uses `DataFrame.append`, which pandas 2.0 removed, so install pandas 1.x.
- `python scripts/HS110.py` needs an HS110 plug on the same network.

## Tech

Python, Keras/TensorFlow, pandas, NumPy, Matplotlib, PyYAML, scikit-learn, python-kasa.

## Credits

Code uploaded by a SaveSmart teammate. `DAYTON_hourly.csv` is hourly load data for the Dayton area from PJM Interconnection.
