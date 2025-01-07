### **1. Profit Margin Calculation (Basic Calculated Column)**

#### **Data Representation**
| **SalesID** | **SalesAmount** | **CostAmount** |
|-------------|-----------------|----------------|
| 1           | 500             | 300            |
| 2           | 300             | 200            |
| 3           | 700             | 400            |

---

#### **Problematic**
You need to calculate the **profit margin percentage** for each sales transaction.

---

#### **Explanation**
Create a **calculated column** in Power BI that calculates profit margin using the formula:
\[ \text{Profit Margin} = \frac{\text{Sales Amount} - \text{Cost Amount}}{\text{Sales Amount}} \times 100 \]

---

#### **Solution**
1. In the Power BI Fields pane, select the `FactSales` table.
2. Add a new calculated column:
   ```DAX
   Profit Margin (%) = 
   DIVIDE(
       FactSales[SalesAmount] - FactSales[CostAmount],
       FactSales[SalesAmount],
       0
   ) * 100
   ```
   - **DIVIDE** is used for safe division to handle zero sales amounts.

---

#### **Expected Result**
| **SalesID** | **SalesAmount** | **CostAmount** | **Profit Margin (%)** |
|-------------|-----------------|----------------|------------------------|
| 1           | 500             | 300            | 40                    |
| 2           | 300             | 200            | 33.33                 |
| 3           | 700             | 400            | 42.86                 |

---

### **2. Total Sales (Basic Measure)**

#### **Data Representation**
| **SalesID** | **SalesAmount** | **Region** |
|-------------|-----------------|------------|
| 1           | 500             | North      |
| 2           | 300             | South      |
| 3           | 700             | East       |

---

#### **Problematic**
You need a measure to calculate the **total sales across all regions**.

---

#### **Explanation**
Create a **measure** to sum up the `SalesAmount`.

---

#### **Solution**
1. In the Fields pane, select the `FactSales` table.
2. Add a new measure:
   ```DAX
   Total Sales = 
   SUM(FactSales[SalesAmount])
   ```

---

#### **Expected Result**
| **Total Sales** |
|-----------------|
| 1500            |

---

### **3. Sales by Region (Basic Measure with Filter)**

#### **Data Representation**
| **SalesID** | **SalesAmount** | **Region** |
|-------------|-----------------|------------|
| 1           | 500             | North      |
| 2           | 300             | South      |
| 3           | 700             | East       |

---

#### **Problematic**
You want to calculate **total sales for a specific region (e.g., North)**.

---

#### **Explanation**
Use the **CALCULATE** function to apply a filter to the measure.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Sales for North = 
   CALCULATE(
       SUM(FactSales[SalesAmount]),
       FactSales[Region] = "North"
   )
   ```

---

#### **Expected Result**
| **Sales for North** |
|---------------------|
| 500                 |

---

### **4. Running Total (Cumulative Measure)**

#### **Data Representation**
| **Date**       | **SalesAmount** |
|----------------|-----------------|
| 2025-01-01    | 100             |
| 2025-01-02    | 200             |
| 2025-01-03    | 150             |
| 2025-01-04    | 250             |

---

#### **Problematic**
You want to calculate a **running total of sales by date**.

---

#### **Explanation**
Use the **CALCULATE** and **FILTER** functions to create a measure for cumulative sales.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Running Total Sales = 
   CALCULATE(
       SUM(FactSales[SalesAmount]),
       FILTER(
           ALL(FactSales[Date]),
           FactSales[Date] <= MAX(FactSales[Date])
       )
   )
   ```

---

#### **Expected Result**
| **Date**       | **SalesAmount** | **Running Total Sales** |
|----------------|-----------------|-------------------------|
| 2025-01-01    | 100             | 100                     |
| 2025-01-02    | 200             | 300                     |
| 2025-01-03    | 150             | 450                     |
| 2025-01-04    | 250             | 700                     |

---

### **5. Average Sales per Region (Basic Measure with Grouping)**

#### **Data Representation**
| **Region** | **SalesAmount** |
|------------|-----------------|
| North      | 500             |
| North      | 700             |
| South      | 300             |
| South      | 400             |
| East       | 600             |

---

#### **Problematic**
You want to calculate the **average sales amount per region**.

---

#### **Explanation**
Use the **AVERAGEX** function to calculate an average across grouped values.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Average Sales per Region = 
   AVERAGEX(
       VALUES(FactSales[Region]),
       CALCULATE(SUM(FactSales[SalesAmount]))
   )
   ```

---

#### **Expected Result**
| **Region** | **Average Sales per Region** |
|------------|------------------------------|
| North      | 600                          |
| South      | 350                          |
| East       | 600                          |

### **6. Sales Contribution by Product (Percentage Measure)**

#### **Data Representation**
| **ProductID** | **SalesAmount** |
|---------------|-----------------|
| 101           | 500             |
| 102           | 300             |
| 103           | 200             |

---

#### **Problematic**
You need to calculate each product's contribution to total sales as a percentage.

---

#### **Explanation**
Use a **measure** to divide a product’s sales by total sales and format it as a percentage.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Sales Contribution (%) = 
   DIVIDE(
       SUM(FactSales[SalesAmount]),
       CALCULATE(SUM(FactSales[SalesAmount])),
       0
   ) * 100
   ```

---

#### **Expected Result**
| **ProductID** | **SalesAmount** | **Sales Contribution (%)** |
|---------------|-----------------|----------------------------|
| 101           | 500             | 50%                        |
| 102           | 300             | 30%                        |
| 103           | 200             | 20%                        |

---

### **7. Year-to-Date (YTD) Sales**

#### **Data Representation**
| **Date**       | **SalesAmount** |
|----------------|-----------------|
| 2025-01-01    | 100             |
| 2025-01-02    | 200             |
| 2025-02-01    | 300             |
| 2025-02-15    | 400             |

---

#### **Problematic**
You need to calculate cumulative sales from the beginning of the year up to the current date.

---

#### **Explanation**
Use the **TOTALYTD** function to calculate year-to-date sales.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   YTD Sales = 
   TOTALYTD(
       SUM(FactSales[SalesAmount]),
       FactSales[Date]
   )
   ```

---

#### **Expected Result**
| **Date**       | **SalesAmount** | **YTD Sales** |
|----------------|-----------------|---------------|
| 2025-01-01    | 100             | 100           |
| 2025-01-02    | 200             | 300           |
| 2025-02-01    | 300             | 600           |
| 2025-02-15    | 400             | 1000          |

---

### **8. Product with Highest Sales (Ranking)**

#### **Data Representation**
| **ProductID** | **SalesAmount** |
|---------------|-----------------|
| 101           | 500             |
| 102           | 700             |
| 103           | 300             |

---

#### **Problematic**
You want to determine which product has the highest sales.

---

#### **Explanation**
Use the **RANKX** function to assign ranks based on sales.

---

#### **Solution**
1. Add a new calculated column:
   ```DAX
   Product Rank = 
   RANKX(
       ALL(FactSales[ProductID]),
       SUM(FactSales[SalesAmount]),
       ,
       DESC
   )
   ```
2. Filter the visual to show only rank 1.

---

#### **Expected Result**
| **ProductID** | **SalesAmount** | **Product Rank** |
|---------------|-----------------|------------------|
| 102           | 700             | 1                |

---

### **9. Sales Growth Percentage**

#### **Data Representation**
| **Year** | **SalesAmount** |
|----------|-----------------|
| 2024     | 1000           |
| 2025     | 1500           |

---

#### **Problematic**
You want to calculate the percentage growth in sales from one year to the next.

---

#### **Explanation**
Use the **CALCULATE** and **PREVIOUSYEAR** functions to compare current and previous year sales.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Sales Growth (%) = 
   DIVIDE(
       SUM(FactSales[SalesAmount]) - 
       CALCULATE(SUM(FactSales[SalesAmount]), PREVIOUSYEAR(FactSales[Year])),
       CALCULATE(SUM(FactSales[SalesAmount]), PREVIOUSYEAR(FactSales[Year])),
       0
   ) * 100
   ```

---

#### **Expected Result**
| **Year** | **SalesAmount** | **Sales Growth (%)** |
|----------|-----------------|----------------------|
| 2024     | 1000           | -                    |
| 2025     | 1500           | 50%                  |

---

### **10. Sales Variance**

#### **Data Representation**
| **Month** | **BudgetedSales** | **ActualSales** |
|-----------|-------------------|-----------------|
| January   | 500               | 600             |
| February  | 700               | 800             |
| March     | 400               | 350             |

---

#### **Problematic**
You need to calculate the variance between budgeted and actual sales.

---

#### **Explanation**
Use a calculated column to subtract budgeted sales from actual sales.

---

#### **Solution**
1. Add a new calculated column:
   ```DAX
   Sales Variance = 
   FactSales[ActualSales] - FactSales[BudgetedSales]
   ```
2. Add another column for variance percentage:
   ```DAX
   Sales Variance (%) = 
   DIVIDE(
       FactSales[ActualSales] - FactSales[BudgetedSales],
       FactSales[BudgetedSales],
       0
   ) * 100
   ```

---

#### **Expected Result**
| **Month**   | **BudgetedSales** | **ActualSales** | **Sales Variance** | **Sales Variance (%)** |
|-------------|-------------------|-----------------|--------------------|------------------------|
| January     | 500               | 600             | 100                | 20%                    |
| February    | 700               | 800             | 100                | 14.29%                 |
| March       | 400               | 350             | -50                | -12.5%                 |

---

### **11. Cumulative Sales per Region**

#### **Data Representation**
| **Region** | **Date**       | **SalesAmount** |
|------------|----------------|-----------------|
| North      | 2025-01-01    | 100             |
| North      | 2025-01-02    | 200             |
| South      | 2025-01-01    | 150             |
| South      | 2025-01-02    | 250             |

---

#### **Problematic**
You need to calculate cumulative sales for each region over time.

---

#### **Explanation**
Use **CALCULATE** and **FILTER** with grouping by region.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Cumulative Sales = 
   CALCULATE(
       SUM(FactSales[SalesAmount]),
       FILTER(
           ALL(FactSales),
           FactSales[Date] <= MAX(FactSales[Date]) &&
           FactSales[Region] = MAX(FactSales[Region])
       )
   )
   ```



#### **Expected Result**
| **Region** | **Date**       | **SalesAmount** | **Cumulative Sales** |
|------------|----------------|-----------------|-----------------------|
| North      | 2025-01-01    | 100             | 100                   |
| North      | 2025-01-02    | 200             | 300                   |
| South      | 2025-01-01    | 150             | 150                   |
| South      | 2025-01-02    | 250             | 400                   |


### **12. Dynamic Ranking of Products by Sales**

#### **Data Representation**
| **ProductID** | **SalesAmount** |
|---------------|-----------------|
| 101           | 500             |
| 102           | 700             |
| 103           | 300             |

---

#### **Problematic**
You want to dynamically rank products based on their sales in a report visual.

---

#### **Explanation**
Use **RANKX** to dynamically calculate the ranking based on the current report filter.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Dynamic Product Rank = 
   RANKX(
       ALLSELECTED(FactSales[ProductID]),
       SUM(FactSales[SalesAmount]),
       ,
       DESC
   )
   ```

---

#### **Expected Result**
| **ProductID** | **SalesAmount** | **Dynamic Product Rank** |
|---------------|-----------------|--------------------------|
| 102           | 700             | 1                        |
| 101           | 500             | 2                        |
| 103           | 300             | 3                        |

---

### **13. Identify First Purchase Date**

#### **Data Representation**
| **CustomerID** | **Date**       | **PurchaseAmount** |
|----------------|----------------|--------------------|
| 1              | 2025-01-01    | 100                |
| 1              | 2025-01-05    | 200                |
| 2              | 2025-01-03    | 300                |

---

#### **Problematic**
You need to find the first purchase date for each customer.

---

#### **Explanation**
Use **CALCULATE** and **MIN** to find the earliest date.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   First Purchase Date = 
   CALCULATE(
       MIN(FactSales[Date]),
       ALLEXCEPT(FactSales, FactSales[CustomerID])
   )
   ```

---

#### **Expected Result**
| **CustomerID** | **First Purchase Date** |
|----------------|--------------------------|
| 1              | 2025-01-01              |
| 2              | 2025-01-03              |

---

### **14. Filtered Total Sales**

#### **Data Representation**
| **ProductID** | **Region** | **SalesAmount** |
|---------------|------------|-----------------|
| 101           | North      | 500             |
| 102           | South      | 300             |
| 103           | East       | 700             |

---

#### **Problematic**
You want to calculate total sales for the "North" and "South" regions only.

---

#### **Explanation**
Use **CALCULATE** and **FILTER** to conditionally sum sales amounts.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Filtered Total Sales = 
   CALCULATE(
       SUM(FactSales[SalesAmount]),
       FactSales[Region] IN {"North", "South"}
   )
   ```

---

#### **Expected Result**
| **Filtered Total Sales** |
|--------------------------|
| 800                      |

---

### **15. Count of Unique Products Sold**

#### **Data Representation**
| **SalesID** | **ProductID** |
|-------------|---------------|
| 1           | 101           |
| 2           | 102           |
| 3           | 101           |
| 4           | 103           |

---

#### **Problematic**
You want to count the unique products sold.

---

#### **Explanation**
Use the **DISTINCTCOUNT** function to count unique values in a column.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Unique Products Sold = 
   DISTINCTCOUNT(FactSales[ProductID])
   ```
---

#### **Expected Result**
| **Unique Products Sold** |
|--------------------------|
| 3                        |

---

### **16. Identify Top N Customers by Sales**

#### **Data Representation**
| **CustomerID** | **SalesAmount** |
|----------------|-----------------|
| 1              | 500             |
| 2              | 300             |
| 3              | 700             |

---

#### **Problematic**
You want to display only the top 2 customers based on sales in a visual.

---

#### **Explanation**
Use **TOPN** to filter the dataset for the top N customers.

---

#### **Solution**
1. Add a calculated table:
   ```DAX
   Top Customers = 
   TOPN(
       2,
       FactSales,
       FactSales[SalesAmount],
       DESC
   )
   ```

---

#### **Expected Result**
| **CustomerID** | **SalesAmount** |
|----------------|-----------------|
| 3              | 700             |
| 1              | 500             |

---

### **17. Year-over-Year Growth**

#### **Data Representation**
| **Year** | **SalesAmount** |
|----------|-----------------|
| 2024     | 1000            |
| 2025     | 1500            |

---

#### **Problematic**
You want to calculate the year-over-year sales growth percentage.

---

#### **Explanation**
Use **CALCULATE** and **SAMEPERIODLASTYEAR**.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   YoY Growth (%) = 
   DIVIDE(
       SUM(FactSales[SalesAmount]) - 
       CALCULATE(SUM(FactSales[SalesAmount]), SAMEPERIODLASTYEAR(FactSales[Year])),
       CALCULATE(SUM(FactSales[SalesAmount]), SAMEPERIODLASTYEAR(FactSales[Year])),
       0
   ) * 100
   ```

---

#### **Expected Result**
| **Year** | **YoY Growth (%)** |
|----------|--------------------|
| 2025     | 50%               |

---

### **18. Average Sales Per Day**

#### **Data Representation**
| **Date**       | **SalesAmount** |
|----------------|-----------------|
| 2025-01-01    | 100             |
| 2025-01-02    | 200             |
| 2025-01-03    | 150             |

---

#### **Problematic**
You want to calculate the average sales per day.

---

#### **Explanation**
Divide total sales by the number of distinct dates.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Avg Sales Per Day = 
   DIVIDE(
       SUM(FactSales[SalesAmount]),
       DISTINCTCOUNT(FactSales[Date]),
       0
   )
   ```

---

#### **Expected Result**
| **Average Sales Per Day** |
|---------------------------|
| 150                       |

---

### **19. Percentage of Total by Region**

#### **Data Representation**
| **Region** | **SalesAmount** |
|------------|-----------------|
| North      | 500             |
| South      | 300             |
| East       | 700             |

---

#### **Problematic**
You want to calculate each region’s percentage contribution to total sales.

---

#### **Explanation**
Use **DIVIDE** to calculate the percentage.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Region Sales (%) = 
   DIVIDE(
       SUM(FactSales[SalesAmount]),
       CALCULATE(SUM(FactSales[SalesAmount])),
       0
   ) * 100
   ```

---

#### **Expected Result**
| **Region** | **Region Sales (%)** |
|------------|-----------------------|
| North      | 29.41%               |
| South      | 17.65%               |
| East       | 41.18%               |

---

### **20. Sales Rank Per Region**

#### **Data Representation**
| **Region** | **ProductID** | **SalesAmount** |
|------------|---------------|-----------------|
| North      | 101           | 500             |
| North      | 102           | 300             |
| South      | 103           | 700             |

---

#### **Problematic**
You want to rank products within each region based on their sales.

---

#### **Explanation**
Use **RANKX** with filtering by region.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Rank Per Region = 
   RANKX(
       FILTER(ALL(FactSales), FactSales[Region] = MAX(FactSales[Region])),
       SUM(FactSales[SalesAmount]),
       ,
       DESC
   )
   ```

---

#### **Expected Result**
| **Region** | **ProductID** | **SalesAmount** | **Rank Per Region** |
|------------|---------------|-----------------|---------------------|
| North      | 101           | 500             | 1                   |
| North      | 102           | 300             | 2                   |
| South      | 103           | 700             | 1                   |

---

### **21. Calculate Sales Per Quarter**

#### **Data Representation**
| **Date**       | **SalesAmount** |
|----------------|-----------------|
| 2025-01-01    | 100             |
| 2025-02-15    | 200             |
| 2025-04-10    | 150             |
| 2025-06-20    | 250             |

---

#### **Problematic**
You need to calculate total sales for each quarter.

---

#### **Explanation**
Use the **QUARTER** function to group data into quarters and sum sales.

---

#### **Solution**
1. Add a new calculated column:
   ```DAX
   Quarter = "Q" & QUARTER(FactSales[Date])
   ```
2. Add a measure:
   ```DAX
   Sales Per Quarter = 
   SUM(FactSales[SalesAmount])
   ```

---

#### **Expected Result**
| **Quarter** | **Sales Per Quarter** |
|-------------|-----------------------|
| Q1          | 300                   |
| Q2          | 400                   |

---

### **22. Calculate Total Orders Per Customer**

#### **Data Representation**
| **CustomerID** | **OrderID** |
|----------------|-------------|
| 1              | 101         |
| 1              | 102         |
| 2              | 103         |
| 3              | 104         |

---

#### **Problematic**
You want to calculate the total number of orders placed by each customer.

---

#### **Explanation**
Use **COUNTROWS** and group by `CustomerID`.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Total Orders = 
   COUNTROWS(
       FILTER(
           FactSales,
           FactSales[CustomerID] = MAX(FactSales[CustomerID])
       )
   )
   ```

---

#### **Expected Result**
| **CustomerID** | **Total Orders** |
|----------------|------------------|
| 1              | 2                |
| 2              | 1                |
| 3              | 1                |

---

### **23. Identify Last Purchase Date**

#### **Data Representation**
| **CustomerID** | **Date**       | **PurchaseAmount** |
|----------------|----------------|--------------------|
| 1              | 2025-01-01    | 100                |
| 1              | 2025-01-10    | 200                |
| 2              | 2025-01-05    | 300                |

---

#### **Problematic**
You want to find the last purchase date for each customer.

---

#### **Explanation**
Use the **CALCULATE** and **MAX** functions.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Last Purchase Date = 
   CALCULATE(
       MAX(FactSales[Date]),
       ALLEXCEPT(FactSales, FactSales[CustomerID])
   )
   ```

---

#### **Expected Result**
| **CustomerID** | **Last Purchase Date** |
|----------------|-------------------------|
| 1              | 2025-01-10             |
| 2              | 2025-01-05             |

---

### **24. Calculate Profit per Product**

#### **Data Representation**
| **ProductID** | **SalesAmount** | **CostAmount** |
|---------------|-----------------|----------------|
| 101           | 500             | 300            |
| 102           | 700             | 400            |

---

#### **Problematic**
You want to calculate the total profit for each product.

---

#### **Explanation**
Subtract the total cost from total sales for each product.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Profit Per Product = 
   SUM(FactSales[SalesAmount]) - SUM(FactSales[CostAmount])
   ```

---

#### **Expected Result**
| **ProductID** | **Profit Per Product** |
|---------------|------------------------|
| 101           | 200                    |
| 102           | 300                    |

---

### **25. Count Active Customers**

#### **Data Representation**
| **CustomerID** | **Status**   |
|----------------|--------------|
| 1              | Active       |
| 2              | Inactive     |
| 3              | Active       |

---

#### **Problematic**
You want to count the number of active customers.

---

#### **Explanation**
Use **COUNTROWS** and a filter condition.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Active Customers = 
   COUNTROWS(
       FILTER(FactSales, FactSales[Status] = "Active")
   )
   ```

---

#### **Expected Result**
| **Active Customers** |
|----------------------|
| 2                    |

---

### **26. Average Sales by Product Category**

#### **Data Representation**
| **ProductID** | **Category**    | **SalesAmount** |
|---------------|-----------------|-----------------|
| 101           | Electronics     | 500             |
| 102           | Electronics     | 300             |
| 103           | Furniture       | 700             |

---

#### **Problematic**
You want to calculate the average sales for each product category.

---

#### **Explanation**
Use **AVERAGEX** to compute averages across categories.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Avg Sales by Category = 
   AVERAGEX(
       VALUES(FactSales[Category]),
       CALCULATE(SUM(FactSales[SalesAmount]))
   )
   ```

---

#### **Expected Result**
| **Category**    | **Avg Sales by Category** |
|-----------------|---------------------------|
| Electronics     | 400                       |
| Furniture       | 700                       |

---

### **27. Total Revenue and Costs**

#### **Data Representation**
| **Month** | **Revenue** | **Cost** |
|-----------|-------------|----------|
| January   | 1000        | 700      |
| February  | 1500        | 800      |

---

#### **Problematic**
You want to calculate total revenue and cost.

---

#### **Explanation**
Use **SUM** to aggregate values.

---

#### **Solution**
1. Add two measures:
   ```DAX
   Total Revenue = SUM(FactSales[Revenue])
   Total Cost = SUM(FactSales[Cost])
   ```

---

#### **Expected Result**
| **Total Revenue** | **Total Cost** |
|-------------------|----------------|
| 2500              | 1500           |

---

### **28. Percentage of Budget Spent**

#### **Data Representation**
| **Project** | **Budget** | **Spent** |
|-------------|------------|-----------|
| A           | 5000       | 3000      |
| B           | 7000       | 4000      |

---

#### **Problematic**
You want to calculate the percentage of the budget that has been spent.

---

#### **Explanation**
Use **DIVIDE** to compute the percentage.

---

#### **Solution**
1. Add a new measure:
   ```DAX
   Budget Spent (%) = 
   DIVIDE(
       SUM(FactSales[Spent]),
       SUM(FactSales[Budget]),
       0
   ) * 100
   ```

---

#### **Expected Result**
| **Project** | **Budget Spent (%)** |
|-------------|-----------------------|
| A           | 60%                  |
| B           | 57.14%               |

---

### **29. Calculate Average Time Between Purchases**

#### **Data Representation**
| **CustomerID** | **Date**       |
|----------------|----------------|
| 1              | 2025-01-01    |
| 1              | 2025-01-10    |
| 2              | 2025-01-05    |

---

#### **Problematic**
You want to calculate the average time (in days) between purchases for each customer.

---

#### **Explanation**
Use **DATEDIFF** and **AVERAGEX** to calculate the time difference.

---

#### **Solution**
1. Add a calculated column:
   ```DAX
   Days Between Purchases = 
   DATEDIFF(
       EARLIER(FactSales[Date]),
       FactSales[Date],
       DAY
   )
   ```
2. Add a measure for average:
   ```DAX
   Avg Days Between Purchases = 
   AVERAGEX(
       FactSales,
       FactSales[Days Between Purchases]
   )
   ```

---

#### **Expected Result**
| **CustomerID** | **Avg Days Between Purchases** |
|----------------|--------------------------------|
| 1              | 9                              |
| 2              | N/A                            |

---

### **30. Sales vs Target Comparison**

#### **Data Representation**
| **ProductID** | **Sales** | **Target** |
|---------------|-----------|------------|
| 101           | 500       | 600        |
| 102           | 800       | 700        |

---

#### **Problematic**
You want to compare actual sales against the target.

---

#### **Explanation**
Use a measure to calculate variance and whether the target was met.

---

#### **Solution**
1. Add two measures:
   ```DAX
   Sales Variance = 
   SUM(FactSales[Sales]) - SUM(FactSales[Target])

   Target Met = 
   IF(
       SUM(FactSales[Sales]) >= SUM(FactSales[Target]),
       "Yes",
       "No"
   )
   ```

---

#### **Expected Result**
| **ProductID** | **Sales Variance** | **Target Met** |
|---------------|--------------------|----------------|
| 101           | -100               | No             |
| 102           | 100                | Yes            |



