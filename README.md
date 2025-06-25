# PML – Cancún

## Introduction
### Context  
Analysis of the Local Marginal Price (PML) behaviour at the Cancún node in the **Day‑Ahead Market**.

### Purpose  
* Understand seasonal patterns  
* Highlight price anomalies  
* Forecast future PML movements  

---

## Seasonality

### Summer vs. Winter  
* **Peaks in May – July** during high‑tourism season → increased air‑conditioning demand.  
* **Price drops in colder months (Dec – Feb)** when overall electricity usage falls.

### Time‑Series: PML & Congestion  
* Congestion component closely tracks total PML, reinforcing that network constraints are a key driver of price spikes.

### Long‑Term Trend  
* Slight volatility decline after the mid‑2024 spike.  
* **Upturn in 2025** hints at rising base demand or persistent transmission limits.

### Anomalies  
* Negative PML values → refunds or atypical reverse‑flow events.  
* Huge PML jump in **June 2024 (~30 000 $/MWh)** signals an exceptional outage or heatwave.

---

## Monthly Trend

| Period | Typical Price Level | Main Drivers |
|--------|---------------------|--------------|
| **Winter Low Season (Dec – Feb)** | ~700 – 1 100 $/MWh | Mild weather, lower demand |
| **Spring Rise (Mar – May)** | ~1 400 → 3 200 $/MWh | Rising temps, tourism |
| **Summer Peak (July)** | ~3 700 $/MWh | Maximum congestion |
| **Autumn Decline (Jul – Oct)** | ~3 200 → 2 100 $/MWh | Flow normalisation |
| **Return to Lows (Nov – Dec)** | ~1 000 → 700 $/MWh | Demand subsides |

---

## Day‑of‑Week Pattern

* **Monday → Friday:** Median PML increases steadily – Friday is the **highest & most volatile** day.  
* **Saturday:** Slight drop compared with Friday.  
* **Sunday:** Lowest median and narrowest variability.  
* **Right‑skewed tails:** Rare but extreme congestion spikes.

---

## Hour‑by‑Day Heat‑Map Highlights

* **Evening maxima (21:00 – 23:00, Mon‑Fri):** 1 800 – 2 000 $/MWh.  
* **Daytime shoulder (13:00 – 16:00):** Moderate congestion.  
* **Weekend relief:** Lower values on Saturdays; significantly calmer on Sundays.  
* **Night‑time lows (00:00 – 06:00):** < 200 $/MWh.  
* **Morning ramp‑up (08:00 – 12:00):** 600 – 1 000 $/MWh.

---

## Feature Engineering

| Category | Description |
|----------|-------------|
| **Cyclical Features** | Encode hour, weekday, and month as sine & cosine pairs. |
| **Time‑Series Lags** | Previous‑hour price and previous‑day same‑hour price capture intra‑day autocorrelation. |
| **Moving Averages** | Smooth recent trends to provide short‑term context. |

---

## Model Development

* **Temporal Split:** 5‑fold expanding `TimeSeriesSplit` (train on older data → test on more recent).  
* **Algorithm:** `RandomForestRegressor` – robust to collinearity, no scaling required.

### Performance  
* **RMSE:** 775 $/MWh  
* **R²:** 0.85 (explains 85 % of variance)  

**Improvements:**  
1. Hyper‑parameter tuning  
2. Add exogenous features (temperature, tourism demand)  
3. Explore seasonal models (SARIMAX, Prophet)

---

## Predictions – 24‑Hour Forecast (Sample Insights)

* General shape captured, **morning/afternoon hours fit well**.  
* **Early‑morning (01:00 – 04:00) under‑predicted.**  
* **Evening peak (17:00 – 23:00) too smoothed.**  
* Small dip around 19:00 – 20:00 slightly over‑estimated.

---

## Dashboard Proposal

* Average PML  
* Maximum & Minimum PML  
* Average Congestion %  
* PML vs. Congestion over time  
* Energy + Congestion + Losses breakdown  
* Hour‑by‑Day heat‑map  

---

## Conclusions

### Key Findings
* Clear **seasonality** in Cancún PML.  
* Highest tariffs cluster **Mon‑Fri, 21:00 – 23:00**.  

### Limitations
* Lack of exogenous variables.  
* Extreme peaks not fully captured.

### Business Value
* Ability to anticipate most hourly congestion peaks → plan hedging or buy/sell strategies based on the forecast.
