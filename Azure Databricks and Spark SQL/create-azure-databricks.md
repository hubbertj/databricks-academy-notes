# Create Azure Databricks

## Create an Azure Databricks Workspace

1. Go to the **Azure Portal** → search for **Azure Databricks** → click **Create**
2. Fill in the basics:
   - **Subscription** — select your subscription
   - **Resource Group** — create new or use existing
   - **Workspace Name** — globally unique name
   - **Region** — pick the same region as your ADLS Gen2 storage
   - **Pricing Tier** — `Standard`, `Premium` (required for RBAC, Unity Catalog), or `Trial`
3. Click **Review + Create** → **Create**
4. Once deployed, click **Launch Workspace**

---

## Create a Cluster

1. In the Databricks workspace, go to **Compute** → **Create Cluster**
2. Configure:
   - **Cluster Mode** — `Single Node` (dev) or `Multi Node` (production)
   - **Databricks Runtime** — pick a runtime (e.g., `13.3 LTS` for stability)
   - **Node Type** — e.g., `Standard_DS3_v2`
   - **Auto-termination** — set to ~30 min to save cost
3. Click **Create Cluster**

---

## Connect to ADLS Gen2 Storage

The most common method for Databricks on Azure is a **Service Principal**:

1. **Register an App** in Azure AD (Entra ID) → note the **Client ID** and **Tenant ID**
2. Create a **Client Secret** → note the secret value
3. Assign the Service Principal the **Storage Blob Data Contributor** role on your ADLS account
4. In Databricks, configure Spark with:

```python
spark.conf.set("fs.azure.account.auth.type.<storage>.dfs.core.windows.net", "OAuth")
spark.conf.set("fs.azure.account.oauth.provider.type.<storage>.dfs.core.windows.net",
               "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
spark.conf.set("fs.azure.account.oauth2.client.id.<storage>.dfs.core.windows.net", "<client-id>")
spark.conf.set("fs.azure.account.oauth2.client.secret.<storage>.dfs.core.windows.net", "<client-secret>")
spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage>.dfs.core.windows.net",
               "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
```

Replace `<storage>`, `<client-id>`, `<client-secret>`, and `<tenant-id>` with your values.
