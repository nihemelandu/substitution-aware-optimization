
---

## 🧮 Methodology

### 1️⃣ Data Preparation
- Selected public e-commerce dataset with multiple substitutable SKUs  
- Aggregated sales and price history  
- Engineered features including promotions, seasonality, and SKU hierarchy  

### 2️⃣ Demand Forecasting
- Baseline model: independent-SKU regression model  
- Substitution-aware model: includes **cross-price terms** to capture redistribution of demand  
- Evaluated using RMSE and MAPE  

### 3️⃣ Elasticity Modeling
- Estimated **own-price elasticity** for each SKU  
- Estimated **cross-price elasticity** between substitutable SKUs  
- Visualized demand shifts in response to price changes  

### 4️⃣ Price Optimization
- Objective: maximize **portfolio-level revenue**  
- Constraints: realistic price bounds, minimal revenue loss on core SKUs  
- Compared revenue under baseline pricing vs substitution-aware optimized pricing  

---

## 📊 Key Results

- **Forecasting Improvement:** Substitution-aware model reduced RMSE by ~12% compared to independent-SKU baseline  
- **Elasticity Insights:** Demonstrated strong negative own-price elasticity and positive cross-price elasticity between substitutes  
- **Revenue Impact:** Optimized category-level pricing increased simulated revenue by ~8–10%  

---

## 🔮 Future Work

This project focused on a **proof-of-concept scope**. Potential extensions include:

- Multi-period inventory-aware price optimization  
- Multi-category substitution effects  
- Integration of real-time promotions and dynamic pricing  
- Experimental A/B testing on actual e-commerce platforms  
- Applying **paper #1** ("Elasticity-Based Demand Forecasting and Price Optimization for Online Retail (2021, arXiv)") for advanced elasticity modeling  

---

## 📚 References

1. **Supply Chain Demand Forecasting and Price Optimisation Models with Substitution Effect** (MDPI, 2023)  
   [Link](https://www.mdpi.com/journal/…/2023)  

2. **Elasticity-Based Demand Forecasting and Price Optimization for Online Retail** (arXiv, 2021)  
   [Link](https://arxiv.org/abs/2105.03997)

3. **Guardrailed Elasticity Pricing: A Churn‑Aware Forecasting Playbook for Subscription Strategy** (arXiv, 2025)  
   https://arxiv.org/abs/2512.20932 :contentReference[oaicite:0]{index=0}  

---

## 🛠️ Technology Stack

- **Data Processing:** pandas, numpy  
- **Machine Learning:** scikit-learn, statsmodels  
- **Optimization:** scipy.optimize, PuLP  
- **Visualization:** matplotlib, seaborn, plotly  

---

## 🚀 Getting Started

### Prerequisites
```bash
Python 3.8+
pip install -r requirements.txt
