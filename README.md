# Driving-Revenue-Optimization-and-Product-Strategy-with-SQL-Instacart-Sales-Demand-Analysis
SQL analysis uncovering revenue drivers, product performance, and demand patterns to support data-driven business decisions.
# Instacart SQL Analytics: Driving Revenue Intelligence, Product Strategy & Demand Optimization

---

## Table of Contents
- [Executive Summary](#executive-summary)
- [Business Context](#business-context)
- [Problem Statement](#problem-statement)
- [Analytical Objectives](#analytical-objectives)
- [Methodology](#methodology)
- [Key Metrics (KPI Overview)](#key-metrics-kpi-overview)
- [Insights & Analysis](#insights--analysis)
  - [Revenue Concentration by Department](#revenue-concentration-by-department)
  - [Profitability Analysis: Bread vs Alcohol](#profitability-analysis-bread-vs-alcohol)
  - [Weekend Demand Patterns](#weekend-demand-patterns)
  - [Unsold Product Identification](#unsold-product-identification)
- [Strategic Recommendations](#strategic-recommendations)
- [Expected Business Impact](#expected-business-impact)
- [Conclusion](#conclusion)
- [SQL Logic](#sql-logic)

---

## Executive Summary

Instacart operates at scale, processing over **1 million orders** and generating **$158M+ in revenue**. However, high transaction volume alone does not guarantee optimized performance.

This analysis was conducted to move beyond surface-level reporting and uncover:

- where revenue is truly concentrated  
- which products drive demand during peak periods  
- how profitability differs across categories  
- where inefficiencies exist in the product catalog  

The result is a **SQL-driven analytical framework** that enables leadership to make **data-informed decisions on pricing, merchandising, inventory, and product strategy**.

---

## Business Context

Instacart is a leading online grocery delivery platform connecting customers with personal shoppers across multiple retail partners.

Given the scale of operations, the business must continuously answer:

- What is driving revenue growth?
- Which categories deserve strategic focus?
- Are high-demand products also high-profit?
- How does customer behavior change across time?
- Which products are underperforming or inactive?

Without clear answers, decision-making becomes reactive rather than strategic.

---

## Problem Statement

Despite access to large volumes of transactional data, leadership lacked a **connected, decision-ready view** of business performance.

Specifically, the organization could not clearly determine:

- which departments are true revenue drivers  
- whether demand aligns with profitability  
- how customer purchasing behavior shifts during peak periods  
- which products contribute no commercial value  

This created risks such as:

- hidden revenue concentration  
- missed profitability opportunities  
- inefficient inventory allocation  
- bloated product catalog with inactive SKUs  

---

## Analytical Objectives

This project was designed to answer critical business questions:

- What is the total scale of orders, customers, and revenue?
- What is the average order value?
- Which departments generate the most revenue?
- What products dominate weekend demand?
- Does bread outperform alcohol in profitability?
- Which products have never been sold?

---

## Methodology

The analysis was conducted using SQL across relational datasets:

**Tables used:**
- `orders`
- `products`
- `departments`
- `aisle`

**Techniques applied:**
- Aggregations (`SUM`, `COUNT DISTINCT`)
- Multi-table joins
- Filtering (weekend logic)
- Ranking and sorting
- Null detection (unsold products)

---

## Key Metrics (KPI Overview)

| Metric | Value |
|------|------|
| Total Revenue | $158,366,495 |
| Total Customers | 63,100 |
| Total Orders | 1,048,575 |
| Average Order Value | 151 |

---
![Instacart Dashboard](https://drive.google.com/uc?export=view&id=1bD5dwWIgjd83CmNZAughpGFqr257tTmA)

---
### Interpretation

- High transaction volume indicates strong demand  
- Broad customer base supports repeat purchasing  
- Average order value suggests opportunity for basket optimization  

---

## Insights & Analysis

### Revenue Concentration by Department

Top-performing departments:

- Personal Care — $20.8M  
- Snacks — $19.9M  
- Pantry — $17.1M  
- Beverages — $14.0M  
- Frozen — $12.8M  

**Insight:**  
Revenue is concentrated in a small number of high-frequency categories.

**Implication:**  
These departments are core revenue drivers and should be prioritized in promotions, pricing, and inventory planning.

---

### Profitability Analysis: Bread vs Alcohol

- Alcohol: $342K  
- Bread: $175K  

**Insight:**  
Alcohol significantly outperforms bread in profitability.

**Business Meaning:**  
High demand does not always translate to high profit. Product strategy must balance:

- volume-driving products  
- margin-driving products  

---

### Weekend Demand Patterns

Top weekend products:

- English Hydro Cucumber  
- Sparkling Blood Orange Soda  
- Shredded Mozzarella Cheese  

**Insight:**  
Weekend demand reflects a mix of:

- fresh consumption  
- meal preparation  
- convenience purchases  

**Implication:**  
Customer behavior shifts during weekends, creating predictable demand spikes.

---

### Unsold Product Identification

Products with no sales were identified using `LEFT JOIN`.

**Insight:**  
Some products generate zero revenue.

**Business Risk:**
- catalog inefficiency  
- increased operational complexity  
- potential inventory waste  

---

## Strategic Recommendations

### 1. Prioritize High-Revenue Departments
- Increase visibility of top categories  
- Bundle complementary products  
- Optimize pricing and promotions  

### 2. Focus on Profitability
- Track margins alongside revenue  
- Promote high-margin categories  
- Reassess low-margin staples  

### 3. Leverage Weekend Demand
- Introduce weekend-specific campaigns  
- Improve demand forecasting  
- Ensure stock availability  

### 4. Clean Product Catalog
- Remove or reposition inactive products  
- Improve discoverability  
- Reduce catalog clutter  

### 5. Strengthen Analytics Capability
- Add time-based trend analysis  
- Track repeat purchases  
- Combine revenue and cost for full profitability view  

---

## Expected Business Impact

- Improved revenue focus through category prioritization  
- Increased profitability via better product mix  
- Enhanced customer experience through availability optimization  
- Reduced inefficiencies in product catalog  
- Stronger data-driven decision-making  

---

## Conclusion

This project demonstrates how SQL can move beyond reporting into **strategic business analysis**.

It provides clear visibility into:

- revenue drivers  
- product demand patterns  
- profitability differences  
- catalog inefficiencies  

The key takeaway:

> Data becomes valuable when it drives better business decisions.

---

## SQL Logic

### Total Orders
```sql
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM orders;

### Total customers
```sql
SELECT COUNT(DISTINCT user_id) AS total_customers
FROM orders;

### Total revenue
```sql
SELECT SUM(unit_price * quantity) AS total_revenue
FROM products p
JOIN orders o ON p.product_id = o.product_id;

### Average Order Value
```sql
SELECT 
    SUM(o.quantity * p.unit_price) / COUNT(DISTINCT o.order_id) AS avg_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id;

### Top 5 Departments
```sql
SELECT department,
       SUM(unit_price * quantity) AS revenue
FROM departments d
JOIN products p ON d.department_id = p.department_id
JOIN orders o ON p.product_id = o.product_id
GROUP BY department
ORDER BY revenue DESC
LIMIT 5;

### Weekend Products
```sql
SELECT p.product_name,
       SUM(o.quantity) AS total_units_sold
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE o.order_dow IN (0,6)
GROUP BY p.product_name
ORDER BY total_units_sold DESC
LIMIT 3;

### Bread vs Alcohol
```sql
SELECT a.aisle,
       SUM(unit_price) AS total_profit
FROM orders o
JOIN products p ON o.product_id = p.product_id
JOIN aisle a ON p.aisle_id = a.aisle_id
WHERE a.aisle IN ('bread', 'alcohol')
GROUP BY a.aisle
ORDER BY total_profit DESC;

### Unsold Products
```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id
WHERE o.product_id IS NULL;
