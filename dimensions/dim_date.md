# Power Query — `DimDate`


---

## 1) Purpose of `DimDate`

`DimDate` is the calendar table used to:
- enable time-based analysis (Year/Month trends, period comparisons)
- standardize date filtering across the report
- support measures that require a date context (for example, attrition over time)

---

## 2) How `DimDate` was created in Power Query

The Date dimension was generated directly in Power Query using a calendar creation approach (date range + derived attributes).

### Step A — Create a continuous date range
- Build a continuous list of dates covering the analysis period.
- The date range should include all relevant business dates used in the model (at minimum, the period covered by employee hire dates and/or review dates).

Reason:
- ensures there are no missing dates, which is important for reliable time series visuals

### Step B — Convert the list to a table and set data types
- Convert the list into a table with a single column named `Date`.
- Set the `Date` column type to Date.

Reason:
- correct typing is required for date intelligence and relationships

### Step C — Add calendar attributes
Add common attributes such as:
- `Year`
- `MonthNumber`
- `MonthName`
- `Quarter`
- `Day`
- `DayName`
- `DayOfWeek`

Reason:
- these fields simplify reporting (for example, attrition by month) and allow drill-down (Year → Month)

---

## 3) Configuration steps in Power BI (outside Power Query)

After creating the table, the following modeling steps were applied in Power BI.

### Step A — Mark `DimDate` as a Date table
- In Power BI: Table tools → Mark as date table
- Select the `Date` column as the date column

Reason:
- improves time intelligence behavior and prevents ambiguous date handling
- ensures consistent time filtering and sorting

### Step B — Create relationships in the data model
Create relationships between `DimDate[Date]` and the date fields used in the model, depending on the analysis needs, for example:
- `DimEmployee[HireDate]` (often created as an inactive relationship and activated in measures)
- `FactPerformanceRating[ReviewDate]` (if review timelines are analyzed)

Reason:
- allows visuals and measures to use a shared calendar for filtering
- supports alternative date contexts using `USERELATIONSHIP()` in DAX when needed

---

## 4) What `DimDate` enables in the report

With `DimDate` created and configured, the report can:
- display trends over time (monthly/yearly)
- enable consistent time filtering with slicers
- support attrition measures evaluated by period (for example, `% Attrition Rate Date`)
- allow drill-down navigation in time-based visuals

---

## 5) Recommended quality checks

- `DimDate[Date]` contains unique values (no duplicates)
- The date range covers all relevant business dates (no missing periods)
- Date columns are correctly typed as Date in Power BI
- Relationships are correctly set (cardinality and filter direction)
- The table is marked as a Date table

---

## 6) Summary of best practices applied

- Continuous calendar table (no gaps)
- Calendar attributes added for analysis and drill-down
- Date table explicitly marked as a Date table in Power BI
- Relationships created at the model level and used by time-based measures
