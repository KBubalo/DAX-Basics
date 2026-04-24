# DAX Basics - Hands-On Lab

## Introduction

Welcome to the DAX Basics hands-on lab! This lab will guide you through fundamental DAX (Data Analysis Expressions) concepts using a simple sales dataset. You'll learn how to create calculations, work with different contexts, and build measures that Power BI users rely on daily.

### Prerequisites

- Power BI Desktop installed on your computer
- Basic understanding of data tables and relationships
- Completion of the DAX Basics lecture slides

### Lab Duration

Approximately 60-90 minutes

---

## Lab Scenario

You work as a data analyst for an electronics retailer. Your manager has asked you to create a Power BI report that analyzes sales performance across different products and customers. You'll need to import data, create a proper data model, and build DAX calculations to answer business questions.

---

## Part 1: Import Data and Build the Data Model

### Step 1.1: Create a New Power BI Report

1. Open **Power BI Desktop**
2. Click **Get Data** > **Text/CSV**
3. Navigate to the `data` folder in this repository
4. Import the following files in order:
   - `Sales.csv`
   - `Products.csv`
   - `Customers.csv`
5. Click **Load** for each file (no transformations needed)

### Step 1.2: Review the Data Structure

In the **Data view**, examine each table:

**Sales** (Fact Table)
- SaleID: Unique identifier for each transaction
- SaleDate: Date of the transaction
- ProductID: Foreign key to Products table
- CustomerID: Foreign key to Customers table
- Quantity: Number of units sold
- SalesAmount: Total revenue for this transaction

**Products** (Dimension Table)
- ProductID: Unique identifier for each product
- ProductName: Name of the product
- Category: Product category
- UnitPrice: Price per unit

**Customers** (Dimension Table)
- CustomerID: Unique identifier for each customer
- CustomerName: Customer's full name
- Country: Customer's country
- City: Customer's city
- Region: Geographic region

---

## Part 2: Create a Calendar Table with DAX

### Step 2.1: Create the Calendar Table Using CALENDARAUTO

A Calendar (or Date) table is essential for time-based analysis in Power BI.

1. Go to the **Modeling** tab
2. Click **New Table**
3. Enter the following DAX formula:

```dax
Calendar = CALENDARAUTO()
```

4. Press Enter to create the table

**What does CALENDARAUTO do?**
- It scans all date columns in your data model
- Creates a calendar table starting from January 1st of the earliest year
- Ends on December 31st of the latest year
- Includes every single day in between

### Step 2.2: Best Practice Discussion ⚠️

> **Important Note:** While `CALENDARAUTO()` is convenient, it's **not recommended** for production reports. Here's why:
>
> **Problems with CALENDARAUTO:**
> - You don't control the start and end dates
> - If your data changes, your calendar might unexpectedly expand or shrink
> - It includes ALL dates, even if you only need specific years
>
> **Better approach - Use CALENDAR function:**
> ```dax
> Calendar = CALENDAR(DATE(2024,1,1), DATE(2024,12,31))
> ```
> This gives you explicit control over the date range.
>
> **Best approach - Use Power Query:**
> Create your calendar table in Power Query where you have more flexibility for complex calendar logic, fiscal years, and custom columns. However, for learning DAX, we'll continue with the DAX-created table.

---

## Part 3: Add Calculated Columns to Calendar Table

Calculated columns are computed row-by-row and stored in the model. They're perfect for adding attributes to dimension tables.

### Step 3.1: Add Year Column

1. Select the **Calendar** table in the Data view
2. Go to **Table tools** > **New column**
3. Enter this formula:

```dax
Year = YEAR(Calendar[Date])
```

4. Press Enter

### Step 3.2: Add Quarter Column

Create a new column:

```dax
Quarter = "Q" & FORMAT(QUARTER(Calendar[Date]), "0")
```

This creates values like "Q1", "Q2", "Q3", "Q4".

### Step 3.3: Add Month Number Column

```dax
MonthNumber = MONTH(Calendar[Date])
```

### Step 3.4: Add Month Name Column

```dax
MonthName = FORMAT(Calendar[Date], "MMMM")
```

This creates full month names like "January", "February", etc.

### Step 3.5: Sort Month Name by Month Number

By default, "April" comes before "January" alphabetically. Let's fix this:

1. Select the **Calendar** table
2. Click on the **MonthName** column
3. Go to **Column tools** tab
4. Click **Sort by column**
5. Select **MonthNumber**

Now when you use MonthName in visuals, it will be sorted correctly!

### Step 3.6: Mark as Date Table

1. Select the **Calendar** table
2. Go to **Table tools** > **Mark as date table**
3. Select **Mark as date table**
4. In the dialog, toggle **Mark as a date table** to on
5. In the dropdown, choose **Date** as the date column
6. Click Save

This tells Power BI that this is your official calendar table, enabling time intelligence functions.

---

## Part 4: Create Relationships

Now let's connect our tables using relationships. Power BI might have auto-detected some relationships, but let's verify and create them properly.

### Step 4.1: Open Model View

Click the **Model view** icon on the left sidebar.

### Step 4.2: Create Relationship: Sales to Products

1. Drag **ProductID** from the **Sales** table
2. Drop it onto **ProductID** in the **Products** table
3. Verify the relationship is **One-to-Many** (1:*) with **1** on the Products side
4. Ensure **Cross filter direction** is set to **Single**
5. Click OK

**Why One-to-Many?**
- One product can appear in many sales transactions
- Each sale relates to exactly one product

### Step 4.3: Create Relationship: Sales to Customers

1. Drag **CustomerID** from the **Sales** table
2. Drop it onto **CustomerID** in the **Customers** table
3. Verify the relationship is **One-to-Many** (1:*)
4. Click OK

### Step 4.4: Create Relationship: Sales to Calendar

1. Drag **SaleDate** from the **Sales** table
2. Drop it onto **Date** in the **Calendar** table
3. Verify the relationship is **One-to-Many** (1:*)
4. Click OK

Your data model is now complete! You should see a **star schema** with Sales (fact table) at the center, connected to three dimension tables. You can now switch back to the **Report view**.

---

## Part 5: Create Basic Measures

Measures are calculations that are evaluated dynamically based on filter context. Unlike calculated columns, they don't store data—they calculate on the fly.

### Step 5.1: Organize with a Measures Table (Optional but Recommended)

While you can add measures to any table, it's best practice to create a dedicated table:

1. Go to **Modeling** > **New Table**
2. Enter: `_Measures = {1}`
3. This creates a single-row table solely for storing measures

Now let's add our first measure to this dedicated table so that all measures are organized in one place. Select the `_Measures` table in the Data pane, then go to **Modeling** > **New measure** to create a measure that will be stored in this table. Repeat this step for all the below measures.

### Step 5.2: Total Sales (SUM)

```dax
Total Sales = SUM(Sales[SalesAmount])
```

**What it does:** Adds up all values in the SalesAmount column, respecting any filters applied in the report.

### Step 5.3: Average Sales (AVERAGE)

```dax
Average Sales = AVERAGE(Sales[SalesAmount])
```

**What it does:** Calculates the mean of all sales transactions.

### Step 5.4: Maximum Sale (MAX)

```dax
Max Sale = MAX(Sales[SalesAmount])
```

**What it does:** Returns the highest single transaction amount.

### Step 5.5: Minimum Sale (MIN)

```dax
Min Sale = MIN(Sales[SalesAmount])
```

**What it does:** Returns the lowest single transaction amount.

### Step 5.6: Total Quantity

```dax
Total Quantity = SUM(Sales[Quantity])
```

### Step 5.7: Number of Transactions

```dax
Transaction Count = COUNTROWS(Sales)
```

**What it does:** Counts how many rows (transactions) are in the Sales table within the current filter context.

Now, you can delete the **Value** column in our **_Measures** table since it was only needed to create the table and is no longer required.

### Step 5.8: Visualize Your Measures

Let's test the measures you just created by building a simple table visual. This helps you verify that your DAX is working correctly and see how measures respond to different filter contexts.

1. Go to the **Report view** (click the report icon on the left sidebar)
2. Click on the **Table** visual in the Visualizations pane
3. In the **Data** pane, expand the **Products** table
4. Check the box next to **ProductName** (or drag it to the **Columns** field)
5. Now expand your **_Measures** table (or wherever you created your measures)
6. Add the following measures to your table:
   - **Total Sales**
   - **Average Sales**
   - **Min Sale**
   - **Max Sale**
   - **Total Quantity**
   - **Transaction Count**

**What you should see:**
- Each product name in its own row
- Total sales aggregated by product
- Average sales per transaction for each product
- Minimum sale amount per product
- Maximum sale amount per product
- Total quantity sold per product
- Number of transactions per product

**Try this experiment:**
1. Create a **slicer** visual
2. Add **Customers[Country]** to the slicer
3. Select a country (e.g., "Germany")
4. Watch how all the measures in your table recalculate automatically!

This demonstrates **filter context** in action—your measures respond dynamically to filters applied in the report.

**Optional:** Create a second table visual showing:
- **Customers[Country]**
- **Total Sales**
- **Transaction Count**

This shows sales performance by country instead of by product.

---

## Part 6: Logical Functions - IF Statements

### Step 6.1: Simple IF Statement - Sales Category

Let's create a measure that categorizes sales as "High" or "Low". Make sure to select your **_Measure** table before creating this new measure so that it is stored in the same dedicated location as your other measures.

```dax
Sales Category (Simple) = 
IF(
    [Total Sales] >= 500,
    "High Sales",
    "Low Sales"
)
```

**How it works:**
- Evaluates if Total Sales is 500 or more
- Returns "High Sales" if TRUE
- Returns "Low Sales" if FALSE
- Simple and straightforward conditional logic

### Step 6.2: Complex IF Statement - Performance Rating

Now let's create a more detailed categorization with 5 levels:

```dax
Performance Rating (IF) = 
IF(
    [Total Sales] >= 2000,
    "Excellent",
    IF(
        [Total Sales] >= 1000,
        "Good",
        IF(
            [Total Sales] >= 500,
            "Average",
            IF(
                [Total Sales] >= 100,
                "Below Average",
                "Poor"
            )
        )
    )
)
```

**How it works:**
- Checks conditions from highest to lowest
- Returns the first matching category
- Nested IF statements can get hard to read (we'll improve this with SWITCH!)

### Step 6.3: Test Your IF Measures in a Visual

Let's see our measures in action! Add them to your table visual.

1. Go back to the **table visual** you created in Step 5.8
2. Add these two new measures to your table:
   - **Sales Category (Simple)**
   - **Performance Rating (IF)**

**What you should see:**
- Each product has a sales category (either "High Sales" or "Low Sales")
- Each product has a performance rating (Excellent, Good, Average, Below Average, or Poor)
- **But wait... there's a blank row at the top showing "Low Sales" and "Poor"!** 🤔

---

### Step 6.4: ❓ Understanding the Blank Row Problem

You've probably noticed an unexpected **blank row** at the top of your table that shows "Low Sales" and "Poor". Let's understand why this happens and how to fix it.

**What causes the blank row?**

The blank row appears because of how **table visuals display data** when you mix dimension attributes with measures. Here's what's happening:

When you add **ProductName** (a column from the Products table) to a table visual:
- Power BI creates one row for each distinct ProductName value
- It also creates rows for any **"missing" or unmatched data**

**Why would there be unmatched data?**

This typically happens when:
1. **Power BI includes a row for calculations that don't tie to a specific product**
2. The table visual automatically tries to show aggregations at multiple levels
3. Your measures evaluate even when there's no specific ProductName in context

Think of it like this: The visual is trying to show "everything" - including a row that represents "no specific product" or an aggregation level.

**Why does the blank row show "Low Sales" and "Poor"?**

- For the blank row, there's **no specific product** in the filter context
- `SELECTEDVALUE(Products[ProductName])` returns BLANK()
- Without our protective check, the IF measures still execute:
  - They evaluate `[Total Sales]` in this blank context
  - Return classification values like "Low Sales" or "Poor"
- This creates a confusing blank row with unexpected values

**The root cause:**

Table visuals in Power BI can create rows for contexts where your dimension attribute (ProductName) has no value, but your measures still evaluate and return results. This is by design - Power BI wants to show all data perspectives.

**How do we fix this?**

We need to modify our measures to only return values when there's an **actual product** in the filter context. We'll use `SELECTEDVALUE()` to check if there's a real ProductName value, and return BLANK() if not. This way, the blank row will show nothing for these measures.

---

### Step 6.5: Fix the Blank Row Issue

Let's update both measures to handle this properly. **Modify your existing measures** with the improved versions below:

**Updated Sales Category measure:**
```dax
Sales Category (Simple) = 
VAR SelectedProduct = SELECTEDVALUE(Products[ProductName])
RETURN
IF(
    NOT(ISBLANK(SelectedProduct)),
    IF(
        [Total Sales] >= 500,
        "High Sales",
        "Low Sales"
    )
)
```

**Updated Performance Rating measure:**
```dax
Performance Rating (IF) = 
VAR SelectedProduct = SELECTEDVALUE(Products[ProductName])
RETURN
IF(
    NOT(ISBLANK(SelectedProduct)),
    IF(
        [Total Sales] >= 2000,
        "Excellent",
        IF(
            [Total Sales] >= 1000,
            "Good",
            IF(
                [Total Sales] >= 500,
                "Average",
                IF(
                    [Total Sales] >= 100,
                    "Below Average",
                    "Poor"
                )
            )
        )
    )
)
```

**How the fix works:**
- `VAR SelectedProduct = SELECTEDVALUE(Products[ProductName])` - Creates a variable and gets the product name if there's exactly one in context, otherwise returns BLANK()
- `NOT(ISBLANK(SelectedProduct))` - Checks if we actually have a product value (not blank)
- For actual product rows: Returns TRUE → evaluates and shows the category/rating
- For the blank row: Returns FALSE → returns BLANK() and the row shows empty
- For the Total row at the bottom: Also returns FALSE → returns BLANK() (which is correct for totals)

**New concept - VAR (Variables):**
We're introducing variables here using `VAR`. Variables:
- Store intermediate calculations
- Make your DAX more readable and easier to maintain
- Improve performance (the value is calculated once, not multiple times)
- Pattern: `VAR VariableName = Expression` followed by `RETURN` for the final result

**After updating the measures, check your table visual again** - the blank row should now show empty values for these text measures!

**Alternative solutions if you still see the blank row:**

**Option 1 - Filter the Visual:**
1. Select your table visual
2. In the **Filters** pane (View → Filters if not visible)
3. Drag **ProductName** to the **Filters on this visual** area
4. Under "Filter type", select **Basic filtering**
5. Uncheck **(Blank)** to remove blank rows

**Option 2 - Hide the _Measures Table (Best Practice):**
1. Go to Data view
2. Right-click the **_Measures** table
3. Select **Hide in report view**

This prevents users from accidentally using the table itself while still allowing them to use all the measures stored in it.

**Note the nested IF structure:** While it works, it's getting harder to read with 5 conditions. In the next section, we'll learn a better way to write this same logic using SWITCH!

---

## Part 7: SWITCH Function - Better Readability

The SWITCH function is cleaner and more readable than nested IF statements, especially with multiple conditions.

### Step 7.1: Performance Rating with SWITCH

Now let's rewrite the Performance Rating using SWITCH, which is much cleaner. Make sure to include the same blank-checking logic we learned in Step 6.5.

```dax
Performance Rating (SWITCH) = 
VAR SelectedProduct = SELECTEDVALUE(Products[ProductName])
RETURN
IF(
    NOT(ISBLANK(SelectedProduct)),
    SWITCH(
        TRUE(),
        [Total Sales] >= 2000, "Excellent",
        [Total Sales] >= 1000, "Good",
        [Total Sales] >= 500, "Average",
        [Total Sales] >= 100, "Below Average",
        "Poor"
    )
)
```

**How it works:**
- Uses the same `SELECTEDVALUE()` and blank-checking pattern from Step 6.5
- `SWITCH(TRUE(), ...)` evaluates each condition in order
- Returns the value associated with the first TRUE condition
- The last value ("Poor") is the default if nothing else matches
- Much cleaner and easier to maintain than nested IFs!

**Compare:** Look at both measures side by side. They produce identical results, but SWITCH is far more readable.

### Step 7.2: Visualize and Compare Your Conditional Measures

Let's test your IF and SWITCH measures to verify they work correctly and produce identical results.

**Option A: Add to Your Existing Table**

1. Go back to the **table visual** you created in Step 5.8 (showing ProductName and various measures)
2. Add these new measures to your table:
   - **Sales Category (Simple)**
   - **Performance Rating (IF)**
   - **Performance Rating (SWITCH)**

**What you should see:**
- Products are categorized as "High Sales" or "Low Sales"
- Each product gets a performance rating (Excellent, Good, Average, Below Average, or Poor)
- The two Performance Rating measures show **identical values**—proving SWITCH and nested IF produce the same results!

**Option B: Create a New Comparison Table**

1. Create a new **Table** visual
2. Add **Customers[Country]** to the Columns
3. Add these measures:
   - **Total Sales**
   - **Sales Category (Simple)**
   - **Performance Rating (SWITCH)**

This shows how different countries are rated based on their total sales performance.

**Understanding the Results:**

- Countries with sales ≥ 2000: **Excellent**
- Countries with sales ≥ 1000: **Good**
- Countries with sales ≥ 500: **Average**
- Countries with sales ≥ 100: **Below Average**
- Countries with sales < 100: **Poor**

**Pro Tip:** You can now **delete** the "Performance Rating (IF)" measure from your model since the SWITCH version is cleaner and easier to maintain. Keep only the SWITCH version for your report.

---

## Part 8: CALCULATE Function - Mastering Filter Context

The CALCULATE function is one of the most powerful functions in DAX. It allows you to modify the filter context of a calculation.

### Step 8.1: Total Sales All Customers

Let's create a measure that shows total sales across ALL customers, regardless of any customer filters applied:

```dax
Total Sales All Customers = 
CALCULATE(
    [Total Sales],
    ALL(Customers)
)
```

**What it does:**
- Takes the [Total Sales] measure
- Removes any filters on the Customers table
- Calculates total sales as if no customer filter exists

### Step 8.2: Total Sales All Filters

This version ignores ALL filters in the entire report:

```dax
Total Sales All Filters = 
CALCULATE(
    [Total Sales],
    ALL(Sales)
)
```

### Step 8.3: Percentage of Total Sales

Now we can use these to calculate each customer's percentage of total sales:

```dax
% of Total Sales = 
DIVIDE(
    [Total Sales],
    [Total Sales All Customers],
    0
)
```

**How it works:**
- Divides the filtered Total Sales (e.g., for one customer)
- By the Total Sales across all customers
- The third parameter (0) handles division by zero
- Format this measure as a percentage!

**To format as percentage:**
1. Select the measure in the Data view
2. Go to **Measure tools** > **Format**
3. Select **Percentage**
4. Set decimal places to 1 or 2

### Step 8.4: Test It Out

Create a table visual with:
- Rows: Customers[CustomerName]
- Values: [Total Sales], [Total Sales All Customers], [% of Total Sales]

Notice how [Total Sales All Customers] stays constant regardless of which customer row you're on, while [Total Sales] changes for each customer!

---

## Part 9: Additional Useful Measures

### Step 9.1: Average Unit Price

```dax
Avg Unit Price = 
AVERAGEX(
    Sales,
    RELATED(Products[UnitPrice])
)
```

This uses AVERAGEX to iterate over Sales and pull in the related product price.

### Step 9.2: Year-to-Date Sales

```dax
YTD Sales = 
TOTALYTD(
    [Total Sales],
    Calendar[Date]
)
```

This is a time intelligence function that calculates sales from January 1st to the current date in the filter context.

### Step 9.3: Sales Previous Year

```dax
Sales PY = 
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Calendar[Date])
)
```

### Step 9.4: Year-over-Year Growth

```dax
YoY Growth = 
DIVIDE(
    [Total Sales] - [Sales PY],
    [Sales PY],
    0
)
```

Format this as a percentage to see year-over-year growth rates.

---

## Part 10: Build Visualizations

Now let's use your measures to create insights:

### Suggested Visuals:

1. **Card Visuals** for KPIs:
   - Total Sales
   - Average Sales
   - Transaction Count

2. **Column Chart**:
   - X-axis: Calendar[MonthName]
   - Y-axis: [Total Sales]
   - Shows sales trends over months

3. **Bar Chart**:
   - Y-axis: Products[ProductName]
   - X-axis: [Total Sales]
   - Shows top-selling products

4. **Table**:
   - Customers[Country]
   - [Total Sales]
   - [% of Total Sales]
   - [Performance Rating (SWITCH)]

5. **Matrix Visual**:
   - Rows: Calendar[Year], Calendar[Quarter]
   - Values: [Total Sales], [YoY Growth]

---

## Part 11: Key Takeaways

### ✅ DAX Fundamentals You've Learned:

1. **Calculated Columns vs Measures**
   - Calculated columns are computed row-by-row and stored
   - Measures are computed dynamically based on context

2. **Basic Aggregation Functions**
   - SUM, AVERAGE, MAX, MIN, COUNT, COUNTROWS

3. **Conditional Logic**
   - IF for simple conditions
   - Nested IF for multiple conditions (but not recommended)
   - SWITCH for cleaner, more maintainable multi-condition logic

4. **Filter Context Manipulation**
   - CALCULATE changes the filter context
   - ALL removes filters
   - Essential for % of Total and similar calculations

5. **Time Intelligence**
   - Requires a proper Calendar table marked as Date Table
   - Functions like TOTALYTD, SAMEPERIODLASTYEAR

6. **Best Practices**
   - Always use fully qualified column references: `Table[Column]`
   - Prefer CALENDAR over CALENDARAUTO for production models
   - Use SWITCH instead of nested IFs
   - Organize measures in a dedicated measures table
   - Format measures appropriately (currency, percentage, etc.)

---

## Additional Resources

- [Microsoft DAX Reference](https://learn.microsoft.com/en-us/dax/)
- [SQLBI - The definitive guide to DAX](https://www.sqlbi.com/books/the-definitive-guide-to-dax-2nd-edition/)
- [DAX Patterns](https://www.daxpatterns.com/)

---

## Challenge Questions (Optional)

Try to answer these questions using your report:

1. Which country generated the highest sales?
2. What percentage of total sales came from Europe?
3. Which product has the highest average transaction value?
4. How many unique customers made purchases?
5. What is the month-over-month sales trend?
6. Which quarter had the best performance rating?

---

## Summary

Congratulations! You've completed the DAX Basics lab. You now understand:
- How to structure a data model with facts and dimensions
- The difference between calculated columns and measures
- Basic DAX aggregation functions
- Conditional logic with IF and SWITCH
- Filter context manipulation with CALCULATE and ALL
- Time intelligence basics

These fundamentals form the foundation for more advanced DAX development. Keep practicing, and remember: understanding context (row context vs filter context) is the key to mastering DAX!

---

**Questions?** Reach out to your instructor or refer to the lecture slides for conceptual explanations.
