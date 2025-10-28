# ⚙️ Commodity Storage Contract Pricing Model

## 📘 Project Objective
The objective of this project was to create a **prototype pricing model** for a commodity storage contract, incorporating key cost components and the commodity's purchase and sale prices to determine the contract value.

The project enables **traders and analysts** to estimate the *fair value* of storage contracts under varying market conditions — helping identify **profitable opportunities** and understand **cost impacts**.

---

## ⚙️ Pricing Model Implementation
A **Python-based model** was developed to calculate the fair value of a storage contract.  
The core function **`calculate_contract_value()`** takes into account the following key input parameters:

- **Injection (purchase) date(s)**  
- **Withdrawal (sale) date(s)**  
- **Commodity purchase and sale prices**  
- **Injection/withdrawal rates**  
- **Storage capacity (MMBtu)**  
- **Monthly storage cost**  
- **Injection and withdrawal costs**  
- **Transportation costs**

The model performs the following steps:
1. Computes **total costs** (storage, injection/withdrawal, and transportation).  
2. Calculates **total revenue** = (sale_price − purchase_price) × storage_capacity.  
3. Derives the **net contract value** = total revenue − total costs.  
4. Automatically finds the *nearest valid date* in the price dataset when an exact match is missing, ensuring **robust real-world applicability**.

---

## 🧪 Model Testing

### Example Calculation

| Parameter | Value |
|------------|--------|
| **Purchase Date** | 2023-10-31 |
| **Sale Date** | 2024-03-31 |
| **Purchase Price** | $11.80 |
| **Sale Price** | $12.70 |
| **Storage Capacity** | 1,000,000 MMBtu |
| **Monthly Storage Cost** | $0.05/MMBtu/month |
| **Injection Cost** | $0.02/MMBtu |
| **Withdrawal Cost** | $0.03/MMBtu |
| **Transportation Cost** | $0.01/MMBtu |

✅ **Calculated Contract Value:** `$590,328.52`

---

### Additional Test Cases

The model was tested using multiple parameter combinations to evaluate its consistency and performance.

| **Test Case** | **Purchase → Sale** | **Contract Value ($)** |
|---------------|----------------------|-----------------------:|
| Case 1 | 2023-03-01 → 2023-09-01 | 562,697.11 |
| Case 2 | 2023-05-01 → 2023-11-01 | 125,376.15 |
| Case 3 | 2023-07-01 → 2023-12-01 | 690,328.52 |
| Case 4 | 2023-09-01 → 2024-02-01 | 709,975.03 |
| Case 5 | 2023-10-31 → 2024-03-31 | 590,328.52 |

---

## 📊 Visualization
**Test Case Comparison**

A **bar chart** was generated comparing contract values across the five scenarios, showing how **timing and market conditions** influence profitability.

---

## 🌦️ Seasonal Analysis
A seasonal analysis was performed to explore how **purchase/sale timing** affects storage profitability while keeping other factors constant (storage duration ≈ 152 days and fixed costs).

| **Seasonal Case** | **Period** | **Contract Value ($)** |
|--------------------|------------|-----------------------:|
| Case 1 | Jan → Jul | -1,209,671.48 |
| Case 2 | Apr → Sep | -509,671.48 |
| Case 3 | Jul → Dec | 990,328.52 |
| Case 4 | Oct → Apr | 1,090,328.52 |
| Case 5 | Jan → Jul (next year) | -1,409,671.48 |

A **seasonal contract value chart** highlights the strong dependence of profitability on timing —  
buying in **summer/fall** and selling in **winter** yields the highest returns.

---

## 🔍 Seasonal Analysis Key Findings

- ❄️ **Negative Contract Values in Winter/Spring:**  
  Occur when gas is purchased during *high-price winter months* and sold in *lower-price summer periods*, leading to losses.  
  *Example:* Jan–Jul 2021 (−$1.2 M), Jan–Jul 2022 (−$1.4 M).

- ☀️ **Positive Contract Values in Summer/Fall:**  
  Buying in *summer/fall* and selling in *winter* results in strong profits due to **seasonal demand spikes**.  
  *Example:* Jul–Dec 2021 ($0.99 M), Oct–Apr 2021-22 ($1.09 M).

- 💰 **Price Differential Dominance:**  
  A **$1/MMBtu** increase in price spread improves profitability by roughly **$1 million** for a storage capacity of 1 million MMBtu.

- ⚙️ **Storage & Operational Costs:**  
  Play a smaller yet significant role in shaping net profitability.  
  Total costs were approximately **$309,671.48** in all cases.

---

## 🧾 Conclusion
The developed **Commodity Storage Pricing Model** effectively estimates **fair contract values** by combining historical price data with operational cost factors.

**Key takeaways:**
- ✅ The model is **accurate, generalizable, and automated** for real-world contract evaluations.  
- 📈 **Seasonal trends** play a crucial role in determining profitability.  
- 🧠 The **nearest-date matching logic** ensures reliability even with incomplete datasets.  
- 💵 Contract values ranged from **−$1.4 M to +$1.1 M**, proving how **market timing** significantly affects outcomes.

This project forms a strong base for further enhancements such as:
- 🔮 Integrating **forecasting models** (e.g., SARIMA, Prophet)  
- 🎲 Adding **risk simulations** (e.g., Monte Carlo)  
- 📊 Developing a **Power BI dashboard** for real-time analytics

---

## 👨‍💻 Author
**Rishabh Chandrakar**  
*Data Analytics | Python | Power BI | SQL | R*  
🔗 [LinkedIn](https://www.linkedin.com/in/rishabh-chandrakar)  
📞 8963976273

