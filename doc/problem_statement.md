# Problem Statement: Substitution-Aware Demand Forecasting for Inventory Optimization

## Executive Summary

Standard inventory optimization models forecast demand for each SKU independently, ignoring **stockout-based substitution effects**—the reality that when a product is unavailable, customer demand partially shifts to substitute products within the category. This leads to systematically biased demand forecasts, which in turn produce suboptimal inventory allocation decisions, resulting in excess holding costs, unnecessary safety stock, and degraded service levels.

This project develops a **substitution-aware demand forecasting framework** that captures cross-SKU demand flows during stockout events, producing more realistic demand estimates that serve as improved inputs to existing inventory optimization models.

---

## Background

### Traditional Approach: Independent SKU Forecasting

In conventional inventory management systems:
```
For each SKU i:
  1. Forecast demand: D̂ᵢ = f(historical_salesᵢ, seasonality, promotions)
  2. Calculate safety stock: SSᵢ = z × σᵢ × √LT
  3. Set reorder point: ROPᵢ = D̂ᵢ × LT + SSᵢ
  4. Optimize: min[holding_cost + stockout_cost]
```

**Critical Assumption:** Demand for SKU *i* is independent of the availability of SKU *j*.

### The Reality: Stockout-Driven Substitution

When a customer's preferred product is out of stock, they may:
1. **Substitute** to an alternative product in the category (captured demand)
2. **Defer** the purchase to a later time (delayed demand)
3. **Exit** the category entirely (lost demand)

**Impact on demand patterns:**
- SKU A stocks out → demand flows to SKUs B, C, D
- During A's stockout, observed sales of A are **artificially low**
- During A's stockout, observed sales of B, C, D are **artificially high**
- Historical sales data reflects a mixture of primary demand + substitution flows

---

## The Problem

### Consequences of Ignoring Substitution

When substitution effects are ignored, three systematic errors occur:

#### 1. **Overestimated Lost Sales**
```
Traditional calculation:
  Lost sales for SKU A = Demand - min(Demand, Stock)
  
Reality with substitution:
  Lost sales for SKU A = (1 - Σⱼ αᵢⱼ) × [Demand - min(Demand, Stock)]
  
Where αᵢⱼ = proportion of A's unsatisfied demand captured by SKU j
```

**Consequence:** Inflated shortage costs lead to excessive safety stock for frequently stocked-out items.

#### 2. **Biased Demand Forecasts**

For a SKU that frequently **captures** substitution demand:
- Historical sales include: Primary demand + Substitution inflows
- Forecast based on historical sales **overestimates** true primary demand
- When inventory policies improve (fewer stockouts), sales may **decline**

For a SKU that frequently **loses** demand to substitutes:
- Historical sales reflect: Primary demand - Substitution outflows  
- Forecast based on historical sales **underestimates** true primary demand
- Improvement in availability reveals hidden primary demand

#### 3. **Suboptimal Category-Level Inventory Allocation**

Without understanding substitution flows:
- High-substitution-rate products are under-stocked (opportunity cost)
- Low-substitution-rate products are over-stocked (holding cost)
- Total category inventory is misallocated across SKUs

**Example scenario:**
```
Premium SKU (low substitutability) → over-stocked
Budget SKU (high substitutability)  → under-stocked

Result: Excess capital tied up in slow-moving premium inventory
        while budget customers frequently face stockouts
```

---

## Proposed Solution

### Two-Stage Framework

#### **Stage 1: Substitution-Aware Demand Forecasting**

Estimate the **substitution flow matrix** [α] from historical transaction data:
```
           To: SKU 1  SKU 2  SKU 3  Lost
From: SKU 1    —      0.35   0.25   0.40
      SKU 2   0.45     —     0.30   0.25  
      SKU 3   0.20   0.40     —     0.40
```

Where αᵢⱼ = proportion of SKU *i*'s stockout demand that flows to SKU *j*.

**Decompose observed demand into components:**
```
Observed_salesᵢ(t) = Primary_demandᵢ(t) + Σⱼ [αⱼᵢ × Stockout_demandⱼ(t)]
```

**Generate improved forecasts:**
```
D̂ᵢ_primary = Forecast of underlying primary demand (before substitution)

D̂ᵢ_effective = D̂ᵢ_primary + Σⱼ [αⱼᵢ × P(stockout_j) × D̂ⱼ_primary]
```

Where `P(stockout_j)` is calculated from current inventory policy.

#### **Stage 2: Inventory Optimization with Improved Inputs**

Use substitution-aware forecasts as inputs to existing inventory optimization models:

**Input transformation:**
```
Old input: D̂ᵢ (biased historical sales-based forecast)
New input: D̂ᵢ_effective (substitution-adjusted forecast)
```

**The optimization model itself remains unchanged:**
```
Decision variables: Qᵢ (order quantities or base stock levels)

Objective: min Σᵢ [hᵢ × E[Inventory_i] + pᵢ × E[Shortage_i]]

Constraints:
  - Service level targets: P(stockout_i) ≤ βᵢ
  - Budget: Σᵢ cᵢ × Qᵢ ≤ B
  - Capacity: Σᵢ vᵢ × Qᵢ ≤ V
```

**Key insight:** We improve the **parameters** of the optimization model (demand forecasts), not the **structure** of the model.

---

## Expected Benefits

### 1. **More Accurate Demand Forecasts**
- Separate true primary demand from substitution-inflated sales
- Reduce forecast bias for both high-capture and high-loss SKUs
- Better anticipate demand when availability patterns change

### 2. **Improved Inventory Allocation**
- Right-size safety stock across the category
- Allocate more inventory to products that effectively capture substitution
- Reduce total inventory while maintaining or improving service levels

### 3. **Better Service Level Attainment**
- True lost sales (category-level) < sum of SKU-level apparent lost sales
- More realistic understanding of customer satisfaction
- Prioritize availability for low-substitutability products

### 4. **Reduced Costs**
- **Holding cost reduction:** Eliminate excess safety stock from inflated forecasts
- **Shortage cost reduction:** Better availability for truly critical SKUs
- **Opportunity cost recovery:** Free up capital from over-stocked items

---

## Success Metrics

### Forecast Accuracy
- **Baseline:** RMSE of traditional independent forecasts
- **Target:** X% improvement in RMSE with substitution-aware forecasts
- **Validation:** Out-of-sample forecast accuracy on holdout period

### Inventory Efficiency
- **Metric 1:** Total inventory holding cost
- **Metric 2:** Inventory turnover ratio
- **Metric 3:** Days of inventory on hand

### Service Level Performance
- **Metric 1:** SKU-level in-stock rate
- **Metric 2:** Category-level customer satisfaction (survey-based)
- **Metric 3:** Lost sales rate (actual vs. perceived)

### Business Impact
- **ROI:** (Cost savings) / (Implementation cost)
- **Payback period:** Time to recover implementation investment
- **Scalability:** Ability to extend to additional product categories

---

## Scope and Constraints

### In Scope
- Stockout-based substitution within a product category
- Demand forecasting improvements for inventory optimization
- Single-echelon inventory systems (retail/distribution center)
- Products with sufficient transaction history (>24 months preferred)

### Out of Scope
- Cross-category substitution effects
- Price-based substitution (promotional cannibalization)
- Multi-echelon supply chain optimization
- Real-time dynamic pricing decisions
- Assortment optimization (which SKUs to carry)

### Key Assumptions
1. Substitution proportions (αᵢⱼ) are relatively stable over time
2. Stockout events are observable in transaction data
3. Lost sales can be estimated (via lost sales tracking or demand modeling)
4. Products within category are genuinely substitutable (consumer perspective)
5. Inventory replenishment lead times are known and relatively stable

---

## Technical Approach

### Data Requirements

**Minimum viable dataset:**
```
1. Transaction-level sales data (SKU, date, quantity, stockout flag)
2. Inventory levels (daily/weekly snapshots)
3. Product attributes (category, brand, size, price)
4. Cost structure (holding cost, shortage cost, purchase cost)
```

**Enhanced dataset (if available):**
```
5. Lost sales tracking (customer requests for OOS items)
6. Customer-level purchase history (loyalty program data)
7. Competitive availability (if multi-retailer)
8. Demand drivers (promotions, seasonality, weather, events)
```

### Methodology Components

#### **Component 1: Substitution Matrix Estimation**

**Approach:** Panel regression with fixed effects
```python
sales_jt = β₀ + β₁ × stockout_it + β₂ × price_jt + β₃ × promotion_jt 
          + αⱼ + γₜ + εⱼₜ

Where:
  - sales_jt = sales of product j at time t
  - stockout_it = indicator that product i is out of stock at time t
  - β₁ captures substitution flow from i to j
  - αⱼ = product fixed effect
  - γₜ = time fixed effect
```

**Alternative:** Structural demand models (MNL, nested logit)

#### **Component 2: Primary Demand Forecasting**

**Options:**
- Time series models (ARIMA, ETS, Prophet)
- Machine learning (Random Forest, XGBoost, LSTM)
- Hybrid approaches (ML with exogenous variables)

**Key:** Model primary demand, not observed sales

#### **Component 3: Effective Demand Calculation**

Iterative process to account for substitution:
```python
for iteration in range(max_iterations):
    for sku_i in category:
        P_stockout_i = calculate_stockout_probability(Q_i, D_primary_i)
        
        D_effective_i = D_primary_i + sum(
            alpha[j][i] * P_stockout_j * D_primary_j 
            for j in category if j != i
        )
    
    if convergence_criterion_met:
        break
```

#### **Component 4: Optimization Integration**

Standard newsvendor or (s,S) policy with adjusted inputs:
```python
# Traditional approach
Q_i = F_inv(critical_ratio) using D_historical

# Substitution-aware approach  
Q_i = F_inv(critical_ratio) using D_effective

Where:
  critical_ratio = (shortage_cost - holding_cost) / shortage_cost
```

---

## Related Work

### Foundational Literature

**Netessine & Rudi (2003)** - *Operations Research*
- Established theoretical framework for inventory with stockout-based substitution
- Proved profit function can be non-concave even for two products
- Derived first-order conditions for optimal stocking with substitution flows

**Mahajan & van Ryzin (2001)** - *Management Science*
- Inventory competition with dynamic substitution
- Consumer choice models integrated with inventory decisions

**Smith & Agrawal (2000)** - *Management Science*  
- Multi-product inventory with substitution and lost sales
- Computational approaches for complex substitution structures

### Practical Applications

**Retail industry case studies:**
- Fast-fashion retailers (Zara, H&M) managing size/color substitution
- Grocery retailers managing brand substitution within categories
- E-commerce platforms with real-time substitute recommendations

---

## Implementation Roadmap

### Phase 1: Proof of Concept (Weeks 1-4)
- Select pilot product category (high substitution potential)
- Estimate substitution matrix from 2+ years of transaction data
- Build primary demand forecasts (baseline models)
- Compare forecast accuracy: traditional vs. substitution-aware

**Deliverable:** PoC report with forecast accuracy improvement metrics

### Phase 2: Optimization Integration (Weeks 5-8)
- Develop effective demand calculation module
- Integrate with existing inventory optimization tool
- Run parallel simulation (current policy vs. proposed policy)
- Validate cost/service level improvements

**Deliverable:** Business case with projected ROI

### Phase 3: Pilot Deployment (Weeks 9-16)
- Deploy in single store/DC for pilot category
- Monitor actual vs. predicted outcomes
- Refine substitution estimates with new data
- Iterate on forecast models

**Deliverable:** Pilot results report

### Phase 4: Scale and Operationalize (Weeks 17-26)
- Expand to additional categories
- Automate substitution matrix updates
- Integrate into production S&OP process
- Train business users

**Deliverable:** Production system ready for enterprise rollout

---

## Risk Mitigation

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Substitution patterns change over time | High | Medium | Monthly re-estimation of α matrix; monitor drift |
| Insufficient stockout data to estimate α | High | Low | Use external benchmarks; survey-based estimates |
| Non-concave optimization landscape | Medium | Medium | Global optimization algorithms; multi-start heuristics |
| Business resistance to forecast changes | Medium | Medium | Phased rollout; strong validation; executive sponsorship |
| IT integration complexity | Low | High | API-based integration; minimize system changes |

---

## Conclusion

By explicitly modeling stockout-based substitution effects, this framework transforms how demand forecasts are generated for inventory optimization. Rather than treating each SKU in isolation, we capture the interconnected nature of customer demand when products are unavailable.

**The core innovation:** Better inputs (substitution-aware forecasts) → Better outputs (optimal inventory allocation), without requiring changes to the underlying optimization model structure.

**The business value:** Reduced inventory costs, improved service levels, and better capital efficiency through more accurate understanding of true customer demand patterns.

---

## References

1. Netessine, S., & Rudi, N. (2003). Centralized and Competitive Inventory Models with Demand Substitution. *Operations Research*, 51(2), 329-335.

2. Mahajan, S., & van Ryzin, G. (2001). Inventory Competition Under Dynamic Consumer Choice. *Management Science*, 47(10), 1452-1466.

3. Smith, S. A., & Agrawal, N. (2000). Management of Multi-Item Retail Inventory Systems with Demand Substitution. *Operations Research*, 48(1), 50-64.

4. Bassok, Y., Anupindi, R., & Akella, R. (1999). Single-Period Multiproduct Inventory Models with Substitution. *Operations Research*, 47(4), 632-642.

---

**Document Version:** 1.0  
**Last Updated:** February 7, 2026  
**Status:** Draft for Review
