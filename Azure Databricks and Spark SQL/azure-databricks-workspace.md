# Azure Databricks Workspace

## What is a Workspace?

A **workspace** is the top-level environment in Azure Databricks. It is the logical container for all your Databricks assets:

- Notebooks
- Clusters
- Jobs
- Pipelines (Delta Live Tables)
- Repos (Git integration)
- Data (tables, schemas via Unity Catalog or Hive metastore)
- Secrets

Each workspace is tied to a single Azure region and a single Azure resource group. Teams or environments (dev/staging/prod) typically get their own workspace.

---

## Workspace Pricing Tiers

| Tier | Use Case | Notable Features |
|---|---|---|
| **Standard** | Basic workloads | Notebooks, clusters, jobs |
| **Premium** | Enterprise | RBAC, Unity Catalog, cluster policies, audit logs |
| **Trial** | 14-day free trial | Full Premium features |

> Unity Catalog (centralized data governance) requires **Premium** tier.

---

## How to Create a Workspace

### Prerequisites
- An Azure subscription
- Contributor or Owner role on the subscription/resource group

### Steps

1. In the **Azure Portal**, search for **Azure Databricks** and click **Create**
2. Fill in the **Basics** tab:
   - **Subscription** — your Azure subscription
   - **Resource Group** — create new (e.g., `rg-databricks-dev`) or use existing
   - **Workspace Name** — must be unique within the resource group
   - **Region** — choose the region closest to your data sources
   - **Pricing Tier** — select `Premium` for production use
3. (Optional) **Networking** tab:
   - By default, Databricks creates its own managed VNet
   - Enable **VNet Injection** if you need the cluster nodes inside your own VNet (required for private endpoints, custom DNS, firewall rules)
4. Click **Review + Create** → **Create**
5. Deployment takes ~2–5 minutes
6. Once complete, click **Go to Resource** → **Launch Workspace**

---

## What Gets Created in Azure

When you create a workspace, Azure provisions:

| Resource | Purpose |
|---|---|
| **Databricks Workspace** | The workspace resource itself |
| **Managed Resource Group** | Auto-created, contains the managed VNet, NSGs, and storage used internally by Databricks |
| **DBFS (Databricks File System)** | Default distributed file system backed by Azure Blob Storage — used for cluster logs, libraries, and temp data |

> DBFS is **not** recommended for production data storage. Use ADLS Gen2 instead.

---

## Key Workspace Concepts

- **Notebooks** — interactive documents mixing code (Python, SQL, Scala, R) with output and markdown
- **Clusters** — the compute that runs your notebooks and jobs (see `create-azure-databricks.md`)
- **Jobs** — scheduled or triggered runs of notebooks/scripts
- **Repos** — Git-backed folders for version-controlled notebooks
- **Secrets** — encrypted key-value store for credentials (avoid hardcoding secrets in notebooks)

---

## Access Control (Premium only)

- **Workspace-level**: Managed via Azure AD (Entra ID) — add users/groups to the workspace
- **Object-level**: Control who can view/edit/run individual notebooks, folders, clusters, and jobs
- **Data-level**: Unity Catalog provides fine-grained table/column/row access control
