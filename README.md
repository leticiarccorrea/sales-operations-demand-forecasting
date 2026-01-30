# Sales Demand Modeling – Data Preparation Pipeline

This repository contains a **data preparation, validation, and filtering pipeline** designed for **sales demand modeling and S&OP analytics**.  
The goal is to transform raw transactional sales data into a **clean, consistent, and model-ready dataset** for time-series forecasting.

---

## Requirements

To run this project, you need:

- **Python 3.9+**
- The following Python libraries:
  ```bash
  pip install pandas numpy
  ```

---

## Input Dataset Requirements

The raw input dataset must be provided as a **pandas DataFrame** and contain at least the following columns:

- `COD_CICLO`
- `COD_MATERIAL`
- `COD_CANAL`
- `COD_REGIAO`
- `QT_VENDA_BRUTO`
- `QT_DEVOLUCAO`
- `PCT_DESCONTO`
- `VL_PRECO`

Numeric columns may be provided as **PT-BR or EN formatted strings** (e.g. `1.234,56` or `1234.56`).

---

## What the Pipeline Does

The pipeline performs the following steps:

1. Schema validation  
2. Numeric conversion (PT-BR / EN formats)  
3. Column standardization to `snake_case`  
4. Discount normalization to range `0–1`  
5. Net demand and return rate calculation  
6. Time feature extraction (year and cycle)  
7. Campaign flag aggregation  
8. Price imputation using expanding median per SKU (no leakage)  
9. Data quality and time-series filtering  

---

## Configuration

All modeling and data quality rules are controlled by the `ModelInputConfig` dataclass, including:

- Minimum history per time series  
- Minimum number of positive sales points  
- Discount clipping rules  
- Handling of invalid cycles  
- Price imputation behavior  

---

## How to Run

```python
cfg = ModelInputConfig(
    drop_invalid_ciclo=True,
    min_target_value=0.0,
    min_history_per_series=8,
    min_positive_sales_points=3,
    price_fallback_to_global_median=True,
)

model_df, model_report = build_model_input_dataset(base_dataframe, cfg)
```

---

## Outputs

- **model_df**  
  Clean dataset ready for lag features, rolling statistics, and forecasting models.

- **model_report**  
  Dictionary with data quality metrics and filtering statistics.

---

## Typical Use Cases

- Demand forecasting  
- Sales & Operations Planning (S&OP)  
- Campaign impact analysis  
- Hierarchical time-series modeling  

---

## Notes

- Leakage-safe preprocessing
- Production-ready logic
- Suitable for notebooks and batch pipelines

