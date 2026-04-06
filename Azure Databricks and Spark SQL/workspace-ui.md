# Databricks Workspace UI

## Layout Overview

The Databricks UI is split into two main areas:

- **Left sidebar** — navigation between sections
- **Main panel** — content for the selected section

---

## Left Sidebar Navigation

| Icon / Section | Purpose |
|---|---|
| **Home** | Your personal folder in the workspace browser |
| **Workspace** | File browser for all notebooks, folders, and repos |
| **Repos** | Git-integrated folders for version-controlled notebooks |
| **Recents** | Recently opened notebooks and files |
| **Data** | Browse databases, tables, and schemas (Hive metastore or Unity Catalog) |
| **Compute** | Create and manage clusters and SQL warehouses |
| **Workflows** | Create and monitor Jobs and Delta Live Tables pipelines |
| **SQL Editor** | Write and run SQL queries against tables |
| **Dashboards** | Build and view visualizations |
| **Marketplace** | Databricks Marketplace for datasets and solutions |
| **Settings** | Workspace settings, user management, tokens |

---

## Workspace Browser

- Organized as a **folder tree** — personal folder at `/Users/<your-email>/`, shared content at `/Shared/`
- Right-click any item to: rename, move, clone, delete, set permissions, or export
- Notebooks show their **default language** (Python, SQL, Scala, R) as a badge
- **Repos** appear as a separate section but are also visible in the Workspace browser under `/Repos/`

---

## Notebooks

- Each notebook cell can run independently or all cells run top-to-bottom with **Run All**
- Switch cell language per-cell with a magic command: `%python`, `%sql`, `%scala`, `%r`, `%md`
- **Attach to cluster** — top-right dropdown; a notebook must be attached to a running cluster to execute
- Results, charts, and tables appear inline below each cell
- **Revision history** — accessible via **File → Revision History**; shows diffs and allows restore

---

## Compute Page

- Lists all **interactive clusters** (used by notebooks) and **job clusters** (spun up by jobs)
- Cluster states: `Running`, `Terminated`, `Pending`, `Error`
- Click a cluster to see: configuration, Spark UI, driver logs, metrics, and event log
- **SQL Warehouses** (formerly SQL Endpoints) are listed separately — serverless compute for SQL Editor and dashboards

---

## Data Explorer

- Browse **catalogs → databases → tables** (Unity Catalog hierarchy) or **databases → tables** (Hive metastore)
- Click a table to see: schema (column names + types), sample data, table details (location, format, size), and history (Delta tables only)
- Upload CSV/JSON files directly to create tables via the **Create Table** UI

---

## SQL Editor

- Multi-tab SQL editor with autocomplete for table/column names
- Results displayed as a table with the option to create a **visualization** (bar, line, pie, etc.)
- Queries can be saved and shared
- Runs against a **SQL Warehouse**, not an interactive cluster

---

## Workflows (Jobs)

- Create multi-task jobs with a **DAG editor** — drag tasks and define dependencies
- Supported task types: Notebook, Python script, JAR, Delta Live Tables pipeline, SQL, dbt
- Monitor runs in the **Job Runs** tab — see status, duration, and logs per task

---

## Settings (Admin)

- **Users & Groups** — add/remove workspace users, assign admin roles
- **Personal Access Tokens** — generate tokens for REST API or CLI auth
- **Cluster Policies** — restrict what cluster configs non-admin users can set
- **Instance Pools** — pre-warmed VM pools to speed up cluster starts
