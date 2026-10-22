# 📊 E-Commerce Sales & Performance Analytics | Power BI & DAX

## 📌 Project Overview

This project showcases an interactive **Power BI Sales & Target Performance Dashboard** developed to analyze e-commerce transactions from **April 1, 2018, to March 31, 2019**.

Using Power BI, Data Analysis Expressions (DAX), and data visualization, this project transforms raw sales data into meaningful business insights. The dashboard explores revenue performance, product profitability, monthly sales trends, sales target achievement, and regional performance across categories, sub-categories, cities, and states.

## 🎯 Business Problem & Objectives

Businesses need to monitor sales performance, profitability, regional demand, and target achievement to make informed decisions.

The key objectives of this project are to:

1. **Track Business KPIs:** Calculate order count, revenue, average profit, year-to-date (YTD) sales, profit margin, and target achievement.
2. **Compare Sales Performance:** Analyze actual sales against targets across product categories and months.
3. **Evaluate Profitability:** Identify profitable and loss-making product sub-categories.
4. **Analyze Trends:** Explore monthly sales patterns and regional order distribution.
5. **Support Decision-Making:** Recommend strategies to improve pricing, product performance, inventory planning, and marketing campaigns.

## 📂 Dataset Architecture

The project uses three datasets covering a 12-month period.

| Dataset | Records | Key Information |
|---|---:|---|
| List of Orders | 500 | Order ID, Order Date, Customer Name, State, City |
| Order Details | 1,500 | Order ID, Amount, Profit, Quantity, Category, Sub-Category |
| Sales Target | 36 | Month of Order Date, Category, Target |

### 🗂️ Data Dictionary

| Table | Column | Data Type | Description |
|---|---|---|---|
| List of Orders | Order ID | Text | Identifier used to connect order records |
| List of Orders | Order Date | Date | Date the order was placed |
| List of Orders | Customer Name | Text | Customer name |
| List of Orders | State | Text | State associated with the order |
| List of Orders | City | Text | City associated with the order |
| Order Details | Order ID | Text | Reference to the corresponding order |
| Order Details | Amount | Currency/Decimal | Recorded line-item amount |
| Order Details | Profit | Currency/Decimal | Profit or loss for the line item |
| Order Details | Quantity | Whole number | Number of units sold |
| Order Details | Category | Text | Clothing, Electronics, or Furniture |
| Order Details | Sub-Category | Text | Product sub-category, such as Printers, Saree, or Tables |
| Sales Target | Month of Order Date | Date/Text | Month associated with the sales target |
| Sales Target | Category | Text | Category for the target |
| Sales Target | Target | Currency/Decimal | Monthly sales target |

## 🧹 Data Preparation & Modeling

Data preparation and modeling were performed using Power Query and Power BI.

- Removed 60 blank rows from the List of Orders dataset.
- Checked column names, data types, and data consistency.
- Created calculated columns and measures using DAX.
- Established relationships between the order, order details, date, and sales target data.
- Prepared the model for category-wise, monthly, and regional analysis.

**Data model relationships:**
- `List of Orders[Order ID]` → `Order Details[Order ID]`
- `Date[Date]` → `List of Orders[Order Date]`
- Connected the Sales Target table to the appropriate category and month dimensions for target comparisons.

*Note: The relationships should be configured so that filters flow correctly through the model and sales targets are not duplicated when calculating totals.*

## 🧮 DAX Calculations

### 1. Calculated Columns

**Category Type**

Combines the main category and sub-category for detailed product segmentation.

```dax
Category Type =
'Order Details'[Category] & " - " &
'Order Details'[Sub-Category]
```

**Revenue**

Calculates line-item revenue using Amount multiplied by Quantity, as defined for this project.

```dax
Revenue =
'Order Details'[Amount] *
'Order Details'[Quantity]
```

**Sales Category**

Classifies each line item as above or below the overall average Amount.

```dax
Sales Category =
VAR AvgAmount =
    CALCULATE(
        AVERAGE('Order Details'[Amount]),
        ALL('Order Details')
    )
RETURN
    IF(
        'Order Details'[Amount] >= AvgAmount,
        "Above Average",
        "Below Average"
    )
```

### 2. DAX Measures

**Distinct Order Count**

```dax
Order Count =
DISTINCTCOUNT('Order Details'[Order ID])
```

**Total Revenue**

```dax
Total Revenue =
SUM('Order Details'[Revenue])
```

**Total Profit**

```dax
Total Profit =
SUM('Order Details'[Profit])
```

**Total Sales Amount**

```dax
Total Sales =
SUM('Order Details'[Amount])
```

**Average Profit in Delhi**

```dax
Average Profit Delhi =
CALCULATE(
    AVERAGE('Order Details'[Profit]),
    'List of Orders'[State] = "Delhi"
)
```

**Year-to-Date Revenue**

```dax
YTD Revenue =
TOTALYTD(
    [Total Revenue],
    'Date'[Date]
)
```

*YTD calculations require a valid Date table related to the order dates. If the assignment defines sales as Amount rather than Amount × Quantity, use the Total Sales measure instead of Total Revenue.*

## 📊 Dashboard Visualizations

The dashboard contains nine visualizations designed to explore different aspects of business performance.

1. **Sales Target Achievement by Category:** Clustered column chart comparing actual sales with targets.
2. **Maximum Profit Margin by Sub-Category:** Donut chart displaying the distribution of the selected maximum-margin metric.
3. **Monthly Sales Trend:** Line chart highlighting changes in sales performance over time.
4. **Profit vs. Quantity Analysis:** Scatter chart comparing sales volume with profitability across sub-categories.
5. **Target KPI Summaries:** Cards displaying key sales and target metrics.
6. **Sales Performance Matrix:** Matrix comparing category-wise sales and targets by month.
7. **Geographic Sales Analysis:** Map showing regional sales distribution across cities.
8. **Sub-Category Sales Distribution:** Treemap comparing performance across product sub-categories.
9. **Order Count by State:** Funnel chart comparing order volumes across states.

## 📈 Key Findings & Business Insights

### 🔍 1. Descriptive Analysis — What Happened?

- **Revenue:** The calculated revenue is approximately **₹2.15 million**, based on Amount × Quantity.
- **Transaction Volume:** The dataset contains 500 unique orders, 1,500 line items, and 5,615 units sold.
- **Category Performance:** Category rankings can be used to identify the strongest contributors to sales.
- **Monthly Trends:** The monthly sales chart helps identify peak and low-performing periods.
- **Regional Performance:** Madhya Pradesh recorded 101 orders and Maharashtra recorded 90 orders in the analysis.

### 🧩 2. Diagnostic Analysis — Why Did It Happen?

- **Target Variance:** Comparing actual sales with targets helps identify categories that meet or miss expectations.
- **Profitability Gaps:** Negative-profit line items and low-margin sub-categories highlight potential profitability issues.
- **Regional Differences:** Order counts and sales by city or state help reveal differences in customer demand.
- **Monthly Fluctuations:** The sales trend helps identify periods that may require further investigation.

### 🔮 3. Predictive Analysis — What Could Happen?

- Categories with consistently strong historical performance may remain important sales contributors if demand continues.
- Historical monthly trends can help guide future demand planning, although additional forecasting is needed for reliable predictions.
- Loss-making products may continue to affect profitability if their pricing and costs remain unchanged.

### 💡 4. Prescriptive Analysis — What Should Be Done?

- **Improve Product Profitability:** Review the pricing, procurement, and delivery costs of loss-making products, including Tables where confirmed by the analysis.
- **Optimize Inventory:** Use category, sub-category, and regional sales trends to support inventory allocation.
- **Improve Target Achievement:** Investigate underperforming categories and set realistic monthly targets.
- **Plan Promotions:** Consider targeted campaigns during historically weaker sales periods.
- **Focus on Strong Markets:** Use regional performance data to guide marketing and distribution decisions.

## 🏁 Conclusion

This project demonstrates how Power BI, DAX, and data visualization can transform raw e-commerce data into meaningful business insights. Through data cleaning, relational modeling, calculated columns, measures, and nine dashboard visualizations, the analysis explores revenue trends, target achievement, product performance, and profitability.

The findings can help businesses identify performance gaps, review loss-making products, improve inventory planning, refine pricing strategies, and make more informed marketing decisions. Overall, this project strengthened my practical understanding of **data analysis, business intelligence, and data-driven decision-making**.

## 🛠️ Tools & Technologies

- **Microsoft Power BI Desktop** — Dashboard development and visualization
- **DAX** — Calculated columns, measures, and time intelligence
- **Power Query Editor** — Data cleaning and transformation
- **Microsoft Excel** — Dataset preparation and analysis
- **GitHub** — Project documentation and portfolio presentation

## 👩‍💻 About Me

**Monisha B | Aspiring Data Analyst**

I am an aspiring Data Analyst interested in transforming raw data into meaningful insights that support better business decisions. Through this project, I developed practical skills in data preparation, data modeling, DAX calculations, dashboard development, and business analysis.

**Skills:** Power BI | DAX | Power Query | Excel | Data Visualization | Data Modeling | Business Intelligence | Data Storytelling

*Created by Monisha B as part of my Data Analytics Portfolio. Feedback and suggestions are welcome!*
