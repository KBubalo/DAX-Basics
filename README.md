# DAX Basics - University Lab

A comprehensive, hands-on learning resource for university students learning DAX (Data Analysis Expressions) in Power BI.

## 📚 About This Lab

This lab provides a simplified, focused introduction to DAX fundamentals. It's designed specifically for university students who are new to Power BI and DAX, offering a more approachable alternative to Microsoft's official labs while still covering essential concepts.

### What You'll Learn

- Creating and configuring a Calendar/Date table with DAX
- Understanding calculated columns vs measures
- Building relationships in a star schema data model
- Basic aggregation functions (SUM, AVERAGE, MAX, MIN)
- Conditional logic with IF and SWITCH functions
- Filter context manipulation with CALCULATE
- Creating percentage of total calculations
- Time intelligence basics
- DAX best practices and common pitfalls

### Prerequisites

- Power BI Desktop installed
- Basic understanding of tables and data relationships
- Completion of the DAX Basics lecture slides

### Lab Duration

60-90 minutes

---

## 🚀 Getting Started

### 1. Download This Repository

**Option A: Download as ZIP**
1. Click the green **Code** button at the top of this page
2. Select **Download ZIP**
3. Extract the ZIP file to a location on your computer

**Option B: Clone with Git**
```bash
git clone https://github.com/KBubalo/DAX-Basics.git
```

### 2. Open the Lab Guide

Once downloaded, open **[LAB-GUIDE.md](LAB-GUIDE.md)** and follow the step-by-step instructions.

---

## 📂 Repository Structure

```
DAX-Basics/
│
├── data/                      # Sample CSV files for the lab
│   ├── Sales.csv             # Fact table with sales transactions
│   ├── Products.csv          # Dimension table with product information
│   └── Customers.csv         # Dimension table with customer information
│
├── solution/                  # Completed Power BI solution file
│   └── DAX Basics Solution File.pbip
│
├── LAB-GUIDE.md              # Complete step-by-step lab instructions
├── DAX-FORMULAS.md           # Quick reference for all DAX formulas
└── README.md                 # This file
```

---

## 📊 Sample Data

The lab uses three simple CSV files representing an electronics retailer:

### Sales (Fact Table)
- 76 transactions spanning 2024
- Fields: SaleID, SaleDate, ProductID, CustomerID, Quantity, SalesAmount

### Products (Dimension Table)
- 5 products in the Electronics and Accessories categories
- Fields: ProductID, ProductName, Category, UnitPrice

### Customers (Dimension Table)
- 30 customers from various countries and regions
- Fields: CustomerID, CustomerName, Country, City, Region

This simplified dataset makes it easy to understand concepts without getting overwhelmed by data complexity.

---

## 📖 Lab Contents

### Part 1: Data Import and Modeling
- Import CSV files into Power BI
- Review data structure
- Create proper relationships (1:many)

### Part 2: Calendar Table Creation
- Create a date table using CALENDARAUTO()
- Discussion of best practices (CALENDAR vs CALENDARAUTO)

### Part 3: Calculated Columns
- Add Year, Quarter, Month Name, Month Number
- Sort by column technique
- Mark table as Date Table

### Part 4: Relationships
- Create proper star schema
- Understand one-to-many relationships

### Part 5: Basic Measures
- SUM, AVERAGE, MAX, MIN
- COUNT and COUNTROWS
- Best practice: dedicated measures table

### Part 6: IF Statements
- Simple conditional logic
- Complex nested IF with 5 conditions

### Part 7: SWITCH Function
- Cleaner alternative to nested IFs
- Same logic, better readability

### Part 8: CALCULATE Function
- Remove filters with ALL()
- Create percentage of total calculations
- Master filter context manipulation

### Part 9: Bonus Measures (Optional)
- Time intelligence (YTD, Previous Year)
- Year-over-Year growth calculations

### Part 10: Visualizations
- Build charts and tables using your measures
- Create an interactive dashboard

---

## 🎯 Learning Objectives

By completing this lab, you will be able to:

✅ Create and configure a proper Calendar table for time-based analysis  
✅ Distinguish between calculated columns and measures  
✅ Build a star schema data model with correct relationships  
✅ Write basic DAX aggregation functions  
✅ Implement conditional logic using IF and SWITCH  
✅ Use CALCULATE to manipulate filter context  
✅ Create percentage of total calculations  
✅ Apply DAX best practices in your own reports  

---

## 📝 Quick Reference

All DAX formulas used in this lab are available in **[DAX-FORMULAS.md](DAX-FORMULAS.md)** for quick copy-paste reference.

---

## 💡 Teaching Tips (For Instructors)

This lab is designed to be taught **after** your introductory DAX lecture slides. The lab focuses on hands-on practice rather than conceptual explanations.

**Recommended flow:**
1. Lecture: DAX syntax, context, and basic concepts (slides provided)
2. Lab: Hands-on practice with this repository
3. Discussion: Review challenging concepts and answer questions

**Common student challenges:**
- Understanding the difference between calculated columns and measures
- Grasping filter context vs row context
- Remembering to use fully qualified references: `Table[Column]`
- Knowing when to use CALCULATE vs basic aggregations

**Timing suggestions:**
- Parts 1-4 (Import & Model): 15-20 minutes
- Parts 5-7 (Basic Measures & Logic): 20-25 minutes
- Part 8 (CALCULATE): 15-20 minutes
- Parts 9-10 (Bonus & Visuals): 10-15 minutes

---

## 🔗 Additional Resources

- [Microsoft DAX Reference](https://learn.microsoft.com/en-us/dax/)
- [SQLBI - The Definitive Guide to DAX](https://www.sqlbi.com/books/the-definitive-guide-to-dax-2nd-edition/)
- [DAX Patterns](https://www.daxpatterns.com/)
- [DAX Formatter](https://www.daxformatter.com/) - Clean up your DAX code

---

## 🤝 Feedback and Contributions

This lab is designed for university teaching. If you're an instructor using this material:
- Feel free to adapt it to your needs
- Share feedback on what worked well or could be improved
- Suggest additional topics to cover

---

## 📄 License

This educational resource is provided for learning purposes.

---

**Ready to start?** Open **[LAB-GUIDE.md](LAB-GUIDE.md)** and begin your DAX learning journey! 🚀
