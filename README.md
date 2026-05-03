# E-commerce Growth Strategy: Revenue & Customer Lifetime Value Optimization

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
- Dashboard Preview

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
The analysis started by building a master dataset through joins across the four tables.

### Master table logic
- `customers.customer_id = orders.customer_id`
- `orders.product_name = product_summary.product_name`

This allowed customer-level, order-level, and product-level analysis in one workflow.

---

## Exploratory Data Analysis

### 1. Revenue Diagnosis
- Quarterly revenue trend
- Orders vs revenue
- Average order value

### 2. Customer Analysis
- Revenue-based customer segmentation
- Churn by membership tier
- Repeat vs non-repeat customers
- Engagement level vs spend

### 3. Product & Revenue Drivers
- Top products
- Category performance
- Discount impact
- Returns impact

### 4. Operations & Experience
- Delivery days vs rating
- Rating vs repeat behavior
- Payment method performance

### 5. Advanced Analysis
- RFM-style customer profiling
- Customer lifetime value estimation
- Revenue concentration / Pareto analysis

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

## Dashboard Preview
![Dashboard Preview](./Dashboard.png)

---

## Project Files
- SQL analysis
- Dashboard
- PowerPoint summary
- Final synthesis slide

---

## Dashboard Storyline
The dashboard and presentation were designed to support the following storyline:

1. Revenue is growing, but not evenly
2. A small group of customers drives most revenue
3. Repeat customers are disproportionately valuable
4. Discounts and returns reduce profitability
5. Growth can improve through retention, pricing efficiency, and customer targeting 

---

## Conclusion
This project translates raw customer and sales data into a consulting-style growth strategy that can support better decision-making across marketing, retention, pricing, and product performance. 
