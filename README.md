# Retail Intelligence | Databricks End-to-End Analytics

An end-to-end portfolio project demonstrating API ingestion, PySpark transformations, exploratory data analysis, data quality, a Bronze–Silver–Gold medallion architecture, and Power BI-ready analytical tables.

## Architecture

```text
SondaHub Store REST API
        |
        v
Raw JSON pages in Unity Catalog Volume
        |
        v
BRONZE  - raw API page payloads + ingestion metadata (Delta)
        |
        v
SILVER  - parsed, flattened, typed entity tables (Delta)
        |
        v
GOLD    - dimensions, facts and business KPI aggregates (Delta)
        |
        v
Power BI semantic model and dashboards
```

## Data source

The notebook uses the public synthetic SondaHub Store API (no API key required):
- Docs: https://www.sondahub.com/apis/store/
- Base URL: https://api.sondahub.com/v1/store

It retrieves categories, products, customers, orders, order items, reviews, warehouses, inventory and carts. API availability and pagination behaviour may change; inspect the response metadata if the source contract changes.

## Run in Databricks

1. Import `notebooks/retail_intelligence_end_to_end.ipynb` into your Databricks workspace.
2. Attach a recent Databricks Runtime with PySpark and Unity Catalog access.
3. Update the catalog name in the configuration cell.
4. Ensure you can create schemas and a Volume in that catalog.
5. Run the notebook from top to bottom. The API ingestion cell writes raw JSON pages to the Volume; Bronze, Silver and Gold are Delta tables.
6. Connect Power BI to the Gold tables using your preferred Databricks SQL Warehouse connection.

## Gold outputs

The notebook builds a sales fact table and supporting dimensions / aggregate tables where source fields permit:
- `dim_product`, `dim_customer`, `dim_date`
- `fact_order_items`
- `agg_monthly_sales`, `agg_category_performance`, `agg_customer_performance`
- `agg_review_performance` and `agg_inventory_health` (when source columns are available)

Because this is synthetic data, validate field names and business definitions against the live API response before interpreting metrics.

## Suggested Power BI pages

1. Executive Overview — revenue, orders, units, average order value and monthly trend.
2. Product & Category — sales, units, product ranking and category mix.
3. Customer Insights — customer value, order frequency and geographic distribution.
4. Inventory — stock on hand and low-stock products.
5. Customer Voice — review volume and average rating.

## Git workflow

```bash
git clone https://github.com/kartikch4/retail_intelligence.git
cd retail_intelligence
git checkout -b feature/<your-change>
# edit notebook / documentation
git add .
git commit -m "Add <change>"
git push -u origin feature/<your-change>
```

Use pull requests to merge changes into `main`. Do not commit credentials, personal data, workspace URLs containing secrets, or exported production data.

## Repository structure

```text
retail_intelligence/
├── notebooks/
│   └── retail_intelligence_end_to_end.ipynb
├── docs/
│   └── power_bi_data_model.md
├── requirements.txt
├── .gitignore
└── README.md
```

## Disclaimer

This is a learning and portfolio project using synthetic public data. It is not affiliated with SondaHub or any retailer. The notebook has not been executed against your specific Databricks workspace; permissions, runtime compatibility and live API responses must be validated there.
