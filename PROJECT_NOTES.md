# HR Attrition Project — My Notes & Interview Prep

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

## Tools I Used and Why

| Tool | Why I used it |
|------|--------------|
| Python | Main programming language |
| Pandas | To read, clean and manipulate the data |
| SQLite | To store clean data and run SQL queries |
| Matplotlib | To create charts and visualizations |
| Jupyter Notebook | To write and run code step by step |

---

## What I Found (My Key Insights)

### 1. Overall Attrition Rate: 16.12%
The company was losing 16% of employees every year.
A healthy company should be around 10-12%.
So this company had a problem worth investigating.

### 2. Sales Department: 20.63% attrition
Sales had the highest attrition — 1 in 5 sales employees left.
This could be due to high pressure targets, low base salary,
or lack of career growth in sales roles.

### 3. Young Employees: 25.91% attrition
Employees under 30 were leaving at nearly 26%.
Almost double the rate of mid and senior employees.
Young employees likely leave for better opportunities or
feel they aren't growing fast enough.

### 4. Overtime: 30.53% vs 10.44%
This was the biggest finding. Employees doing overtime
left at 3x the rate of those who didn't.
Overwork is clearly burning people out.

### 5. Sales Representatives: 39.76% attrition
Nearly 1 in 2 Sales Reps left the company.
This is a critical problem that needs immediate HR attention.

### 6. Salary Gap: $6,832 vs $4,787
Employees who stayed earned 30% more than those who left.
Low pay is a major driver of attrition.

---

## My Recommendations (For Interview)
1. Review and increase compensation for Sales and junior roles
2. Introduce overtime limits or additional pay for overtime work
3. Build career development programs for employees under 30
4. Focus retention efforts on Sales Representatives specifically

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

---

## Key Numbers to Remember
- 1,470 employees, 35 columns
- 16.12% overall attrition rate
- Sales dept: 20.63%
- Young employees: 25.91%
- Overtime: 30.53% vs 10.44%
- Sales Reps: 39.76%
- Salary gap: $6,832 vs $4,787 (30% difference)
