# 🏋️ Gym Plan Analytics
### 12-Week Hypertrophy Mesocycle — Data Engineering Portfolio Project

> **End-to-end data pipeline** built on real training data.  
> Demonstrates Python ETL, SQL analytics, SQLite, and Tableau-ready exports.

---

## 📌 Project Overview

This project transforms a 12-week hypertrophy training plan (Excel) into a fully queryable data pipeline — from raw spreadsheet to clean database to analytical insights.

Built as a **Data Engineering portfolio piece** to demonstrate:
- Data extraction and transformation from messy, multi-sheet Excel sources
- Relational database design and SQL analytics
- Progressive overload tracking using window functions
- Tableau-ready data exports for dashboarding

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Source Data | Excel (.xlsx) — 17 sheets |
| ETL | Python · Pandas · openpyxl |
| Storage | SQLite (portable, no server needed) |
| Analytics | SQL — 12 analytical queries |
| Visualisation | Matplotlib · Tableau (CSV exports) |
| Version Control | Git / GitHub |

---

## 📁 Project Structure

```
gym_plan_project/
│
├── data/
│   ├── raw/
│   │   └── Mesocycle_12_Weeks_Hypertrophy_Plan.xlsx   # Source data
│   └── processed/
│       ├── exercises.csv                # 18 exercises × 6 attributes
│       ├── weekly_volume.csv            # 216 rows — volume per exercise per week
│       ├── performance_tracker.csv      # Progress log across all 12 weeks
│       ├── volume_allocation.csv        # Target sets per muscle group (3 plans)
│       ├── gym_plan.db                  # SQLite database
│       └── tableau_*.csv               # 5 Tableau-ready exports
│
├── python/
│   ├── etl_pipeline.py                 # Extract → Transform → Load
│   └── analytics.py                    # SQL queries + charts + Tableau prep
│
├── sql/
│   └── analysis_queries.sql            # 12 analytical SQL queries
│
├── docs/
│   └── charts/                         # Auto-generated PNG charts
│
├── requirements.txt
└── README.md
```

---

## ⚙️ How It Works

### 1. ETL Pipeline (`python/etl_pipeline.py`)

Reads all 17 Excel sheets and transforms them into 5 clean, flat tables:

```
Excel (17 sheets, complex layout)
        ↓
Python (pandas + openpyxl)
        ↓  extract → transform → load
SQLite DB  +  CSV exports
```

Key transformations:
- Parses multi-row, merged-cell layouts into flat relational tables
- Handles `#DIV/0!` and `#REF!` Excel errors gracefully
- Infers training day context from sparse row headers
- Outputs both CSV (for portability) and SQLite (for SQL queries)

### 2. SQL Analytics (`sql/analysis_queries.sql`)

12 queries covering:

| # | Query | Technique |
|---|---|---|
| 1 | Workout overview by day | Basic SELECT + ORDER BY |
| 2 | Volume by muscle group (Week 1) | GROUP BY + SUM |
| 3 | Weekly volume per exercise | Aggregation over time |
| 4 | 12-week totals (avg, peak, sum) | Multi-aggregate |
| 5 | **Progressive overload check** | `LAG()` window function |
| 6 | Exercises per training day | GROUP BY + GROUP_CONCAT |
| 7 | Actual vs. target volume | LEFT JOIN + variance |
| 8 | Rest duration by muscle group | MIN / MAX / AVG |
| 9 | Unlogged exercises | Filtered SELECT |
| 10 | Top 5 highest volume exercises | LIMIT + ORDER BY |
| 11 | All volume allocation plans | Multi-plan comparison |
| 12 | Session intensity proxy | Derived metric |

### 3. Analytics & Visualisation (`python/analytics.py`)

Generates 4 charts and 5 Tableau-ready CSVs automatically:

```bash
python analytics.py
# → docs/charts/01_weekly_volume_trend.png
# → docs/charts/02_volume_by_muscle_week1.png
# → docs/charts/03_top_exercises_total_volume.png
# → docs/charts/04_sets_per_day.png
# → data/processed/tableau_*.csv  (x5)
```

---

## 🗄️ Database Schema

### `exercises` — Workout plan
| Column | Type | Description |
|---|---|---|
| day | TEXT | Day 1 / Day 2 / Day 3 |
| muscle_group | TEXT | Back / Chest / Abs / Quadriceps |
| exercise | TEXT | Exercise name |
| sets | INTEGER | Number of sets |
| reps | TEXT | Rep range e.g. `"4 - 6"` |
| rest_seconds | INTEGER | Rest between sets |

### `weekly_volume` — Per-week volume log
| Column | Type | Description |
|---|---|---|
| week | INTEGER | 1 – 12 |
| day | TEXT | Training day |
| muscle_group | TEXT | Muscle group |
| exercise | TEXT | Exercise name |
| volume | FLOAT | Total reps logged |

### `performance_tracker` — Progress over 12 weeks
| Column | Type | Description |
|---|---|---|
| day | TEXT | Training day |
| exercise | TEXT | Exercise name |
| week | INTEGER | Week number (1–12) |
| volume | FLOAT | Weekly volume |

### `volume_allocation` — Target weekly sets
| Column | Type | Description |
|---|---|---|
| plan_type | TEXT | Balanced / Upper Emphasis / Lower Emphasis |
| muscle_group | TEXT | Muscle group |
| weekly_sets_target | INTEGER | Recommended sets per week |

---

## 🚀 Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/YOUR-USERNAME/gym-plan-analytics.git
cd gym-plan-analytics

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run ETL — Excel → CSV + SQLite
cd python
python etl_pipeline.py

# 4. Run analytics — SQL + charts + Tableau CSVs
python analytics.py
```

---

## 📊 Tableau Setup

Connect Tableau Desktop to any `tableau_*.csv` in `data/processed/`:

| File | Recommended Chart |
|---|---|
| `tableau_weekly_volume.csv` | Line chart — Volume trend over 12 weeks |
| `tableau_volume_per_muscle.csv` | Horizontal bar — Volume by muscle group |
| `tableau_exercise_totals.csv` | Bar chart — Top exercises by total volume |
| `tableau_sets_per_day.csv` | Stacked bar — Sets per day per muscle |
| `tableau_allocation_vs_actual.csv` | Side-by-side bar — Actual vs. target volume |

---

## 💡 Key Insights from the Data

- **Chest and Quads** dominate Week 1 volume — Push-Up (36 reps) and Squat (30 reps) are the foundation
- **Back pull movements** (Pull-Up, Chin-Up) start at low volume — clear target for progressive overload
- Most exercises show **zero logged volume** in Weeks 2–12, making this dataset ideal for building a real tracking tool on top of
- Rest periods range **120–300 seconds**, with heavier compound back movements getting the longest rest
- The plan uses a **3-day upper/lower split** with intentional push-pull-leg sequencing

---

## 🔮 Potential Extensions

- [ ] Add a FastAPI layer to serve workout data as a REST API
- [ ] Build an Airflow DAG to automate weekly data ingestion
- [ ] Migrate SQLite to PostgreSQL for multi-user support
- [ ] Connect to Tableau Server for live dashboard
- [ ] Add dbt models for transformation layer

---

## 👤 Author

**Azrul** — Transitioning Data Engineer | Python · SQL · AWS · ETL

---

*Built as a Data Engineering portfolio project — Python · SQL · SQLite · Tableau · Git*
