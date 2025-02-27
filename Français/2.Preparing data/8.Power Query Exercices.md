### **Exercise 1: Remove Duplicates**

#### **Data Sample**
| **CustomerID** | **Name**      | **Region** |
|----------------|---------------|------------|
| 1              | John Doe      | North      |
| 2              | Jane Smith    | South      |
| 1              | John Doe      | North      |
| 3              | Alice Brown   | East       |
| 2              | Jane Smith    | South      |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select all columns in the table.
3. Click on the **"Remove Duplicates"** button in the toolbar.
   - This removes rows that are exact duplicates based on all columns.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **CustomerID** | **Name**      | **Region** |
|----------------|---------------|------------|
| 1              | John Doe      | North      |
| 2              | Jane Smith    | South      |
| 3              | Alice Brown   | East       |

---

### **Exercise 2: Split Column by Delimiter**

#### **Data Sample**
| **FullName**       |
|--------------------|
| John Doe           |
| Jane Smith         |
| Alice Brown        |
| Michael Johnson    |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the `FullName` column.
3. Click on **"Split Column"** > **"By Delimiter"**.
   - Choose **Space** as the delimiter.
   - Select **"Each occurrence of the delimiter"** as the split option.
4. Rename the new columns to `FirstName` and `LastName`.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **FirstName** | **LastName**  |
|---------------|---------------|
| John          | Doe           |
| Jane          | Smith         |
| Alice         | Brown         |
| Michael       | Johnson       |

---

### **Exercise 3: Filter Rows**

#### **Data Sample**
| **OrderID** | **Region** | **SalesAmount** |
|-------------|------------|-----------------|
| 1           | North      | 100             |
| 2           | South      | 200             |
| 3           | East       | 150             |
| 4           | North      | 50              |
| 5           | West       | 300             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the `Region` column.
3. Click on the dropdown menu in the column header.
4. Uncheck all regions except **North**.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **OrderID** | **Region** | **SalesAmount** |
|-------------|------------|-----------------|
| 1           | North      | 100             |
| 4           | North      | 50              |

### **Exercise 4: Replace Values**

#### **Data Sample**
| **CustomerID** | **Name**      | **Status**    |
|----------------|---------------|---------------|
| 1              | John Doe      | Active        |
| 2              | Jane Smith    | Inactive      |
| 3              | Alice Brown   | N/A           |
| 4              | Michael Jones | N/A           |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the `Status` column.
3. Click **"Replace Values"** from the toolbar.
   - Replace `N/A` with `Unknown`.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **CustomerID** | **Name**      | **Status**    |
|----------------|---------------|---------------|
| 1              | John Doe      | Active        |
| 2              | Jane Smith    | Inactive      |
| 3              | Alice Brown   | Unknown       |
| 4              | Michael Jones | Unknown       |

---

### **Exercise 5: Add a Custom Column**

#### **Data Sample**
| **ProductID** | **Unit Price** | **Quantity** |
|---------------|----------------|--------------|
| 101           | 10.00          | 5            |
| 102           | 20.00          | 3            |
| 103           | 15.00          | 7            |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Click on **"Add Column"** > **"Custom Column"**.
3. Enter a formula: `[Unit Price] * [Quantity]`.
4. Name the column as `Total Price`.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **ProductID** | **Unit Price** | **Quantity** | **Total Price** |
|---------------|----------------|--------------|-----------------|
| 101           | 10.00          | 5            | 50.00           |
| 102           | 20.00          | 3            | 60.00           |
| 103           | 15.00          | 7            | 105.00          |

---

### **Exercise 6: Group By**

#### **Data Sample**
| **Region** | **SalesAmount** |
|------------|-----------------|
| North      | 100             |
| South      | 200             |
| North      | 50              |
| East       | 150             |
| South      | 100             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the `Region` column.
3. Click **"Group By"** from the toolbar.
   - Group by `Region`.
   - Aggregate `SalesAmount` using the **Sum** function.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **Region** | **Total SalesAmount** |
|------------|-----------------------|
| North      | 150                   |
| South      | 300                   |
| East       | 150                   |

---

### **Exercise 7: Conditional Column**

#### **Data Sample**
| **OrderID** | **SalesAmount** |
|-------------|-----------------|
| 1           | 100             |
| 2           | 300             |
| 3           | 500             |
| 4           | 50              |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Click on **"Add Column"** > **"Conditional Column"**.
3. Define a condition:
   - If `SalesAmount` > 200, then "High".
   - Else "Low".
4. Name the column as `Sales Category`.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **OrderID** | **SalesAmount** | **Sales Category** |
|-------------|-----------------|--------------------|
| 1           | 100             | Low                |
| 2           | 300             | High               |
| 3           | 500             | High               |
| 4           | 50              | Low                |

---

### **Exercise 8: Merge Queries**

#### **Data Sample 1: Orders**
| **OrderID** | **CustomerID** | **Amount** |
|-------------|----------------|------------|
| 1           | 101            | 500        |
| 2           | 102            | 300        |
| 3           | 103            | 200        |

#### **Data Sample 2: Customers**
| **CustomerID** | **Customer Name** |
|----------------|-------------------|
| 101            | John Doe          |
| 102            | Jane Smith        |
| 103            | Alice Brown       |

---

#### **Procedure**
1. Load both datasets into Power Query Editor.
2. Click on **"Merge Queries"**.
   - Merge `Orders` with `Customers` using `CustomerID` as the key.
   - Select the join type as **Left Outer**.
3. Expand the `Customer Name` column from the `Customers` table.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **OrderID** | **CustomerID** | **Amount** | **Customer Name** |
|-------------|----------------|------------|-------------------|
| 1           | 101            | 500        | John Doe          |
| 2           | 102            | 300        | Jane Smith        |
| 3           | 103            | 200        | Alice Brown       |

---

### **Exercise 9: Unpivot Columns**

#### **Data Sample**
| **Region** | **Q1** | **Q2** | **Q3** | **Q4** |
|------------|--------|--------|--------|--------|
| North      | 100    | 200    | 150    | 50     |
| South      | 300    | 400    | 250    | 100    |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the columns `Q1`, `Q2`, `Q3`, and `Q4`.
3. Click **"Unpivot Columns"**.
4. Rename the columns:
   - `Attribute` → `Quarter`.
   - `Value` → `SalesAmount`.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **Region** | **Quarter** | **SalesAmount** |
|------------|-------------|-----------------|
| North      | Q1          | 100             |
| North      | Q2          | 200             |
| North      | Q3          | 150             |
| North      | Q4          | 50              |
| South      | Q1          | 300             |
| South      | Q2          | 400             |
| South      | Q3          | 250             |
| South      | Q4          | 100             |

---

### **Exercise 10: Transpose Table**

#### **Data Sample**
| **Metric**       | **North** | **South** | **East** | **West** |
|-------------------|-----------|-----------|----------|----------|
| Total Sales       | 1000      | 1200      | 900      | 1100     |
| Number of Orders  | 50        | 60        | 45       | 55       |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the entire table.
3. Click on **"Transpose"** from the toolbar.
4. Rename the columns appropriately (e.g., `Region`, `Metric`, `Value`).
5. Close and load the data back to your report.

---

#### **Expected Result**
| **Region** | **Metric**         | **Value** |
|------------|--------------------|-----------|
| North      | Total Sales        | 1000      |
| North      | Number of Orders   | 50        |
| South      | Total Sales        | 1200      |
| South      | Number of Orders   | 60        |
| East       | Total Sales        | 900       |
| East       | Number of Orders   | 45        |
| West       | Total Sales        | 1100      |
| West       | Number of Orders   | 55        |

---

### **Exercise 11: Replace Errors**

#### **Data Sample**
| **ProductID** | **Price**  | **Quantity** | **Total**  |
|---------------|------------|--------------|------------|
| 101           | 10.00      | 5            | 50.00      |
| 102           | 20.00      | 3            | 60.00      |
| 103           | Error      | 2            | Error      |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Right-click on the `Price` and `Total` columns and choose **"Replace Errors"**.
   - Replace errors with `0`.
3. Close and load the data back to your report.

---

#### **Expected Result**
| **ProductID** | **Price**  | **Quantity** | **Total**  |
|---------------|------------|--------------|------------|
| 101           | 10.00      | 5            | 50.00      |
| 102           | 20.00      | 3            | 60.00      |
| 103           | 0.00       | 2            | 0.00       |

---

### **Exercise 12: Append Queries**

#### **Data Sample 1: January Sales**
| **OrderID** | **Region** | **SalesAmount** |
|-------------|------------|-----------------|
| 1           | North      | 500             |
| 2           | South      | 300             |

#### **Data Sample 2: February Sales**
| **OrderID** | **Region** | **SalesAmount** |
|-------------|------------|-----------------|
| 3           | North      | 400             |
| 4           | East       | 200             |

---

#### **Procedure**
1. Load both datasets (`January Sales` and `February Sales`) into Power Query Editor.
2. Click **"Append Queries"** > **"Append Queries as New"**.
   - Append `February Sales` to `January Sales`.
3. Close and load the appended table back to your report.

---

#### **Expected Result**
| **OrderID** | **Region** | **SalesAmount** |
|-------------|------------|-----------------|
| 1           | North      | 500             |
| 2           | South      | 300             |
| 3           | North      | 400             |
| 4           | East       | 200             |

---

### **Exercise 13: Create Running Total**

#### **Data Sample**
| **Date**       | **SalesAmount** |
|----------------|-----------------|
| 2025-01-01     | 100             |
| 2025-01-02     | 200             |
| 2025-01-03     | 150             |
| 2025-01-04     | 250             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Sort the `Date` column in ascending order.
3. Add a custom column:
   - Use this formula in M:
     ```M
     List.Sum(List.FirstN(#"Previous Step"[SalesAmount], List.PositionOf(#"Previous Step"[Date], [Date]) + 1))
     ```
4. Rename the new column to `Running Total`.
5. Close and load the data back to your report.

---

#### **Expected Result**
| **Date**       | **SalesAmount** | **Running Total** |
|----------------|-----------------|-------------------|
| 2025-01-01     | 100             | 100               |
| 2025-01-02     | 200             | 300               |
| 2025-01-03     | 150             | 450               |
| 2025-01-04     | 250             | 700               |

---

### **Exercise 14: Pivot Columns**

#### **Data Sample**
| **Region** | **Quarter** | **SalesAmount** |
|------------|-------------|-----------------|
| North      | Q1          | 100             |
| North      | Q2          | 200             |
| South      | Q1          | 300             |
| South      | Q2          | 400             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Select the `Quarter` column.
3. Click **"Pivot Columns"**.
   - Use `SalesAmount` as the values.
   - Choose **Sum** as the aggregation function.
4. Close and load the pivoted table back to your report.

---

#### **Expected Result**
| **Region** | **Q1** | **Q2** |
|------------|--------|--------|
| North      | 100    | 200    |
| South      | 300    | 400    |

---

### **Exercise 15: Extract Date Parts**

#### **Data Sample**
| **OrderID** | **OrderDate**       |
|-------------|---------------------|
| 1           | 2025-01-01 10:00AM |
| 2           | 2025-01-02 11:00AM |
| 3           | 2025-01-03 09:30AM |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Use the "Date" transformations to extract:
   - **Year**, **Month**, and **Day** from the `OrderDate` column.
3. Close and load the data back to your report.

---

#### **Expected Result**
| **OrderID** | **Year** | **Month** | **Day** |
|-------------|----------|-----------|---------|
| 1           | 2025     | 1         | 1       |
| 2           | 2025     | 1         | 2       |
| 3           | 2025     | 1         | 3       |


### **Exercise 16: Filter Top N Rows**

#### **Data Sample**
| **CustomerID** | **SalesAmount** |
|----------------|-----------------|
| 1              | 500             |
| 2              | 300             |
| 3              | 700             |
| 4              | 200             |
| 5              | 400             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Click on **"Keep Rows"** > **"Keep Top Rows"**.
   - Specify **Top 3 Rows**.
3. Sort the `SalesAmount` column in descending order **before** keeping rows.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **CustomerID** | **SalesAmount** |
|----------------|-----------------|
| 3              | 700             |
| 1              | 500             |
| 5              | 400             |

---

### **Exercise 17: Create Index Column**

#### **Data Sample**
| **CustomerID** | **Name**      | **SalesAmount** |
|----------------|---------------|-----------------|
| 1              | John Doe      | 500             |
| 2              | Jane Smith    | 300             |
| 3              | Alice Brown   | 700             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Click on **"Add Column"** > **"Index Column"**.
   - Choose **From 1** for a sequential index starting from 1.
3. Close and load the data back to your report.

---

#### **Expected Result**
| **Index** | **CustomerID** | **Name**      | **SalesAmount** |
|-----------|----------------|---------------|-----------------|
| 1         | 1              | John Doe      | 500             |
| 2         | 2              | Jane Smith    | 300             |
| 3         | 3              | Alice Brown   | 700             |

---

### **Exercise 18: Dynamic Column Renaming**

#### **Data Sample**
| **Column1** | **Column2** | **Column3** |
|-------------|-------------|-------------|
| A           | B           | C           |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Add a custom step in the M code to rename columns dynamically:
   ```M
   Table.RenameColumns(PreviousStepName, {{"Column1", "Region"}, {"Column2", "Product"}, {"Column3", "Sales"}})
   ```
3. Close and load the renamed table back to your report.

---

#### **Expected Result**
| **Region** | **Product** | **Sales** |
|------------|-------------|-----------|
| A          | B           | C         |

---

### **Exercise 19: Web Data Extraction**

#### **Data Sample**
(No initial data required—data will be pulled from a web page.)

---

#### **Procedure**
1. Open Power BI Desktop and select **Get Data** > **Web**.
2. Enter the URL of a public web page (e.g., a stock market or weather report page).
3. Select the desired table from the preview.
4. Transform the data to remove unnecessary columns.
5. Close and load the data back to your report.

---

#### **Expected Result**
A cleaned dataset from the selected web page. For example:
| **Stock**    | **Price** | **Change** |
|--------------|-----------|------------|
| Company A    | $150      | +5%        |
| Company B    | $120      | -2%        |

---

### **Exercise 20: Conditional Transformation**

#### **Data Sample**
| **ProductID** | **StockLevel** |
|---------------|----------------|
| 101           | 50             |
| 102           | 0              |
| 103           | 10             |

---

#### **Procedure**
1. Load the dataset into Power Query Editor.
2. Create a **Conditional Column**:
   - If `StockLevel` = 0, then "Out of Stock".
   - Else if `StockLevel` <= 10, then "Low Stock".
   - Otherwise, "In Stock".
3. Name the column `Stock Status`.
4. Close and load the data back to your report.

---

#### **Expected Result**
| **ProductID** | **StockLevel** | **Stock Status** |
|---------------|----------------|------------------|
| 101           | 50             | In Stock         |
| 102           | 0              | Out of Stock     |
| 103           | 10             | Low Stock        |

---

