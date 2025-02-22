# Power BI 360

🔗 **[View the Full Power BI Report](https://app.powerbi.com/view?r=eyJrIjoiYzBhYzc4OGMtNmI2Ni00OTdiLWJmOTgtN2EzNDQ4MTY3MjM1IiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9)** 

Please Wait for a few seconds after clicking the link, Background needs little bit of time to load. Thank You

---

## 📊 Project Overview  
In this project, I built an end-to-end Power BI dashboard for sales, cost, and market analysis. The report is designed to help stakeholders track financial performance, forecast accuracy, market share, and operational expenses across various regions and products. I connected data from multiple sources, cleaned and transformed the data, created an optimized data model, wrote powerful DAX measures, and designed dynamic, interactive visualizations.

The report consists of 6 pages:

 - `Home Page`
 - `Finance View`
 - `Sales View`
 - `Market View`
 - `Supply Chain View`
 - `Executive View`.

Each view provides detailed insights with navigation icons to switch between pages. Let’s break it all down step by step!  

---

## 📂 Data Sources

I collected data from multiple sources, including both **SQL databases** and **Excel files**:  

- **SQL Databases:**  
  - `gdb041` — Contains historical sales, cost, and profit data.  
  - `gdb056` — Stores future sales forecasts and predictions.  

- **Excel Files:**  
  1. **Targets:** Market-level monthly targets for **Net Sales**, **Net Profits**, and **Gross Margin**.  
  2. **Market Share:** Category-level market share data split across sub-zones.  
  3. **Operating Expenses:** Breakdown of costs like **freight charges**, **manufacturing expenses**, and **operational overheads**.

---

![Data Sources](https://github.com/user-attachments/assets/8e28c656-c066-421b-b4bb-e27367254d25)


---

## 🛠️ Schema Design and Data Model

![Data Model](https://github.com/user-attachments/assets/6b2171f5-b95f-4719-bad5-b6dfb83e40ff)

The entire data model is built using a **Snowflake Schema**. This structure helps handle hierarchical data, improves query performance, and reduces redundancy by normalizing dimension tables. Let’s break it down layer by layer!  

### **Schema Overview**  
The data model is structured across **3 layers**:  

1. **Fact Tables (Central Layer)**  
2. **Primary Dimension Tables (First Layer around Fact Tables)**  
3. **Hierarchical Breakdown Dimensions (Outer Layer)**  

Each layer plays a crucial role in supporting dynamic data aggregation and filtering.

---

### **1. Fact Tables (Central Layer)**  
The core of the model consists of **fact tables** that store granular, event-level data. These tables capture actual values, forecasts, costs, and invoice adjustments, serving as the foundation for the dashboard’s analysis.  

| **Fact Table**                  | **Description**                                                                 |
|----------------------------------|---------------------------------------------------------------------------------|
| **fact_Actual_Estimates**        | Stores actual sales/revenue data by customer, product, and date.                 |
| **fact_forecast_monthly**        | Contains monthly sales forecasts for performance comparisons.                   |
| **manufacturing_cost**           | Tracks manufacturing costs by product and region.                               |
| **freight_cost**                 | Records freight and shipping costs.                                             |
| **Operational_charges**          | Logs various operational expenses.                                              |
| **post_invoice_deductions**      | Captures adjustments made after invoices are issued (e.g., discounts, penalties).|
| **marketshare**                  | Stores category-wise market share data across sub-zones.                        |
| **NsGmTarget**                   | Contains net sales and gross margin targets by market and month.                |

These tables are connected to dimension tables through **many-to-one relationships**, making it easy to slice and dice data across multiple dimensions.

---

### **2. Primary Dimension Tables (First Layer)**  
Dimension tables provide **context** to the fact data. They store descriptive attributes like product names, customer details, and time periods — allowing for data aggregation and filtering.  

| **Dimension Table**              | **Description**                                                                |
|-----------------------------------|----------------------------------------------------------------------------------|
| **dim_date**                     | Tracks the timeline with **Date**, **Month**, and **Fiscal Year** fields.        |
| **dim_product**                  | Stores product details like **product_code**, **category**, and **brand**.       |
| **dim_customer**                 | Manages customer data, including **customer_code**, **customer_name**, and **market**.|


These tables sit on the **one side** of the relationship, enabling dynamic roll-ups of sales, costs, and profits across various attributes.

---

### **3. Hierarchical Breakdown Dimensions (Outer Layer)**  
This outer layer offers a more detailed breakdown of the primary dimensions, supporting **hierarchical analysis**.  

| **Breakdown Table**              | **Purpose**                                                                     |
|-----------------------------------|----------------------------------------------------------------------------------|
| **Category**                     | Provides a higher-level classification of products (e.g., **Electronics**, **Apparel**). |
| **Sub_zone**                     | Allows geographic segmentation within larger markets (e.g., **North America → West Coast**). |
| **dim_market**                   | Captures market-level data, linking to sub-zones for geographic segmentation.    |
| **Fiscal_Year**                  | Defines fiscal periods (e.g., **FY2023**, **Q1 2024**) to align financial reporting. |

---

### **Why Snowflake Schema?**  
I chose a **Snowflake Schema** for this project because it:  
- **Handles Hierarchical Data:** Splitting dimensions into smaller, normalized tables makes it easier to navigate multi-level hierarchies.  
- **Improves Query Performance:** By reducing redundancy and storing data in separate tables, queries run faster and use less memory.  
- **Supports Complex Queries:** The model can easily aggregate data across multiple dimensions, supporting advanced time-series and comparative analysis.

---

### **What This Enables:**  
With this model, I can:  
- 📈 **Analyze Sales Performance:** Compare actuals vs. forecasts and measure growth.  
- 💰 **Track Cost Components:** Split costs into **manufacturing**, **freight**, and **operational charges**.  
- 📊 **Measure Market Share:** Calculate category-wise market share and compare it against targets.  
- 🔍 **Segment Data:** Drill down by **product**, **customer**, **market**, and **sub-zone**.  
- ⏱️ **Conduct Time-Series Analysis:** Use the **dim_date** and **Fiscal_Year** tables to analyze data over time.

---

## DAX Measures

To bring the data to life, I created over **30 DAX measures** that power the dashboard's visuals and calculations. These measures help aggregate, calculate, and analyze critical financial metrics, making the report dynamic and flexible for user interactions. Let’s break them down!

### Key DAX Measures

- **Sales and Profitability**
  - `GS $`: Sum of Gross Sales.
  - `Pre Invoice Deduction $`: Difference between Gross Sales and Net Invoice Sales.
  - `NIS $`: Sum of Net Invoice Sales.
  - `Post Invoice Deduction $`: Sum of post-invoice deductions (like discounts).
  - `Post Invoice other Deduction $`: Sum of additional deductions after invoicing.
  - `Total Post Invoice Deduction $`: Sum of both invoice and other deductions.
  - `NS $`: Total Net Sales.
  - `Net Profit $`: Total Net Profit.

- **Cost Components**
  - `Manufacturing Cost $`: Sum of all manufacturing costs.
  - `Freight Cost $`: Sum of freight and shipping costs.
  - `Other Cost $`: Sum of miscellaneous operational costs.
  - `Operational charges $`: Sum of various operational charges.
  - `Total COGS $`: Total Cost of Goods Sold (Manufacturing + Freight + Other Costs).

- **Performance & Accuracy**
  - `Forecast Accuracy %`: Measures the accuracy of sales forecasts.
  - `ABS Error`: Absolute error between actual and forecasted values.
  - `ABS Error %`: Percentage error based on absolute error.
  - `Net Error`: Difference between actual and forecast sales.
  - `Net Error %`: Net error as a percentage of forecast sales.

- **Profitability Ratios**
  - `Gross Margin $`: Difference between Net Sales and Total COGS.
  - `Gross Margin %`: Gross Margin as a percentage of Net Sales.
  - `GM / Unit`: Gross Margin divided by quantity sold.
  - `Net Profit %`: Net Profit as a percentage of Net Sales.

- **Market Share and Risk**
  - `Market Share %`: Market share percentage based on category sales.
  - `RC %`: Risk Contribution percentage.
  - `Sales Qty`: Total quantity of products sold.

These measures not only helped create essential charts but also saved a **ton of manual calculation time**, making the dashboard incredibly responsive to user selections.

---

## Dynamic P&L Table (Key DAX Queries)

One of the most complex and rewarding parts of this project was building a fully dynamic **Profit & Loss (P&L) statement table**. The challenge was to display a detailed P&L view across multiple periods, allowing users to switch metrics with a click — all powered by DAX!

The P&L statement table rows included:
- **Gross Sales**
- **Pre Invoice Deduction**
- **Net Invoice Sales**
- **Post Invoice Deduction**
- **Total Post Invoice Deduction**
- **Net Sales**
- **Manufacturing Cost**
- **Freight Cost**
- **Other Cost**
- **Total COGS**
- **Gross Margin**
- **Operational Charges**
- **Net Profit**

The columns were equally dynamic:
- **Selected Year**
- **Last Year (LY)**
- **Year-over-Year (YoY) Change**
- **YoY % Change**

To achieve this, I wrote several DAX queries. Let’s break them down!

### 1. P&L Values Measure

This measure handles what value to show for each row in the P&L table:

```DAX
P & L values = 
VAR y = SWITCH(TRUE(),
    MAX('P & L Rows'[Order]) = 1, [GS $] / 1000000,
    MAX('P & L Rows'[Order]) = 2, [Pre Invoice Deduction $] / 1000000,
    MAX('P & L Rows'[Order]) = 3, [NIS $] / 1000000,
    MAX('P & L Rows'[Order]) = 4, [Post Invoice Deduction $] / 1000000,
    MAX('P & L Rows'[Order]) = 5, [Post Invoice other Deduction $] / 1000000,
    MAX('P & L Rows'[Order]) = 6, [Post Invoice Deduction $] / 1000000 + [Post Invoice other Deduction $] / 1000000,
    MAX('P & L Rows'[Order]) = 7, [NS $] / 1000000,
    MAX('P & L Rows'[Order]) = 8, [Manufacturing Cost $] / 1000000,
    MAX('P & L Rows'[Order]) = 9, [Freight Cost $] / 1000000,
    MAX('P & L Rows'[Order]) = 10, [Other Cost $] / 1000000,
    MAX('P & L Rows'[Order]) = 11, [Total COGS $] / 1000000,
    MAX('P & L Rows'[Order]) = 12, [GM $] / 1000000,
    MAX('P & L Rows'[Order]) = 13, [GM %] * 100,
    MAX('P & L Rows'[Order]) = 14, [GM / Unit],
    MAX('P & L Rows'[Order]) = 15, [Operational charges $] / 1000000,
    MAX('P & L Rows'[Order]) = 16, [Net Profit $] / 1000000,
    MAX('P & L Rows'[Order]) = 17, [Net Profit %] * 100
)
RETURN
IF(HASONEVALUE('P & L Rows'[Description]), y, [NS $] / 1000000)
```

### 2. P&L Columns Measure

This measure generates dynamic columns:
```P & L col = 
VAR x = ALLNOBLANKROW(Fisical_Year[FY_Desc])
RETURN
UNION(
    ROW("Col headers", "LY"),
    ROW("Col headers", "YoY"),
    ROW("Col headers", "YoY %"),
    x
)
```

### 3. Final P&L Table Measure
   
Finally, this measure brings it all together to produce the complete table:
```
P & L final = 
SWITCH(TRUE(),
    SELECTEDVALUE(Fisical_Year[FY_Desc]) = MAX('P & L col'[Col headers]), [P & L values],
    MAX('P & L col'[Col headers]) = "LY", [P & L LY],
    MAX('P & L col'[Col headers]) = "YoY", [P & L CHG],
    MAX('P & L col'[Col headers]) = "YoY %", [P & L YoY Chg %]
)
```

---

## 🖼️ Dashboard Layout & Navigation

The report contains **6 pages**, including a **Home Page** and **5 detailed dashboards**:  

- **🏠 Home Page:** Clickable icons that navigate to each view.  
- **📊 Finance View:** P&L statements, COGS, and profitability metrics.  
- **🛒 Sales View:** Sales trends, forecasts, and target comparisons.  
- **📈 Market View:** Market share analysis by category and region.  
- **🚚 Supply Chain View:** Freight costs, operational expenses, and risk metrics.  
- **👔 Executive View:** High-level KPIs and summary statistics.

Each page includes a **navigation bar** for easy switching between views.

---

## 📘 Conclusion

This project was a complete learning experience, where I combined **data modeling**, **DAX programming**, and **Power BI visual design**. The **Snowflake Schema** provided the flexibility to handle complex relationships, while **DAX measures** powered dynamic and interactive insights.  

---

## 🙏 **Your Feedback Matters!**  
Thank you for taking the time to view my project! 😊 Your thoughts, suggestions, or constructive feedback are highly valued and will help me improve. Feel free to share your insights—I’m open to learning and growing.  

---

## 🤝 **Contact Details**  

**Karthikeya Kanumuri**  
📧 **karthikeya.kanumuri.work@gmail.com**  
🔗 [**LinkedIn**](https://www.linkedin.com/in/karthikeya-kanumuri)  
