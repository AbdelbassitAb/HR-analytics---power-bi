#  DAX Measures Documentation

This document lists **all DAX measures** used in the *HR Analytics – Employee Attrition Dashboard* (Atlas Labs).  
Each measure includes a short **business meaning** and, when useful, a **technical note**.

---

##  1-Employee Count Measures

### TotalEmployees
**Business:** Total number of unique employees.
```DAX
TotalEmployees =
DISTINCTCOUNT(DimEmployee[EmployeeID])
```

### ActiveEmployees
**Business:** Employees currently active (Attrition = "No").
```DAX
ActiveEmployees =
CALCULATE(
    [TotalEmployees],
    FILTER(DimEmployee, DimEmployee[Attrition] = "No")
)
```

### InactiveEmployees
**Business:** Employees who left the company (Attrition = "Yes").
```DAX
InactiveEmployees =
CALCULATE(
    [TotalEmployees],
    FILTER(DimEmployee, DimEmployee[Attrition] = "Yes")
)
```

---

##  2-Attrition Measures

### % Attrition Rate
**Business:** Share of employees who left (inactive / total).
```DAX
% Attrition Rate =
DIVIDE([InactiveEmployees], [TotalEmployees])
```

### TotalEmployeesDate
**Business:** Total employees under a date context (based on HireDate).
**Technical:** Activates an inactive relationship with the Date table.
```DAX
TotalEmployeesDate =
CALCULATE(
    [TotalEmployees],
    USERELATIONSHIP(DimEmployee[HireDate], DimDate[Date])
)
```

### InactiveEmployeesDate
**Business:** Inactive employees under a date context (based on HireDate).
```DAX
InactiveEmployeesDate =
CALCULATE(
    [InactiveEmployees],
    USERELATIONSHIP(DimEmployee[HireDate], DimDate[Date])
)
```

### % Attrition Rate Date
**Business:** Attrition rate over time (inactive / total in the selected period).
```DAX
% Attrition Rate Date =
DIVIDE([InactiveEmployeesDate], [TotalEmployeesDate])
```

---

##  3-Compensation

### AverageSalary
**Business:** Average salary in the current filter context.
```DAX
AverageSalary =
AVERAGE(DimEmployee[Salary])
```

---

##  4-Satisfaction Measures

### JobSatisfaction
**Business:** Job satisfaction score (from performance rating fact).
```DAX
JobSatisfaction =
MAX(FactPerformanceRating[JobSatisfaction])
```

### EnvironmentSatisfaction
**Business:** Satisfaction with the work environment.
**Technical:** Uses `USERELATIONSHIP` to map the score to `DimSatisfiedLevel`.
```DAX
EnvironmentSatisfaction =
CALCULATE(
    MAX(FactPerformanceRating[EnvironmentSatisfaction]),
    USERELATIONSHIP(
        FactPerformanceRating[EnvironmentSatisfaction],
        DimSatisfiedLevel[SatisfactionID]
    )
)
```

### RelationshipSatisfaction
**Business:** Satisfaction with workplace relationships.
```DAX
RelationshipSatisfaction =
CALCULATE(
    MAX(FactPerformanceRating[RelationshipSatisfaction]),
    USERELATIONSHIP(
        FactPerformanceRating[RelationshipSatisfaction],
        DimSatisfiedLevel[SatisfactionID]
    )
)
```

### WorkLifeBalance
**Business:** Work-life balance score.
```DAX
WorkLifeBalance =
CALCULATE(
    MAX(FactPerformanceRating[WorkLifeBalance]),
    USERELATIONSHIP(
        FactPerformanceRating[WorkLifeBalance],
        DimSatisfiedLevel[SatisfactionID]
    )
)
```

---

##  5-Performance Rating Measures

### ManagerRating
**Business:** Manager evaluation score.
**Technical:** Uses `USERELATIONSHIP` to map the score to `DimRatingLevel`.
```DAX
ManagerRating =
CALCULATE(
    MAX(FactPerformanceRating[ManagerRating]),
    USERELATIONSHIP(
        FactPerformanceRating[ManagerRating],
        DimRatingLevel[RatingID]
    )
)
```

### SelfRating
**Business:** Employee self-evaluation score.
```DAX
SelfRating =
CALCULATE(
    MAX(FactPerformanceRating[SelfRating]),
    USERELATIONSHIP(
        FactPerformanceRating[SelfRating],
        DimRatingLevel[RatingID]
    )
)
```

---

##  6-HR Review Dates

### LastReviewDate
**Business:** Last review date, or a message if no review exists.
```DAX
LastReviewDate =
IF(
    MAX(FactPerformanceRating[ReviewDate]) = BLANK(),
    "No Review Yet",
    MAX(FactPerformanceRating[ReviewDate])
)
```

### NextReviewDate
**Business:** Next review date (1 year after last review, otherwise 1 year after hire date).
```DAX
NextReviewDate =
VAR reviewOrHire =
    IF(
        MAX(FactPerformanceRating[ReviewDate]) = BLANK(),
        MAX(DimEmployee[HireDate]),
        MAX(FactPerformanceRating[ReviewDate])
    )
RETURN
    reviewOrHire + 365
```

---

## ✅ Best Practices Used
- Centralized measures in a dedicated `_Measures` table
- Used `DIVIDE()` instead of `/` to avoid division-by-zero errors
- Used `USERELATIONSHIP()` for advanced time logic and mapping scores to dimension tables
- Kept business logic in DAX (not hard-coded in Power Query)
