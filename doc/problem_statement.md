# Problem Statement: Substitution-Aware Demand Forecasting for Inventory Optimization

## Executive Summary

Most inventory planning systems forecast demand for each SKU independently, implicitly assuming that unmet demand disappears when a product is out of stock. In reality, customers often **substitute** to similar products within the same category when their first choice is unavailable.

Ignoring stockout-driven substitution leads to **systematically biased demand forecasts**, which propagate downstream into:
- Excess safety stock for some SKUs  
- Chronic under-stocking of others  
- Poor inventory allocation across the category  

This project develops a **data-driven, substitution-aware demand forecasting layer** that estimates how demand flows across SKUs during stockouts and uses these estimates to produce **better demand inputs** for existing inventory optimization models—without changing the underlying optimization logic.

The goal is **practical impact**: improved forecast accuracy, lower inventory costs, and better service levels using methods grounded in well-established operations research theory and modern data analytics.

---

## Business Context

### How Inventory Forecasting Works Today

In most production environments, demand forecasting and inventory optimization follow a pattern like this:

1. Forecast demand for each SKU using historical sales  
2. Compute safety stock based on forecast error and lead time  
3. Optimize reorder points or order-up-to levels independently per SKU  

This approach is scalable and operationally convenient—but it assumes that:
- Historical sales reflect true underlying demand  
- Demand for one SKU is independent of availability of others  

These assumptions break down in categories with **close substitutes**.

---

## The Core Problem

### Sales Data Is Contaminated by Substitution

When a product stocks out, observed sales no longer represent only its own demand. Instead:

- Some customers substitute to other SKUs
- Some customers leave without purchasing
- The resulting sales data mixes **primary demand** with **substitution inflows**

As a result:
- High-substitutability SKUs appear to have inflated demand
- Low-substitutability SKUs appear to have weak demand
- Lost-sales metrics are overstated
- Safety stock calculations are distorted

These biases are structural and **cannot be fixed by better time-series models alone**.

---

## Why This Matters Operationally

Ignoring substitution leads to three recurring failure modes:

### 1. Misestimated Lost Sales and Shortage Costs

Traditional systems treat all unmet demand as lost. In reality, a significant fraction of demand may shift to other products. This causes planners to:
- Overestimate shortage penalties
- Carry excessive safety stock for certain SKUs

### 2. Biased Demand Forecasts

- SKUs that capture substitution during competitors’ stockouts appear to have strong baseline demand
- SKUs that lose demand during their own stockouts appear weaker than they truly are
- When availability improves, forecasts often fail catastrophically

### 3. Poor Category-Level Inventory Allocation

Inventory is allocated without understanding:
- Which SKUs act as demand sinks
- Which SKUs act as demand sources
- Where incremental inventory creates the most system-wide value

The result is **lower return on inventory investment**, even if overall service levels appear acceptable.

---

## Proposed Solution

### Substitution-Aware Demand Forecasting Layer

This project introduces a lightweight analytical layer that sits **upstream** of existing inventory optimization systems.

The approach has three key components:

1. **Estimate substitution behavior from transaction data**  
   - Quantify how demand shifts across SKUs during stockouts  
   - Produce an interpretable substitution matrix at the category level  

2. **Recover primary (first-choice) demand**  
   - Separate true underlying demand from substitution-driven sales  
   - Generate cleaner demand signals for forecasting models  

3. **Produce substitution-aware demand forecasts**  
   - Adjust expected demand based on stockout risk across the category  
   - Feed improved forecasts into existing inventory optimization tools  

Importantly:
- No changes are required to ERP systems or optimization solvers  
- The solution improves **inputs**, not decision logic  

---

## Theoretical Foundation (Industry-Relevant)

The solution is grounded in established inventory theory, particularly the work of:

**Netessine & Rudi (2003)** — *Centralized Inventory Models with Demand Substitution*

Their key insight:
> Optimal inventory decisions must account for both demand that flows **to** a product and demand that flows **away from** a product during stockouts.

However, classical models assume substitution behavior is **known**. In real operations, it is not.

This project operationalizes that theory by:
- Estimating substitution behavior empirically from data  
- Applying it to demand forecasting in multi-period, real-world systems  

The emphasis is **implementation and impact**, not new theory.

---

## Success Criteria

The solution is evaluated using business-relevant metrics:

- **Forecast accuracy:** Reduction in RMSE vs. independent SKU forecasts  
- **Inventory cost:** Lower holding and shortage costs at equal or better service levels  
- **Service performance:** Stable or improved fill rates with less inventory  
- **Operational fit:** Minimal disruption to existing planning workflows  

---

## Scope

**In scope:**
- Stockout-driven substitution within a single category  
- Centralized inventory management  
- Multi-SKU demand forecasting improvements  
- Integration with standard inventory policies ((s,S), reorder points, safety stock)

**Out of scope:**
- Dynamic pricing and promotions  
- Competitive multi-retailer behavior  
- Assortment optimization  
- Real-time demand sensing  

---

## Outcome

By correcting demand signals for substitution effects, this project enables:

- More accurate forecasts  
- Better inventory allocation across SKUs  
- Lower working capital without sacrificing service  
- A practical path to applying cutting-edge OR insights in production systems  

The result is a **more intelligent demand input layer** that allows existing inventory optimization tools to perform as intended—using data that better reflects how customers actually behave.
