# Power Query — `DimEducationLevel`


## 1) Purpose

`DimEducationLevel` is a lookup (dimension) table that translates numeric education codes into readable labels.  
It enables segmentation of employees and attrition analysis by education level.

---

## 2) Structure

Columns:
- `EducationLevelID` (primary key, numeric)
- `EducationLevel` (text label)

Example values:
- 1 → No Formal Qualifications  
- 2 → High School  
- 3 → Bachelors  
- 4 → Masters  
- 5 → Doctorate  

---

## 3) Power Query Processing

No manual transformations were applied.

The table was:
- Loaded directly from the source dataset
- Automatically typed by Power BI
- Integrated into the model without additional cleaning

The dataset was already clean and properly structured.

---

## 4) Data Model Relationship

A one-to-many relationship was created between:
- `DimEducationLevel[EducationLevelID]`
- `DimEmployee[EducationLevelID]`

This allows filtering and analysis of employees by education level.

---

## 5) Analytical Contribution

This table enables:
- Workforce distribution analysis by education
- Attrition comparison across education levels
- Clear labels in visuals instead of numeric codes

---

## 6) Design Note

Keeping education levels in a separate dimension follows standard BI modeling practices:
- Clear separation of dimensions and facts
- Improved readability
- Better scalability
