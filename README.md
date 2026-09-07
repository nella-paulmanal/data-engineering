# Becoming a Data Engineer

Hi, I'm Nella, I'm currently working as a chemist.

In July 2026, I started my journey toward becoming a data engineer through the FTW Foundation Data Engineering program. This repository documents what I learn along the way through FTW sessions, DataCamp courses, hands-on exercises, and projects.

I'm building this repository as I learn, so it includes notes, experiments, errors, fixes, decisions, and reflections.

---

## Learning Journey

My current learning is mainly built around two things:

**FTW Foundation**
Weekly Saturday sessions covering data engineering concepts, SQL, Databricks, data modeling, data quality, pipelines, and related topics.

**DataCamp**
Self-paced courses used to reinforce concepts and practice technical skills between sessions.

```text
FTW Sessions
     ↓
Learn concepts
     ↓
DataCamp
     ↓
Practice
     ↓
Build projects
     ↓
Reflect & document
     ↓
Repeat
```

---
The repository will evolve as I learn more.

---

# Current Project

## Data Engineering Pipeline

My current hands-on projects involve data pipelines built using Databricks and SQL.

The goal is to take raw CSV files, ingest them into Databricks, clean and validate the data, transform the datasets, and prepare them for reporting and dashboard use.

### Pipeline

```text
CSV Files
    ↓
Databricks Volume
    ↓
Bronze (Raw)
    ↓
Silver (Cleaned)
    ↓
Gold (Mart)
    ↓
Dashboard/Insights
```

---

### Layer Responsibilities

| Layer           | Purpose                                               |
| --------------- | ----------------------------------------------------- |
| Source / Volume | Stores the original raw CSV files                     |
| Bronze          | Ingests source data with minimal transformation       |
| Silver          | Cleans, validates, and standardizes the data          |
| Gold            | Prepares data for analysis, reporting, and dashboards |

---

# What I'm Learning

The projects are helping me practice:

* SQL
* Databricks
* Unity Catalog
* Volumes
* Temporary views
* Window functions
* Data cleaning
* Data validation
* Data quality checks
* Data modeling
* Medallion architecture
* ETL / ELT
* Job scheduling
* Git / GitHub
* Dashboard preparation

---

# Tools

```text
Databricks
SQL
Python
GitHub
Google Sheets
DataCamp
```

---

# Progress

This repository is a work in progress.

I'm documenting the process as I learn rather than only documenting the final solution. Some notebooks and notes may therefore contain failed approaches, debugging, questions, and decisions that were later changed or resolved.

The progression I'm aiming for is:

```text
Learn individual concepts
        ↓
Practice
        ↓
Understand data quality
        ↓
Write SQL
        ↓
Transform data
        ↓
Understand data modeling
        ↓
Build pipelines
        ↓
Make pipelines repeatable
        ↓
Automate and orchestrate
        ↓
Build end-to-end data engineering projects
```

The goal is not simply to collect tools or certifications, but to understand how the pieces fit together to build reliable and reusable data systems.
