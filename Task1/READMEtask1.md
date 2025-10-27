# 🧮 Natural Gas Price Forecasting Project

## 📘 Project Overview
This project analyzed historical natural gas prices to identify patterns such as seasonality and develop accurate forecasting models. We explored, cleaned, and transformed the data, implemented multiple models, and created an interactive forecasting tool for future predictions.

---

## 🔍 Data Analysis and Key Findings

- **Data Overview:** 48 monthly records containing `Dates` and `Prices`.
- **Data Preprocessing:** Converted `Dates` to datetime for time series analysis.
- **Initial Visualization:** Line plots showed clear seasonality and yearly cyclic behavior.
- **Seasonality Analysis:** Decomposition confirmed peaks in **winter months** and troughs in **spring/fall**.
- **Stationarity Test (ADF):**
  - Original Series → *Non-stationary* (ADF = 0.218, p = 0.973)
  - After Differencing → *Stationary* (ADF = -6.845, p = 1.75e-09)

---

## 🤖 Forecasting Models and Evaluation

We implemented three time series forecasting models:

| Model | Description | RMSE | MAE |
| :---- | :----------- | ----: | ---: |
| **SARIMA** | Seasonal ARIMA with tuned order (2,0,0)(0,1,1,12) | 0.288 | 0.208 |
| **Prophet** | Additive model by Meta | 0.399 | 0.336 |
| **Holt-Winters** | Triple exponential smoothing (trend + seasonality) | **0.147** | **0.124** |

✅ **Best Model:** Holt-Winters achieved the lowest RMSE and MAE, making it the most accurate for this dataset.

---

## 📈 Forecasting & Interactive Tool

Using the **Holt-Winters model**, we generated a **12-month forecast** and visualized it alongside historical data.

We also developed an **interactive tool** using `ipywidgets`:
- Select **model (SARIMA / Prophet / Holt-Winters)**
- Choose **future date**
- Get predicted price + comparison plot

🛠️ *Default model:* Holt-Winters (best performer)

---

## 📊 Example Forecast — October 31, 2025

| Model | Predicted Price |
| :---- | ---------------: |
| SARIMA | 12.94 |
| Prophet | 12.94 |
| **Holt-Winters** | **13.13** |

Despite close individual predictions, Holt-Winters consistently achieved better accuracy on test data.

---

## 🧠 Conclusion

- Natural gas prices exhibit **strong seasonal patterns**.  
- Stationarity achieved after first differencing.  
- Among SARIMA, Prophet, and Holt-Winters — the **Holt-Winters model outperformed** others with RMSE = 0.147, MAE = 0.124.  
- Recommended model for forecasting this dataset: **Holt-Winters**.

---

## 🚀 Future Work
- Incorporate **external factors** like weather or energy demand.  
- Experiment with **advanced models** (LSTM, XGBoost Regressor).  
- Add **confidence intervals** to quantify uncertainty.

---

## 👨‍💻 Author
**Rishabh Chandrakar**  
📊 Data Analytics | Power BI | Python | SQL  
🌐 [LinkedIn](https://www.linkedin.com/in/rishabh-chandrakar)  
📞 8963976273  

