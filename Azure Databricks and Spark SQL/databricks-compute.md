# Databricks Compute

## Overview

Compute in Databricks refers to the **clusters** and **SQL warehouses** that execute your code. You pay for compute while it is running — choosing the right type and size directly impacts cost and performance.

---

## Compute Types

| Type | Used For |
|---|---|
| **All-Purpose Cluster** | Interactive notebooks, ad-hoc exploration, development |
| **Job Cluster** | Automated jobs — spun up for a job run, terminated when done |
| **SQL Warehouse** | SQL Editor, dashboards, BI tool connections |
| **Instance Pool** | Pre-warmed VMs shared across clusters to reduce start time |

---

## All-Purpose Clusters

Used when working interactively in notebooks.

### Cluster Modes

| Mode | Description |
|---|---|
| **Single Node** | Driver only, no workers — good for small data, ML model development |
| **Multi Node** | Driver + 1 or more workers — distributed Spark processing |

### Key Configuration Options

- **Databricks Runtime (DBR)** — the software stack on the cluster (Spark version, Python version, pre-installed libraries). Use an **LTS** (Long Term Support) version for stability.
- **Node Type** — Azure VM SKU for driver and worker nodes (e.g., `Standard_DS3_v2`)
- **Autoscaling** — automatically adds/removes workers based on workload (set min and max workers)
- **Auto-termination** — terminates the cluster after N minutes of inactivity to save cost
- **Spot Instances** — use Azure Spot VMs for workers to reduce cost (risk of eviction)

### Cluster States

| State | Meaning |
|---|---|
| `Running` | Active and ready |
| `Terminated` | Shut down — no cost incurred |
| `Pending` | Starting up |
| `Resizing` | Autoscaling in progress |
| `Error` | Failed to start |

---

## Job Clusters

- Created automatically when a **Job** runs, terminated when the run completes
- Cheaper than keeping an all-purpose cluster running
- Configuration is defined in the Job task settings
- Cannot be used interactively

---

## Databricks Runtime (DBR)

The runtime determines the version of Spark, Python, and pre-installed libraries on the cluster.

| Runtime Variant | Description |
|---|---|
| **Standard** | Core Spark + Python/Scala/R |
| **ML** | Adds ML libraries: TensorFlow, PyTorch, scikit-learn, MLflow |
| **Photon** | Optimized query engine for SQL/Delta workloads — faster for SQL-heavy workloads |
| **GPU** | GPU-enabled nodes for deep learning |

Always prefer **LTS (Long Term Support)** runtimes in production for stability and longer support windows.

---

## SQL Warehouses

SQL Warehouses are purpose-built compute for SQL workloads — used by the SQL Editor, dashboards, and BI tools (Power BI, Tableau).

| Type | Description |
|---|---|
| **Serverless** | Fully managed by Databricks, instant start, no cluster config needed |
| **Pro** | Runs in your Azure subscription, more control, supports Unity Catalog |
| **Classic** | Legacy — use Pro or Serverless instead |

- Scale using **T-shirt sizes** (2X-Small → 4X-Large) which map to underlying cluster sizes
- **Auto-stop** terminates the warehouse after inactivity
- Multiple users share one warehouse (queries queue or run concurrently)

---

## Instance Pools

- A pool of **pre-warmed Azure VMs** that clusters can draw from
- Reduces cluster start time from ~5 minutes to ~30 seconds
- Clusters in the pool keep VMs on standby (you pay for idle VMs in the pool)
- Best for teams that frequently start/stop clusters

---

## Autoscaling

When enabled on a multi-node cluster:

- Databricks monitors the **backlog of pending tasks**
- Adds workers when tasks are queued (scale out)
- Removes idle workers after a cooldown period (scale in)
- Set **min workers** (always running) and **max workers** (upper limit)

> Autoscaling is not always faster — for steady, predictable workloads a fixed-size cluster can outperform autoscaling due to scale-out latency.

---

## Cost Considerations

| Practice | Impact |
|---|---|
| Use **auto-termination** | Prevents idle clusters running overnight |
| Use **job clusters** for production | Pay only during job execution |
| Use **Spot instances** for workers | Up to 80% cheaper, acceptable for fault-tolerant jobs |
| Use **Serverless SQL Warehouses** | No idle cost, instant start |
| Use **Photon runtime** for SQL | Faster queries = shorter run time = lower cost |
| Right-size node types | Avoid over-provisioning memory/CPU |

---

## Cluster Policies (Premium)

Cluster policies restrict what configuration options users can set when creating clusters:

- Enforce auto-termination
- Limit available VM sizes
- Pre-set the runtime version
- Control spot instance usage

Useful for cost governance and preventing misconfigured clusters in shared workspaces.
