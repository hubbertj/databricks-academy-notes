# Databricks Notebooks

## What is a Notebook?

A notebook is an interactive document that combines **live code**, **output**, and **markdown** in a sequence of cells. It is the primary way to write and run code in Databricks.

---

## Default Language

Each notebook has a **default language** set at creation (Python, SQL, Scala, or R). Every cell runs in that language unless overridden with a magic command.

---

## Cell Types & Magic Commands

Magic commands override the default language for a single cell:

| Magic | Language |
|---|---|
| `%python` | Python |
| `%sql` | SQL |
| `%scala` | Scala |
| `%r` | R |
| `%md` | Markdown (documentation, not executed) |
| `%sh` | Shell commands (runs on driver node only) |
| `%fs` | Shorthand for `dbutils.fs` commands (file system operations) |
| `%run` | Runs another notebook inline, importing its variables/functions |

---

## Running Cells

- **Run cell** — `Shift + Enter` or click the play button on the cell
- **Run all** — toolbar → **Run All**
- **Run above / Run below** — right-click a cell for targeted execution
- Cells share state — variables defined in one cell are available in all subsequent cells within the same session
- Output appears directly below the cell (tables, charts, print statements, errors)

---

## Attaching to a Cluster

- A notebook must be **attached to a running cluster** to execute code
- Use the cluster dropdown in the top-right corner to attach/detach
- If the cluster is terminated, you can start it from the same dropdown
- Detaching clears the notebook's in-memory state (variables, imports, cached data)

---

## dbutils

`dbutils` is a Databricks utility library available in Python and Scala notebooks:

| Module | Purpose |
|---|---|
| `dbutils.fs` | File system operations (list, copy, move, delete files in DBFS/ADLS) |
| `dbutils.secrets` | Retrieve secrets from a secret scope (avoids hardcoding credentials) |
| `dbutils.widgets` | Create interactive input widgets (dropdowns, text boxes) in notebooks |
| `dbutils.notebook` | Call other notebooks and pass/return values (`dbutils.notebook.run()`) |

```python
# List files in DBFS
dbutils.fs.ls("/mnt/datalake/")

# Get a secret
token = dbutils.secrets.get(scope="my-scope", key="my-token")

# Run another notebook and get its return value
result = dbutils.notebook.run("/path/to/notebook", timeout_seconds=60, arguments={"param": "value"})
```

---

## Widgets

Widgets make notebooks parameterizable — useful for jobs and interactive exploration:

```python
dbutils.widgets.text("date", "2024-01-01", "Run Date")
dbutils.widgets.dropdown("env", "dev", ["dev", "staging", "prod"], "Environment")

# Read widget values
date = dbutils.widgets.get("date")
env = dbutils.widgets.get("env")
```

Widgets appear as interactive controls at the top of the notebook.

---

## Notebook Workflows with `%run`

`%run` executes another notebook in the **same session**, making its functions and variables available:

```python
%run ./utils/helper-functions
# Now you can call functions defined in helper-functions notebook
```

- Unlike `dbutils.notebook.run()`, `%run` shares the calling notebook's state
- Useful for splitting reusable code into separate notebooks

---

## Revision History

- Databricks auto-saves notebooks and tracks revisions
- Access via **File → Revision History**
- Restore any previous version or view a diff between versions
- Not a replacement for Git — use Repos for proper version control

---

## Exporting Notebooks

Notebooks can be exported from **File → Export** as:

| Format | Notes |
|---|---|
| `.ipynb` | Jupyter notebook — compatible with VS Code, JupyterLab |
| `.dbc` | Databricks archive — preserves formatting, re-importable |
| `.html` | Read-only rendered view |
| `.py` / `.sql` | Source file only, no outputs |

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Shift + Enter` | Run cell and move to next |
| `Ctrl + Enter` | Run cell in place |
| `Esc` | Enter command mode |
| `A` (command mode) | Insert cell above |
| `B` (command mode) | Insert cell below |
| `D D` (command mode) | Delete cell |
| `M` (command mode) | Convert cell to Markdown |
