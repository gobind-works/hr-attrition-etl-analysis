# 📊 HR Attrition ETL Pipeline & Analysis

## 📌 Overview
End-to-end ETL pipeline built to analyze employee attrition patterns 
using IBM HR Analytics dataset. The project extracts raw HR data, 
transforms and cleans it using Python, loads it into a SQL database, 
and performs business analysis to identify key attrition drivers.

## 🔄 ETL Pipeline Stages

### Extract
- Loaded IBM HR Analytics dataset (1,470 employee records, 35 columns)
- Source: CSV file processed using Pandas

### Transform
- Checked and confirmed zero missing values
- Dropped redundant columns (EmployeeCount, Over18, StandardHours)
- Converted Attrition column to numeric (Yes=1, No=0)
- Engineered new feature: Age Group (Young/Mid/Senior)

### Load
- Saved clean data into SQLite database
- Created `employees` table with 1,470 records ready for SQL querying

## 📊 Key Findings

| Finding | Insight |
|---------|---------|
| Overall Attrition | 16.12% — above healthy 10-12% benchmark |
| Highest Dept | Sales at 20.63% |
| Highest Age Group | Young employees at 25.91% |
| Overtime Impact | 30.53% vs 10.44% — 3x higher risk |
| Highest Role | Sales Representatives at 39.76% |
| Salary Gap | Employees who left earned 30% less |

## 🛠️ Tech Stack
Python | Pandas | NumPy | SQLite | Matplotlib | Seaborn

## 📁 Files
- `HR_Attrition_ETL_Analysis.ipynb` — Main notebook
- `hr_attrition_insights.png` — Visualization charts
- `PROJECT_NOTES.md` — Project explanation and interview prep

## 👤 Author
Gobind Kumar | gobind1721@gmail.com
