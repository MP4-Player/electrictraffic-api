# ElectricTraffic API

Backend of **ElectricTraffic**: a hackathon project for forecasting electricity consumption and helping customers choose the most cost-effective tariff category.

> This is a copy of the team repository [Dodosters/electrictraffic-api](https://github.com/Dodosters/electrictraffic-api), kept with its full commit history.
> Frontend: [electrictraffic-web](https://github.com/MP4-Player/electrictraffic-web).

## What it does

- **Consumption forecasting**: upload historical consumption data and get a forecast for the requested number of months ahead.
- **Tariff calculation**: compares the four electricity price categories using configurable coefficients (tariff zones, hourly rates).
- **Hourly consumption analysis**: parses Excel reports with hourly consumption and computes costs.
- **Admin endpoints**: read and update tariff coefficients for each price category.

## Forecasting model

Ensemble of time-series and regression models; the final prediction is the mean of all component forecasts:

| Group | Models |
|---|---|
| Linear | Linear Regression, Ridge, Lasso, ElasticNet |
| Tree-based | Random Forest, Gradient Boosting, XGBoost |
| Time series | Holt-Winters (Exponential Smoothing) |

Forecasts beyond one step are generated recursively, and trained models are cached on disk per uploaded dataset.

## Architecture

```
electrictraffic-web (React + Vite)
          │  REST / JSON
          ▼
electrictraffic-api (FastAPI)
   ├── main.py            – tariff coefficients, Excel analysis, hourly consumption
   ├── prediction_api.py  – ensemble training & forecasting
   └── analyse.py         – consumption analysis helpers
```

## Quick start

```bash
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000
```

Interactive API docs are then available at `http://localhost:8000/docs`.

Run tests:

```bash
pytest
```

## Tech stack

Python · FastAPI · Pydantic · pandas · scikit-learn · XGBoost · statsmodels

## Team

Built by the **Dodosters** team at a hackathon.

**My role (Mark Bulgarov, [@MP4-Player](https://github.com/MP4-Player))**: ML models for consumption forecasting and the AI chat assistant. The team shared laptops during the event, so my work was committed from teammates' accounts.

## License

[Apache License 2.0](LICENSE)
