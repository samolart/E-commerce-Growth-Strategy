# E-commerce Growth Strategy: Revenue & Customer Lifetime Value Optimization

<img width="1280" height="720" alt="Dashboard" src="https://github.com/user-attachments/assets/4975e2f3-20c7-4bf2-ab54-7c42bd9d0f19" />


## Summary
This project analyzes e-commerce customer behavior, sales performance, product mix, discounts, churn, and engagement patterns to identify the main drivers of revenue growth and customer lifetime value.

The analysis shows that revenue is concentrated in a relatively small group of high-value and repeat customers, while category performance, discounting, and customer experience all influence profitability and retention. The final dashboard and presentation were built to support a consulting-style growth strategy focused on retention, pricing efficiency, and customer value. 

---

## Table of Contents
- Objective
- Data Source
- Tools
- Data Preparation
- Exploratory Data Analysis
- Key Findings
- Recommendations

---

## Objective
The objective of this project is to analyze e-commerce sales and customer behavior to answer one core business question:

**How can the business increase revenue and customer lifetime value?**

To answer this, the project examines customer segmentation, repeat purchase behavior, churn, product performance, discount impact, and operational experience. 


---

## Data Source
**Kaggle Dataset:**  
[E-commerce Customer Behavior & Sales 2020–2026](https://www.kaggle.com/datasets/meruvakodandasuraj/e-commerce-customer-behavior-and-sales-20202026/data?select=orders.csv)

### Tables used
- `customers`
- `monthly_revenue`
- `orders`
- `product_summary`

---

## Tools
- SQL (PostgreSQL)
- Excel
- PowerPoint

The workflow is similar in style to your previous portfolio projects, which also used Excel for cleaning, PostgreSQL for analysis, and Tableau for reporting. :contentReference[oaicite:4]{index=4}

---

## Data Preparation
A master dataset was created by joining all tables:

```sql
SELECT 
    c.customer_id,
    c.country,
    c.age,
    c.gender,
    c.membership_tier,
    c.total_orders,
    c.total_spend_usd,
    c.avg_order_value_usd,
    c.days_since_last_purchase,
    c.churned,
    o.order_id,
    o.order_date,
    o.category,
    o.product_name,
    o.total_amount_usd,
    o.quantity,
    o.discount_pct,
    o.discount_amount_usd,
    o.delivery_days,
    o.customer_rating,
    o.session_duration_minutes,
    o.pages_viewed_before_purchase,
    o.is_repeat_customer,
    p.total_revenue_usd AS product_revenue,
    p.avg_discount_pct AS product_discount,
    p.return_rate AS product_return_rate
FROM customers c
LEFT JOIN orders o 
    ON c.customer_id = o.customer_id
LEFT JOIN product_summary p
    ON o.product_name = p.product_name;

```

---

## Exploratory Data Analysis

### 1. Revenue Diagnosis
- Quarterly revenue trend
- Orders vs revenue
- Average order value

```sql
-- Quarterly Revenue Trend
SELECT 
    year,
    month,
    SUM(revenue_usd) AS total_revenue
FROM monthly_revenue
GROUP BY year, month
ORDER BY year, month;

-- Orders vs Revenue
SELECT 
    year,
    month,
    COUNT(order_id) AS total_orders,
    SUM(total_amount_usd) AS total_revenue
FROM orders
GROUP BY year, month
ORDER BY year, month;

-- Average Order Value (AOV)
SELECT 
    SUM(total_amount_usd) / COUNT(order_id) AS avg_order_value
FROM orders;
```

### 2. Customer Analysis
- Revenue-based customer segmentation
- Churn by membership tier
- Repeat vs non-repeat customers
- Engagement level vs spend

```sql
-- Customer Segmentation (Revenue-Based)
SELECT 
    customer_id,
    total_spend_usd,
    CASE 
        WHEN total_spend_usd >= (
            SELECT PERCENTILE_CONT(0.8) WITHIN GROUP (ORDER BY total_spend_usd)
            FROM customers
        ) THEN 'High Value'
        WHEN total_spend_usd >= (
            SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_spend_usd)
            FROM customers
        ) THEN 'Medium Value'
        ELSE 'Low Value'
    END AS customer_segment
FROM customers;


-- Churn Analysis
SELECT 
    membership_tier,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN churned = B'1' THEN 1 ELSE 0 END) AS churned_customers,
    AVG(CASE WHEN churned = B'1' THEN 1.0 ELSE 0 END) AS churn_rate
FROM customers
GROUP BY membership_tier;


-- Repeat vs New Customeers
SELECT 
    is_repeat_customer,
    COUNT(order_id) AS orders,
    SUM(total_amount_usd) AS revenue
FROM orders
GROUP BY is_repeat_customer;


-- Customer Behavior → Revenue
SELECT 
    CASE 
        WHEN session_duration_minutes < 5 THEN 'Low Engagement'
        WHEN session_duration_minutes < 15 THEN 'Medium Engagement'
        ELSE 'High Engagement'
    END AS engagement_level,
    COUNT(order_id) AS orders,
    AVG(total_amount_usd) AS avg_spend
FROM orders
GROUP BY engagement_level;

```
### 3. Product & Revenue Drivers
- Top products
- Category performance
- Discount impact
- Returns impact

```sql
 --Top Products
SELECT
    product_name,
    SUM(total_amount_usd) AS revenue,
    COUNT(order_id) AS orders
FROM orders
GROUP BY product_name
ORDER BY revenue DESC
LIMIT 10;

-- Category Performance
SELECT 
    category,
    SUM(total_amount_usd) AS revenue,
    COUNT(order_id) AS orders
FROM orders
GROUP BY category
ORDER BY revenue DESC;

-- Discount Impact
SELECT 
    CASE 
        WHEN discount_pct = 0 THEN 'No Discount'
        WHEN discount_pct < 20 THEN 'Low Discount'
        ELSE 'High Discount'
    END AS discount_group,
    COUNT(order_id) AS orders,
    AVG(total_amount_usd) AS avg_revenue
FROM orders
GROUP BY discount_group;

-- Returns Impact
SELECT 
    category,
    COUNT(*) AS total_orders,
    SUM(CASE WHEN returned = B'1' THEN 1 ELSE 0 END) AS returns,
    AVG(CASE WHEN returned = B'1' THEN 1.0 ELSE 0 END) AS return_rate
FROM orders
GROUP BY category
ORDER BY return_rate DESC;

```

### 4. Operations & Experience
- Delivery days vs rating
- Rating vs repeat behavior
- Payment method performance

```sql

-- Delivery vs Rating
SELECT 
    delivery_days,
    AVG(customer_rating) AS avg_rating
FROM orders
GROUP BY delivery_days
ORDER BY delivery_days;

-- Rating vs Repeat Behavior
SELECT 
    customer_rating,
    ROUND(
        AVG(CASE WHEN is_repeat_customer = B'1' THEN 1.0 ELSE 0 END),
        3
    ) AS repeat_rate
FROM orders
GROUP BY customer_rating
ORDER BY customer_rating;

-- Payment Method Performance
SELECT 
    payment_method,
    COUNT(order_id) AS orders,
    AVG(total_amount_usd) AS avg_revenue
FROM orders
GROUP BY payment_method;

```
### 5. Advanced Analysis
- RFM-style customer profiling
- Customer lifetime value estimation
- Revenue concentration / Pareto analysis

```sql
 -- RFM Segmentation
SELECT 
    customer_id,
    days_since_last_purchase AS recency,
    total_orders AS frequency,
    total_spend_usd AS monetary
FROM customers;

-- CLV Calculation
SELECT 
    customer_id,
    avg_order_value_usd * total_orders AS estimated_clv
FROM customers
ORDER BY estimated_clv DESC;

-- Revenue Concentration (Pareto)
SELECT 
    customer_id,
    SUM(total_amount_usd) AS revenue
FROM orders
GROUP BY customer_id
ORDER BY revenue DESC;

```
---

## Key Findings
- Total revenue reached **$3,136,404.66**
- Total orders were **25,000**
- Average order value was **$125.46**
- Unique customers totaled **7,663**
- Repeat customers generated **64.6%** of total revenue, while non-repeat customers generated **35.4%**
- Electronics was the top revenue category by a wide margin
- Revenue was highly concentrated in customers who did not receive discounts
- Higher engagement groups contributed the largest share of spending
- Churn varies by membership tier, showing retention risk is not evenly distributed 

---

## Recommendations
- Focus on high-value customers with targeted campaigns and loyalty programs
- Improve retention through lifecycle messaging, post-purchase engagement, and repeat-purchase incentives
- Optimize discounts by using targeted promotions instead of blanket markdowns
- Protect revenue from leakage by monitoring returns, low-margin products, and underperforming categories
- Prioritize operational improvements that support better customer experience and repeat purchases 

---

## Project Files
- SQL analysis
- Dashboard
- PowerPoint summary
- Final synthesis slide

