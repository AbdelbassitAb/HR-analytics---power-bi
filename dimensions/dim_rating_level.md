# Power Query — `DimRatingLevel`

This document describes the **Power Query (ETL) steps** applied to the `DimRatingLevel` table in the *HR Analytics – Employee Attrition Dashboard*.

> Notes  
> - `DimRatingLevel` is a **lookup (dimension) table**.  
> - It translates numeric performance rating codes into **readable business labels**.  

---

## 1) Table role (why this table exists)
`DimRatingLevel` is used to:
- Convert numeric rating values (e.g., 1–5) into meaningful categories
- Improve readability of dashboards and reports
- Support DAX measures using `USERELATIONSHIP`

---

## 2) Main columns
- `RatingID` (primary key, numeric)
- `RatingLevel` (text label)

**Example values:**
- 1 → Unacceptable  
- 2 → Needs Improvement  
- 3 → Meets Expectation  
- 4 → Exceeds Expectation  
- 5 → Above and Beyond  

---

## 3) Power Query transformations

### Step A — Load lookup data
- Load the rating level dataset
- Promote headers
- Rename columns for clarity (`RatingID`, `RatingLevel`)

**Why:** ensures clean and consistent column naming.

---

### Step B — Data types
- `RatingID` → Whole number
- `RatingLevel` → Text

**Why:** correct typing ensures reliable joins and clean visuals.

---

### Step C — Data validation
- Check for duplicate `RatingID`
- Ensure rating scale consistency (1 to 5)
- Remove unnecessary columns (if any)

**Why:** lookup tables must remain small, clean, and stable.

---

## 4) Relationships in the model
`DimRatingLevel` is linked via **inactive relationships** to:
- `FactPerformanceRating[ManagerRating]`
- `FactPerformanceRating[SelfRating]`

These relationships are activated in DAX using:
- `USERELATIONSHIP()`

---

## 5) Output (what this table enables)
`DimRatingLevel` enables:
- Readable performance ratings in visuals
- Consistent interpretation of rating scores
- Cleaner DAX measures and model logic

---

## 6) Good practices applied
- Small, dedicated lookup table
- No business logic in Power Query
- Relationships handled dynamically in DAX
- Improves dashboard usability

---

## 7) Quick checklist (for reviewers / recruiters)
✅ Clean rating scale (1–5)  
✅ Readable business labels  
✅ Proper use of lookup table  
✅ Integrated with DAX via USERELATIONSHIP  

