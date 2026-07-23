# 🏥 Hospital ER Operations Overview - Power BI Project

An end-to-end interactive Power BI dashboard designed to monitor, analyze, and optimize Hospital Emergency Room (ER) performance. This project provides actionable insights into patient admittance trends, wait times, satisfaction scores, and departmental workload distribution.


---

## 💡 Key Features & Business Insights

* **KPI Performance Tracking:** Real-time tracking of `# Patients`, `Avg Satisfaction Score`, and `Avg Wait Time (m)` with MoM (Month-over-Month) growth indicators.
* **Patient Demographics Analysis:** Breaks down visits by Gender, Age Groups (Adult, Old, Children), and Race/Ethnicity.
* **Departmental Workload:** Identifies top referral departments (e.g., General Practice, Orthopedics) to highlight operational bottlenecks.
* **Wait Time vs. Satisfaction Analysis:** Visualizes the impact of waiting times on overall patient satisfaction scores across departments.
* **Dynamic Time Intelligence:** Interactive custom date slicers for Year and Month filtering.

---

## 🏗️ Data Architecture & Modeling

The project follows a **Star Schema** dimensional model to ensure high performance, scalability, and easy DAX formulation.

![Star Schema Data Model](1.png)

### Data Model Structure:
* **`Hospital ER`** *(Fact Table)*: Contains ER visit records, admittance flags, satisfaction scores, and wait times.
* **`Dim patient`** *(Dimension Table)*: Patient demographic details (Age, Gender, Name).
* **`Dim Department`** *(Dimension Table)*: Referral department lookup.
* **`Dim Race`** *(Dimension Table)*: Race and demographic classifications.
* **`Dim date`** *(Dimension Table)*: Comprehensive calendar created using DAX for Time Intelligence functions.

---

## 🛠️ Data Cleaning & ETL (Power Query)

Data transformation was executed in **Power Query Editor**:
* Cleaned missing values and filtered out null/invalid records (`Filtered Rows`).
* Normalised lookup tables (`Dim Race`, `Dim Department`, `Dim Patient`) and generated surrogate keys (`Index Columns`).
* Applied proper data typing, merged queries, and removed redundant columns for optimal memory storage.

---

## 📐 DAX Measures & Time Intelligence

Advanced DAX measures were written to calculate dynamic growth and dynamic formatting:

```dax
// Example: Average Satisfaction Previous Month
Avg Satis_Score PM = 
CALCULATE(
    [Avg Satis_Score], 
    PREVIOUSMONTH('Dim date'[Date])
)

// Example: Month-over-Month Satisfaction Growth
Avg Satis_Score Growth = 
DIVIDE(
    [Avg Satis_Score] - [Avg Satis_Score PM], 
    [Avg Satis_Score PM]
)

// Dynamic Color Indicators for Formatting
Avg Satis_Score Color = 
IF([Avg Satis_Score Growth] > 0, "Green", "Red")
