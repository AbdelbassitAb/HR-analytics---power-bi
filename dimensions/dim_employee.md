# Power Query — `DimEmployee`

This document describes the Power Query steps for the `DimEmployee` table used in the HR Analytics – Employee Attrition Dashboard.

## 1) Purpose
`DimEmployee` is the central dimension table. It stores employee attributes used for filtering and segmentation (department, job role, demographics, salary, hire date, attrition status).

---

## 2) Main Fields Used
Common fields used across the report include:
- `EmployeeID` (key)
- `Attrition` (Yes/No)
- `HireDate`
- `Department`, `JobRole`
- `Gender`, `MaritalStatus`, `Ethnicity`
- `Age` and the derived field `AgeBins`
- `Salary`
- Other HR descriptors from the dataset (for example, travel, overtime, distance), depending on the source version

---

## 3) Power Query Processing
Most steps were handled automatically during load:
- Headers detected correctly
- Data types inferred by Power BI
- No manual cleaning required

### Manual transformation applied: `AgeBins`
A single transformation was performed: creating an `AgeBins` column using a conditional rule to group ages into ranges:
- `<20`
- `20–29`
- `30–39`
- `40–49`
- `50+`

This makes demographic analysis easier and improves chart readability.

---

## 4) How This Table Is Used in the Model
`DimEmployee` supports:
- Employee counts and attrition measures (Total, Active, Inactive, Attrition Rate)
- Segmentation by department and job role (Overview and Attrition pages)
- Demographic breakdowns (Demographics page)
- Salary analysis (AverageSalary)
- Time-based analysis using `HireDate` with `DimDate` (activated in DAX via `USERELATIONSHIP`)

---

## 5) Design Note
Business logic is implemented in DAX measures (in the `_Measures` table). Power Query was only used for the AgeBins grouping.
