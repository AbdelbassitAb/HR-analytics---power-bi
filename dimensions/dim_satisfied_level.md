# Power Query — `DimSatisfiedLevel`

This document describes the `DimSatisfiedLevel` table used in the HR Analytics – Employee Attrition Dashboard.

## 1) Purpose
`DimSatisfiedLevel` is a lookup (dimension) table that converts numeric satisfaction scores into readable categories.  
It improves clarity in satisfaction-related visuals and analysis.

---

## 2) Structure

Columns:
- `SatisfactionID` (primary key, numeric)
- `SatisfactionLevel` (text label)

Example values:
- 1 → Very Dissatisfied  
- 2 → Dissatisfied  
- 3 → Neutral  
- 4 → Satisfied  
- 5 → Very Satisfied  

---

## 3) Power Query Processing

No manual transformations were applied.

The table was:
- Loaded directly from the dataset
- Automatically typed by Power BI
- Integrated into the model without additional cleaning

The source data was already clean and properly structured.

---

## 4) Data Model Relationship

Inactive relationships were created between:
- `DimSatisfiedLevel[SatisfactionID]`
- `FactPerformanceRating[JobSatisfaction]`
- `FactPerformanceRating[EnvironmentSatisfaction]`
- `FactPerformanceRating[RelationshipSatisfaction]`
- `FactPerformanceRating[WorkLifeBalance]`

These relationships are activated in DAX using `USERELATIONSHIP()` when required.

---

## 5) Analytical Contribution

This table enables:
- Clear display of satisfaction levels in visuals
- Consistent interpretation of satisfaction scores
- Cleaner DAX logic by separating codes from labels

---

## 6) Design Note

Using a separate satisfaction dimension:
- Keeps the model aligned with star schema principles
- Avoids duplication of labels
- Improves scalability and maintainability
