# 🗄️ SQL Fundamental Queries

> A structured journey through SQL query writing, aggregations, joins, subqueries, and set operations — across two real-world databases, with well-commented, readable solutions.

[![Language](https://img.shields.io/badge/language-SQL-blue.svg)](https://en.wikipedia.org/wiki/SQL)
[![Platform](https://img.shields.io/badge/platform-SQL%20Server-CC2927.svg)](https://www.microsoft.com/en-us/sql-server)
[![Problems](https://img.shields.io/badge/problems-70%2B-red.svg)](https://github.com/YahYa-mzn/SQL-Fundamental-Queries)
[![Databases](https://img.shields.io/badge/databases-2-green.svg)](https://github.com/YahYa-mzn/SQL-Fundamental-Queries)
[![Purpose](https://img.shields.io/badge/purpose-Learning%20%26%20Practice-orange.svg)](https://github.com/YahYa-mzn/SQL-Fundamental-Queries)

---

## 🔍 Quick Reference

| Database | File | Problems | Key Concepts |
|---|---|---|---|
| 🚗 **DBVclDetails** | `Problems_11-15.sql` to `Problems_36-40.sql` | 50 | Filtering, Aggregation, Subqueries, LIKE, NULL handling, CASE |
| 👥 **DBMisc** | `Misc.sql` | 20 | JOINs, Set Operators, Self-JOIN, EXISTS, Indexes |

**Total: 70 Problems across 2 Databases**

---

## 🎯 Project Overview

This repository contains structured SQL Server problem-solving exercises covering the fundamentals of data querying from simple `SELECT` statements to complex multi-table joins, subqueries, and set-based operations.

Problems are split across two distinct databases — one focused on vehicle data and one on a persons/employees/students schema — making each concept feel practical and contextual rather than purely academic.

## ❓ Why SQL?

SQL is the universal language of data. Whether you're a backend developer, data analyst, or system architect, the ability to query and reason about relational data is non-negotiable. Mastering it early means every database you ever work with becomes readable and manageable.

---

## 📚 Curriculum Breakdown

### 🚗 DBVclDetails — Vehicle Details Database (50 Problems)

A rich vehicle catalog database with tables for `Makes`, `Models`, `Bodies`, `DriveTypes`, `FuelTypes`, `SubModels`, and `VehicleDetails`. Problems range from basic filtering to multi-level subqueries.

---

#### 🔎 Problems 1–10 — Basic Filtering & Selection

**Master the Foundation**

```sql
-- Example: Get all vehicles with more than 3 liters engine and only 2 doors
SELECT *
FROM VehicleDetails
WHERE (Engine_Liter_Display > 3) AND (NumDoors = 2);
```

**Concepts Covered:**
- `SELECT`, `WHERE`, `ORDER BY` basics
- Filtering with comparison and logical operators (`AND`, `OR`, `NOT`)
- Working with `LIKE` for pattern matching (`B%`, `%W`)
- Handling `NULL` values with `IS NULL` / `IS NOT NULL`

---

#### 📊 Problems 11–20 — Aggregation & Grouping

**Summarize and Analyze Data**

```sql
-- Example: Count vehicles per make, filter only makes with 10,000–20,000 vehicles
SELECT
    M.MakeName,
    COUNT(*) AS NumberOfVehicles
FROM VehicleDetails VD
INNER JOIN Makes M ON VD.MakeID = M.MakeID
GROUP BY MakeName
HAVING COUNT(*) BETWEEN 10000 AND 20000
ORDER BY NumberOfVehicles DESC;
```

**Concepts Covered:**
- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- `GROUP BY` with single and multiple columns
- `HAVING` vs `WHERE` — filtering on aggregated vs raw values
- `COUNT(DISTINCT ...)` vs subquery approach
- `ORDER BY` with `ASC` / `DESC`

---

#### 🔗 Problems 21–30 — JOINs, Subqueries & CASE

**Connect Tables and Add Logic**

```sql
-- Example: Label every vehicle's door count in words, handle NULLs
SELECT
    Vehicle_Display_Name,
    NumDoors,
    CASE
        WHEN NumDoors = 2 THEN 'Two Doors'
        WHEN NumDoors = 4 THEN 'Four Doors'
        WHEN NumDoors IS NULL THEN 'Not Set'
        ELSE 'Unknown'
    END DoorsDescription
FROM VehicleDetails
ORDER BY NumDoors ASC;
```

**Concepts Covered:**
- `INNER JOIN` across multiple tables
- Scalar subqueries inside `SELECT` and `WHERE`
- `EXISTS` vs scalar subquery — efficiency considerations
- `CASE WHEN` for conditional column output
- `IN (...)` for multi-value filtering
- Percentage calculations using `CAST` to `FLOAT`

---

#### 🧠 Problems 31–40 — Advanced Subqueries & Analytics

**Go Deeper with Data**

```sql
-- Example: Get all vehicles that have one of the top 3 Engine_CC values
SELECT *
FROM VehicleDetails
WHERE Engine_CC IN (
    SELECT DISTINCT TOP 3
        Engine_CC
    FROM VehicleDetails
    ORDER BY Engine_CC DESC
);
```

**Concepts Covered:**
- Correlated vs non-correlated subqueries
- `AVG()` in subquery for threshold comparisons (above/below average)
- `SELECT DISTINCT TOP N ... ORDER BY` for ranked filtering
- Subquery inside `IN (...)` for multi-row matching
- `DISTINCT` to deduplicate result sets

---

### 👥 DBMisc — Persons / Employees / Students Database (20 Problems)

A normalized HR-style schema with `Persons`, `Employees`, `Students` tables and self-referencing manager relationships. Problems focus on real-world relational reasoning.

---

#### 🔗 Misc Problems 1–10 — Multi-Table JOINs & Set Operators

**Combine and Compare Data Sources**

```sql
-- Example: Persons who are employees but NOT students (two approaches)

-- Approach 1: LEFT JOIN + NULL check
SELECT P.PersonID, E.EmployeeID, P.FirstName + ' ' + P.LastName AS FullName
FROM Persons P
INNER JOIN Employees E ON P.PersonID = E.PersonID
LEFT OUTER JOIN Students S ON P.PersonID = S.PersonID
WHERE S.StudentID IS NULL;

-- Approach 2: NOT IN with subquery
SELECT P.PersonID, E.EmployeeID
FROM Persons P
INNER JOIN Employees E ON P.PersonID = E.PersonID
WHERE P.PersonID NOT IN (SELECT PersonID FROM Students);
```

**Concepts Covered:**
- `INNER JOIN` — matching rows only
- `LEFT OUTER JOIN` — all left rows, NULL when no match
- `UNION` — combining result sets, removing duplicates
- `INTERSECT` — rows that appear in both results
- `EXCEPT` — rows in first result but not second
- Self-referencing joins (employees and their managers)

---

#### ⚙️ Misc Problems 11–20 — Advanced Logic & Optimization

**Write Smarter Queries**

```sql
-- Example: Classify every person as Student, Employee, Both, or Neither
SELECT
    P.*,
    CASE
        WHEN S.StudentID IS NOT NULL AND E.EmployeeID IS NOT NULL THEN 'Student & Employee'
        WHEN S.StudentID IS NOT NULL THEN 'Student'
        WHEN E.EmployeeID IS NOT NULL THEN 'Employee'
        ELSE 'Normal Person'
    END AS Status
FROM Persons P
LEFT JOIN Students S ON P.PersonID = S.PersonID
LEFT JOIN Employees E ON P.PersonID = E.PersonID;
```

**Concepts Covered:**
- `EXISTS` / `NOT EXISTS` for existence checks (efficient, stops at first match)
- Correlated subqueries in `SELECT` columns
- Self-JOIN for manager/employee hierarchy
- `YEAR()`, `MONTH()` for date extraction and grouping
- `CASE WHEN` with multi-condition logic
- `NONCLUSTERED INDEX` creation for query optimization
- `ANY` / `MAX` pattern for cross-gender salary comparisons

---

## 📁 File Structure

```
SQL-Fundamental-Queries/
│
├── 🚗 DBVclDetails/
│   ├── Problems_1-10.sql
│   ├── Problems_11-15.sql
│   ├── Problems_16-20.sql
│   ├── Problems_21-25.sql
│   ├── Problems_26-30.sql
│   ├── Problems_31-35.sql
│   └── Problems_36-40.sql
│
├── 👥 DBMisc/
│   └── Misc.sql             ← 20 problems (Problems 1–20)
│
└── README.md
```

---

## 🚀 Quick Start

### Prerequisites

```
✅ SQL Server (any recent version)
✅ SQL Server Management Studio (SSMS) or Azure Data Studio
✅ The DBVclDetails and DBMisc databases restored/available
✅ Basic understanding of relational databases
```

### Get Started

```sql
-- 1. Open any .sql file in SSMS or Azure Data Studio

-- 2. Every file starts by selecting the correct database
USE DBVclDetails;  -- or USE DBMisc;

-- 3. Run individual problem blocks (each is clearly delimited)

-- 4. Read the comment below each query — it explains the why, not just the what
```

### 📖 How to Use These Files

1. **Open a file** matching the problem range you want to study
2. **Read the problem header** — each block has a clear title describing the goal
3. **Run the query** and observe the result set
4. **Read the inline comment** below the query explaining the approach
5. **Compare alternatives** — many problems include a second solution with a different technique
6. **Experiment** — modify filters, change columns, break and fix things

> 💡 **Tip**: Problems with multiple solutions are intentional. Understanding *why* one approach is better than another (e.g. `HAVING` vs subquery with `WHERE`, or `EXISTS` vs scalar subquery) is where real SQL mastery comes from.

---

## 🏆 Skills You'll Build

### 🔧 Query Writing
- ✅ Clean, readable SQL with consistent formatting
- ✅ Meaningful column aliases and result labeling
- ✅ Choosing `INNER` vs `LEFT` JOIN based on intent
- ✅ Filtering at the right stage (`WHERE` vs `HAVING`)

### 🧠 Analytical Thinking
- ✅ Breaking complex questions into composable subqueries
- ✅ Comparing aggregated results to thresholds (above/below average)
- ✅ Classifying rows with multi-condition `CASE` logic
- ✅ Reasoning about NULL — when it appears and how to handle it

### ⚡ Performance Awareness
- ✅ Knowing when `EXISTS` is faster than `IN` with subqueries
- ✅ Understanding when `COUNT(DISTINCT)` beats a wrapped subquery
- ✅ Recognizing which columns benefit from `NONCLUSTERED INDEX`
- ✅ Using `TOP N` with `ORDER BY` for efficient ranked filtering

---

## 🔮 Roadmap

### ✅ Currently Available
- ✅ **50 DBVclDetails problems** — filtering, aggregation, joins, subqueries, CASE
- ✅ **20 DBMisc problems** — multi-table joins, set operators, hierarchy queries, indexing
- ✅ **Dual-solution problems** — alternative approaches with tradeoff explanations
- ✅ **Inline comments** — every query explained below the code

### 🚀 Potentially Coming
- 🔄 **Views** — reusable virtual tables for common query patterns
- 🔄 **Stored Procedures** — parameterized, reusable query logic
- 🔄 **Triggers** — automated responses to data changes
- 🔄 **Window Functions** — `ROW_NUMBER`, `RANK`, `PARTITION BY`
- 🔄 **Transactions & Error Handling** — `TRY/CATCH`, `ROLLBACK`

---

## 👨‍💻 About

**YahYa-mzn** — *Building solid fundamentals, one query at a time.*

🎯 **Goal**: Develop strong SQL intuition through practice on real, structured schemas — not toy examples.

📚 **Sources**:
- **DBVclDetails problems** (1–50): Curated from [ProgrammingAdvices](https://programmingadvices.com/) — a structured programming education resource
- **DBMisc problems** (Misc 1–20): Self-made, AI-assisted, and adapted from various SQL learning videos

🔗 **Connect**: [@YahYa-mzn](https://github.com/YahYa-mzn)

---

⭐ **Found this useful? Star the repo and keep querying.**

*Clean queries. Clear thinking. Strong foundations.* 🗄️
