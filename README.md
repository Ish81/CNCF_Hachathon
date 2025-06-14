## 🚀 Project Overview: **ETL Pipeline using Apache Airflow, NASA API, and Postgres**

This project showcases a modern, containerized ETL (Extract, Transform, Load) pipeline powered by **Apache Airflow** for orchestration. It pulls data daily from **NASA’s Astronomy Picture of the Day (APOD) API**, processes it, and stores it in a **PostgreSQL database**—all within a Dockerized environment for seamless reproducibility and deployment.

The primary goal is to build a reliable data pipeline that can be scaled or adapted for other APIs or data sources in the future.

---

## 🔧 Key Components

### 🛠️ Apache Airflow – Workflow Orchestration

* Defines and schedules tasks using a **DAG (Directed Acyclic Graph)**.
* Uses `SimpleHttpOperator`, `PostgresOperator`, and TaskFlow’s `@task` for modular and readable pipelines.
* Handles dependencies, retries, and logging out of the box.

### 🛰️ NASA APOD API – Data Source

* Public API that returns daily astronomy media (images or videos) with rich metadata.
* JSON response includes: `title`, `explanation`, `url`, and `date`.

### 🐘 PostgreSQL – Data Warehouse

* Stores the transformed data for analytics or visualization.
* Managed inside a **Docker container**, with persistent storage via Docker volumes.
* Accessed via Airflow's `PostgresHook` for flexible query execution.

---

## 🎯 Objectives

✅ **Extract** daily data from the NASA APOD API
✅ **Transform** the JSON response into a structured format
✅ **Load** cleaned data into a PostgreSQL table
✅ **Automate** the entire process using Airflow’s scheduler

---

## 🧱 System Architecture Diagram

```
                         +---------------------+
                         |   Airflow Scheduler |
                         +----------+----------+
                                    |
                    +---------------+---------------+
                    |                               |
         +----------v----------+         +----------v----------+
         | SimpleHttpOperator  |         |  PostgresOperator   |
         | (Extract from NASA) |         | (Create Table if    |
         +----------+----------+         | not exists)         |
                    |                    +----------+----------+
         +----------v----------+                   |
         |  Transform Task     |                   |
         | (@task decorator)   |                   |
         +----------+----------+                   |
                    |                    +----------v----------+
                    +-------------------> PostgresHook         |
                                         | (Insert Transformed  |
                                         |   Data into Table)   |
                                         +----------------------+
```

---

## 🗂️ ETL Workflow Breakdown

### 1️⃣ **Extract**

* Scheduled via Airflow’s cron syntax (`0 0 * * *`).
* `SimpleHttpOperator` makes a `GET` request to the NASA APOD API.
* Captures and stores the JSON response for downstream tasks.

### 2️⃣ **Transform**

* Decorated with `@task` using TaskFlow API for clarity.
* Parses the JSON and extracts necessary fields.
* Ensures data types and structure are suitable for insertion into SQL.

### 3️⃣ **Load**

* Uses `PostgresHook` for secure, parameterized inserts.
* Auto-creates the `apod_data` table if it doesn’t exist.
* Inserts data with transactional integrity.

---

## 🐳 Why Docker?

* **Reproducibility:** All services (Airflow, Postgres, Scheduler, Web UI) are containerized.
* **Isolation:** Each service runs in its own environment.
* **Persistence:** Docker volumes retain Postgres data across container restarts.

---

## 🧠 Possible Extensions

* Add image download automation using the URL from the API.
* Visualize APOD data using Superset or Grafana.
* Use a different API (e.g., SpaceX launches) for pipeline flexibility.
* Schedule error notifications via Slack/Email using Airflow’s alerting features.

