[Česky](../cz/14_SPOT_ENERGY_PREDIKCE.md) | [English](14_SPOT_ENERGY_PREDICTION.md)

# SPOT, energy and prediction

SPOT prices feed automatic export control, morning/evening sales and Spot-Grid Charging. `isSpotAutoCtrlEnabled` enables automatic price-based export decisions using `spotTresholdPrice`. The same price data also feed independent battery strategies.

`nSetPeak` controls the selected peak-window length. `switchSpotGridCharging`, `nCharging_DurationGRID`, `nAcceptable_Price_GRID` and `nMAX_Grid_Point` configure continuous low-cost charging. The block search includes fallback to the first hours; unreliable prices are not guaranteed to disable charging.

Prediction uses VRM energy forecasts. `nPredictionThreshold` enables comparison against `sPredictionThresholdKW`, which is **kWh despite the key name**. Solar forecast must exceed the larger of consumption forecast and this threshold. Invalid source data cause the reference filter to be bypassed (`bPredikce = true`), not a fail-closed stop.

Set the correct server timezone. API SPOT prices are indexed as hourly samples and do not independently validate source interval or daylight-saving day length. Validate them before using them for cost accounting. See [API conventions](api/05_CONVENTIONS_AND_FRESHNESS.md).

[← Documentation](README.md)
