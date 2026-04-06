# Databricks Workspace Objects

## Overview

Workspace objects are the assets stored inside a Databricks workspace. They live in the **Workspace browser** (the folder tree) and are organized under:

- `/Users/<email>/` — personal folder, only visible to you by default
- `/Shared/` — shared folder, accessible to all workspace users
- `/Repos/` — Git-backed folders (version-controlled)

---

## Object Types

| Object | Description |
|---|---|
| **Notebook** | Interactive document with code, output, and markdown cells |
| **Folder** | Container for organizing other workspace objects |
| **Repo** | A Git repository clone — contains version-controlled notebooks and files |
| **Library** | A package (PyPI, Maven, CRAN, or custom) installed on a cluster |
| **Dashboard** | A read-only view of notebook visualizations or SQL charts |
| **MLflow Experiment** | Tracks ML training runs, parameters, metrics, and artifacts |
| **Model** | A registered ML model in the MLflow Model Registry |

---

## Notebooks

- The most common workspace object
- Tied to the workspace (not a cluster) — persist independently of compute
- See `databricks-notebooks.md` for full details

---

## Folders

- Used to organize notebooks, other folders, and files
- Permissions can be set at the folder level and inherited by contents
- Deleting a folder deletes all objects inside it

---

## Repos

- A **Repo** is a clone of a remote Git repository (GitHub, Azure DevOps, GitLab, Bitbucket)
- Notebooks inside a Repo are files on disk (`.py`, `.sql`, `.ipynb`) — not workspace-native objects
- Supports `pull`, `push`, `commit`, `branch`, `merge`, and `tag` operations from the UI
- Best practice for team development and CI/CD pipelines

```
/Repos/
  <your-email>/
    my-project/        ← cloned Git repo
      notebooks/
      src/
```

---

## Libraries

Libraries extend the default cluster environment with additional packages:

| Source | Example |
|---|---|
| PyPI | `pandas`, `scikit-learn` |
| Maven | JVM packages (e.g., Spark connectors) |
| CRAN | R packages |
| Custom (DBFS/ADLS) | `.whl`, `.jar`, or `.egg` files you upload |

Libraries can be installed at two levels:
- **Cluster-level** — installed once on the cluster, available to all notebooks attached to it
- **Notebook-level** — installed in the notebook session using `%pip install <package>` (recommended for reproducibility)

```python
# Notebook-level install (resets on cluster restart)
%pip install great-expectations==0.18.0
```

---

## Permissions Model

Workspace objects support fine-grained access control (Premium tier required):

| Permission Level | What It Allows |
|---|---|
| **No Permissions** | Object is not visible |
| **Can Read** | View and run (notebooks), view (dashboards) |
| **Can Run** | Run notebooks without editing them |
| **Can Edit** | Modify the object |
| **Can Manage** | Change permissions, rename, delete |

- Permissions are set per object or per folder (inherited by contents)
- Set via right-click → **Permissions** in the Workspace browser
- Managed through **Azure AD (Entra ID)** users and groups

---

## Object Lifecycle

- Workspace objects (notebooks, folders) are **persistent** — they survive cluster termination
- Deleting an object moves it to **Trash** (recoverable for 30 days) before permanent deletion
- Repos follow Git lifecycle — changes must be committed and pushed to persist outside Databricks

---

## Key Differences: Workspace Notebooks vs Repo Notebooks

| | Workspace Notebook | Repo Notebook |
|---|---|---|
| Storage | Databricks internal store | Git repository |
| Version control | Revision history only | Full Git history |
| Collaboration | Permission-based sharing | Git branching/PRs |
| CI/CD | Manual export | Native Git integration |
| File format | Proprietary | `.py`, `.sql`, `.ipynb` |

For production workloads, **Repos** are preferred over workspace notebooks.
