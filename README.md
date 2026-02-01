# Data Virtualization Project (Denodo) — Unified Sales Performance Reporting

This repository contains the **documentation deliverables** for my individual Data Virtualization project built in **Denodo**.  
I designed an end-to-end **virtualized reporting solution** that integrates 7 retail CSV datasets into a single business-ready Reporting View (RV), then published it as a **REST web service** for lightweight downstream consumption.

---

## Project Overview

**Goal:** Build a maintainable virtualization model that answers sales performance questions across:
- Store/location performance over time
- Brand & category performance
- Discount impact on revenue
- Customer purchasing patterns (orders/customer, revenue/customer)

**Approach:** Layered modeling for clarity & reuse:
**Datasources (ds_*) → Base Views (bv_*) → Integrated Views (iv_*) → Reporting View (RV) → REST API**

---

## Data Sources (7 CSV files)

The solution integrates these operational entities:
- `orders.csv`
- `order_items.csv`
- `products.csv`
- `brands.csv`
- `categories.csv`
- `stores.csv`
- `customers.csv`

*(Some provided files such as `staffs.csv` / `stocks.csv` were not used as they were out of scope for the sales performance reporting objective.)*

---

## Modeling Architecture

### 1) Base Views (bv_*)
Base views standardize schema, datatypes, and naming, exposing only relevant attributes for downstream joins.

**Base Views created:**
- `bv_brands`
- `bv_categories`
- `bv_customers`
- `bv_orderitems`
- `bv_orders`
- `bv_products`
- `bv_stores`

### 2) Integrated Views (iv_*)
Integrated views progressively enrich and engineer the dataset at the correct grain.

**Integrated Views created:**
- `iv_orders_enriched` — order-level context (joins with store + customer)
- `iv_orders_enriched_features` — time feature engineering (year/month derived)
- `iv_order_items_enriched` — item-level enrichment (product + brand + category)
- `iv_order_items_revenue` — financial KPI engineering at item grain
- `iv_sales_fact` — unified analytic dataset combining order + item layers

### 3) Final Reporting View (RV)
**`final_sales_performance_rv`** is the final business-ready dataset aggregated for reporting and REST exposure.

---

## KPI Engineering (examples)

Item-level measures (engineered before aggregation):
- `gross_revenue = quantity × list_price`
- `discount_amount = gross_revenue × discount`
- `net_revenue = gross_revenue − discount_amount`

Final RV metrics (aggregated monthly / by dimensions):
- `total_net_revenue`
- `total_gross_revenue`
- `total_discount_amount`
- `total_units`
- `total_orders`
- `unique_customers`
- `avg_discount`
- `avg_selling_price` (net revenue per unit)
- `discount_rate`
- `avg_order_value`
- `net_revenue_per_customer`
- `orders_per_customer` (and other derived customer/order metrics)

---

## REST Deployment

The final RV was published as a **REST web service**:
- Service name: `sales_performance_rest`
- Resource exposed: `final_sales_performance_rv`

REST was chosen over SOAP to support modern analytics workflows (simple calls, lower overhead, easier integration with dashboards/scripts).

---

