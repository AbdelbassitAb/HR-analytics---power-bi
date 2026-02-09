# Power Query — `DimSatisfiedLevel`

This document describes the **Power Query (ETL) steps** applied to the `DimSatisfiedLevel` table in the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - `DimSatisfiedLevel` is a **lookup (dimension) table**.  
> - It translates numeric satisfaction scores into **business-readable labels**.  

---

## 1) Table role (why this table exists)
`DimSatisfiedLevel` is used to:
- Convert numeric satisfaction values (e.g., 1–5) into meaningful categories
- Improve readability of satisfaction-related visuals
- Support DAX measures using `USERELATIONSHIP`

---

## 2) Main columns
- `SatisfactionID` (primary key, numeric)
- `SatisfactionLevel` (text label)

**Example values:**
- 1 → Very Dissatisfied  
- 2 → Dissatisfied  
- 3 → Neutral  
- 4 → Satisfied  
- 5 → Very Satisfied  

---

## 3) Power Query transformations

### Step A — Load lookup data
- Load the satisfaction level dataset
- Promote headers
- Rename columns for clarity (`SatisfactionID`, `SatisfactionLevel`)

**Why:** ensures clean and consistent column naming.

---

### Step B — Data types
- `SatisfactionID` → Whole number
- `SatisfactionLevel` → Text

**Why:** correct typing ensures reliable joins and readable visuals.

---

### Step C — Data validation
- Check for duplicate `SatisfactionID`
- Ensure satisfaction scale consistency (1 to 5)
- Remove unnecessary columns (if any)

**Why:** lookup tables must remain small, clean, and stable.

---

## 4) Relationships in the model
`DimSatisfiedLevel` is linked via **inactive relationships** to:
- `FactPerformanceRating[JobSatisfaction]`
- `FactPerformanceRating[EnvironmentSatisfaction]`
- `FactPerformanceRating[RelationshipSatisfaction]`
- `FactPerformanceRating[WorkLifeBalance]`

These relationships are activated in DAX using:
- `USERELATIONSHIP()`

---

## 5) Output (what this table enables)
`DimSatisfiedLevel` enables:
- Readable satisfaction levels in dashboards
- Consistent interpretation of satisfaction scores
- Cleaner and more maintainable DAX measures

---

## 6) Good practices applied
- Small, dedicated lookup table
- No business logic in Power Query
- Relationships activated dynamically in DAX
- Improves dashboard clarity and user understanding

---

## 7) Quick checklist (for reviewers / recruiters)
✅ Clean satisfaction scale (1–5)  
✅ Clear business labels  
✅ Proper use of lookup table  
✅ Integrated with DAX via USERELATIONSHIP  

