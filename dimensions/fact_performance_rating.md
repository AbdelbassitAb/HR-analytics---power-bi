# Power Query — `FactPerformanceRating`


## 1) Purpose

`FactPerformanceRating` is the main fact table storing employee evaluation data.  
It supports performance tracking, satisfaction analysis, and time-based review insights.

---

## 2) Structure

Key columns:
- `PerformanceID` (primary key)
- `EmployeeID` (foreign key to `DimEmployee`)
- `ReviewDate`
- `JobSatisfaction`
- `EnvironmentSatisfaction`
- `RelationshipSatisfaction`
- `WorkLifeBalance`
- `SelfRating`
- `ManagerRating`

Each row represents one employee review record.

---

## 3) Power Query Processing

No manual transformations were applied.

The table was:
- Loaded directly from the source dataset
- Automatically typed by Power BI
- Integrated into the model without additional cleaning

The dataset was already structured and analysis-ready.

---

## 4) Data Model Relationships

Relationships were created between:
- `FactPerformanceRating[EmployeeID]` → `DimEmployee[EmployeeID]`
- `FactPerformanceRating[ReviewDate]` → `DimDate[Date]` (depending on analysis setup)

Inactive relationships are used with:
- `DimRatingLevel`
- `DimSatisfiedLevel`

These are activated in DAX using `USERELATIONSHIP()` when required.

---

## 5) Analytical Contribution

This table enables:
- Individual employee performance tracking
- Satisfaction trend analysis
- Comparison between self and manager ratings
- Time-based review monitoring
- Calculation of review-related measures (LastReviewDate, NextReviewDate)

---

## 6) Design Note

Keeping evaluations in a dedicated fact table:
- Preserves a clean star schema
- Separates measurable events from descriptive attributes
- Supports scalable analytical modeling
