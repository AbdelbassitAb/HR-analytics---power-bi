# Power Query — `DimEmployee`

This document describes the **Power Query (ETL) steps** applied to the `DimEmployee` table in the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - The dataset comes from the *Atlas Labs* HR dataset (DataCamp).  
> - The transformations below focus on **cleaning, standardization, and analytics-ready fields**.  
> - Business calculations (KPIs) are handled in **DAX** (not in Power Query), except for simple derived columns used for segmentation (e.g., age groups).

---

## 1) Table role (why this table exists)
`DimEmployee` is the **central dimension** of the model:
- It stores employee attributes used for filtering/segmenting visuals (department, job role, demographics, etc.)
- It provides keys and fields used by DAX measures (e.g., `EmployeeID`, `Attrition`, `HireDate`, `Salary`)

---

## 2) Main columns used in the report
Typical fields used across pages:
- `EmployeeID` (primary key)
- `Attrition` (Yes/No)
- `HireDate`
- `Department`, `JobRole`
- `Gender`, `MaritalStatus`, `Ethnicity`
- `Age` + **`AgeBins`** (created in Power Query)
- `Salary`
- Other HR descriptors (e.g., `BusinessTravel`, `OverTime`, `DistanceFromHome`, etc., depending on the dataset version)

---

## 3) Power Query transformations (step-by-step)

### Step A — Load & header normalization
- Load the employee dataset into Power Query
- Promote first row to headers (if needed)
- Ensure column names are consistent and readable

**Why:** avoids messy headers and ensures columns are stable for modeling.

---

### Step B — Data types standardization
Set correct data types:
- `EmployeeID` → Whole number (or text if the source uses strings)
- `HireDate` → Date
- `Age` → Whole number
- `Salary` → Decimal number (or Whole number)
- `Attrition`, `Department`, `JobRole`, `Gender`, etc. → Text

**Why:** correct typing improves model reliability and prevents DAX errors (especially with dates and numeric aggregations).

---

### Step C — Create `AgeBins` (age groups)
Create a **conditional column** named `AgeBins` to group employees into age ranges:

- `<20`
- `20–29`
- `30–39`
- `40–49`
- `50+`

**Why:** age bins are more readable in dashboards than raw ages, and improve segmentation (Demographics page).

---

### Step D — Basic cleanup (quality)
Common cleanup actions applied:
- Trim/Clean text columns (remove extra spaces)
- Replace nulls/blanks where necessary (only for display fields)
- Remove duplicates on `EmployeeID` **if the source contains duplicates**
- Keep only relevant columns (optional) to reduce model size

**Why:** improves data quality and report performance.

---

## 4) Output (what this table enables in the dashboard)
`DimEmployee` powers:
- Employee counts: `TotalEmployees`, `ActiveEmployees`, `InactiveEmployees`
- Attrition segmentation by department/job role
- Demographics analysis (age, gender, marital status, ethnicity)
- Salary analysis (`AverageSalary`)
- Time analysis using `HireDate` with `DimDate` (via `USERELATIONSHIP` in DAX)

---

## 5) Good practices applied
- Keep **business logic in DAX** (measures), not in Power Query
- Use Power Query mainly for:
  - typing
  - cleaning
  - lightweight derived fields for segmentation (`AgeBins`)
- Centralize measures in a dedicated `_Measures` table (model best practice)

---

## 6) Quick checklist (for reviewers / recruiters)
✅ Clean column types (dates, numbers, texts)  
✅ Analytics-ready segmentation (`AgeBins`)  
✅ Central employee dimension used consistently across visuals  
✅ Minimal transformations to keep the pipeline maintainable  

