# 🗄️ SQL Learning Roadmap

> **Who this is for:** Someone who wants to understand how data is stored and queried in the real world. SQL is arguably the highest ROI skill on this list — it's everywhere, it's learnable in weeks, and it will come up in almost every data-related context you'll encounter in medicine, research, or software.

---

## What SQL Actually Is

SQL (Structured Query Language) is how you talk to relational databases. A relational database is data stored in tables with rows and columns — like a spreadsheet, but with relationships between tables and a way to query across them.

You don't *write programs* in SQL the way you do in Python or Rust. You write *queries* — questions you ask of your data. It's more declarative: you say *what* you want, not *how* to get it.

```sql
-- "Give me all patients over 65 from California, sorted by last name"
SELECT name, age, state
FROM patients
WHERE age > 65 AND state = 'California'
ORDER BY name;
```

---

## Setup — Pick a Database to Practice On

You need a database engine to run queries. Pick one:

- **SQLite** — Zero setup. One file. Great for learning. Download [DB Browser for SQLite](https://sqlitebrowser.org/) for a visual interface.
- **PostgreSQL** — The professional choice. More features. Install via [postgresql.org](https://www.postgresql.org/download/) and use [pgAdmin](https://www.pgadmin.org/) as your GUI.
- **MySQL** — Also widely used. Install via [dev.mysql.com](https://dev.mysql.com/downloads/).

> **Recommendation for beginners:** Start with SQLite + DB Browser. No setup friction, and the SQL you write will transfer to any other database with minor changes.

---

## Phase 1 — Core Query Syntax (Week 1–2)

**Goal:** Write the queries that cover 80% of real-world use cases.

### 📖 Official / Core Docs
- [SQLite Documentation](https://www.sqlite.org/docs.html) — if using SQLite
- [PostgreSQL Documentation](https://www.postgresql.org/docs/) — if using Postgres
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/) — Not official, but excellent for quick lookup of any SQL statement. Keep this tab open.
- [SQLZoo](https://sqlzoo.net/) — Interactive SQL exercises in the browser. No setup required.

### 🎥 YouTube
- **Programming with Mosh** — [SQL Tutorial for Beginners (3 hours)](https://www.youtube.com/watch?v=7S_tz1z_5bA) — Best single video for getting started. Structured, clear, covers all the essentials.
- **freeCodeCamp** — [SQL Full Course (4 hours)](https://www.youtube.com/watch?v=HXV3zeQKqGY) — More comprehensive, use as a follow-up reference.

### Topics to Cover

**The basics:**
```sql
SELECT column1, column2 FROM table_name;   -- pick columns
SELECT * FROM patients;                     -- all columns
SELECT DISTINCT state FROM patients;        -- unique values only
```

**Filtering:**
```sql
WHERE age > 30
WHERE name LIKE 'John%'       -- starts with John
WHERE state IN ('CA', 'NY')
WHERE diagnosis IS NOT NULL
WHERE age BETWEEN 18 AND 65
```

**Sorting and limiting:**
```sql
ORDER BY age DESC
LIMIT 10
```

**Aggregations:**
```sql
SELECT COUNT(*) FROM patients;
SELECT AVG(age), MAX(age), MIN(age) FROM patients;
SELECT state, COUNT(*) as patient_count
FROM patients
GROUP BY state
HAVING COUNT(*) > 100;   -- HAVING filters on aggregated results (WHERE can't)
```

---

## Phase 2 — Joins (Week 3)

**Goal:** Learn to query across multiple tables — this is where SQL gets powerful.

Joins are how you combine data from two tables based on a shared column (a key).

```
patients table          diagnoses table
--------------          ---------------
patient_id              patient_id
name                    diagnosis_code
age                     diagnosis_date
```

### 📖 Official / Core Docs
- [Visual JOIN reference (highly recommended)](https://www.sql-join.com/sql-join-types)
- [PostgreSQL Joins docs](https://www.postgresql.org/docs/current/queries-table-expressions.html)

### 🎥 YouTube
- **Programming with Mosh** — includes JOINs in his SQL tutorial above
- **Database Star (Ben Brumm)** — [SQL Joins Explained](https://www.youtube.com/@DatabaseStar) — The clearest JOIN explanations on YouTube

### Types of Joins

```sql
-- INNER JOIN: only rows that match in both tables
SELECT p.name, d.diagnosis_code
FROM patients p
INNER JOIN diagnoses d ON p.patient_id = d.patient_id;

-- LEFT JOIN: all rows from left table, matched rows from right (or NULL)
SELECT p.name, d.diagnosis_code
FROM patients p
LEFT JOIN diagnoses d ON p.patient_id = d.patient_id;

-- You can join more than two tables
SELECT p.name, d.diagnosis_code, dr.name AS doctor_name
FROM patients p
JOIN diagnoses d ON p.patient_id = d.patient_id
JOIN doctors dr ON d.doctor_id = dr.doctor_id;
```

---

## Phase 3 — Schema Design and Data Modeling (Week 4)

**Goal:** Learn how to *design* a database, not just query one. This is how you'll create your own projects.

### 📖 Official / Core Docs
- [Database Normalization Explained](https://www.guru99.com/database-normalization.html)
- [Primary Keys and Foreign Keys](https://www.postgresql.org/docs/current/ddl-constraints.html)

### 🎥 YouTube
- **Database Star** — [Database Design Course](https://www.youtube.com/@DatabaseStar) — Ben Brumm's channel is excellent for understanding database design fundamentals
- **freeCodeCamp** — [Database Design Course (8 hours)](https://www.youtube.com/watch?v=ztHopE5Wnpc)

### Topics to Cover

**Creating tables:**
```sql
CREATE TABLE patients (
    patient_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name       TEXT NOT NULL,
    age        INTEGER CHECK(age >= 0 AND age <= 150),
    email      TEXT UNIQUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE diagnoses (
    diagnosis_id   INTEGER PRIMARY KEY AUTOINCREMENT,
    patient_id     INTEGER NOT NULL,
    diagnosis_code TEXT NOT NULL,
    diagnosed_on   DATE,
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id)
);
```

**Modifying data:**
```sql
INSERT INTO patients (name, age) VALUES ('Jane Doe', 34);
UPDATE patients SET age = 35 WHERE patient_id = 1;
DELETE FROM patients WHERE patient_id = 1;
```

**Normalization** — the principle that each piece of data should be stored in one place only. If a patient's name appears in three tables, that's a design problem. This is what foreign keys solve.

---

## Phase 4 — Advanced Queries (Week 5–6)

**Goal:** Learn the queries that separate beginners from competent SQL users.

### 📖 Official Docs
- [PostgreSQL Window Functions](https://www.postgresql.org/docs/current/tutorial-window.html)
- [Common Table Expressions (CTEs)](https://www.postgresql.org/docs/current/queries-with.html)

### 🎥 YouTube
- **Alex the Analyst** — [Advanced SQL Tutorials](https://www.youtube.com/@AlexTheAnalyst) — Alex focuses on practical, interview-ready SQL. His window function tutorials are particularly good.
- **TechTFQ** — [SQL Window Functions](https://www.youtube.com/@techTFQ) — Best dedicated window function tutorial channel

### Topics to Cover

**Subqueries:**
```sql
-- Find patients older than the average age
SELECT name, age
FROM patients
WHERE age > (SELECT AVG(age) FROM patients);
```

**CTEs (Common Table Expressions)** — named subqueries that make complex queries readable:
```sql
WITH elderly_patients AS (
    SELECT * FROM patients WHERE age > 65
),
their_diagnoses AS (
    SELECT p.name, d.diagnosis_code
    FROM elderly_patients p
    JOIN diagnoses d ON p.patient_id = d.patient_id
)
SELECT * FROM their_diagnoses;
```

**Window Functions** — the most powerful feature for analytics:
```sql
-- Rank patients by age within each state
SELECT
    name,
    age,
    state,
    RANK() OVER (PARTITION BY state ORDER BY age DESC) AS age_rank
FROM patients;

-- Running total of diagnoses per day
SELECT
    diagnosed_on,
    COUNT(*) AS daily_diagnoses,
    SUM(COUNT(*)) OVER (ORDER BY diagnosed_on) AS running_total
FROM diagnoses
GROUP BY diagnosed_on;
```

---

## 🏗️ Capstone Project: Hospital Database System

**What you'll build:** A complete relational database for a fictional hospital, with a schema you design yourself and a set of analytical queries to answer real questions.

**Step 1 — Design the schema.** Create tables for at minimum:
- `patients` (demographics, contact info)
- `doctors` (name, specialty, department)
- `appointments` (patient, doctor, date, status)
- `diagnoses` (patient, ICD code, date, severity)
- `medications` (name, dosage class)
- `prescriptions` (patient, medication, prescribing doctor, start/end date)

**Step 2 — Populate with sample data.** Write `INSERT` statements or find a script to generate fake data. Aim for at least 50 patients, 10 doctors, 200 appointments.

**Step 3 — Write these analytical queries:**

```sql
-- 1. Most common diagnoses overall
-- 2. Busiest doctors (by appointment count) this month
-- 3. Patients with more than 3 diagnoses (frequent visitors)
-- 4. Average time between first appointment and first diagnosis
-- 5. Doctors who have never prescribed medication
-- 6. Top 5 most prescribed medications per department (window function)
-- 7. Month-over-month change in new patient registrations
-- 8. Patients on 3+ active medications simultaneously (complex JOIN)
```

**Repo structure:**
```
sql-hospital-db/
├── README.md
├── schema.sql          # CREATE TABLE statements
├── seed_data.sql       # INSERT statements with sample data
├── queries/
│   ├── 01_common_diagnoses.sql
│   ├── 02_busiest_doctors.sql
│   ├── 03_frequent_patients.sql
│   └── ...
└── notes.md            # Your notes on design decisions
```

---

## Practice Resources

| Resource | Use It For |
|---|---|
| [SQLZoo](https://sqlzoo.net/) | Interactive browser-based practice, no setup |
| [HackerRank SQL](https://www.hackerrank.com/domains/sql) | Graded challenges, good for measuring progress |
| [LeetCode SQL](https://leetcode.com/problemset/database/) | Hard interview-style problems for when you're ready |
| [Mode SQL Tutorial](https://mode.com/sql-tutorial/) | Strong intermediate content with real datasets |
| [Database Star YouTube](https://www.youtube.com/@DatabaseStar) | Any time you're confused about a concept |

---

*Next step after this roadmap: Learn PostgreSQL-specific features (window functions, JSON support, full-text search), or connect SQL to Python using `pandas` and `SQLAlchemy` for data science workflows.*
