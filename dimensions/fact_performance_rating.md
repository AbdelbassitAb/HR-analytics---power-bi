# Power Query — `FactPerformanceRating`

This document describes the **Power Query (ETL) steps** applied to the `FactPerformanceRating` table in the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - This table represents the **fact table** of the model.  
> - It stores employee performance reviews and satisfaction scores.  
> - It is designed to be analyzed over time and linked to multiple dimensions.

---

## 1) Table role (why this table exists)
`FactPerformanceRating` is the **central fact table** used to:
- Track employee performance evaluations
- Store satisfaction and rating scores
- Enable time-based analysis (reviews over time)
- Support individual employee monitoring (Performance Tracker page)

---

## 2) Main columns used in the report
Key fields used in the dashboard:
- `PerformanceID` (primary key)
- `EmployeeID` (foreign key → `DimEmployee`)
- `ReviewDate`
- Satisfaction scores:
  - `JobSatisfaction`
  - `EnvironmentSatisfaction`
  - `RelationshipSatisfaction`
  - `WorkLifeBalance`
- Performance ratings:
  - `SelfRating`
  - `ManagerRating`

---

## 3) Power Query transformations (step-by-step)

### Step A — Load & header normalization
- Load the performance rating dataset
- Promote first row to headers
- Rename columns where necessary for clarity and consistency

**Why:** ensures clean and readable field names for modeling and DAX.

---

### Step B — Data types standardization
Apply appropriate data types:
- `PerformanceID` → Whole number
- `EmployeeID` → Whole number
- `ReviewDate` → Date
- Satisfaction and rating columns → Whole number

**Why:** numeric typing is required for aggregations and mapping to rating/satisfaction dimensions.

---

### Step C — Data quality checks
Typical checks applied:
- Ensure `EmployeeID` is never null
- Validate that satisfaction and rating values fall within expected ranges (1–5)
- Remove duplicate rows if any exist at the `(EmployeeID, ReviewDate)` level

**Why:** avoids incorrect aggregations and duplicated evaluations.

---

### Step D — Keep raw scores (no business logic)
- Raw numeric values (1–5) are kept as-is
- No textual mapping (e.g., “Satisfied”, “Excellent”) is done in Power Query

**Why:**  
Mapping is handled later through:
- `DimRatingLevel`
- `DimSatisfiedLevel`
using **DAX + USERELATIONSHIP**, which keeps the model flexible and scalable.

---

## 4) Output (what this table enables in the dashboard)
`FactPerformanceRating` enables:
- Performance and satisfaction tracking per employee
- Comparison between **SelfRating** and **ManagerRating**
- Analysis of satisfaction dimensions over time
- Calculation of:
  - `JobSatisfaction`
  - `EnvironmentSatisfaction`
  - `RelationshipSatisfaction`
  - `WorkLifeBalance`
- HR review dates:
  - `LastReviewDate`
  - `NextReviewDate`

---

## 5) Good practices applied
- Clear separation between **facts** and **dimensions**
- No business logic embedded in Power Query
- Designed for easy extension (new review dates, new rating dimensions)
- Clean numeric fields for reliable DAX calculations

---

## 6) Quick checklist (for reviewers / recruiters)
✅ Proper fact table grain (one row per employee review)  
✅ Clean foreign key to `DimEmployee`  
✅ Numeric scores ready for analytical mapping  
✅ Optimized for time-based analysis  

