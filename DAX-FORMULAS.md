# DAX Formulas Quick Reference

This document contains all DAX formulas used in the lab for quick copy-paste reference.

---

## Calendar Table

### Create Calendar Table
```dax
Calendar = CALENDARAUTO()
```

### Alternative (Best Practice)
```dax
Calendar = CALENDAR(DATE(2023,1,1), DATE(2025,12,31))
```

---

## Calculated Columns (Calendar Table)

### Year
```dax
Year = YEAR(Calendar[Date])
```

### Quarter
```dax
Quarter = "Q" & FORMAT(QUARTER(Calendar[Date]), "0")
```

### Month Number
```dax
MonthNumber = MONTH(Calendar[Date])
```

### Month Name
```dax
MonthName = FORMAT(Calendar[Date], "MMMM")
```

---

## Measures Table (Optional Setup)

### Create Measures Table
```dax
_Measures = {1}
```

---

## Basic Aggregation Measures

### Total Sales
```dax
Total Sales = SUM(Sales[SalesAmount])
```

### Average Sales
```dax
Average Sales = AVERAGE(Sales[SalesAmount])
```

### Maximum Sale
```dax
Max Sale = MAX(Sales[SalesAmount])
```

### Minimum Sale
```dax
Min Sale = MIN(Sales[SalesAmount])
```

### Total Quantity
```dax
Total Quantity = SUM(Sales[Quantity])
```

### Transaction Count
```dax
Transaction Count = COUNTROWS(Sales)
```

---

## Conditional Logic - IF Statements

### Simple IF
```dax
Sales Category (Simple) = 
IF(
    [Total Sales] >= 500,
    "High Sales",
    "Low Sales"
)
```

### Complex Nested IF
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

### Optional: Defensive Version (If You See Blank Rows)

If your Calendar-Sales relationship is incorrect (1:1 or Both filter direction), use these versions with protective checks:

**Sales Category with SELECTEDVALUE:**
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

**Performance Rating with SELECTEDVALUE:**
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

> **Note:** Fix your relationship instead of using these defensive versions. See LAB-GUIDE Step 4.4 and Step 6.4 for details.

---

## SWITCH Function (Better Alternative)

### Performance Rating with SWITCH
```dax
Performance Rating (SWITCH) = 
SWITCH(
    TRUE(),
    [Total Sales] >= 2000, "Excellent",
    [Total Sales] >= 1000, "Good",
    [Total Sales] >= 500, "Average",
    [Total Sales] >= 100, "Below Average",
    "Poor"
)
```

### Optional: SWITCH with Defensive Checks (If You See Blank Rows)
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

> **Note:** Fix your Calendar-Sales relationship instead (see Step 6.4).

---

## CALCULATE Function - Filter Context

### Total Sales Ignoring Customer Filters
```dax
Total Sales All Customers = 
CALCULATE(
    [Total Sales],
    ALL(Customers)
)
```

### Total Sales Ignoring All Filters
```dax
Total Sales All Filters = 
CALCULATE(
    [Total Sales],
    ALL(Sales)
)
```

### Percentage of Total
```dax
% of Total Sales = 
DIVIDE(
    [Total Sales],
    [Total Sales All Customers],
    0
)
```

---

## Bonus Measures

### Average Unit Price
```dax
Avg Unit Price = 
AVERAGEX(
    Sales,
    RELATED(Products[UnitPrice])
)
```

### Year-to-Date Sales
```dax
YTD Sales = 
TOTALYTD(
    [Total Sales],
    Calendar[Date]
)
```

### Sales Previous Year
```dax
Sales PY = 
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Calendar[Date])
)
```

### Year-over-Year Growth
```dax
YoY Growth = 
DIVIDE(
    [Total Sales] - [Sales PY],
    [Sales PY],
    0
)
```

---

## Tips

- **Formatting:** Select measures and format them appropriately:
  - Currency: Total Sales, Average Sales, Max Sale, Min Sale
  - Percentage: % of Total Sales, YoY Growth
  - Whole Number: Total Quantity, Transaction Count

- **Sort by Column:** For MonthName column, sort by MonthNumber

- **Mark as Date Table:** Select Calendar table → Table tools → Mark as date table → Choose "Date" column

---

## Common Mistakes to Avoid

1. ❌ **Incorrect Calendar-Sales relationship** (1:1 or Both filter direction)
   ✅ Must be **One-to-Many (1:*)** with **Single** direction
   **Symptom:** Blank rows appearing in table visuals with unexpected values
   **Fix:** Delete and recreate relationship following Step 4.4 in LAB-GUIDE.md

2. ❌ Not using fully qualified references: `SUM(SalesAmount)` 
   ✅ Always use: `SUM(Sales[SalesAmount])`

3. ❌ Creating calculated columns when you need measures
   ✅ Use measures for aggregations that change with filters

4. ❌ Using CALENDARAUTO in production
   ✅ Use CALENDAR with explicit dates or create in Power Query

5. ❌ Nested IFs for many conditions
   ✅ Use SWITCH for better readability

6. ❌ Forgetting to mark Calendar as Date Table
   ✅ Required for time intelligence functions to work properly
