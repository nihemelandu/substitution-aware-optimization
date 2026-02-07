# Substitution-Aware Inventory Optimization

> Improving inventory decisions by modeling stockout-based demand substitution

## Overview

Traditional inventory optimization forecasts demand for each SKU independently, ignoring the reality that when products stock out, customer demand shifts to substitutes. This leads to:

- **Overestimated lost sales** for frequently stocked-out items
- **Biased demand forecasts** that don't reflect true primary demand
- **Suboptimal inventory allocation** across product categories

This project improves inventory optimization by estimating substitution flows between products and generating substitution-aware demand forecasts.

**Core Idea:** Better demand forecasts (accounting for substitution) → Better inventory decisions → Lower costs + Higher service levels

---

## Problem Statement

When SKU A stocks out:
- Some customers buy SKU B instead (substitution)
- Some customers leave without buying (lost sales)
- Historical sales data shows: Sales_A ↓ and Sales_B ↑

**Traditional approach:**
```
Forecast each SKU independently from historical sales
→ Forecasts confuse primary demand with substitution effects
→ Inventory optimization uses biased inputs
```

**Our approach:**
```
1. Estimate substitution matrix from stockout events
2. Separate primary demand from substitution flows
3. Generate substitution-aware forecasts
4. Use improved forecasts in inventory optimization
```

📄 [Full problem statement](docs/PROBLEM_STATEMENT.md)

---

## Quick Start
```bash
# Clone repository
git clone https://github.com/yourusername/substitution-inventory.git
cd substitution-inventory

# Install dependencies
pip install -r requirements.txt

# Run example notebook
jupyter notebook notebooks/01_data_exploration.ipynb
```

---

## Project Status

🚧 **Work in Progress** - Currently in development phase

**Completed:**
- [ ] Data collection and preprocessing
- [ ] Substitution matrix estimation
- [ ] Primary demand forecasting
- [ ] Effective demand calculation
- [ ] Optimization integration
- [ ] Validation and testing

📅 README will be updated with detailed documentation once core functionality is complete.

---

## Repository Structure
```
substitution-inventory/
├── data/                  # Transaction and inventory data
├── src/                   # Source code
│   ├── substitution/      # Substitution estimation
│   ├── forecasting/       # Demand models
│   └── optimization/      # Inventory optimization
├── notebooks/             # Analysis and examples
├── docs/                  # Documentation
└── tests/                 # Unit tests
```

---

## Key References

- Netessine, S., & Rudi, N. (2003). Centralized and Competitive Inventory Models with Demand Substitution. *Operations Research*, 51(2), 329-335.

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

## Contact

Questions or suggestions? Open an [issue](https://github.com/yourusername/substitution-inventory/issues).
