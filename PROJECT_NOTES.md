# HR Attrition Project — My Notes & Interview Prep

---

## What is this project in simple words?
A company had data about 1470 employees — their age, salary, department,
whether they left the company or not. I used Python and SQL to find out
WHY employees were leaving and WHICH groups were leaving the most.

This is exactly what a Business Analyst or Data Analyst does in real jobs.

---

## What is ETL? (Very Important Interview Question)
ETL stands for Extract, Transform, Load.

- EXTRACT = Get the raw data (I loaded a CSV file using Pandas)
- TRANSFORM = Clean and improve the data (removed useless columns,
  fixed data types, created new columns)
- LOAD = Save clean data to a database (I used SQLite)

After ETL, I ran SQL queries to find business insights.

---

## STEP 1: IMPORT LIBRARIES
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import sqlite3
import warnings
warnings.filterwarnings('ignore')
```
### What is happening here?
Before doing anything in Python, we need to bring in tools called libraries.
Think of it like opening apps on your phone before using them.

- `pandas` → Used to read, clean and work with data tables (like Excel in Python)
- `numpy` → Used for mathematical calculations on numbers
- `matplotlib` → Used to create charts and graphs
- `seaborn` → Makes charts look better and more colorful
- `sqlite3` → Used to create and query a SQL database
- `warnings` → We tell Python to hide unnecessary warning messages so output stays clean

---

## STEP 2: EXTRACT — Load the Data
```python
df = pd.read_csv(r"C:\Users\Lucky\Downloads\WA_Fn-UseC_-HR-Employee-Attrition.csv")
print(df.shape)
df.head()
```
### What is happening here?
- `pd.read_csv()` → Opens the CSV file and loads it into Python as a table
- We store it in a variable called `df` which stands for DataFrame
- DataFrame is just like an Excel table — rows and columns
- `df.shape` → Tells us how many rows and columns we have (1470 rows, 35 columns)
- `df.head()` → Shows the first 5 rows so we can see what the data looks like

### Output we got:
- 1470 employees (rows)
- 35 columns (Age, Department, Salary, Attrition etc.)

---

## STEP 3: TRANSFORM — Clean and Improve Data

### 3a. Check for missing values
```python
df.isnull().sum().sum()
```
### What is happening here?
- `df.isnull()` → Checks every cell — is it empty? Returns True/False
- `.sum()` → Counts how many True (empty) cells per column
- Second `.sum()` → Adds all columns together to get total missing values
- We got 0 missing values — this dataset is very clean!

---

### 3b. Check Attrition distribution
```python
df['Attrition'].value_counts()
```
### What is happening here?
- `df['Attrition']` → Selects only the Attrition column
- `.value_counts()` → Counts how many Yes and how many No
- Result: 1233 stayed (No), 237 left (Yes)
- Attrition rate = 237/1470 * 100 = 16.12%

---

### 3c. Drop useless columns
```python
df = df.drop(columns=['EmployeeCount', 'Over18', 'StandardHours'])
```
### What is happening here?
- Some columns had the same value for ALL employees
- EmployeeCount = 1 for everyone, Over18 = Y for everyone, StandardHours = 80 for everyone
- These columns tell us nothing useful so we remove them
- `drop()` removes columns we don't need
- After dropping: 35 columns → 34 columns (wait that's wrong, let me recount)

---

### 3d. Convert Attrition to numbers
```python
df['Attrition_Num'] = df['Attrition'].map({'Yes': 1, 'No': 0})
```
### What is happening here?
- SQL and math can't work with words like "Yes" and "No"
- We convert: Yes = 1, No = 0
- `.map()` replaces each value using the dictionary we give it
- Now we can do calculations like SUM and AVG on attrition

---

### 3e. Feature Engineering — Add Age Group column
```python
def age_group(age):
    if age <= 30:
        return 'Young'
    elif age <= 45:
        return 'Mid'
    else:
        return 'Senior'

df['Age_Group'] = df['Age'].apply(age_group)
```
### What is happening here?
- Feature Engineering means creating NEW useful columns from existing ones
- We created a function called `age_group` that takes an age number
- If age is 30 or below → return "Young"
- If age is 31-45 → return "Mid"
- If age is above 45 → return "Senior"
- `df['Age'].apply(age_group)` → runs this function on every row automatically
- Result: New column Age_Group added with Young/Mid/Senior values

---

## STEP 4: LOAD — Save to SQL Database
```python
conn = sqlite3.connect('hr_attrition.db')
df.to_sql('employees', conn, if_exists='replace', index=False)
```
### What is happening here?
- `sqlite3.connect()` → Creates a new database file called hr_attrition.db on laptop
- Think of it like creating a new Excel file but it's a database
- `df.to_sql()` → Saves our entire cleaned DataFrame as a table called 'employees'
- `if_exists='replace'` → If the table already exists, replace it with new data
- `index=False` → Don't save the row numbers as a column
- Now 1470 clean employee records are stored in a real SQL database!

---

## STEP 5: SQL ANALYSIS — Find Business Insights

### Query 1: Attrition by Department
```sql
SELECT Department,
       COUNT(*) as Total_Employees,
       SUM(Attrition_Num) as Employees_Left,
       ROUND(SUM(Attrition_Num) * 100.0 / COUNT(*), 2) as Attrition_Rate
FROM employees
GROUP BY Department
ORDER BY Attrition_Rate DESC
```
### What is happening here?
- `SELECT` → Choose which columns to show
- `COUNT(*)` → Count total employees in each department
- `SUM(Attrition_Num)` → Add up all the 1s (people who left)
- `SUM/COUNT * 100` → Calculate percentage
- `GROUP BY Department` → Do this calculation separately for each department
- `ORDER BY Attrition_Rate DESC` → Show highest attrition first

### Result:
- Sales: 20.63% (highest)
- HR: 19.05%
- R&D: 13.84% (lowest)

---

### Query 2: Attrition by Age Group
Same logic as Query 1 but grouped by Age_Group instead of Department.

### Result:
- Young (<30): 25.91% — 1 in 4 young employees leaving!
- Mid (31-45): 12.70%
- Senior (45+): 12.45%

---

### Query 3: Overtime Impact
Same logic but grouped by OverTime column (Yes/No).

### Result:
- With Overtime: 30.53%
- Without Overtime: 10.44%
- 3x higher attrition for overtime employees!

---

### Query 4: By Job Role
Same logic but grouped by JobRole, showing top 5.

### Result:
- Sales Representative: 39.76% (nearly 1 in 2 leaving!)
- Lab Technician: 23.94%
- HR: 23.08%

---

### Query 5: Salary vs Attrition
```sql
SELECT Attrition,
       ROUND(AVG(MonthlyIncome), 2) as Avg_Monthly_Income
FROM employees
GROUP BY Attrition
```
### What is happening here?
- `AVG(MonthlyIncome)` → Calculate average salary
- Group by Attrition (Yes/No) to compare salaries of those who left vs stayed

### Result:
- Stayed: $6,832/month average
- Left: $4,787/month average
- People who left earned 30% less!

---

## STEP 6: VISUALIZATIONS
```python
fig, axes = plt.subplots(2, 2, figsize=(14, 10))
```
### What is happening here?
- `plt.subplots(2, 2)` → Creates a grid of 4 charts (2 rows, 2 columns)
- `figsize=(14, 10)` → Sets the size of the overall image
- Each `axes[row, col]` → Refers to one specific chart in the grid
- `axes[0,0]` = top left, `axes[0,1]` = top right
- `axes[1,0]` = bottom left, `axes[1,1]` = bottom right
- `.bar()` → Creates a bar chart
- `.boxplot()` → Creates a box plot showing salary distribution
- `plt.savefig()` → Saves the charts as a PNG image file

---

## Key Numbers to Remember
- 1,470 employees, 35 columns
- 16.12% overall attrition rate
- Sales dept: 20.63%
- Young employees: 25.91%
- Overtime: 30.53% vs 10.44%
- Sales Reps: 39.76%
- Salary gap: $6,832 vs $4,787 (30% difference)

---

## Interview Questions & My Answers

### Q: Tell me about this project.
"I built an end-to-end ETL pipeline to analyze employee attrition
at IBM. I extracted HR data for 1470 employees, cleaned and
transformed it using Python and Pandas, loaded it into a SQLite
database, and ran SQL queries to identify key attrition drivers.
I then visualized the findings using Matplotlib."

### Q: What is ETL and how did you implement it?
"ETL stands for Extract, Transform, Load. In this project I
extracted data from a CSV file using Pandas, transformed it by
removing redundant columns, converting data types, and engineering
new features like Age Group. Finally I loaded the clean data into
a SQLite database for SQL analysis."

### Q: What was your most interesting finding?
"The most striking finding was the overtime impact. Employees
working overtime had a 30.5% attrition rate compared to just
10.4% for those without overtime — nearly 3 times higher. This
strongly suggests overwork is a key driver of employee turnover."

### Q: What SQL did you use?
"I used SELECT, GROUP BY, ORDER BY, COUNT, SUM, AVG and ROUND
functions to aggregate employee data by department, age group,
overtime status and job role to calculate attrition rates for
each segment."

### Q: What would you do differently?
"I would add predictive modeling using logistic regression to
predict which employees are at risk of leaving, so HR can
intervene proactively rather than reactively."

### Q: What libraries did you use?
"I used Pandas for data manipulation, SQLite3 for database
operations, Matplotlib and Seaborn for visualizations, and
NumPy for numerical operations."

### Q: How big was the dataset?
"The dataset had 1,470 employee records with 35 columns covering
demographics, job details, compensation, and satisfaction scores."
