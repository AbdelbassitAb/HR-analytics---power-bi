# Power Query — `DimDate`

This document describes the **Power Query (ETL) steps** used to create and prepare the `DimDate` table for the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - `DimDate` is a **calendar (date) dimension** created to support time-based analysis.  
> - It is used in combination with **active and inactive relationships** (via `USERELATIONSHIP` in DAX).  

---

## 1) Table role (why this table exists)
`DimDate` enables:
- Time-based analysis of hiring and attrition
- Trend analysis (monthly, yearly)
- Date slicing for HR KPIs
- Advanced DAX calculations using alternative date contexts

---

## 2) Date range definition
The date table covers a range that includes:
- Minimum employee **HireDate**
- Maximum **ReviewDate**
- Additional buffer dates to ensure full coverage

**Why:** ensures all time-based visuals work correctly without missing dates.

---

## 3) Power Query creation steps

### Step A — Generate date range
- Create a continuous list of dates
- Start date = earliest relevant HR date
- End date = latest relevant HR date

**Why:** guarantees a complete calendar without gaps.

---

### Step B — Convert list to table
- Convert the date list into a table
- Rename the column to `Date`
- Set data type to **Date**

---

### Step C — Create calendar attributes
Add derived columns commonly used in BI reporting:
- `Year`
- `Month`
- `MonthNumber`
- `MonthName`
- `Quarter`
- `Day`
- `DayName`
- `DayOfWeek`

**Why:** simplifies slicing, grouping, and trend analysis in visuals.

---

### Step D — Mark as Date Table (Power BI)
- Mark `DimDate` as the official **Date Table** in Power BI
- Use the `Date` column as the key

**Why:** improves time intelligence behavior and ensures correct DAX evaluation.

---

## 4) Relationships in the data model
`DimDate` is linked to:
- `DimEmployee[HireDate]` (inactive relationship)
- `FactPerformanceRating[ReviewDate]` (active or inactive depending on model)

DAX measures activate the relevant relationship using:
- `USERELATIONSHIP()`

---

## 5) Output (what this table enables in the dashboard)
`DimDate` enables:
- Hiring and attrition trends over time
- Time-based attrition rate calculations
- Drill-down analysis (Year → Month)
- Consistent time slicing across all pages

---

## 6) Good practices applied
- Continuous calendar (no missing dates)
- No business logic in Power Query
- Time intelligence handled in DAX
- Single, centralized date dimension

---

## 7) Quick checklist (for reviewers / recruiters)
✅ Complete and continuous calendar  
✅ Marked as Date Table  
✅ Supports inactive relationships  
✅ Ready for advanced DAX time analysis  

