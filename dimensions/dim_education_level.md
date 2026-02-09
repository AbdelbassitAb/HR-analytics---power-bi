# Power Query — `DimEducationLevel`

This document describes the **Power Query (ETL) steps** applied to the `DimEducationLevel` table in the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - `DimEducationLevel` is a **lookup (dimension) table**.  
> - It standardizes education level codes into **clear, business-readable categories**.

---

## 1) Table role (why this table exists)
`DimEducationLevel` is used to:
- Translate numeric education level codes into meaningful labels
- Enable education-based segmentation in HR analysis
- Improve readability and consistency across dashboards

---

## 2) Main columns
- `EducationLevelID` (primary key, numeric)
- `EducationLevel` (text label)

**Example values:**
- 1 → No Formal Qualifications  
- 2 → High School  
- 3 → Bachelors  
- 4 → Masters  
- 5 → Doctorate  

---

## 3) Power Query transformations

### Step A — Load lookup data
- Load the education level dataset
- Promote headers
- Rename columns for clarity (`EducationLevelID`, `EducationLevel`)

**Why:** ensures consistent naming and avoids ambiguity in the data model.

---

### Step B — Data types
- `EducationLevelID` → Whole number
- `EducationLevel` → Text

**Why:** correct typing is essential for reliable joins and clean report visuals.

---

### Step C — Data validation
- Check for duplicate `EducationLevelID`
- Ensure values are within the expected range
- Remove unnecessary or technical columns (if any)

**Why:** lookup tables must remain small, stable, and error-free.

---

## 4) Relationships in the data model
`DimEducationLevel` is linked to:
- `DimEmployee[EducationLevelID]`

Relationship type:
- One-to-many (1 → N)

**Why:** allows slicing employee data by education level in reports.

---

## 5) Output (what this table enables)
`DimEducationLevel` enables:
- Analysis of workforce composition by education level
- Comparison of attrition rates across education categories
- Cleaner visuals with readable education labels

---

## 6) Good practices applied
- Dedicated lookup table
- No business logic in Power Query
- Small and stable dimension for performance
- Consistent labeling across the model

---

## 7) Quick checklist (for reviewers / recruiters)
✅ Clean education hierarchy  
✅ Proper dimension design  
✅ Simple and maintainable transformations  
✅ Ready for HR segmentation analysis  

