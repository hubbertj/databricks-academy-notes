## What is a datalake house? 
A data lakehouse is a data architecture that combines the best traits of data lakes and data warehouses:

Data Lake	Data Warehouse	Lakehouse
Storage	Raw files (cheap)	Structured tables (expensive)	Raw files (cheap)
Structure	Schema-on-read	Schema-on-write	Both
ACID transactions	No	Yes	Yes
BI/SQL support	Limited	Yes	Yes
ML/unstructured data	Yes	Poor	Yes
Key idea: Store data in open file formats (like Parquet) on cheap object storage (S3, ADLS), but add a metadata/transaction layer on top that gives you warehouse-like features — ACID guarantees, versioning, indexing, and SQL query optimization.

In the Databricks world, this is implemented via Delta Lake — an open-source storage layer that adds:

ACID transactions to Parquet files
Time travel (query previous versions)
Schema enforcement and evolution
Efficient upserts/deletes (MERGE, UPDATE, DELETE)
So instead of maintaining separate systems for raw data (lake) and analytics (warehouse), you have one unified platform.


## Azure offers four data redundancy strategies, trading cost for durability/availability:

Option	Full Name	Copies	Where
LRS	Locally Redundant Storage	3	Single datacenter
ZRS	Zone Redundant Storage	3	3 availability zones, same region
GRS	Geo-Redundant Storage	6	3 local + 3 in a paired region
GZRS	Geo-Zone Redundant Storage	6	3 zones local + 3 in paired region
Read-access variants (GRS → RA-GRS, GZRS → RA-GZRS) let you read from the secondary region without waiting for a failover.

For a Databricks/data lakehouse context:

LRS — cheapest, fine for dev/test or easily-reproducible data
ZRS — good for production workloads where regional zone failures are a concern
GRS/RA-GRS — for business-critical data that must survive a full regional outage
GZRS — highest durability, use when you need both zone and geo protection
For ADLS Gen2 (the typical backing store for Databricks on Azure), LRS or ZRS is the most common choice for cost reasons, with GRS reserved for disaster recovery requirements.