# Olist Customer Cohort Retention Analysis

A Python/Pandas cohort analysis of customer retention using the Brazilian Olist e-commerce dataset.

## Business Problem

Olist needs to understand whether customers return after their first purchase. A high volume of first-time customers does not necessarily mean strong customer value if customers rarely purchase again.

This project answers:

- How many customers are acquired in each month?
- What percentage of each cohort returns in Month 1, Month 3, Month 6, etc.?
- Which customer cohorts have stronger or weaker retention?
- What is the overall repeat-purchase rate?
- How does revenue retention compare with customer retention?
- Which product categories can be investigated as potential drivers of repeat behavior?

## Approach

### 1. Data preparation
The analysis uses:

- `customers_dataset.csv`
- `orders_dataset.csv`
- `order_payments_dataset.csv`
- `order_items_dataset.csv`
- `products_dataset.csv`
- `product_category_name_translation.csv`

`order_purchase_timestamp` is converted to datetime.

For the core retention analysis, orders with `order_status == "delivered"` are treated as completed purchases.

### 2. Customer identification
`customer_unique_id` is used as the customer-level identifier because a customer can have multiple records associated with different `customer_id` values.

### 3. Cohort definition
A customer's cohort month is the month of their first delivered purchase.

Example:

| Customer | First Purchase | Cohort |
|---|---|---|
| A | 2017-01-15 | 2017-01 |
| B | 2017-03-10 | 2017-03 |

### 4. Retention calculation
For each customer order, the number of months since the customer's first purchase is calculated.

Month 0 = first purchase month  
Month 1 = one month after first purchase  
Month 2 = two months after first purchase

Retention is calculated as:

`customers active in month N / customers in Month 0 × 100`

### 5. Revenue extension
Payment values are aggregated at order level and connected to customer cohorts to produce a revenue cohort view.

### 6. Product-category extension
Order items are connected to products and translated category names to investigate acquisition categories and their relationship with repeat purchasing.

## Key Findings

> **Important:** The repository notebook calculates the exact findings from the supplied CSV files. The statements below are intended as the final findings template; 

- The overall repeat-purchase rate is **[3.00%]**.
- Average Month 1 customer retention is **[5.45%]**.
- Average Month 3 customer retention is **[0.25%]**.
- Average Month 6 customer retention is **[0.27%]**.
- The strongest acquisition cohort is **[2017-10]**, with Month 1 retention of **[0.72%]**.
- The weakest mature cohort is **[2017-02]**, with Month 1 retention of **[0.18%]**.
- Revenue retention should be compared with customer retention to determine whether returning customers are becoming more or less valuable over time.
- Product-category analysis can identify categories associated with high initial acquisition and should be combined with repeat-purchase analysis before making category-level investment decisions.

## Business Recommendations

### 1. Focus on the second purchase
If Month 1 retention is low, the biggest opportunity is converting first-time customers into second-time buyers.

Recommended actions:
- Post-purchase email campaigns
- Personalized product recommendations
- Second-purchase incentives
- Replenishment reminders where relevant
- Cross-sell campaigns

### 2. Replicate high-performing cohorts
Identify the acquisition months with consistently stronger retention and investigate:
- Marketing channel
- Promotions
- Product mix
- Seasonality
- Customer geography
- Seller/category mix

Use these insights to reproduce the conditions associated with stronger cohorts.

### 3. Segment retention campaigns
Avoid treating all customers identically. Segment customers by:
- First-purchase category
- Order value
- Geography
- Purchase frequency
- Recency

Then use targeted retention campaigns instead of broad discounts.

### 4. Optimize for profitable retention
Customer retention alone is not enough. Compare customer retention with revenue retention and, where possible, contribution margin.

A cohort with fewer returning customers but substantially higher revenue per returning customer may be more valuable than a cohort with higher customer retention.

### 5. Monitor cohort retention as a recurring KPI
Track:
- Month 1 retention
- Month 3 retention
- Month 6 retention
- Repeat-purchase rate
- Revenue retention
- Average order value of returning customers

This creates a consistent framework for evaluating customer lifecycle performance.

## Repository Structure

```text
olist-cohort-retention-analysis/
│
├── data/
│   ├── customers_dataset.csv
│   ├── orders_dataset.csv
│   ├── order_payments_dataset.csv
│   ├── order_items_dataset.csv
│   ├── products_dataset.csv
│   └── product_category_name_translation.csv
│
├── olist_cohort_retention_analysis.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## How to Run

```bash
git clone <repository-url>
cd olist-cohort-retention-analysis
pip install -r requirements.txt
jupyter notebook
```

Place the Olist CSV files inside the `data/` directory and run:

`olist_cohort_retention_analysis.ipynb`

## Tools

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook

## Analytical Note

The core cohort analysis intentionally uses delivered orders as completed purchases. If the business defines a completed purchase differently, update the order-status filter and rerun the analysis.
