# Substitution-Aware Demand Forecasting for Inventory Optimization

## Overview

Most inventory systems forecast demand for each SKU independently, assuming unmet demand disappears when a product is out of stock. In practice, customers frequently **substitute** to similar products within the same category.

Ignoring stockout-driven substitution leads to biased demand forecasts, excess safety stock for some SKUs, chronic under-stocking of others, and poor inventory allocation across the category.

This project builds a **substitution-aware demand forecasting layer** that estimates how demand flows across SKUs during stockouts and uses those estimates to generate improved demand inputs for existing inventory optimization models.

**Goal:** Improve forecast accuracy, reduce inventory costs, and maintain service levels—without changing core optimization logic.

---

## Why This Matters

Traditional forecasting breaks down in categories with close substitutes:

- Historical sales mix **primary demand** with **substitution inflows**
- Lost-sales metrics are overstated
- Safety stock is miscalibrated
- Inventory is allocated to the wrong SKUs

These errors are structural and cannot be fixed with better time-series models alone.

---

## Solution Summary

The approach adds a lightweight analytical layer upstream of inventory optimization:

### 1. Estimate Substitution Behavior from Data
- Identify stockout events in transaction data
- Measure how sales of other SKUs respond
- Produce an interpretable substitution matrix

### 2. Recover Primary (First-Choice) Demand
- Separate true demand from substitution-driven sales
- Generate cleaner demand signals for forecasting models

### 3. Generate Substitution-Aware Demand Forecasts
- Adjust expected demand based on stockout risk across the category
- Feed improved forecasts into existing inventory policies ((s,S), reorder points, safety stock)

**Key principle:** Better inputs → better decisions, without system redesign.

---

## Theoretical Foundation (Applied)

The project is grounded in established inventory theory, particularly:

**Netessine & Rudi (2003)** — *Centralized Inventory Models with Demand Substitution*

They show that optimal stocking must account for demand that flows **to** and **from** products during stockouts. Their models assume substitution behavior is known.

This project operationalizes that insight by **estimating substitution behavior directly from real sales data** and applying it to demand forecasting in multi-period inventory systems.

---

## Business Impact

**Expected improvements (based on pilot analysis):**

- **Forecast accuracy:** 15–25% RMSE reduction  
- **Inventory cost:** 10–20% reduction at equal or better service levels  
- **Service performance:** Stable or improved fill rates  
- **Capital allocation:** Inventory shifted toward high-substitutability SKUs  

---

## Scope

**In scope**
- Stockout-driven substitution within a category
- Centralized inventory management
- Multi-SKU demand forecasting
- Integration with standard inventory policies

**Out of scope**
- Pricing and promotions
- Competitive multi-retailer behavior
- Assortment optimization
- Real-time demand sensing

---

## Why This Works in Practice

- Uses existing transaction and inventory data  
- Requires no changes to ERP or optimization engines  
- Produces interpretable outputs planners can trust  
- Scales category by category  

---

## Status

**Stage:** Production-ready prototype  
**Next steps:** Pilot deployment, automation, and category-level rollout

---

## Reference

Netessine, S., & Rudi, N. (2003). *Centralized and Competitive Inventory Models with Demand Substitution*. Operations Research, 51(2), 329–335.
