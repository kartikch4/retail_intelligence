# Power BI hand-off

Recommended model:
- `fact_order_items` at order-line grain.
- `dim_product` joined by product_id.
- `dim_customer` joined by customer_id.
- `dim_date` joined to the order date key.

Suggested measures (adjust names to the actual Gold schema):
- Revenue = SUM(fact_order_items[line_revenue])
- Units = SUM(fact_order_items[quantity])
- Orders = DISTINCTCOUNT(fact_order_items[order_id])
- AOV = DIVIDE([Revenue], [Orders])
- Average selling price = DIVIDE([Revenue], [Units])

Keep dimensions on the one-side and the fact on the many-side. Use a proper date table and mark it as the date table in Power BI. Validate treatment of discounts, shipping, tax, refunds and cancellations before presenting revenue as a financial KPI.
