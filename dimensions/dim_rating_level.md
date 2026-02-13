# Power Query — `DimRatingLevel`

This document describes the `DimRatingLevel` table used in the HR Analytics – Employee Attrition Dashboard.

## 1) Purpose
`DimRatingLevel` is a lookup (dimension) table that converts numeric rating values into readable performance categories.  
It improves clarity in visuals and supports performance-related analysis.

---

## 2) Structure

Columns:
- `RatingID` (primary key, numeric)
- `RatingLevel` (text label)

Example values:
- 1 → Unacceptable  
- 2 → Needs Improvement  
- 3 → Meets Expectation  
- 4 → Exceeds Expectation  
- 5 → Above and Beyond  

---

## 3) Power Query Processing

No manual transformations were applied.

The table was:
- Loaded directly from the dataset
- Automatically typed by Power BI
- Integrated into the model without additional cleaning

The source data was already structured and clean.

---

## 4) Data Model Relationship

Inactive relationships were created between:
- `DimRatingLevel[RatingID]`
- `FactPerformanceRating[ManagerRating]`
- `FactPerformanceRating[SelfRating]`

These relationships are activated in DAX using `USERELATIONSHIP()` when needed.

---

## 5) Analytical Contribution

This table enables:
- Clear display of performance categories in visuals
- Comparison between ManagerRating and SelfRating
- Cleaner and more maintainable DAX logic

---

## 6) Design Note

Keeping rating levels in a separate dimension:
- Maintains a clean star schema
- Avoids hardcoding labels in fact tables
- Supports scalable model design
