# 📊 HR Analytics – Employee Attrition Dashboard (Power BI)

## 📌 Project Overview
This project focuses on analyzing **employee attrition (turnover)** using **Power BI**.  
The main objective is to help **HR teams and managers** understand why employees leave, identify at-risk populations, and support **data-driven retention strategies**.

The dashboard combines **demographic analysis**, **performance and satisfaction tracking**, and **attrition analysis** into a single interactive BI solution.

> This project was developed as part of a **DataCamp learning program**, with an emphasis on applying **real-world Business Intelligence best practices**.

---

## 🎯 Business Objectives
- Measure the **overall attrition rate**
- Monitor attrition **over time**
- Identify departments and job roles with **high turnover**
- Analyze the impact of:
  - employee tenure
  - overtime requirements
  - business travel frequency
  - salary
  - satisfaction and performance ratings
- Provide **actionable insights** to support HR decision-making

---

## 🛠️ Tools & Technologies
- **Power BI Desktop**
- **DAX** (CALCULATE, FILTER, DISTINCTCOUNT, USERELATIONSHIP, VAR)
- **Power Query**
- **Data Modeling (Star Schema)**
- HR Analytics & Data Visualization

---

## 🗂️ Dataset Description
The dataset represents a fictional company called **Atlas Labs** and includes:
- Employee demographic data (age, gender, department, job role)
- Employment details (hire date, salary, attrition status)
- Performance reviews and satisfaction scores
- Education levels
- Rating and satisfaction scales
- Calendar table for time analysis

📌 *The dataset is provided for educational purposes via DataCamp.*

---

## 🧱 Data Model
The project uses a **Star Schema** optimized for analytical performance and DAX readability.

### Tables used:
- **DimEmployee** – employee attributes (central dimension)
- **FactPerformanceRating** – performance and satisfaction reviews
- **DimDate** – calendar table
- **DimEducationLevel**
- **DimRatingLevel**
- **DimSatisfiedLevel**
- **_Measures** – dedicated table for DAX measures (BI best practice)

### Modeling choices:
- One-to-many relationships between dimensions and fact table
- Use of **inactive relationships**, activated dynamically using `USERELATIONSHIP`
- Clear separation between:
  - data preparation (Power Query)
  - data model
  - business logic (DAX)

📁 Data model diagram available in:  
`/data_model/data_model.png`

---

## 🧼 Data Preparation (Power Query)
Key transformations performed in Power Query:
- Promotion of headers
- Standardization of column data types
- Creation of derived columns:
  - **AgeBins** (`<20`, `20–29`, `30–39`, `40–49`, `50+`)
- Normalization of lookup tables (education, rating, satisfaction levels)
- No business logic hard-coded in Power Query (handled in DAX instead)

📁 Detailed transformations are documented in:  
`/power_query/`

---

## 📐 DAX Measures
All KPIs and calculations are centralized in a dedicated **_Measures** table.

### Examples of key measures:
```DAX
TotalEmployees =
DISTINCTCOUNT(DimEmployee[EmployeeID])```

```DAX
InactiveEmployees =
CALCULATE(
    [TotalEmployees],
    FILTER(DimEmployee, DimEmployee[Attrition] = "Yes")
)```

```DAX
% Attrition Rate =
DIVIDE([InactiveEmployees], [TotalEmployees])
```

```DAX
TotalEmployeesDate =
CALCULATE(
    [TotalEmployees],
    USERELATIONSHIP(DimEmployee[HireDate], DimDate[Date])
)
```

```DAX
NextReviewDate =
VAR reviewOrHire =
IF(
    MAX(FactPerformanceRating[ReviewDate]) = BLANK(),
    MAX(DimEmployee[HireDate]),
    MAX(FactPerformanceRating[ReviewDate])
)
RETURN reviewOrHire + 365
```

📌 Full list and explanations available in:  
`/dax_measures/measures.md`

## 📄 Dashboard Pages

### 1️⃣ Overview
Provides a high-level view of the workforce:
- Total, active, and inactive employees  
- Overall attrition rate  
- Hiring and attrition trends over time  
- Active employees by department and job role  

📁 **Screenshots:** `/screenshots/overview/`

---

### 2️⃣ Demographics
Focuses on employee population characteristics:
- Age distribution and age groups  
- Gender distribution by age  
- Marital status  
- Ethnicity vs average salary  
- Youngest and oldest employees  

📁 **Screenshots:** `/screenshots/demographics/`

---

### 3️⃣ Performance Tracker
Allows individual employee monitoring:
- Job satisfaction  
- Relationship satisfaction  
- Environment satisfaction  
- Work-life balance  
- Self rating  
- Manager rating  

**HR key dates:**
- Hire date  
- Last review date  
- Next review date (calculated)  

📁 **Screenshots:** `/screenshots/performance_tracker/`

---

### 4️⃣ Attrition Analysis
Dedicated to understanding employee turnover:
- Attrition by department and job role  
- Attrition by tenure  
- Attrition by overtime requirement  
- Attrition by business travel frequency  
- Attrition trends over time  

📁 **Screenshots:** `/screenshots/attrition/`

---

## 🔍 Key Business Insights
- Attrition is significantly higher among employees with **low tenure**  
- Employees required to work **overtime** show higher attrition rates  
- **Frequent business travel** is correlated with increased turnover  
- Certain departments and job roles are more exposed to attrition risk  
- Lower satisfaction and performance ratings often precede employee exits  

📁 **Detailed insights available in:**  
`/insights/business_insights.md`

---

## 🚀 How to Use the Project
1. Download the `.pbix` file from the `dashboard` folder  
2. Open it using **Power BI Desktop**  
3. Navigate through the report pages using slicers and filters  

---

## ✅ What This Project Demonstrates
- End-to-end Business Intelligence workflow  
- Solid data modeling fundamentals  
- Clean, maintainable, and scalable DAX  
- Business-oriented data storytelling  
- Practical HR analytics use cases  

---

## 📁 Repository Structure
```text
HR-Attrition-PowerBI/
├── README.md
├── dashboard/
│   └── HR_Attrition_Dashboard.pbix
├── screenshots/
├── data_model/
├── power_query/
├── dax_measures/
└── insights/



